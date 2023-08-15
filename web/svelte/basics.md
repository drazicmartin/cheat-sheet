# Svelte Basics

- [Reactivity](#reactivity)
- [Props](#props)
- [Logic](#logic)
- [Event](#events)
- [Binding](#binding)
- [Lifecycle](#lifecycle)
- [Store](#store)
- [Motion](#motion)

## Quick

- [Svelt example](https://svelte.dev/examples/hello-world)
- UI
  - [flowbite-svelte](https://github.com/themesberg/flowbite-svelte)

## File Structure

```svelte
<script>
  [CODE GOES HERE]
</script>

[MARKUP GOES HERE]

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

<a name="events">

# Event
## Event handler
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
  | :-:   |  :-:   |     :-:   |
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

## Creating And Passing Event
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

<a name="reactivity">

# Reactive values
```js
let count = 0;
$: doubled = count * 2;

$: if (count >= 10) {
  alert('count is dangerously high!');
  count = 9;
}
```

<a name="binding">

# Binding
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
- Binding in for each block
  ```svelte
  {#each todos as todo}
  <div class:done={todo.done}>
    <input type="checkbox" bind:checked={todo.done} />

    <input placeholder="What needs to be done?" bind:value={todo.text} />
  </div>
  {/each}
  ```
- Binding dimentions
  - They are `readonly`
    | clientWidth | clientHeight | offsetWidth  | offsetHeight  |
    | :-:         |  :-:         |     :-:      |     :-:       |
    ```svelte
    <div bind:clientWidth={w} bind:clientHeight={h}>
      <span style="font-size: {size}px">{text}</span>
    </div>
    ```
- Bindig `this`\
  Note that the value of canvas will be undefined until the component has mounted, so we put the logic inside the onMount
  ```svelte
  <canvas bind:this={canvas} width={32} height={32} />
  ```

<a name="props">

# Props with default value
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


<a name="logic">
  
# Logic

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

<a name="lifecycle"/>

Every component has a lifecycle that starts when it is created, and ends when it is destroyed. There are a handful of functions that allow you to run code at key moments during that lifecycle.

- Component lifecycles in order : 
  - `onMount` : runs after the component is first rendered to the DOM
    - import
      ```svelte
      import { onMount } from 'svelte';
      ```
    - usage :
        ````svelte
        onMount(() => { <CODE> });

        onMount(async () => { <CODE> });
        ```   
    - It's recommended to put the fetch in onMount rather than at the top level of the <script> because of server-side rendering (SSR).
      - example:
        ```svelte
        onMount(async () => {
          const res = await fetch(`/tutorial/api/album`);
          photos = await res.json();
        });
        ```
   - `beforeUpdate` : Schedules work to happen immediately before the DOM is updated
   - `afterUpdate` : Used for running code once the DOM is in sync with your data
   - `onDestroy` : run code when your component is destroyed
     - import
       ```svelte
       import { onDestroy } from 'svelte';
       ```
- `tick` : The tick function is unlike other lifecycle functions in that you can call it any time, not just when the component first initialises. It returns a promise that resolves as soon as any pending state changes have been applied to the DOM (or immediately, if there are no pending state changes).
  - usage in async function
    ````svelte
    await tick();
    ```

<a name="store"/>
    
# Store

A store is simply an object with a subscribe method that allows interested parties to be notified whenever the store value changes

- `writable`
  - stores.js
    ```js
    import { writable } from 'svelte/store';
    export const count = writable(0);
    ```
  - methods
    - `subscribe`
      ```js
      <script>
        import { count } from './stores.js';
        import { onDestroy } from 'svelte';
  
        let countValue;
        const unsubscribe = count.subscribe((value) => {
      		countValue = value;
      	});
  
        onDestroy(unsubscribe);
      </script>

      <h1>The count is {countValue}</h1>
      ```
      Or using `$` reserved character
      ```js
      import { count } from './stores.js';
      
      <h1>The count is {$count}</h1>
      ```
    - `update`
      ```js
      import { count } from './stores.js';
      function decrement() {
        count.update((n) => n - 1);
    	}
      ```
    - `set`
      ```js
      import { count } from './stores.js';
      function reset() {
        count.set(0);
        // equivalent to
        count = 0;
    	}
      ```
- `readable` : The first argument to readable is an initial value, which can be null or undefined if you don't have one yet. The second argument is a start function that takes a set callback and returns a stop function. The start function is called when the store gets its first subscriber; stop is called when the last subscriber unsubscribes.
  - example stores.js
    ```js
    import { readable } from 'svelte/store';
    
    export const time = readable(new Date(), function start(set) {
    	const interval = setInterval(() => {
    		set(new Date());
    	}, 1000);
    
    	return function stop() {
    		clearInterval(interval);
    	};
    });
    ```
- `derived` : value is based on the value of one or more other stores
  - example stores.js
    ```js
    import { derived } from 'svelte/store';
    const start = new Date();

    export const elapsed = derived(time, ($time) => Math.round(($time - start) / 1000));
    ```

Custom store
- example stores.js
  ```js
  import { writable } from 'svelte/store';
  
  function createCount() {
  	const { subscribe, set, update } = writable(0);
  
  	return {
  		subscribe,
  		increment: () => update((n) => n + 1),
  		decrement: () => update((n) => n - 1),
  		reset: () => set(0)
  	};
  }
  
  export const count = createCount();
  ```

Store binding
- example
  - stores.js
    ```js
    import { writable, derived } from 'svelte/store';

    export const name = writable('world');
    
    export const greeting = derived(name, ($name) => `Hello ${$name}!`);
    ```
  - App.svelte
    ```svelte
    <script>
    	import { name, greeting } from './stores.js';
    </script>
    
    <h1>{$greeting}</h1>
    <input bind:value={$name} />
    
    <button on:click={() => ($name += '!')}> Add exclamation mark! </button>
    ```
  - `$name += '!'` assignment is equivalent to `name.set($name + '!')`

<a name="motion"/>

# Motion

They work like store that allow animation

- spring
  - import
    ```svelte
    import { spring } from 'svelte/motion';
    ```
  - example
    ```svelte
    import { spring } from 'svelte/motion';
  
    let coords = spring(
      { x: 50, y: 50 },
      {
        stiffness: 0.1,
        damping: 0.25
      }
    );
    ```
- tweened
  - import
    ```svelte
    import { tweened } from 'svelte/motion';
    ```
  - example
    ```svelte
    import { tweened } from 'svelte/motion';
    import { cubicOut } from 'svelte/easing';
    
    const progress = tweened(0, {
  		duration: 400,
  		easing: cubicOut
  	});
    ```
