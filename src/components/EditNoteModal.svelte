<script lang="ts">
  // -------------------
  //  Global Imports
  // -------------------

  import Fa from 'svelte-fa/src/fa.svelte'
  import { faTimes } from '@fortawesome/free-solid-svg-icons';
  import { createEventDispatcher } from 'svelte';

  // -------------------
  //  Props
  // -------------------

  export let id: number | undefined = undefined
  export let title: string | undefined = undefined
  export let content: string | undefined = undefined
  export let date: string | undefined = undefined
  export let isFavorite: boolean | undefined = undefined
  export let tags: string[] | undefined = undefined

  // -------------------
  //  Methods Declarations
  // -------------------

  const dispatch = createEventDispatcher();

  console.log(id, title, content, date, isFavorite, tags)
</script>


<div class="modal-overlay">
  <div class="modal">
    <div class="header">
      <div class="title"> {id ? 'Edit' : 'Create' } Post</div>
      <div class="close-icon" on:click="{() => dispatch('close')}">
        <Fa icon={faTimes} color="#afaeae" size="1x" />
      </div>
    </div>
    <div>
      <!-- TODO; Add edit post here -->
      Work in progress
    </div>
  </div>
</div>

<style lang="scss">
  .modal {
    background-color: white;
    padding: 15px;
    border-radius: 10px;
    min-width: 350px;
    box-shadow: rgb(0 0 0 / 24%) 0px 3px 8px;

    &-overlay {
      position: absolute;
      top: 0;
      height: 100vh;
      width: 100vw;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: rgb(0, 0, 0, 0.4);
    }

    .header {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;

      .title {
        font-weight: bold;
      }
    }

    .close-icon {
      cursor: pointer;
    }
  }
</style>