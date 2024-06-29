<script>
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'
  let lastId = 0;
  if (localStorage.getItem('lastId')){
    lastId = parseInt(localStorage.getItem('lastId'),10);
  }
  
  let blocks = [{ id:`Notebook${lastId}`,title: "", deccription: "", content:''}];
  if (localStorage.getItem('storedBlocks'))
  blocks = JSON.parse(localStorage.getItem('storedBlocks'));
  function createBlock(){
    lastId++;
    blocks = [...blocks,{
      id: `Notebook${lastId}`,
      title: "",
      deccription: "",
      content:''
    }]
  };
  function deleteBlock(id){
    blocks = blocks.filter(block => block.id != id); 
    saveBloc()
  };
  function saveBloc(){
    localStorage.setItem('storedBlocks', JSON.stringify(blocks));
    localStorage.setItem('lastId', lastId);
  };
  function handleTitleChange(event, blockId){
    const updatedBlock = blocks.map(block =>{
      if (block.id === blockId){
        return{...block, title: event.target.value};
      }
      return block;
    });
    blocks = updatedBlock
  };
  function handleDecriptionChange(event, blockId) {
  const updatedBlocks2 = blocks.map(block => {
    if (block.id === blockId) {
      return { ...block, deccription: event.target.value };
    }
    return block;
  });
  blocks = updatedBlocks2;
};

</script>
<header class="p-3 m-auto max-w-[1400px]">
  <h1 class=" font-medium text-4xl">Памяти<span class=" text-red-600">Нет</span></h1>
</header>
<main>
<section class=" p-3 flex flex-col align-left gap-3 max-w-[1400px] m-auto">
  {#each blocks as block}
  <div id={block.id} class="relative   flex flex-col gap-3 p-3 bg-white h-45 w-auto shadow rounded-xl justify-between"> 
    {block.content}
    <div class=" flex gap-3 flex-col">
      <input on:input={(event) => handleTitleChange(event, block.id)}  bind:value={block.title} class=" text-2xl w-4/5  font-normal " placeholder="Заголовок"/>
      <textarea on:input={(event) => handleDecriptionChange(event, block.id)} bind:value={block.deccription} class="font-light w-auto text-stone-600 h-32" placeholder="Начните ввод"/>
    </div>
    <button on:click={() => deleteBlock(block.id)} class="h-7 absolute w-22 top-2 right-2 text-center">Удалить</button>
    <button on:click={saveBloc} class=" w-24 h-7  bg-orange-200 rounded-xl text-stone-600 hover:bg-orange-300 hover:text-stone-800 transition-colors">Сохранить</button>
  </div>
  {/each}
  
  <button on:click={createBlock} class=" text-stone-400 hover:text-stone-800 transition-colors text-left w-32"> + Новая заметка</button>
</section>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <div class="card">
    <Counter />
  </div> -->
 
</main>

<style>
  
</style>
