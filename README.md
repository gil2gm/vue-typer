# vue-typer
Vue component that simulates a user typing, selecting, and deleting text.

## Demo
TODO
Link here (also add link to github repo description at the top)
<p align="center"><img src="" alt="VueTyper demo gif"/></p>

## Getting Started

### Prerequisites
- Vue v2.x ([See here for migration instructions from Vue 1.x to 2.x.](https://vuejs.org/v2/guide/migration.html))

### Installation
TODO:
#### npm
Use this method if you wish to import/require VueTyper as a module.
```
npm install --save vue-typer?
```

#### CDN
Use this method if you wish to access VueTyper globally via `window.VueTyper`.

```html
<script src="path/to/vue-typer.min.js"></script>
```

## Usage
You may register VueTyper either globally or locally. [What's the difference? See the Vue documentation here.](https://vuejs.org/v2/guide/components.html#Registration)

#### Local Registration
1. Import the VueTyper component directly from your Vue component file:
  ```javascript
  // ES6
  import { VueTyper } from 'vue-typer'
  // CommonJS
  var VueTyper = require('vue-typer').VueTyper
  // Global
  var VueTyper = window.VueTyper.VueTyper
  ```

2. Register it as a local component in your Vue component options:
  ```javascript
  var MyComponent = {
    // ...
    components: {
      // ES6; property shorthand + Vue should automatically dasherize the key for us
      VueTyper
      // pre-ES6
      'vue-typer': VueTyper
    }
  }
  ```

3. Use vue-typer in your Vue component's template:

  ```html
  <vue-typer text='Hello World! I was registered locally!'></vue-typer>
  ```

#### Global Registration
1. Import the VueTyper plugin in your application entry point:
  ```javascript
  // ES6
  import VueTyperPlugin from 'vue-typer'
  // CommonJS
  var VueTyperPlugin = require('vue-typer').default
  // Global
  var VueTyperPlugin = window.VueTyper.default
  ```

2. Register the VueTyper plugin with Vue
  ```javascript
  Vue.use(VueTyperPlugin)
  ```

3. Now you can freely use vue-typer in any Vue component template:
  ```html
  <vue-typer text='Hello World! I was registered globally!'></vue-typer>
  ```

## Props
It may be helpful to play around with these props in the interactive demo(TODO: Link to gh-pages.com#playground)

#### `text`
- type: `String || Array`
- **required**
- validator: Non-empty
- Usage:

  ```html
  <vue-typer text='watermelon'></vue-typer>
  ```
  
  Either a single string, or an ordered list of strings, for VueTyper to type. Strings will not be trimmed.
  
- Note: Dynamically changing this value after VueTyper has mounted will cause VueTyper to reset itself and start typing from scratch.
- See also: [`shuffle`](#shuffle)

#### `repeat`
- type: `Number`
- default: `Infinity`
- validator: Non-negative
- Usage:

  ```html
  <vue-typer text='watermelon' :repeat='0'></vue-typer>
  ```
  
  Number of _extra_ times to type `text` after the first time. Setting 0 will type `text` once; 1 will type twice; Infinity will type forever.

- Note: Dynamically changing this value after VueTyper has mounted will cause VueTyper to reset itself and start typing from scratch.

#### `shuffle`
- type: `Boolean`
- default: `false`
- Usage:

  ```html
  <vue-typer text='watermelon' :shuffle='true'></vue-typer>
  ```

  Randomly shuffles `text` ([using the Fisher-Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)) before typing it. If `repeat > 0`, `text` will always be shuffled before repeat typings. `text` is **not** shuffled after every word is typed. This implies that:
  - all strings in `text` will be typed the same number of times, but just in a shuffled order
  - the frequencies of the order of strings typed will have equal distributions, e.g.: 
    - given `text=['a','b']`, 'ab' will be printed 50% of the time, and 'ba' the other 50%
    - given `text=['a','b','c']`, there are 3!=6 possible permutations, and they will each be printed 100%/6=16.7% of the time
  - the only scenarios where the same word can be typed twice in a row are when:
    1. `text` contains duplicate strings, or
    2. `repeat > 0`, `text` is typed where the last word is W, and the next repeat typing shuffled `text` such that it starts with W.

- Note: Dynamically changing this value after VueTyper has mounted will cause VueTyper to reset itself and start typing from scratch.

#### `initialAction`
- type: `String`
- default: `"typing"`
- validator: `"typing"` || `"erasing"`
- Usage:

  ```html
  <vue-typer text='watermelon' initial-action='erasing'></vue-typer>
  ```
  
  `typing` starts VueTyper off in the "typing" state; there will be empty space as VueTyper begins to type the first string in `text`.
  
  `erasing` starts VueTyper off in the "erasing" state; the first string in `text` will already be typed and visible as VueTyper begins to erase.

#### `preTypeDelay`
- type: `Number`
- default: `70`
- validator: Non-negative
- Usage:

  ```html
  <vue-typer text='watermelon' pre-type-delay='1000'></vue-typer>
  ```
  
  Milliseconds to wait before typing the first character of every string in `text`. 
  
  This is useful to have an idle period to show a blank space for a period of time before VueTyper types the first character.

#### `typeDelay`
- type: `Number`
- default: `70`
- validator: Non-negative
- Usage:

  ```html
  <vue-typer text='watermelon' type-delay='100'></vue-typer>
  ```
  
  Milliseconds to wait after typing a character, until the next character is typed.


#### `preEraseDelay`
- type: `Number`
- default: `2000`
- validator: Non-negative
- Usage:

  ```html
  <vue-typer text='watermelon' pre-erase-delay='1000'></vue-typer>
  ```
  
  Milliseconds to wait after a string in `text` is fully typed, until the first erase action (i.e. backspace, highlight) is performed.
  
  This is useful to have an idle period that gives users time to read the typed string before it is erased.


#### `eraseDelay`
- type: `Number`
- default: `250`
- validator: Non-negative
- Usage:

  ```html
  <vue-typer text='watermelon' erase-delay='250'></vue-typer>
  ```
  
  Milliseconds to wait after performing an erase action (i.e. backspace, highlight), until the next erase action can start.

#### `eraseStyle`
- type: `String`
- default: `select-all`
- validator: `"backspace"` || `"select-back"` || `"select-all"` || `"clear"`
- Usage:

  ```html
  <vue-typer text='watermelon' erase-style='backspace'></vue-typer>
  ```
  
  `backspace` erases one character at a time, simulating the backspace key.
  
  `select-back` highlights backward one character at a time, simulating Shift+LeftArrow, and erases once all characters are highlighted.
  
  `select-all` immediately highlights all characters at once, simulating Ctrl/Cmd+A, and erases all characters afterwards.

  `clear` immediately erases all characters at once; the typed string simply disappears.

#### `eraseOnComplete`
- type: `Boolean`
- default: `false`
- Usage:

  ```html
  <vue-typer text='watermelon' :erase-on-complete='true'></vue-typer>
  ```
  
  By default, after VueTyper completes all its typing (i.e. it finishes typing all strings in `text`, `repeat+1` times), the last string typed will not be erased and stay visible. Enabling this flag will tell VueTyper to erase the final string as well.

- Note: Has no effect if `repeat === Infinity`.

#### `caretAnimation`
- type: `Boolean`
- default: `false`
- validator: `"solid"` || `"blink"` || `"smooth"` || `"phase"` || `"expand"`
- Usage:

  ```html
  <vue-typer text='watermelon' caret-animation='smooth'></vue-typer>
  ```
  
  Specifies a built-in caret animation to use, similar to Sublime and VS Code animations.

- Note: Alternatively, custom animations can be applied via CSS. 

- See also: [Styles](#Styles), Example CSS Styles (TODO: Link to gh-pages.com#style-showcase)

## Events
TODO

#### EventName
- Event data:
  - (String) word that finished typing
```html
Code sample of attaching event listener in template
```
```javascript
Code sample of event handler function
```

Emitted when VueTyper has finished typing all words in `text`, `repeat+1` times. If `eraseOnComplete` is enabled, the final string must be erased before this event is emitted.

## Styles
TODO

To keep the separation of concern between component code and styles, VueTyper can be fully styled through CSS (as opposed to props):

```css
CSS selector format here
```

For examples, see (TODO: Link to gh-pages.com#style-showcase)

## Changelog
TODO

## TODO
- Update to stable releases of:
  - [ ] webpack
  - [ ] webpack-dev-server
  - [ ] extract-text-webpack-plugin
  - [ ] Bootstrap v4
- [ ] Revisit community discussions around the best way to obtain deterministic hashes so we can remove HashedModuleIdsPlugin
- Potential features (contributions are welcome!):
  - [ ] start typing only when VueTyper is on-screen; potentially pause typing when off-screen
  - [ ] smarter typing algorithm: erase only up to the longest common starting substring
  - [ ] is it worth it to eliminate time-drifting from setInterval? If so, it could be a self-correcting interval (implemented as a series of timeouts)
- Vue documentation considers rendering-specific tests to still be 'unit' tests. Should we split this out into 'integration' tests?

## License

[MIT](http://opensource.org/licenses/MIT)

Copyright &copy; 2016 Chris Nguyen. All rights reserved.
