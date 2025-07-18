[[Object]] cloning is the process of creating the exact copy of the Object with a new instance of the class and therefore all the fields.

There are three methods of cloning

## 1. `=` operator

Let's say that we have the following class
```java
public class Rectangle {
    public int x;
    public int y;

    public Rectangle(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Then we want to copy
**`Input`**

```java
public class Main {
    public static Rectangle firstRectangle = new Rectangle(10, 20);
    public static Rectangle clonedRectangle = firstRectangle;

    public static void main(String[] args) {

        clonedRectangle.x = 30;
        clonedRectangle.y = 50;
        System.out.println(firstRectangle.x + " " + firstRectangle.y);
        System.out.println(clonedRectangle.x + " " + clonedRectangle.y);
    }
}
```

**`Output`**

```java
30 50
30 50
````

And why is that ? Because:
**`=` operator doesn't create a copy of an Object, it creates a copy of  a [[Reference]]**. So, due to usage of `Reference`, any changes applied to the `clonedRectangle` in our case will be reflected on the `firstRectangle`.

## 2. `clone()`
As you can see from the name of the method, it actually copies the Object itself, not the `Reference`. However, it requires bare minimum configuration to work:
1. Make your class `implements` `Cloneable` interface
2. `override` `clone()` method in your class.
```java
@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}
```

So our `Rectangle` class looks like this
```java
public class Rectangle implements Cloneable {
    public int x;
    public int y;

    public Rectangle(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

So our previous clone example could be refined to this:
**`Input`**
```java
public class Main {
    public static Rectangle firstRectangle = new Rectangle(10, 20);

    public static void main(String[] args) throws CloneNotSupportedException {
        Rectangle clonedRectangle = (Rectangle) firstRectangle.clone();

        clonedRectangle.x = 30;
        clonedRectangle.y = 50;
        System.out.println(firstRectangle.x + " " + firstRectangle.y);
        System.out.println(clonedRectangle.x + " " + clonedRectangle.y);
    }
}
```

**`Output`**
```java
10 20
30 50
```