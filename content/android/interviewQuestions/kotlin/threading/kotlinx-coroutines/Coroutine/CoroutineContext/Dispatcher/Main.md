A [[Dispatcher]] that is confined to the main thread operating with UI objects. Usually single-threaded. Doing any work outside manipulation with UI interface is prohibited in order not to freeze or make UI lag. For operations not related to UI, use [[IO]], [[Default]].

In order to access Main Dispatcher, you have to have one of the following artifacts:

- `kotlinx-coroutines-android`
- `kotlinx-coroutines-javafx`
- `kotlinx-coroutines-swing`