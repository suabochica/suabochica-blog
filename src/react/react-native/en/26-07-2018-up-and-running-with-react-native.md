Up and Running with React Native
===============================

Welcome. This is the first post in a series on React Native. Here's what we'll cover:

- **Post 1** — Why React Native exists, and how to get a dev environment going without losing your mind.
- **Post 2** — The real differences between React and React Native (it's not just `<div>` vs `<View>`).
- **Post 3** — Styling and layout. Because flexbox on mobile is its own beast.
- **Post 4** — Navigation. Routing on native is a completely different paradigm.
- **Post 5** — Native features like geolocation, notifications, and shipping to the app store.

Throughout this series we'll be building an app called **UdaciFitness** — a triathlon training tracker. It covers five activities:

- Run
- Bike
- Swim
- Sleep
- Eat

We'll wire up a training calendar, set daily itineraries, and add reminders to log data. By the end of the series you'll have a real, working React Native app.

What Is React Native and Why Does It Exist?
-------------------------------------------

**React Native** lets you use React to build native iOS and Android applications. The real win? You can have a single UI team instead of a web team, an iOS team, *and* an Android team.

You've probably heard the phrase:

*"Write once, run anywhere."*

The idea is seductive: one codebase for web, iOS, and Android. Tools like Adobe PhoneGap and Apache Cordova ran with this. But in practice it's brutal — each platform has such a unique experience that a single codebase ends up feeling wrong everywhere.

React Native's motto is different:

*"Learn once, write anywhere."*

Once you know React, you can take those same principles — component composition, declarative UI — and build not just for the web, but for native platforms too. You're not sharing a codebase across platforms. You're sharing a mindset.

### React Native under the hood

When React was first introduced, the big selling point was the **Virtual DOM**. The idea is standard now, but back then it was groundbreaking.

The key process inside the Virtual DOM is **reconciliation**. The goal is to update the UI based on new state in the most efficient way possible. React builds a new tree of elements, diffs it against the previous tree, and figures out exactly what changed. It then makes only the updates that are absolutely necessary.

Here's the insight: what if instead of targeting the web's DOM, we target iOS or Android views? That's React Native. Under the hood, many of the same principles — Virtual DOM, reconciliation, the diffing algorithm — apply whether you're building for the web or for mobile. In other words, React Native's "learn once, write anywhere" approach isn't a gimmick — it's literally the same engine, targeting a different render surface.

Dev Environment Setup
---------------------

### Create React Native App

Building for both Android and iOS means supporting two separate development environments: Xcode and Android Studio. That's a lot of configuration — each of those tools has *its own set of courses*.

Enter **Create React Native App** (CRNA). It's like Create React App but for mobile. Install the CLI via npm, scaffold a project, and you're off. No Xcode. No Android Studio. It's the quickest path to "Hello World" on a real device, with a single build tool, and zero lock-in.

#### Pros
- Minimal time to "Hello World."
- Develop on your own device.
- A single build tool.

#### Cons
- Won't work if you're adding React Native to an existing native app.
- Won't work if you need to build your own bridge to a native API that CRNA doesn't expose.

#### Installing CRNA

```bash
npm install -g create-react-native-app
```

Or with **yarn**:

```bash
yarn global add create-react-native-app
```

Or even shorter:

```bash
yarn create react-native-app my-app
```

> You'll need Watchman installed (`brew install watchman`) for the `yarn start` command to work.

### Expo

Expo is the secret weapon. It makes everything about React Native easier. No Android Studio. No Xcode. You can even develop for iOS from Windows or Linux.

With Expo, you load and run CRNA projects using plain JavaScript — no native code compilation. Download the Expo app on your device and you're ready.

> Simulators are an alternative, but personally I prefer developing on a real device. You save time on configuration and you get an accurate feel for how your app actually behaves.

### The Environment

A CRNA project supports:

- ES5 and ES6
- Object Spread Operator
- Async functions
- JSX
- Flow
- Fetch

Since we're building with pure JavaScript, this shouldn't be surprising. Before we start coding, grab the utility files from the `utils` folder — they're scaffolding we'll use later. The bottom line: regardless of whether you target iOS or Android, or whether you're on Mac, Windows, or Linux, you're building with the same old JavaScript you already know.

Using the Debugger
------------------

### How to Debug

One of the best things about React Native is that it takes the web development experience you're used to and brings it to native. Live reloading and debugging just work.

To open the developer menu, *shake your phone* while the app is running with Expo. You'll see an option called **Debug JS Remote**. Selecting it opens a browser tab running your React Native JavaScript code as a web worker. From there, open Developer Tools and debug just like you would on the web.

Another gem is **Toggle Inspector**. It's like the box model inspector we use on the web — tap any element and see every style applied to it.

### Refreshing the App

On the web, when something breaks you refresh. In native development, you'd normally have to recompile. With React Native, you just *shake your phone* and hit **Reload**.

Summary
-------

React Native's "learn once, write anywhere" approach means you build for both web and native using the same principles — Virtual DOM, reconciliation, and declarative components — just targeting a different render surface.

On the tooling side, Create React Native App scaffolds a working app with minimal configuration and no Xcode or Android Studio. Pair it with Expo and you're developing on your own device, with a single build tool, and no lock-in. When something breaks, shake your phone to debug via the browser's Developer Tools, reload your JavaScript without recompiling, or inspect elements just like you would on the web.
