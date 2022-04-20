<script>

  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let autofocus = false;

  let name = "";
  let nameEl;

  import { onMount } from "svelte";
  onMount(() => autofocus && nameEl.focus())

  const addTodo = () => {
    dispatch("addTodo", name);
    name = "";
    nameEl.focus();
  };

  const onCancel = () => {
    name = "";
    nameEl.focus();
  };
</script>

<form
  on:submit|preventDefault={addTodo}
  on:keydown={(e) => e.key === "Escape" && onCancel()}
>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done?</label>
  </h2>
  <input
    bind:value={name}
    bind:this={nameEl}
    type="text"
    id="todo-0"
    autoComplete="off"
    class="input input__lg"
  />
  <button type="submit" disabled={!name} class="btn btn__primary___lg"
    >Add</button
  >
</form>
