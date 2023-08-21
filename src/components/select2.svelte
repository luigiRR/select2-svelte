<script>
// @ts-nocheck

    import { afterUpdate } from "svelte";
  
    /*Los ARRAYS de OBJETOS exportados en el componente deben tener la propiedad name en cada objetos */
    export let arr=[]
    export let itemId;
    export let address = undefined;

    // @ts-ignore
    let originalarr = [];
    let selectedOption = {};
    let searchValue = '';
    let isOpen = false;
    let sizeInput = 0;

    function toggleOptions() {
      isOpen = !isOpen;
    }

    function openOptions() {
      isOpen = true;
    }

    function closeOptions() {
      isOpen = false;
    }

   // @ts-ignore
     $: if (arr.length > 0 && sizeInput==0) {
      // @ts-ignore
      originalarr = [...arr];
      sizeInput++;
    }
  
    // @ts-ignore
    function searchOption(event) {
      searchValue = event.target.value.toLowerCase();
      // @ts-ignore
      arr=[...originalarr];
    }
  
    // @ts-ignore
    function selectOption(option) {
      itemId = option.id
      address = option.address || "va 123";
      selectedOption = option;
      isOpen = false;
    }

    // @ts-ignore
    $: arr = arr.filter((option) =>
      option.name.toLowerCase().includes(searchValue)
    );

    afterUpdate(()=>{
      // @ts-ignore
      if(itemId && itemId!=0){   
        // @ts-ignore
        let obj = arr.filter(e=>e.id==itemId)[0]
        selectedOption = obj
      } else { selectedOption={} }
    })
</script>

<div class="select-container">

    <div class="select-header" on:click={toggleOptions}>
        {selectedOption.id ? selectedOption.name : 'Seleccionar'}
    </div>

   <div class="select-options" class:open={isOpen}>
        <input 
            type="text"
            class="select-search"
            placeholder="Buscar..."
            on:input={searchOption}
            on:focus={openOptions}
            on:blur={closeOptions}
        />

        {#if arr.length===0}
            <div class="select-option">No se encontraron resultados</div>
        {:else}
            {#each arr as option}
                <div class="select-option" 
                class:selected={selectedOption.id === option.id}
                on:click={() => selectOption(option)}>{option.name}</div>
            {/each}
        {/if}
   </div>
</div>

<style>
    .select-container {
      position: relative;
      width: 220px;
    }

    .select-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      border: 1px solid #ccc;
      border-radius: 4px;
      padding: 6px;
      cursor: pointer;
    }

    .select-header:hover {
      background-color: #f2f2f2;
    }
  
    .select-header i {
      font-size: 12px;
    }
    .select-options {
    position: absolute;
    top: calc(100% - 1px);
    left: 0;
    width: 100%;
    max-height: 350px;
    overflow-y: auto;
    background-color: #fff;
    border: 1px solid #ccc;
    border-top: none;
    border-radius: 0 0 4px 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: opacity 0.2s ease, max-height 0.2s ease;
    opacity: 0;
    max-height: 0;
    z-index: 1;
  }

  .select-options.open {
    opacity: 1;
    max-height: 350px;
  }

  .select-search {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    margin-bottom: 8px;
  }

  .select-option {
    padding: 8px;
    cursor: pointer;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .select-option:hover {
    background-color: #f2f2f2;
  }

  .select-option.selected {
    font-weight: bold;
  }
</style>