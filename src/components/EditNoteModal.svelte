<script lang="ts">
  // ---------------------------------------------------------
  //  Global Imports
  // ---------------------------------------------------------

  import moment from 'moment'
  import Fa from 'svelte-fa/src/fa.svelte'
  import { faTimes } from '@fortawesome/free-solid-svg-icons';
  import { createEventDispatcher, onMount } from 'svelte';

  // ---------------------------------------------------------
  //  Props
  // ---------------------------------------------------------

  export let id: number | undefined = undefined
  export let title: string | undefined = undefined
  export let content: string | undefined = undefined
  export let isFavorite: boolean | undefined = undefined
  export let tags: string[] | undefined = undefined

  // ---------------------------------------------------------
  //  Varaible Declarations
  // ---------------------------------------------------------

  let tagString = tags && tags.length ? tags.join(',') : ''

  // Simple validation
  $: canSave = Boolean(title && content)

  // ---------------------------------------------------------
  //  Note Edit Methods
  // ---------------------------------------------------------

  const dispatch = createEventDispatcher();

  /**
   * Save the note if allowed by dispatching a save event
   * with the new memo object
   */
  const saveNote = () => {
    if (!canSave) return

    const newMemo = {
      id,
      title,
      content,
      date: moment().format('YYYYMMDDHHmmss'),
      isFavorite,
      tags: tagString.replace(/\s/g,'').split(',').filter(item => item)
    }

    if (!newMemo.id) {
      newMemo.id = moment().valueOf()
    }

    dispatch('save', newMemo)
  }

  // ---------------------------------------------------------
  //  TextArea Resizing
  // ---------------------------------------------------------

  let textarea: HTMLElement

  const onInput = () => {
    if (textarea) {
      textarea.style.height = 'auto';
      textarea.style.height = `${textarea.scrollHeight}px`
    }
  }

  onMount(() => {
    onInput()
  })
</script>


<div class="modal-overlay">
  <div class="modal">
    <div class="modal-header">
      <div class="title"> {id ? 'Edit' : 'Create' } Post</div>
      <div class="close-icon" on:click="{() => dispatch('close')}">
        <Fa icon={faTimes} color="#afaeae" size="1x" />
      </div>
    </div>
  
    <div class="modal-body">
      <label class="label" for="note-title">Title:</label>
      <input bind:value={title} id="note-title" class="input" type="text" />

      <label class="label" for="note-tags">Tags:</label>
      <input bind:value={tagString}  id="note-tags" class="input" type="text" />

      <label class="label" for="note-content">Content:</label>
      <textarea
        id="note-content"
        bind:this={textarea}
        bind:value={content}
        class="input"
        type="textarea"
        on:input="{onInput}"
      />
    </div>

    <div class="modal-footer">
      <div class="delete-wrapper">
        {#if id}
          <button
            class="button delete"
            on:click|stopPropagation="{() => dispatch('delete', id)}"
          >
            Delete
          </button>
        {/if}
      </div>
      <div class="buttons-wrapper">
        <button
          class="button save {!canSave? 'disabled' : ''}"
          on:click|stopPropagation="{saveNote}"
        >
          Save
        </button>
        <button
          class="button"
          on:click|stopPropagation="{() => dispatch('close')}"
        >
          Cancel
      </button>
      </div>
    </div>
  </div>
</div>

<style lang="scss">
  .modal {
    background-color: white;
    padding: 15px 0;
    border-radius: 10px;
    width: 60vw;
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

    &-header {
      display: flex;
      justify-content: space-between;
      padding: 0 15px 10px;
      border-bottom: 3px solid #e2e2e2;

      .title {
        font-weight: bold;
      }
    }

    &-body {
      padding: 20px 15px 10px;
      display: grid;
      grid-template-columns: 1fr 3fr;
      gap: 15px 0;

      .label {
        grid-column: 1;
        line-height: 30px;
      }

      .input {
        grid-column: 2;
        height: 30px;
        border-radius: 5px;
        border: 1px solid #c3c3c3;
      }
    }

    &-footer {
      padding: 0 15px;
      display: grid;
      grid-template-columns: 1fr 1fr;

      .buttons-wrapper {
        text-align: right;
      }

      .button {
        height: 30px;
        padding: 0 10px;
        text-align: center;
        box-sizing: content-box;
        border-radius: 3px;
        border: 1px solid #000;



        &:active {
          background-color: #b9b7b7;
        }

        &.save {
          background-color: #1dbd73;
          color: #fff;

          &:active {
             background-color: #1a8e56;
          }

          &.disabled {
            opacity: 0.5;

            &:active {
              background-color: #1dbd73;
            }
          }
        }

        &.delete {
          background-color: #e81414;
          color: #fff;

          &:active {
             background-color: #b11111;
          }
        }
      }
    }

    .close-icon {
      cursor: pointer;
    }
  }

  #note-content {
    resize: vertical;
    height: fit-content;
    min-height: 30px;
  }
</style>