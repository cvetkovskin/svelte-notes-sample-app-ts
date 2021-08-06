<script lang="ts">
  // -------------------
  //  Imports
  // -------------------

  import moment from 'moment'
  import Fa from 'svelte-fa/src/fa.svelte'
  import { faStar } from '@fortawesome/free-solid-svg-icons';
  import { createEventDispatcher } from 'svelte';

  // -------------------
  //  Props
  // -------------------

  export let id: number
  export let title: string
  export let content: string
  export let date: string
  export let isFavorite: boolean
  export let tags: string[]


  // -------------------
  //  Methods Declarations
  // -------------------

  const dispatch = createEventDispatcher();

  /**
   * Format a date into a humanized time sentence if it's in the same 
   * day or otherwise display a date in the YYYY/MM/DD format
   *
   * @param {String} dateString
   * @returns {String}
   */
  const formatDate = dateString => {
    const date = moment(dateString, 'YYYYMMDDHHmmss')

    if (moment().isSame(date, 'day')) {
      return moment.duration(date.diff(moment(), 'milliseconds')).humanize(true)
    } else {
      return date.format('YYYY/MM/DD')
    }  
  }

  /**
   * Trim the content if longer than 100 characters and add ellipsis
   *
   * @param {String} content
   */
  const trimContent = (content: string) => {
    if (content.length > 100) {
      return `${content.substring(0, 100)}...`
    }

    return content
  }
</script>

<div class="note-card" on:click >
  <div class="title">{title}</div>
  <div class="preview">{trimContent(content)}</div>

  <div class="tag-wrapper">
  {#each tags as tag (tag)}
    <div class="tag">{tag}</div>
  {/each}
  </div>

  <div class="card-footer">
    <div class="date">{formatDate(date)}</div>
    <div class="fav-icon" on:click|stopPropagation="{() => dispatch('toggleFavorite', id)}">
      <Fa icon={faStar} color="{isFavorite ? '#ffda00' : '#afaeae'}"  
      />
    </div>
  </div>
</div>

<style lang="scss">
  .note-card {
    width: 150px;
    height: 195px;
    margin-right: 15px;
    background-color: #fff;
    padding: 15px;
    border-radius: 10px;
    box-shadow: rgb(0 0 0 / 24%) 0px 3px 8px;
    display: grid;
    grid-template-rows: 1fr 4fr 2fr 1fr;

    &-add {
      background-color: #d6d4d4;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: none;
      border: 4px dashed #a29f9f;
      width: 144px;
      height: 189px;
      color: #a29f9f;
      margin-right: 15px;
      padding: 15px;
      border-radius: 10px;

      &:hover {
          background-color: #c5c5c5;
      }
    }

    .title {
      font-weight: bold;
      margin-bottom: 10px;
    }

    .preview {
      font-size: 15px;
    }

    .tag {
      background-color: #d6d2d2;
      padding: 2px 10px;
      border-radius: 20px;
      font-size: 12px;
      margin-right: 5px;
      height: 15px;

      &-wrapper {
        display: flex;
        align-items: flex-end;
      }
    }

    .card-footer {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;

      .date {
        color: #afaeae;
        font-size: 14px;
      }
    }
  }

  .fav-icon {
    cursor: pointer;
  }
</style>