## 1. Difference between abstract class concrete class and interface class with example java?



In Java, abstract classes, concrete classes, and interfaces are different constructs used for defining classes and their behavior. Here's an explanation of each along with an example:

#### Abstract Class:

<!-- An abstract class is a class that cannot be instantiated on its own and serves as a blueprint for other classes.
It can contain both abstract and non-abstract methods.
Abstract methods are declared without an implementation and must be implemented by any non-abstract subclass.
It can also have regular methods with implementations.
Abstract classes are useful when you want to provide a common interface for a group of related classes. -->
An abstract class is a class that cannot be instantiated on its own and serves as a blueprint for other classes. It can contain both abstract and non-abstract methods. Abstract methods are declared without an implementation and must be implemented by any non-abstract subclass. It can also have regular methods with implementations. Abstract classes are useful when you want to provide a common interface for a group of related classes.

#### Example:

```
abstract class Animal {
    public abstract void makeSound();
    
    public void sleep() {
        System.out.println("Zzz");
    }
}

class Dog extends Animal {
    public void makeSound() {
        System.out.println("Woof");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.makeSound();
        animal.sleep();
    }
}
```
#### Output:
```
Woof
Zzz
```

#### Concrete Class:

A concrete class is a regular class that can be instantiated directly. It can have both abstract and non-abstract methods, but all the abstract methods defined in its parent abstract class must be implemented. Concrete classes provide the actual implementation of the methods defined in their parent classes. They are used to create objects that have specific behavior and attributes.

#### Example:


```
abstract class Vehicle {
    public abstract void start();
}

class Car extends Vehicle {
    public void start() {
        System.out.println("Car started");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle vehicle = new Car();
        vehicle.start();
    }
}

```
#### Output:
```
Car started

```

#### Interface

An interface is a collection of abstract methods that defines a contract for classes to adhere to. It cannot be instantiated directly; instead, classes implement interfaces to provide their own implementation of the interface methods. An interface can contain constant fields (public static final) and default methods (with implementations) since Java 8. It is useful when you want to enforce certain behavior across unrelated classes.

#### Example:



```
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

public class Main {
    public static void main(String[] args) {
        Shape shape = new Circle();
        shape.draw();
    }
}


```
#### Output:
```
Drawing a circle
```
In summary, abstract classes provide a common interface and may have implemented methods, concrete classes are regular classes that provide the actual implementation, and interfaces define contracts for classes to implement.

## 2. Explain android activity with example?
In Android, an activity is a core component of an application that represents a single screen with a user interface (UI). It serves as a container for UI elements such as buttons, text fields, and images, and handles user interactions and events. Activities are used to present different screens to the user, allowing them to navigate through different parts of the application.

Each activity in an Android application is implemented as a subclass of the Activity class. Activities have a lifecycle that defines their behavior and state as they are created, started, paused, resumed, stopped, and destroyed. The activity lifecycle is managed by the Android system, and developers can override specific methods to customize the behavior and respond to lifecycle events.

Activities have several important responsibilities:

1. **UI Presentation:** An activity is responsible for presenting the UI to the user. It defines the layout and arrangement of UI elements on the screen and handles user interactions such as button clicks or text input.

2. **Lifecycle Management:** Activities manage their own lifecycle, which includes methods for creation, starting, pausing, resuming, stopping, and destroying. Developers can override these methods to perform specific actions at each lifecycle stage, such as initializing resources, saving and restoring state, or releasing system resources.

3. **Inter-Activity Communication:** Activities can communicate with each other using intents. An intent is a message that allows activities to request actions from other activities or pass data between them. For example, one activity can start another activity to perform a specific task or provide data.

4. **Backstack Management:** The Android system maintains a backstack of activities that allows users to navigate backward through the screens they have visited. Activities can be added to the backstack, removed from it, or reordered to control the navigation flow.

5. **Result Handling:** Activities can start other activities for a result. For example, an activity can start a "pick contact" activity and receive the selected contact as a result. This mechanism allows activities to interact and exchange data with each other.

6. **Permissions:** Activities can require permissions to perform certain actions, such as accessing the internet or reading the user's contacts. Permissions are requested from the user at runtime and can be granted or denied by the user.

Here's an example of an activity with a layout file:
1. **activity_main.xml** (layout file):
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, Android Activity!"
        android:textSize="24sp" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

</LinearLayout>
```
2. **MainActivity.java** (activity class):
```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(MainActivity.this, "Button clicked", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```
In this example, the `activity_main.xml` layout file defines a `LinearLayout` with a `TextView` and a `Button`. The `MainActivity` class extends `AppCompatActivity` and overrides the `onCreate()` method. In `onCreate()`, the layout is inflated using `setContentView(R.layout.activity_main)`. The `Button` is initialized using `findViewById(R.id.button)`, and an `OnClickListener` is set to display a toast message when the button is clicked.

This example demonstrates how an activity can use a layout file to define the UI elements and how the activity class can interact with those elements by referencing their IDs and setting event listeners.


## 3. Explain manifest file in android with example ?
The manifest file (AndroidManifest.xml) is an essential configuration file for an Android application.This file contains information of your package, including components of the application such as activities, services, broadcast receivers, content providers etc. It performs some other tasks also: 
1. `<manifest>:` This is the root element of the manifest file. It contains attributes like package name and version code.
2.`<uses-permission>:` This tag is used to declare the permissions required by the application, such as accessing the internet, reading contacts, or using the camera.
3. `<application>:` This tag encapsulates the entire application and contains attributes and child elements related to the application.
4. `<activity>:` Represents an activity (screen) in your application. It defines the name of the activity, its launch mode, intent filters, and more.
5. ` <service>:` Declares a service component in your application, which runs in the background to perform tasks independently of any user interface.
6. `<receiver>:` Defines a broadcast receiver component in your application, which listens for system-wide or application-specific broadcast events.
7. `<provider>:` Declares a content provider component that allows your application to share data with other applications.
8. `<intent-filter>:` Specifies the type of intents that an activity, service, or receiver can respond to. It includes actions, categories, and data types.
9. `<meta-data>:` This tag allows you to attach metadata to various components in your application, providing additional information or configuration options.

an example of a simple Android manifest file:
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:label="@string/app_name"
        android:icon="@drawable/app_icon">

        <activity
            android:name=".MainActivity"
            android:label="@string/main_activity_title">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```

In this example, the manifest file declares the package name, uses the INTERNET permission, and defines an application with an activity called MainActivity. The activity has an intent filter specifying that it should be launched when the application is started.

