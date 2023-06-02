# React Native Basics

# 1) Explain how React Native works under the hood.

React Native is a JavaScript framework for writing real, natively rendering mobile applications for iOS and Android, based on React, Facebook’s JavaScript library for building user interfaces.

Here's how React Native works under the hood:

## JavaScript Thread and Native Thread

React Native uses two separate threads, the JavaScript thread and the Native thread, to execute its operations. User interfaces are rendered through the Native thread, and business logic is executed in the JavaScript thread.

## Bridge

React Native uses a "bridge" to communicate between JavaScript and Native threads. JavaScript does not directly manipulate the native platform. Instead, it communicates the intended actions to the native side, which performs the actions.

## JavaScriptCore

React Native uses JavaScriptCore at runtime to interpret the JavaScript code. On iOS, JavaScriptCore is bundled with the operating system. But on Android, it needs to be bundled with the application.

## React

React Native leverages React and its principles. React components are composed to form a complex UI. When a component's state changes, React decides whether to update the DOM (or in this case, the mobile equivalent). This reduces the complexity of keeping the UI state consistent with the JavaScript state.

## Native Modules

React Native is designed to be “learn once, write anywhere”. For features not yet supported, you can write Native code (Objective-C/Swift for iOS, Java/Kotlin for Android) and then use React Native's bridge to communicate with the JavaScript code. This is done via Native Modules.

## Rendering

React Native communicates the information needed to render the Native Components on the JavaScript side to the Native side. React Native then uses this information to instantiate and render the actual platform-specific Native components on the UI thread.

## Flexbox

React Native uses Flexbox to layout its components, which means that you can use the same layout code for both iOS and Android.

This design allows developers to write mobile applications in JavaScript while still giving the performance and look and feel of a native application. The fact that React Native actually translates your markup to real, native UI elements and leverages existing means of rendering views on whatever platform you are working with gives it a big advantage over other methods of building mobile apps.

# 2) How do you handle state management in React Native? Can you explain the concept of Redux?

State management in React Native can be done in multiple ways, such as using the internal component state or context API provided by React itself, or using external libraries like Redux, MobX, or Recoil.

## Internal Component State

React components have their own internal state. You can create state variables and use the `setState` method (in class components) or `useState` hook (in functional components) to update the state.

```jsx
class ExampleComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      text: "",
    };
  }
  //...
}

// or

function ExampleComponent() {
  const [text, setText] = useState("");
  //...
}
```

## Context API

React’s Context API allows you to share global state across multiple components without resorting to props drilling.

```jsx
const MyContext = React.createContext(defaultValue);

<MyContext.Provider value={/* some value */}>
  <MyComponent />
</Context.Provider>

// Then within MyComponent or any of its children
React.useContext(MyContext)
```

## Redux

Redux is an open-source JavaScript library used for managing the application state. It is often used with React or React Native, but it can be used with any other JavaScript framework or library.

Redux is based on the Flux architecture and increases the predictability of the outcome by restricting the way the state can be updated. It follows three fundamental principles

:

1. **Single source of truth**: The state of the entire application is stored in an object tree within a single store. This makes it easier to keep track of changes over time and debug or inspect the application.
2. **State is read-only**: The only way to change the state is to emit an action, an object describing what happened. This ensures that neither the views nor the network callbacks will ever write directly to the state.
3. **Changes are made with pure functions**: To specify how the state tree is transformed by actions, you write pure reducers. Reducers are just pure functions that take the previous state and an action, and return the next state.

Redux uses actions and reducers to handle state. An action is a plain JavaScript object that describes what happened (like a user clicking a button), and a reducer is a function that decides how to change the state in response to that action.

Here is a basic example of using Redux:

```jsx
import { createStore } from "redux";

// This is a reducer - a function that determines what to do based on the action type
function counter(state = 0, action) {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
}

// Create a Redux store holding the state of your app, with the `counter` function as the reducer
let store = createStore(counter);

// The current state is 0
store.getState();

// You can dispatch actions to the store
store.dispatch({ type: "INCREMENT" });

// After dispatching the INCREMENT action, the state is now 1
store.getState();
```

## MobX and Recoil

Apart from Redux, there are other external libraries like MobX and Recoil that can also be used for state management in React Native, each with its own design and API.

Overall, the choice of state management strategy depends on the needs of the project. For simple projects, React's built-in state management may be enough. For larger projects with more complex state management needs, Redux or other libraries can be a good choice.

# 3) What is the difference between React and React Native?

React and React Native are both open-source projects maintained by Facebook, and they both leverage the power of JavaScript and React. However, they serve different purposes and have some fundamental differences:

**1. Purpose**:

- **React (also known as React.js)** is a JavaScript library for building user interfaces, specifically for web applications. React allows developers to create large web applications that can change data, without reloading the page.

- **React Native** is a mobile framework that compiles to native app components, allowing you to build native mobile applications for different platforms (iOS, Android, and Windows Mobile) in JavaScript that allows you to use React.js to build your components, and implements React.js under the hood.

**2. DOM**:

- **React** uses the Virtual DOM to render browser code in JavaScript. The Virtual DOM is a representation of the actual DOM, which React uses to quickly determine changes and apply them in the most efficient way.

- **React Native**, instead of using the Virtual DOM, uses native views during rendering. This gives a significant performance boost compared to other JavaScript-for-mobile approaches, like Cordova or Ionic.

**3. Styling**:

- In **React**, you can write CSS in JavaScript, or use other approaches like CSS modules, styled-components, or traditional CSS/SASS files.

- In **React Native**, styling is done via a JavaScript abstraction similar to CSS. Flexbox is used for layout, but there's no support for the full range of CSS properties. Stylesheets are created by the `StyleSheet.create` method for improved performance.

**4. Components**:

- **React** uses HTML tags for components like `<div>`, `<span>`, `<section>`, etc.

- **React Native** has its own set of primitive components like `<View>`, `<Text>`, `<Image>`, etc. These map directly to native views in the respective platform, providing a more consistent look and feel.

**5. Development Environment**:

- **React** applications can be developed and tested in any web browser.

- **React Native** applications require emulators or actual devices to test the mobile UI.

In summary, while React and React Native share a lot of similarities thanks to their common base - React, they're used for different purposes (web vs mobile) and have different ways to render components and handle styles.

# 4) How do you optimize performance in a React Native app?

Performance optimization is crucial for any application, including those developed using React Native. Here are some ways to optimize performance in a React Native app:

1. **Use Production Mode**: Always ensure that the app is running in production mode before deploying it. In development mode, React Native includes extra checks and messages which can slow down the app. These are not included in production mode, which can significantly improve performance.

2. **Optimize Images**: Images often consume a significant portion of memory. Compress images before bundling them into the app. Use appropriate formats (.jpg, .png, .webp) based on requirements. Also, consider using an image cache library.

3. **Use Lists Effectively**: If your app has long lists of data to display, use the `FlatList` or `SectionList` components instead of mapping through arrays to create lists. These components have performance benefits as they only render what is visible on the screen.

4. **Avoid unnecessary renders**: Use PureComponent, shouldComponentUpdate, or React.memo to avoid unnecessary renders. Also, be mindful of how often your state/props are changing - frequent changes can cause frequent re-renders.

5. **Use Profiling Tools**: Use React DevTools, and the performance tab in Chrome DevTools to profile your React Native app and discover potential bottlenecks.

6. **Throttle and Debounce Heavy Computations**: If you're performing heavy computations or actions in response to user input (like search), consider using throttle or debounce to limit the rate at which a function can fire.

7. **Use State Management Libraries Wisely**: If you're using Redux or other state management libraries, be careful with how often you're triggering state changes. Unnecessary state changes can lead to unnecessary component re-renders.

8. **Offload Tasks to Background**: If there are tasks that can run in the background (like image processing, heavy computations), consider offloading them to a different thread using something like a Web Worker.

9. **Reduce the Size of your JavaScript Bundle**: Split your code into multiple bundles to reduce the size of the initial JavaScript bundle. This technique is known as "code splitting".

10. **Upgrade to the latest React Native version**: Newer versions often include performance optimizations, so it's beneficial to stay up-to-date.

Remember, before making any performance optimizations, it's important to identify the actual bottlenecks in your application. Making arbitrary optimizations without understanding the problem can lead to wasted effort or even new problems. Always measure the impact of your changes to ensure they're improving performance as expected.

# 5) What are some differences between Android and iOS that you need to account for when developing a React Native app?

When developing a React Native app, there are several platform differences to consider between Android and iOS. Here are some key differences that may affect your development:

**1. User Interface & Experience (UI/UX)**:

The design and user interface guidelines for Android and iOS are different. For example, the placement of navigation controls differs between these two platforms. Android uses a hardware or software back button for navigation, while iOS uses a navigation bar at the top.

**2. Components and APIs**:

React Native provides some platform-specific components and APIs. For example, the `DatePickerIOS` component is available only for iOS. On the other hand, some properties of the `TextInput` component are only available on Android.

**3. Permissions**:

The way permissions are handled is also different. Android has more fine-grained permissions that need to be defined in the AndroidManifest.xml file and are also requested at runtime. iOS permissions are requested at runtime and are added in the Info.plist.

**4. Gesture Handling**:

Gesture handling can be different on Android and iOS, especially when you are using libraries like `react-navigation`. Certain swipe gestures might work differently on Android compared to iOS.

**5. Push Notifications**:

Push notifications are handled differently on each platform. For Android, you might use Firebase Cloud Messaging (FCM), and for iOS, you would use Apple Push Notification service (APNs).

**6. App Store Submission Process**:

The process of submitting the application to the App Store and Play Store are different and have different guidelines.

**7. File Storage**:

The file systems on Android and iOS are different, and this can impact how you handle file storage and access.

In order to handle these differences in React Native, the platform module can be used to run different code for different platforms. This can be done by either using `Platform.select` or by using the `.android.js` and `.ios.js` file extensions.

Despite these differences, one of the primary advantages of using React Native is its ability to write most of your codebase in a cross-platform manner, which significantly reduces the amount of time and resources required to develop apps for both Android and iOS.

# 6) How would you handle networking in a React Native app?

Networking in a React Native application can be done using a variety of approaches, but the most common approach is to use the Fetch API or libraries like axios.

**Fetch API**:

The Fetch API provides a global `fetch()` method that provides an easy, logical way to fetch resources asynchronously across the network. This kind of operation is often referred to as an 'AJAX request'. Here is an example of how you might use it:

```jsx
fetch("https://myapi.com/data-endpoint", {
  method: "GET",
})
  .then((response) => response.json())
  .then((json) => console.log(json))
  .catch((error) => console.error(error))
  .finally(() => setLoading(false));
```

**Axios**:

Axios is another popular library used to make HTTP requests from the browser. It's often preferred over the Fetch API because of its wide browser support and automatic transformation of data into JSON.

Here is the same request using Axios:

```jsx
axios
  .get("https://myapi.com/data-endpoint")
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });
```

When handling network requests, you'll often want to display a loading spinner or some other indication that data is being loaded. For this, you can use React's state to keep track of whether a network request is currently in progress.

If you're working with a more complex application, you might consider using a state management library like Redux or MobX to manage your data. These libraries can help handle more complicated scenarios, like caching data, synchronizing data across different parts of the app, or handling updates from websockets or other real-time data sources.

For handling network errors, it's usually a good idea to have some global error handling strategy, so that if a network request fails, the user can be informed, and the issue can be logged for debugging later.

In addition to the above, remember that on Android 9 (API level 28) and above, cleartext (unencrypted HTTP) support is disabled by default, so you have to use HTTPS or you need to enable it manually for specific domains in your app manifest file.

# 7) What is the purpose of the componentDidMount lifecycle method, and how has it changed with the introduction of Hooks?

The `componentDidMount` lifecycle method is a function in a class component that gets executed right after the first render of the component on the DOM, meaning it runs once after the first render only. This makes it an ideal place to run statements that need to occur once and require DOM nodes to be available.

In practice, `componentDidMount` is used for a number of different tasks such as setting up any subscriptions (e.g., event listeners, network calls) and for asynchronous operations. If you're subscribing to anything, it is also important to unsubscribe in `componentWillUnmount` to avoid memory leaks.

However, since the introduction of Hooks in React 16.8, the `componentDidMount` lifecycle method can be replaced with the `useEffect` hook in functional components. This is part of the move towards functional components in React.

The `useEffect` Hook can mimic `componentDidMount` when you pass an empty array `[]` as the second argument. This effectively tells React that your `useEffect` doesn’t depend on any values from props or state, and therefore it should only run on mount and un

mount, similar to `componentDidMount` and `componentWillUnmount`:

```jsx
useEffect(() => {
  // code to run on component mount
}, []); // <-- empty dependency array
```

This way, `useEffect` serves as a unified API for handling side effects in React components, consolidating the behavior previously split between `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`. This change simplifies the mental model and makes it easier to write correct code.

# 8) What are some challenges you've faced in React Native development and how did you overcome them?

# 9) Why would you use RealmDB in a React Native project and what are the pros and cons?

Realm is an open-source, object-oriented database that can be used in React Native apps as an alternative to SQLite and other storage solutions. It's designed to be fast, easy to use, and, most importantly, to provide a seamless way to store and retrieve data.

**Why Use Realm in a React Native Project?**

1. **Performance**: Realm is incredibly fast, often outperforming SQLite and CoreData.

2. **Real-time and Offline-first**: Realm includes real-time, automatic data syncing capabilities and offline-first functionality, making it a good choice for applications where these features are crucial.

3. **Easy to Use**: Realm's API is straightforward and simple, which makes it easier to work with compared to some other databases.

4. **Cross-platform**: The same Realm database can be used across iOS and Android, which simplifies development when targeting multiple platforms.

5. **Live Objects**: With Realm, you can have live and reactive objects that update in real-time.

6. **Support for Complex Data**: Realm supports more complex data structures, such as lists of primitive types.

**Pros of Using Realm:**

1. **Fast**: Realm has been benchmarked as faster than its competitors.

2. **Real-time Capabilities**: Realm supports real-time applications, which is a big pro for apps that require this feature.

3. **Offline-first**: Realm supports building offline-first apps, as you can query your data directly from disk.

4. **Rich Data Model**: Realm supports object relationships, and complex data types, offering a rich data model.

**Cons of Using Realm:**

1. **File Size**: Realm's file size is larger than SQLite's, which can be a con for apps that need to be lightweight.

2. **Threading**: Objects from a Realm instance (i.e., the database) can only be used on the thread they were created on. This can lead to complexities in your code.

3. **Memory Handling**: Realm holds all data in memory, and as a memory-mapped database, it can result in larger memory usage.

4. **Community and Ecosystem**: While growing, the Realm community and the amount of resources, tutorials, and solutions available are not as extensive as SQLite or CoreData.

In conclusion, whether to use Realm or not depends on the specific needs and requirements of your project. It's crucial to consider the trade-offs and determine whether the benefits of using Realm outweigh the potential drawbacks for your specific use case.
