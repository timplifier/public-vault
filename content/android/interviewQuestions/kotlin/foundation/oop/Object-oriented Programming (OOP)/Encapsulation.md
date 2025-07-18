Let’s imagine that we have following variables and functions.

```kotlin
var age: Double = 18.0
var weight: Double = 55.0
var height: Double = 166.0

fun eat() {
    if (doesWantToEat()) {
        weight += 0.5f
        println("Om nom nom")
    }

}

fun sleep() {
    age += 0.01

    if (Random.nextBoolean()) {
        height += 0.1f
    }
    println("A human being has just fell asleep")
}

fun doesWantToEat(): Boolean = Random.nextBoolean()
```

It might look okay for a beginner, who is not familiar with OOP. But, the variables are global so are functions, we can access all of them in any place. Consequently, any project maintainer can use it without any problems. That’s not what we are meant to do with **Encapsulation**. The first concept of **Encapsulation** is uniting variables and functions into single object, in order to create them using some kind of blueprint, like this:

```kotlin
class Human(private var age: Double, private var weight: Double, private var height: Double) {

    fun getAge(): Double = age
    
    fun getWeight(): Double = weight

    fun getHeight(): Double = height

    fun eat() {
        weight += 0.5f
        println("Om nom nom")
    }

    fun sleep() {
        age += 0.01

        if (Random.nextBoolean()) {
            height += 0.1f
        }
        println("A human being has just fell asleep")
    }

    private fun doesWantToEat(): Boolean = Random.nextBoolean()
}
```

Now, we are able to construct a human class as an object using our blueprint. We have to pass the age, weight and height into class’s constructor in order to create our class. Also, we can access all the parameters, `eat()` and `sleep()` functions. However, we can’t access `age`, `weight`, `height` fields and`doesWantToEat()` function, because it is annotated with `private` modifier, which makes it accessible only for `Human` class. At this moment, new maintainer can functions that are not `private`. In the end, we united the variables and functions into a single unit, that is called `class`.

## How to achieve Encapsulation

**Encapsulation** is basically achievable by:

- Defining a unit for fields and functions that have some link with each other.
- Annotating fields and functions with access modifiers.

### Defining a unit for fields and functions that have some link with each other.

Every time we face fields and functions that are related to each other, we should and must create a class that holds the data they provide, like we did in previous example. This in exchange makes the structure clear, coherent and more readable. If we have a class, we can read its name and understand what it is. Just by looking at functions’ name you can know their purpose, but not what they are used for.

### Annotating fields and functions with access modifiers.

We have to control which fields and functions are accessible and which are not. The point of this is to not expose the data that should not be used in the wrong place. Let’s take a look at `age`, `weight`, `height` and `doesNotWantToEat()`.

```kotlin
class Human(private var age: Double, private var weight: Double, private var height: Double) {

    fun getAge(): Double = age
    
    fun getWeight(): Double = weight

    fun getHeight(): Double = height

    fun eat() {
        weight += 0.5f
        println("Om nom nom")
    }

    fun sleep() {
        age += 0.01

        if (Random.nextBoolean()) {
            height += 0.1f
        }
        println("A human being has just fell asleep")
    }

    private fun doesWantToEat(): Boolean = Random.nextBoolean()
}
```

So they are annotated with `private` because they are supposed to be used only in `Human` class. If it is`public`, it could be used in the wrong places where it shouldn’t be used at all. Junior programmer can use it to do some shit, what we don’t actually want. However, `age`, `weight` and `height` are mutable so we don’t want to make it modified directly. They can be modified via `eat()` and `sleep()` functions. But we still can get their value by using `getAge()`, `getWeight()` and `getHeight()`.

```kotlin
@HiltViewModel
class CharactersViewModel @Inject constructor(
    private val fetchCharactersUseCase: FetchCharactersUseCase,
    private val fetchSingleEpisodeUseCase: FetchSingleEpisodeUseCase,
    private val getCharactersUseCase: GetCharactersUseCase,
    private val getSingleEpisodeUseCase: GetSingleEpisodeUseCase
) :
    BaseViewModel() {

    private val _localCharactersState = MutableStateFlow<List<CharacterUI>>(emptyList())
    val localCharactersState = _localCharactersState.asStateFlow()

    private val _searchQueryState = MutableStateFlow<String?>(null)
    val searchQueryState = _searchQueryState.asStateFlow()

    fun fetchCharacters(
        status: String? = null,
        species: String? = null,
        gender: String? = null
    ) =
        fetchCharactersUseCase(
            _searchQueryState.value,
            status,
            species,
            gender
        ).gatherPagingRequest {
            it.toUI()
        }

    fun fetchSingleEpisode(id: Int) =
        fetchSingleEpisodeUseCase(id)

    fun getLocalCharacters(
        status: String? = null,
        species: String? = null,
        gender: String? = null
    ) {
        viewModelScope.launch {
            getCharactersUseCase(_searchQueryState.value, status, species, gender).collectLatest {
                _localCharactersState.value = it.map { characterModel -> characterModel.toUI() }
            }
        }
    }

    fun getSingleEpisode(url: String) = getSingleEpisodeUseCase(url)

    fun modifySearchQuery(newQuery: String?) {
        _searchQueryState.value = newQuery
    }
}
```

In this case, we don’t want to expose our `UseCases`. You can learn more about it in [Clean Architecture](https://www.notion.so/Clean-Architecture-69b6d8dbe62647d2b8a2169f62dd5d55?pvs=21), but for now it doesn’t matter. The point is that it should be used only in `CharactersViewModel`, so they are not accessible from outside the class. Talking about states, we don’t want to expose mutable versions, because their value can be modified, we want to expose only immutable versions, because they are not mutable of course. It is related to `_localCharactersState` and `_searchQueryState`. Mutable versions of state usually have `_` as prefix.

```kotlin
abstract class BaseViewModel<State : Any, SideEffect : Any>(initialState: State) :
    ContainerHost<State, SideEffect>, ViewModel() {
    protected fun <T> mutableUiStateFlow() = MutableStateFlow<UIState<T>>(UIState.Idle())

    override val container = container<State, SideEffect>(initialState = initialState)

    fun postSideEffect(sideEffect: SideEffect) = intent {
        postSideEffect(sideEffect)
    }

    protected fun <T, S> Flow<Either<String, T>>.gatherRequest(
        state: MutableStateFlow<UIState<S>>,
        mappedData: (data: T) -> S,
    ) {
        viewModelScope.launch(Dispatchers.IO) {
            state.value = UIState.Loading()
            this@gatherRequest.collect {
                when (it) {
                    is Either.Left -> state.value = UIState.Error(it.value)
                    is Either.Right -> state.value =
                        UIState.Success(mappedData(it.value))
                }
            }
        }
    }

    protected fun <T> Flow<Either<String, T>>.gatherRequest(
        state: MutableStateFlow<UIState<T>>,
    ) {
        viewModelScope.launch(Dispatchers.IO) {
            state.value = UIState.Loading()
            this@gatherRequest.collect {
                when (it) {
                    is Either.Left -> state.value = UIState.Error(it.value)
                    is Either.Right -> state.value =
                        UIState.Success(it.value)
                }
            }
        }
    }

    protected fun <T : Any, S : Any> Flow<PagingData<T>>.gatherPagingRequest(
        mappedData: (data: T) -> S,
    ) = map {
        it.map { data -> mappedData(data) }
    }.cachedIn(viewModelScope)
```

So we have some kind of foundation class for `ViewModel`, we annotate functions with `protected` so it could be used only by its children. So if we create an instance of `CharactersViewModel`, those functions are not accessible.