# vuera [![Build Status](https://travis-ci.org/akxcv/vuera.svg?branch=master)](https://travis-ci.org/akxcv/vuera)

Use [Vue] components in your [React] app:
```jsx
import React from 'react'
import MyVueComponent from './MyVueComponent.vue'

export default () =>
  <div>
    <h1>I'm a react component</h1>
    <div>
      <MyVueComponent message='Hello from Vue!' />
    </div>
  </div>
```

Or use [React] components in your [Vue] app:
```vue
<template>
  <div>
    <h1>I'm a Vue component</h1>
    <my-react-component :passedProps="passedProps"></my-react-component>
  </div>
</template>

<script>
  import { ReactWrapper } from 'vuera'
  import MyReactComponent from './MyReactComponent'

  export default {
    data () {
      passedProps: {
        message: 'Hello from React!',
      },
    },
    components: { 'my-react-component': MyReactComponent },
  }
</script>
```

## Use cases

- 👨‍👩‍👧 Using both Vue and React in an app
- 🏃 Migrating from React to Vue or from Vue to React

## Installation

Install with yarn:

```sh
$ yarn add vuera
# or with npm:
$ npm i -S vuera
```

Add `vuera/babel` to `plugins` section of your `.babelrc`:
```json
{
  "presets": "react",
  "plugins": ["vuera/babel"]
}
```

## Usage

### Vue in React

Using a Vue component in a React app is very simple! All you need to do is import your Vue component
normally and just use it in JSX as you would use a React component:

```jsx
import React from 'react'
import MyVueComponent from './MyVueComponent.vue'

export default () => (
  <div>
    <h1>I'm a react component</h1>
    <div>
      <MyVueComponent message='Hello from Vue!' />
    </div>
  </div>
)
```

If you don't want to use the babel plugin, you can still use a Vue component inside your React app
like so:

```jsx
import React from 'react'
import { VueWrapper } from 'vuera'
import MyVueComponent from './MyVueComponent.vue'

export default () => (
  <div>
    <h1>I'm a react component</h1>
    <div>
      <VueWrapper
        component={MyVueComponent}
        message='Hello from Vue!'
      />
    </div>
  </div>
)
```

Functions work normal, too.

### React in Vue

Using a React component in a Vue app is very simple, too! First, enable our Vue plugin:

```js
import Vue from 'vue'
import { VuePlugin } from 'vuera'
Vue.use(VuePlugin)
/* ... */
```

Now just import your React component normally, register it in your Vue component as you would
register another Vue component, and use it. There's just one caveat: you always have to enumerate
component's props when defining it, so all the data you want to pass to the React component, you
**must** pass via the `passedProps` prop. Here's an example:

```vue
<template>
  <div>
    <h1>I'm a Vue component</h1>
    <my-react-component :passedProps="passedProps"></my-react-component>
  </div>
</template>

<script>
  import { ReactWrapper } from 'vuera'
  import MyReactComponent from './MyReactComponent'

  export default {
    data () {
      passedProps: {
        message: 'Hello from React!',
      },
    },
    components: { 'my-react-component': MyReactComponent },
  }
</script>
```

You can also use React components without the plugin, here's how:

```vue
<template>
  <div>
    <h1>I'm a Vue component</h1>
    <div>
      <react :component="component" :passedProps="passedProps"></react>
    </div>
  </div>
</template>

<script>
  import { ReactWrapper } from 'vuera'
  import MyReactComponent from './MyReactComponent'

  export default {
    data () {
      component: MyReactComponent,
      message: 'Hello from React!',
    },
    /**
     * It's recommended to declare `passedProps` as a computed property. This way you won't bloat
     * your Vue component, and you will be able to pass methods as well.
     */
    computed: {
      passedProps () {
        return {
          message: this.message,
        }
      },
    },
    components: { react: ReactWrapper }
  }
</script>
```

[react]: https://facebook.github.io/react
[vue]: https://vuejs.org