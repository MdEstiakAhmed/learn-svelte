## Table of content

- [Introduction](#introduction)
- [Why Svelte](#why-svelte)
- [Special notes](#special-notes)
- [VS code extensions](#vs-code-extensions)
- [Create command for Svelte application](#create-command-for-svelte-application)
- [folder and file structure](#folder-and-file-structure)
- [packages note](#packages-note)
- [What is `.svelte` file extension](#what-is-`.svelte`-file-extension)
- [Syntax and feature](#syntax-and-feature)
- [Binding text](#binding-text)
- [Binding HTML](#binding-html)
- [Binding attributes](#binding-attributes)
- [Binding classes](#binding-classes)
- [Conditional render](#conditional-render)
- [render list](#render-list)
- [event handle](#event-handle)
- [form handling](#form-handling)
- [`this` bind](#this-bind)
- [Reactive declaration](#reactive-declaration)
- [Reactive statement](#reactive-statement)
- [Component concept](#component-concept)
- [Component props](#component-props)
- [Context API](#context-api)
- [Component event](#component-event)
- [Component event foreword](#component-event-foreword)
- [Component event interface create](#component-event-interface-create)
- [Slot](#slot)
- [Named slot](#named-slot)
- [Slot props](#slot-props)
- [Styling in svelte](#styling-in-svelte)
- [Life cycle hooks](#life-cycle-hooks)
- [HTTP request](#http-request)
- [Dynamic component](#dynamic-component)
- [Special component](#special-component)
- [Module context](#module-context)
- [Store](#store)
  - [Create a store](#create-a-store)
  - [Store types](#store-types)
  - [writable store](#writable-store)
  - [readable store](#readable-store)
  - [derived store](#derived-store)
  - [Custom store](#custom-store)

## Introduction:

Svelte is a component framework, where user can use to build high performance web applications. And helps user write application code in a declarative manner.

## Why Svelte:

- unlike other frontend library or framework, which do a bulk of their work in the browser, svelte shifts that work into a compile step that happen you build your app.
- there is no need to bundle the framework code and the bundle size is smaller.
- developer experience is better then others.
- builtin store management system. No need to goto other 3rd party library.
- built-in motion and element transitions.
- easy form handling.
- scoping css from global to local scope.
- maintain head tag for SPA with the component.

## Special notes:

- svelte reactivity only works in assigning data.

## VS code extensions:

- Svelte for VS Code
- Svelte 3 Snippets

## Create command for Svelte application:

`npx degit sveltejs/template my-svelte-application`

> `degit` command clone the svelte template from the repository.

`npm install`

> install all the dependencies.

`npm run dev`

> start the application.

## folder and file structure:

- node_module: all dependencies libraries
- public: contain static assets
- scripts: necessary script files
- src: contain `main.js`. where application start. `App.svelte` is a svelte file where we write codes for the UI.
- package.json: application dependencies, dev dependencies, scripts, details
- package-lock.json: version history of packages
- rollup.config.js: configuration file for rollup. This is used when we user `npm run build` or `npm run dev`

## packages note:

- `svelte` is used only for development. Not is production mode.
- `rollup` is used for module bundler. which bundle the app.
- `sirve-cli` is used for run static file server

## What is `.svelte` file extension:

- A \*.svelte file is a custom file format that uses HTML-like syntax to describe a portion of the UI.
- Each \*.svelte file contain 3 parts. script, markup and style.
- browser doesn't understand .svelte extension. Rollup plugin parse the file for browser.

## Syntax and feature:

### Binding text:

```html
<!-- App.svelte -->
<script>
    const name = "estiak";
</script>

<div>
    <h1>Hello {name}</h1>
    <h3>2 + 2 = {2+2}<h3>
</div>
```

### Binding HTML:

> Binding HTML is a risky task. Only verified content should be bind. Because any one can add script.

```html
<!-- App.svelte -->
<script>
  const htmlData = "<b>Hello estiak</b>";
</script>

<div>
  <h1>{@html htmlData}</h1>
</div>
```

### Binding attributes:

```html
<!-- App.svelte -->
<script>
  // string attributes
  const id = "heading";
  const paragraphId = "paragraph-id";

  // boolean attributes
  const disabled = true;
  const readonly = false;
</script>

<div>
  <!-- short hand -->
  <h1 {id}>heading</h1>
  <!-- regular -->
  <p id={paragraphId}>paragraph</p>

  <!-- boolean true -->
  <button {disabled}>disable button</button>
  <!-- boolean false (this is not exist. because this attribute value is false.) -->
  <input {readonly} value="i am read only" />
</div>
```

### Binding classes:

```html
<!-- App.svelte -->
<script>
    const textStatus = "success";
    const isActive = true;
    const opacity50 = true;
    const inactive = true;
</script>

<div>
    <!-- static -->
    <h2 class="underline-text">underline text</h2>

    <!-- dynamic -->
    <h3 class={textStatus}>status text</h3>

    <!-- conditional -->
    <h3 class={isActive ? "active" : "inactive"}>hello</h3>

    <!-- short hand for boolean type -->
    <h3 class:inactive={opacity50}>Opacity 50</h3>
    <h3 class:inactive>Opacity 50</h3>
</div>

<style>
    .underline-text{
        text-decoration: underline;
    }.
    .danger {
        color: red;
    }
    .success {
        color: green;
    }
    .active {
        opacity: 1;
    }
    .inactive {
        opacity: 0.5;
    }
</style>
```

### Conditional render:

```html
<!-- App.svelte -->
<script>
  const number = 5; // -5, 0, 5
</script>

<div>
  {#if number === 0}
  <h2>Number zero</h2>
  {:else if number < 0}
  <h2>Number is negative</h2>
  {:else if number > 0}
  <h2>Number is positive</h2>
  {:else}
  <h2>Not a number</h2>
  {/if}
</div>
```

### render list:

```html
<!-- App.svelte -->
<script>
  const fruits = ["apple", "mango", "orange"];
</script>

<div>
  <ul>
    <!-- last (fruit) one used as key -->
    {#each fruits as fruit, index (fruit)}
    <li>{index + 1} {fruit}</li>
    {/each}
  </ul>
</div>
```

### event handle:

```html
<!-- App.svelte -->
<script>
    let count = 0;
    function handleCountClick (event) {
        console.log(event);
        count++;
    }
</script>

<div>
    <button on:click={handleCountClick}>Count {count}</button>
    <button on:click={(event) => handleCountClick(event)}>Count {count}</button>
</div>
```

### form handling:

```html
<!-- App.svelte -->
<script>
  let test = "";
  let name = "";
  let number = 0;
  let info = "";
  let country = "";
  let countries = [];
  let skills = [];
  let yearsOfExperience = "";

  function handleSubmit(event) {
    event.preventDefault();
    console.log({
      name,
      number,
      info,
      country,
      countries,
      status,
      skills,
      yearsOfExperience,
    });
  }

  function handleSubmitWithoutEvent() {
    console.log({
      name,
      number,
      info,
      country,
      countries,
      status,
      skills,
      yearsOfExperience,
    });
  }
</script>

<div>
  <!-- with modifier -->
  <form on:submit|preventDefault={handleSubmitWithoutEvent}>
    <input type="text" name="test" id="test" bind:value={test} />
  </form>

  <!-- without modifier -->
  <form on:submit={handleSubmit}>
    <div>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div>
      <input type="number" id="number" bind:value={number} />
    </div>
    <div>
      <textarea id="info" bind:value={info} />
    </div>
    <div>
      <select id="country" bind:value={country}>
        <option value="">Choose one</option>
        <option value="bangladesh">Bangladesh</option>
        <option value="india">india</option>
      </select>
    </div>
    <div>
      <select id="countries" bind:value={countries} multiple>
        <option value="">Choose one</option>
        <option value="bangladesh">Bangladesh</option>
        <option value="india">India</option>
        <option value="Nepal">Nepal</option>
        <option value="Bhutan">Bhutan</option>
      </select>
    </div>
    <div>
      <input type="checkbox" id="status" bind:checked={status} />
    </div>
    <div>
      <input type="checkbox" id="skills" bind:group={skills} value="html" />
      <label for="skills">HTML</label>
      <input type="checkbox" id="skills" bind:group={skills} value="css" />
      <label for="skills">CSS</label>
      <input type="checkbox" id="skills" bind:group={skills} value="js" />
      <label for="skills">JS</label>
    </div>
    <div>
      <input
        type="radio"
        id="yearsOfExperience"
        bind:group={yearsOfExperience}
        value="1"
      />
      <label for="yearsOfExperience">1</label>
      <input
        type="radio"
        id="yearsOfExperience"
        bind:group={yearsOfExperience}
        value="2"
      />
      <label for="yearsOfExperience">2</label>
      <input
        type="radio"
        id="yearsOfExperience"
        bind:group={yearsOfExperience}
        value="3"
      />
      <label for="yearsOfExperience">3</label>
    </div>
    <div>
      <button>Submit</button>
    </div>
  </form>
</div>
```

### `this` bind:

```html
<script>
  import { onMount } from "svelte";
  let inputRef;

  onMount(() => {
    inputRef.focus();
  });
</script>

<input type="text" bind:this={inputRef} />
```

### Reactive declaration:

```html
<!-- App.svelte -->
<script>
  let firstName = "ABC";
  let lastName = "XYZ";
  $: fullName = `${firstName} ${lastName}`;
  let books = [
    {
      title: "Harry Potter and the Sorcerer's stone",
      author: "J.K. Rowling",
      price: 10,
    },
    { title: "Jurassic Park", author: "Michael Crichton", price: 20 },
  ];
  $: totalPrice = books.reduce((acc, book) => acc + book.price, 0);
</script>

<div>
  <h2>{firstName} {lastName}</h2>
  <h2>{fullName}</h2>
  <h2>{totalPrice}</h2>
</div>
```

### Reactive statement:

```html
<!-- App.svelte -->
<script>
    let volume = 10;
	$: {
		if (volume > 0) {
			document.title = `Volume: ${volume}`;
		} else {
			document.title = `Muted`;
		}
	}
</script>

<div>
    <button on:click={() => volume = 0}>Mute</button>
	<button on:click={() => volume = 10}>Un-mute</button>
</div>
```

### Component concept:

```html
<!-- Child.svelte -->
<h1>I am from child</h1>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<Child />
```

### Component props:

```html
<!-- Child.svelte -->
<script>
  export let name = "default";
</script>
<h1>my name is {name}</h1>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
  let info = {
    name: "xyz",
  };
</script>

<Child name="abc" />
<Child name={info.ame} />
<Child {...info} />
<Child />
```

### Context API:

```html
<!-- Child.svelte -->
<script>
  import { getContext } from "svelte";
  let username = getContext("name-context");
</script>
<h1>my name is {username}</h1>

<!-- App.svelte -->
<script>
  import { setContext } from "svelte";
  import Child from "./components/Child.svelte";
  let name = "abc";
  setContext("name-context", name);
</script>
<Child />
```

### Component event:

```html
<!-- Child.svelte -->
<script>
    import {createEventDispatcher} from "svelte"
    const dispatch = createEventDispatcher();
    function closePopup () {
        /**
         * @param {string} type - event name
         * @param {any} detail - data
         */
        dispatch("close", "any data type")
    }
</script>

<div>
    <h1>I am a popup</h1>
    <button on:click={closePopup}>close</button>
</div>

<!-- App.svelte -->
<script>
	import Child from "./components/Child.svelte"
	let isShowPopup = true;

	const closePopup = (event) => {
		isShowPopup = false;
		console.log(event.detail);
	}
</script>
<div>
	<button on:click={() => isShowPopup = true}>show popup</button>
	{#if isShowPopup}
		<Child on:close={closePopup} />
	{/if}
</div>
```

### Component event foreword:

```html
<!-- SubChild.svelte -->
<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  const showGreat = () => {
    dispatch("great", "hello");
  };
</script>

<div>
  <button on:click={showGreat}>click me</button>
</div>
<!-- Child.svelte -->
<script>
  import SubChild from "./SubChild.svelte";
</script>

<div>
  <SubChild on:great />
</div>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";

  const showGreat = (event) => {
    console.log(event.detail);
  };
</script>
<div>
  <Child on:great={showGreat} />
</div>
```

### Component event interface create:

```html
<!-- Child.svelte -->
<script>
  import SubChild from "./SubChild.svelte";
</script>

<div>
  <SubChild on:great />
</div>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";

  const showGreat = () => {
    alert("hello");
  };
</script>
<div>
  <Child on:great={showGreat} />
</div>
```

### Slot:

```html
<!-- Child.svelte -->
<div>
  <slot>Default content</slot>
</div>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<div>
  <Child>
    <h1>hello</h1>
  </Child>
  <Child>
    <h3>hello</h3>
  </Child>
  <Child />
</div>
```

### Named slot:

```html
<!-- Child.svelte -->
<div>
  {#if $$slots.header}
  <slot name="header" />
  {/if} {#if $$slots.body}
  <slot name="body" />
  {/if} {#if $$slots.footer}
  <slot name="footer" />
  {/if}
</div>

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<div>
  <Child>
    <div slot="header">
      <h1>Header</h1>
    </div>
    <div slot="body">
      <p>Body</p>
    </div>
    <div slot="footer">
      <p>Footer</p>
    </div>
  </Child>
</div>
```

### Slot props:

```html
<!-- Child.svelte -->
<script>
  let users = [
    { firstName: "John", lastName: "cine" },
    { firstName: "Jane", lastName: "water" },
    { firstName: "Jack", lastName: "man" },
  ];
</script>

{#each users as user}
<slot name="user" firstName={user.firstName} lastName={user.lastName} />
{/each}

<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<div>
  <Child>
    <h3 slot="user" let:firstName let:lastName>{firstName} {lastName}</h3>
  </Child>

  <Child>
    <h3 slot="user" let:firstName>{firstName}</h3>
  </Child>
</div>
```

### Styling in svelte:

#### scope style

```html
<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<h4>parent title</h4>
<Child />

<style>
  h4 {
    color: red;
  }
</style>

<!-- Child.svelte -->
<h4>child title</h4>
<style>
  h4 {
    color: blue;
  }
</style>
```

#### global style

```html
<!-- App.svelte -->
<script>
  import Child from "./components/Child.svelte";
</script>

<h4>parent title</h4>
<Child />

<style>
  :global(h4) {
    color: red;
  }
</style>

<!-- Child.svelte -->
<h4>child title</h4>
```

### Life cycle hooks:

there are total 5 life cycle hooks.

- `onMount` - execute on load of a component. It also return a function which called at the time of unmount a component. It doesn't render in server-side component.
- `onDestroy` - execute a function on unmount a function. it run in server side component.
- `beforeUpdate` - execute immediately before a component update
- `afterUpdate` - execute immediately after a component update
- `tick` - return a promise that resolved once any pending state changed have been applied

### HTTP request:

```html
<script>
  import { onMount } from "svelte";

  let posts = [];
  onMount(async () => {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts = await response.json();
    console.log(posts);
  });
</script>

<div>
  {#each posts as post}
  <div>
    <h1>{post.title}</h1>
    <p>{post.body}</p>
  </div>
  {:else}
  <div>
    <h1>loading...</h1>
  </div>
  {/each}
</div>
```

### Dynamic component:

```html
<!-- TabA.svelte -->
<h1>tab A</h1>

<!-- TabB.svelte -->
<h1>tab B</h1>

<!-- TabC.svelte -->
<h1>tab C</h1>

<!-- App.svelte -->
<script>
  import TabA from "./components/TabA.svelte";
  import TabB from "./components/TabB.svelte";
  import TabC from "./components/TabC.svelte";

  let activeTab = TabA;
</script>

<div>
  <button on:click={() => (activeTab = TabA)}>Tab A</button>
  <button on:click={() => (activeTab = TabB)}>Tab B</button>
  <button on:click={() => (activeTab = TabC)}>Tab C</button>

  <svelte:component this={activeTab} />
</div>
```

### Special component:

- `svelte:component` - for dynamic component
- `svelte:self` - allow a component to contain itself recursively
- `svelte:window` - add event listener to window object
- `svelte:body` - listen for events that fire on the document body
- `svelte.head` - insert element in `<head>` tag
- `svelte:options` - specify compiler options

### Module context:

> variable of module context are not reactive

```html
<!-- Child.svelte -->
<script context="module">
  // variable  of module context are not reactive
  let totalCount = 0;

  export function getTotalCount() {
    return totalCount;
  }
</script>
<script>
  let count = 0;

  function handleClick() {
    count += 1;
    totalCount += 1;
  }
</script>

<h1>{count}</h1>
<button on:click={handleClick}>Click me</button>

<!-- App.svelte -->
<script>
  import Child, {getTotalCount} from "./components/Child.svelte";
</script>

<div>
  <button on:click={() => console.log(getTotalCount())}>get total</button>
  <Child />
  <Child />
</div>
```

### Store:

Store in svelte is a feature to share variables across the application. It helps to manage the global states across the application.

#### Create a store:

create a `store` folder in `src` and create a `stores.js` file in `store` folder.

#### Store types:

there are 3 types of stores.

- writable
- readable
- derived

#### writable store:

```javascript
// stores.js
import { writable } from "svelte/store";

export const count = writable(0);
```

```html
<!-- App.svelte -->
<script>
  import { count } from "./store/stores";
  import { onDestroy } from "svelte";

  // pattern 1 - subscribe  and unsubscribe
  let counter;
  let counterStore = count.subscribe((value) => {
    counter = value;
  });
  onDestroy(counterStore);

  // pattern 2 - use $count

  // handle increment
  function increment() {
    count.update((value) => value + 1);
  }
  // handle decrement
  function decrement() {
    count.update((value) => value - 1);
  }
  // handle reset
  function reset() {
    count.set(0);
  }
</script>

<div>
  <!-- pattern 1 -->
  <h2>counter is {counter}</h2>
  <!-- pattern 2 -->
  <h2>counter is {$count}</h2>
  <!-- increment -->
  <button on:click={increment}>increment</button>
  <!-- decrement -->
  <button on:click={decrement}>decrement</button>
  <!-- reset -->
  <button on:click={reset}>reset</button>
</div>
```

#### readable store:

```javascript
// stores.js
import { readable } from "svelte/store";

export const time = readable(new Date(), (set) => {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return () => {
    clearInterval(interval);
  };
});
```

```html
<script>
  import { time } from "./store/stores";

  const dateFormatter = new Intl.DateTimeFormat("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
    hour: "numeric",
    minute: "numeric",
    second: "numeric",
  });
</script>

<div>
  <h2>the time is {dateFormatter.format($time)}</h2>
</div>
```

#### derived store:

```javascript
// stores.js
import { readable, derived } from "svelte/store";

export const time = readable(new Date(), (set) => {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return () => {
    clearInterval(interval);
  };
});

const start = new Date();

export const elapsed = derived(time, ($time) => {
  return Math.round(($time - start) / 1000);
});
```

```html
<!-- App.svelte -->
<script>
  import { time, elapsed } from "./store/stores";

  const dateFormatter = new Intl.DateTimeFormat("en-US", {
    year: "numeric",
    month: "long",
    day: "numeric",
    hour: "numeric",
    minute: "numeric",
    second: "numeric",
    timeZoneName: "short",
  });
</script>

<div>
  <h2>the time is {dateFormatter.format($time)}</h2>
  <p>elapsed: {$elapsed} seconds</p>
</div>
```

#### Custom store:

```javascript
// stores.js
import { writable } from "svelte/store";

function createCount() {
  const { subscribe, set, update } = writable(0);

  return {
    subscribe,
    increment: (size = 1) => update((n) => n + size),
    decrement: (size = 1) => update((n) => n - size),
    reset: () => set(0),
  };
}

export const count = createCount();
```

```html
<!-- App.svelte -->
<script>
	import {count} from "./store/stores";
</script>

<div>
	<h2>counter is {$count}</h2>
	<!-- increment -->
	<button on:click={() => count.increment()}>increment by 1</button>
	<button on:click={() => count.increment(5)}>increment by 5</button>
	<!-- decrement -->
	<button on:click={() => count.decrement()}>decrement by 1</button>
	<button on:click={() => count.decrement(5)}>decrement by 5</button>
	<!-- reset -->
	<button on:click={count.reset}>reset</button>
</div>
```

### Animation, transition, motion, tweened:
svelte has build in motion and animation system

#### tweened:
```html
<script>
    import {tweened} from "svelte/animate"
    const progress = tweened(0, {
        duration: 1000,    
    })
    
    onMount(() => {
        progress.set(10);
    })
</script>

<progress max="20" min="0" value={progress} />
```
