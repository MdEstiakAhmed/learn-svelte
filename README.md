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
- A *.svelte file is a custom file format that uses HTML-like syntax to describe a portion of the UI.
- Each *.svelte file contain 3 parts. script, markup and style.
- browser doesn't understand .svelte extension. Rollup plugin parse the file for browser.

## Syntax:
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
    const number = 5;   // -5, 0, 5
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