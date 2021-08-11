<script lang="ts">
  // ---------------------------------------------------------
  //  Global Imports
  // ---------------------------------------------------------
  import Modal from '@/components/Modal.svelte'
  import { createEventDispatcher } from 'svelte';

  // ---------------------------------------------------------
  //  Props
  // ---------------------------------------------------------

  export let id: number | undefined = undefined
  export let title: string | undefined = undefined

  // ---------------------------------------------------------
  //  Methods
  // ---------------------------------------------------------

  const dispatch = createEventDispatcher();
</script>

<Modal
  on:closeModal="{() => dispatch('close')}"
>
  <div slot="title" class="title">Confirm Delete</div>
  <div slot="body" class="modal-body">
    Are you sure you want to delete "{title}" ?
  </div>

  <div slot="footer" class="modal-footer">
    <button
      class="button delete"
      on:click|stopPropagation="{() => dispatch('delete', id)}"
    >
      Delete
    </button>
    <button
      class="button"
      on:click|stopPropagation="{() => dispatch('close')}"
    >
      Cancel
  </button>
  </div>

</Modal>

<style lang="scss">
  .modal {
    &-body {
      padding: 30px 15px;
      width: 340px;
   }

   &-footer {
      padding: 0 15px;
      text-align: right;

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

        &.delete {
          background-color: #e81414;
          color: #fff;

          &:active {
             background-color: #b11111;
          }
        }
      }
    }
  }
</style>