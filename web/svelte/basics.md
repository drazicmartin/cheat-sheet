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
<img src={src} />
 is the same as
<img {src} />
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
- event list:
  | Event Name |
  | :-:   |
  | click |
  | mousemove |

Reactive values
```js
let count = 0;
$: doubled = count * 2;

$: if (count >= 10) {
  alert('count is dangerously high!');
  count = 9;
}
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
