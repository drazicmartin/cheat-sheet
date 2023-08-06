# Svelte Basics

## Quick

- [Svelt example](https://svelte.dev/examples/hello-world)

## File Structure

```svelte
<script>
  [CODE GOES HERE]
</script>

[HTML GOES HERE]

<style>
  [CSS GOES HERE]
</style>
```

Show varibale value in HTML like so : 
```svelte 
<h1> {variable} <h1>
```

Shorthand attributes if attribute and variable name have the same name : 
```svelte
<img src={src} /> = <img {src} />
```

Import with
```svelte
<script>
  import Nested from './Nested.svelte';
</script>

<Nested />
```

Function
```js
function incrementCount() {
  count += 1;
}
```

Event handler
- Simple Example
  ```svelte
  <button on:click={incrementCount}>
  ```
- Inline Example
  ```svelte
  <div on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })}>
    The mouse position is {m.x} x {m.y}
  </div>
  ```
- List of events:
  | click | change | mousemove |
  | :-:   |
- Modifier : DOM event handlers can have modifiers that alter their behaviour
  ```svelte
  <button on:click|once={handleClick}> Click me </button>
  ```
  - You can chain modifiers together, e.g. `on:click|once|capture={...}`
  - List of event Modifier
    - `preventDefault` : calls event.preventDefault() before running the handler. Useful for client-side form handling, for example.
    - `stopPropagation` : calls event.stopPropagation(), preventing the event reaching the next element
    - `passive` : improves scrolling performance on touch/wheel events (Svelte will add it automatically where it's safe to do so)
    - `nonpassive` : explicitly set passive: false
    - `capture` : fires the handler during the capture phase instead of the bubbling phase (MDN docs)
    - `once` : remove the handler after the first time it runs
    - `self` : only trigger handler if event.target is the element itself
    - `trusted` : only trigger handler if event.isTrusted is true. I.e. if the event is triggered by a user action.

Creating And Passing Event
- Simple Example
  - ```svelte
    <script>
    	import { createEventDispatcher } from 'svelte';
    
    	const dispatch = createEventDispatcher();
    
    	function sayHello() {
    		dispatch('message', {
    			text: 'Hello!'
    		});
    	}
    </script>
  
    <button on:click={sayHello}> Click to say hello </button>
    ```
  - ```svelte
    <script>
    	import Inner from './Inner.svelte';
    
    	function handleMessage(event) {
    		alert(event.detail.text);
    	}
    </script>
    
    <Inner on:message={handleMessage} />
    ```
- Event Forwarding
  ```svelte
  <script>
    import Inner from './Inner.svelte';
  </script>
  
  <Inner on:message />
  ```

Reactive values
```js
let count = 0;
$: doubled = count * 2;

$: if (count >= 10) {
  alert('count is dangerously high!');
  count = 9;
}
```

Binding
- value
  ```svelte
  <input bind:value={name} />
  ```
- checked
  ```svelte
  <input type="checkbox" bind:checked={yes} />
  ```
- group
  ```svelte
  {#each menu as flavour}
  	<label>
  		<input type="checkbox" bind:group={flavours} name="flavours" value={flavour} />
  		{flavour}
  	</label>
  {/each}
  ```
- Shorthand if variable has same name as binding
  ```svelte
  <textarea bind:value={value} /> == <textarea bind:value />
  ```

Props with default value
- Nested.svelte
  ```svelte
  <script>
    export let answer = 'a mystery';
  </script>
  ```
- App.svelte
  ```svelte
  <Nested answer={42} />
  <Nested />
  ```

If you have an object of properties, you can 'spread' them onto a component instead of specifying each one:
```svelte
<Info {...pkg} />
```

Globals
 - `$$props` : reference all the props that were passed into a component, including ones that weren't declared with export

If Logic
- ```svelte
  {#if user.loggedIn}
    <button on:click={toggle}> Log out </button>
  {/if}
  
  {#if !user.loggedIn}
    <button on:click={toggle}> Log in </button>
  {/if}
  ```
- ```svelte
  {#if user.loggedIn}
    <button on:click={toggle}> Log out </button>
  {:else}
    <button on:click={toggle}> Log in </button>
  {/if}
  ```

Loop Logic
- ```svelte
  {#each cats as cat, i}
    <li>
      <a target="_blank" href="https://www.youtube.com/watch?v={cat.id}" rel="noreferrer">
        {i + 1}: {cat.name}
      </a>
    </li>
  {/each}
  ```
- Deal with removing item in each block
  ```svelte
  {#each things as thing (thing.id)}
  	<Thing name={thing.name} />
  {/each}
  ```

Await block
- Full Promise example
  ```svelte
  {#await promise}
    <p>...waiting</p>
  {:then number}
    <p>The number is {number}</p>
  {:catch error}
    <p style="color: red">{error.message}</p>
  {/await}
  ```
- Quick Promise example
  ```svelte
  {#await promise then number}
    <p>the number is {number}</p>
  {/await}
  ```
