Styling & Layout in React Native
================================

Time to talk presentation. Styling in React Native is different from the web — and honestly, it takes some getting used to. Let's walk through it.

CSS in JS
--------

Here's some plain old HTML and CSS:

```html
<!-- index.css -->
.avatar {
    border-radius: 5px;
    margin: 10px;
    width: 48px;
    height: 48px;
}

<!-- // index.html -->
<div>
    <img class='avatar' src='https://tylermcginnis.com/tylermcginnis_glasses-300.png' />
</div>
```

Nothing surprising. But since we're not using HTML or CSS files to build mobile apps, how does this look in React Native?

Every core component in React Native accepts a `style` prop. You can pass inline JavaScript objects:

```js
function Avatar ({ src }) {
    return (
        <View>
            <Image
                style={{borderRadius: 5, margin: 10, width: 48, height: 48}}
                source={{uri: 'https://tylermcginnis.com/tylermcginnis_glasses-300.png'}}
            />
        </View>
    );
}
```

The `style` prop is just a plain JavaScript object. Properties are written in *camelCase* — `borderRadius`, not `border-radius`.

This works, but it gets messy fast. Imagine a dozen properties on a single component, or reusing the same styles across multiple places. One way to keep things DRY is to store the object in a variable:

```js
const styles = {
    image: {
        borderRadius: 5,
        margin: 10,
        width: 48,
        height: 48,
    }
}

function Avatar ({ src }) {
    return (
        <View>
            <Image
                style={styles.image}
                source={{uri: 'https://tylermcginnis.com/tylermcginnis_glasses-300.png'}}
            />
        </View>
    );
}
```

Better. But React Native goes a step further with the **`StyleSheet` API**:

```js
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';

export default class TextExample extends React.Component {
    render() {
        return (
            <View>
                <Text style={styles.greenLarge}>This is large green text!</Text>
                <Text style={styles.red}>This is smaller red text!</Text>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    greenLarge: {
        color: 'green',
        fontWeight: 'bold',
        fontSize: 40
    },
    red: {
        color: 'red',
        padding: 30
    },
});
```

`StyleSheet.create()` looks similar to using a variable, but it gives you real benefits:

#### Code quality
- Moving styles out of the render function makes code easier to read.
- Naming styles adds meaning to low-level components.

#### Performance
- Stylesheets are referenced by ID instead of creating a new object every render.
- Styles are sent through the bridge only once. Subsequent uses reference an ID.

`StyleSheet` also *validates* your styles. Errors in your style objects throw at compile time, not runtime. And if you need multiple styles on a component, the `style` prop accepts an array.

So CSS-in-JS means styling is handled by JavaScript objects instead of traditional CSS. You can write styles inline or use object variables, but `StyleSheet` is the idiomatic choice — it's performant, compositional, and validates your styles at compile time.

Flexbox Guide
-------------

Whenever I learn a new technology, I like to answer the question: *why does this exist?* With flexbox, the answer might just be that creating an all-purpose layout with regular CSS is a pain.

The goal of flexbox is to "lay out, align, and distribute space among items in a container, even when their size is unknown and/or dynamic." In short: *flexbox is about dynamic layouts*.

The core idea: give the parent element control over the layout of its (immediate!) children, rather than having each child manage its own layout. The parent becomes a **flex container**; the children become **flex items**. Instead of floating children left and adding margin to each one, you tell the parent: lay these out in a row with even spacing. Layout responsibility moves from children to parent. That gives you finer control.

React Native uses flexbox precisely because it provides consistent layouts across different screen sizes. Every flex container has two axes — a **main axis** and a **cross axis** — and the critical properties are `flex-direction`, `justify-content`, and `align-items`. But React Native's flexbox has some quirks compared to the CSS spec, so let's look at those now.

Layout in React Native
----------------------

### React Native's Flexbox Implementation

React Native implements flexbox, but there are key differences from the web.

First: all containers in React Native are *flex containers by default*. On the web, you'd write:

```css
/* example.css */
.container {
    display: flex;
}
```

In React Native, this is completely unnecessary. Everything is `display: flex;` out of the box.

Second: `flex-direction`. On the web, items default to `row`. On mobile, the default is `column` — items lay out vertically. Makes sense for a phone screen.

Third: the `flex` property works differently. On the web, `flex` specifies how a flex item grows or shrinks. In React Native, `flex` is used with sibling items that hold different values:

```js
import React from 'react';
import { View } from 'react-native';

const FlexDemo = props => (
    <View style={{flex: 1}}>
        <View style={{flex: 1, backgroundColor: 'red'}} />
        <View style={{flex: 2, backgroundColor: 'green'}} />
        <View style={{flex: 3, backgroundColor: 'blue'}} />
    </View>
);

export default FlexDemo;
```

The outer container (`flex: 1`) fills the full screen. Its children split the space proportionally: blue takes 3× as much space as red, green takes 2×.

### Other Differences

React Native applies these defaults that differ from the web:

```css
box-sizing: border-box;
position: relative;
align-items: stretch;
flex-shrink: 0;
align-content: flex-start;
border: 0 solid black;
margin: 0;
padding: 0;
min-width: 0;
```

Most of these are just different defaults — nothing to overthink.

### Platform API

React's philosophy is "learn once, write anywhere." But sometimes you need *distinct* code for each platform — like different styling for iOS and Android visual components. React Native's `Platform` API handles exactly this, so you can keep your iOS and Android UIs feeling native.

### Dimensions API

React Native also gives you the [Dimensions API](https://facebook.github.io/react-native/docs/dimensions.html) to grab the device's window width and height:

```js
import { Dimensions } from 'react-native'

const { width, height } = Dimensions.get('window')
```

Use these to plan how your `<View>`s will look across different screen sizes.

So to sum up the layout story: React Native uses flexbox with some minor default differences from the web spec (column by default, everything's a flex container). The `Platform` API lets you fork styles per platform, and `Dimensions` gives you screen measurements when flexbox alone won't cut it.

How Professionals Handle Styling
--------------------------------

### StyleSheet vs Inline

We introduced `StyleSheet` earlier. Let's go deeper on why it matters.

Take this inline-styled component:

```js
<View style={{
    borderRadius: 4,
    borderWidth: 0.5,
    borderColor: '#d6d7da',
}}>
    <Text style={[
        {fontSize: 19, fontWeight: 'bold'},
        props.isActive && { color: 'red' }
    ]}>
        Welcome
    </Text>
</View>
```

Even for a simple UI, the styling makes the JSX hard to read. That's the biggest win of `StyleSheet`: *moving styles out of the render function makes the code readable*. Naming styles also makes components more declarative. Here's the same UI with `StyleSheet`:

```js
var styles = StyleSheet.create({
    container: {
        borderRadius: 4,
        borderWidth: 0.5,
        borderColor: '#d6d7da',
    },
    title: {
        fontSize: 19,
        fontWeight: 'bold',
    },
    activeTitle: {
        color: 'red',
    },
});

<View style={styles.container}>
    <Text style={[styles.title, props.isActive && styles.activeTitle]} />
</View>
```

On top of readability, there's a performance win: styles are referenced by ID instead of being recreated every render.

### Media Queries

React Native's `StyleSheet` doesn't support media queries. For the most part, flexbox handles responsive grids without them. When flexbox won't cut it, use the `Dimensions` API for similar results.

### CSS-in-JS Libraries

Styling in React is going through a renaissance — similar to what Flux went through a few years ago (which eventually gave us Redux). There are a lot of styling libraries, each with different tradeoffs.

Two of the most popular are **Glamorous** and **Styled Components**. The core idea: styling is a primary concern of the component, so it should be *coupled* with the component itself.

Let's look at Styled Components with React Native:

```js
import React, { Component } from 'react'
import { Text, StyleSheet, View } from 'react-native'
import styled from 'styled-components/native'

const CenterView = styled.View`
    align-items: center;
    background: #333;
    flex: 1;
    justify-content: center;
`

const WelcomeText = styled.Text`
    color: white;
    font-size: 20;
`

const WelcomeButton = styled.TouchableOpacity`
    align-items: center;
    background: red;
    border-radius: 5px;
    height: 50px;
    justify-content: center;
    width: 100px;
`

export default class App extends Component {
    render () {
        return (
            <CenterView>
                <WelcomeText>
                    Cool Styling
                </WelcomeText>
                <WelcomeButton onPress={() => alert('Hello')}>
                    <WelcomeText>
                        Push Me
                    </WelcomeText>
                </WelcomeButton>
            </CenterView>
        )
    }
}
```

Summary
-------

Styling in React Native is CSS-in-JS: JavaScript objects instead of stylesheets, camelCase properties instead of kebab-case. The `StyleSheet` API is the idiomatic choice — it moves styles out of render functions, validates at compile time, and references styles by ID for better performance.

Layout uses flexbox, but with different defaults than the web: `column` direction, everything's a flex container, and `flex` works proportionally rather than as a growth/shrink factor. The `Platform` API handles per-platform styling differences, and `Dimensions` gives you screen measurements for when flexbox alone isn't enough.

On top of the built-in tools, CSS-in-JS libraries like Styled Components offer a different philosophy — coupling styles directly to components. Both approaches are valid. Pick the one that feels less awkward for your project.
