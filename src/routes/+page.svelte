<script>
     import '../style.css'
     let todoItem = '';
     let todoList = [];
     function addToArray() {
          if (todoItem == '') {
               return;
          }
          // todoList.push(todoItem);
          todoList = [...todoList, {
               text: todoItem,
               done: false
          }];
          todoList = todoList;
          console.log(todoList);
          todoItem = '';
     }

     function removeThis(index) {
          todoList.splice(index, 1);
          todoList = todoList;
     }

</script>

<div class="marquee">
     <span>My To Do List | My To Do List | My To Do List</span>
</div>

<div class="content">
     <form on:submit|preventDefault={addToArray}>
          <input type="text" bind:value={todoItem}>
          <button type="submit">ADD</button>
     </form>
     
     <ul>
          {#each todoList as item, index}
               <li>
                    <input type="checkbox" bind:checked={item.done}>
                    <span class:done={item.done }>{item.text}</span>
                    <span on:click={() => removeThis (index)} class="remove" role="button" tabindex="0">&times;</span>
               </li>
          {/each}
     </ul>
     <style>
          ul {
               list-style: none;
          }
          li {
               font-size: 3vw
          }
          .done {
               list-style: underline;
               color: grey;
               text-decoration: 2px solid red line-through;
          }
          .remove {
               cursor: pointer;
         }
         .remove:hover {
               color: red;
               cursor: pointer;
         }
     </style>
</div>
