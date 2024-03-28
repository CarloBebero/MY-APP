<!-- Design Inspired From @CodeGrid -->


<div class="marquee">
     <span>My To Do List | My To Do List | My To Do List</span>
</div>
<div class="container">

     <nav>
       <div class="nav-1">
         <p class="logo">A2</p>
         <p class="title">
          ToDO<br />
          APP
         </p>
       </div>
       <div class="nav-2">
         <p class="intro">Carlo </p>
         <p class="pattern-2">Bebero</p>
       </div>
     </nav>

     <!-- site content -->
     <div class="content-wrapper">
       <div class="col-1 col">
         <div class="header">
           <h1>
             My To DO <br /> 
             List
           </h1>
         </div>
         <div class="hero-img">
          <div class="img-revealer"></div>
          <img src="/src/images/list-pic.jpg" alt="" />
        </div>
         <div class="hero">
           <p>A2: Svelte ToDo App</p>
           <p>course code: IM 415 B01</p>
           <p>course title: Application Development</p>
         </div>
         <div class="cta">
           <div class="cta-btn">
             <h3>Create List :</h3>
           </div>
         </div>
       </div>

       <div class="col-2 col">
         <div class="img">
           <div class="img-data">
                 <div class="img-name">
                         <p>My List</p>
                         <form on:submit|preventDefault={addToArray} class="form-container">
                              <input type="text" placeholder="Add a new task" bind:value={todoItem}>
                              <div class="checkbox-container">
                                   <label><input type="checkbox" bind:checked={urgent}> URGENT</label>
                                   <label><input type="checkbox" bind:checked={someday}> SOMEDAY</label>
                              </div>
                              <button class="submit" type="submit">Add Task</button>
                         </form>
                         <ul class="todo-list">
                              {#each $todoList as item, index}
                                   <li>
                                        <input type="checkbox" bind:checked={item.done} on:change={updateList}>&nbsp;
                                        <span class:done={item.done}>{item.text}</span>
                                        <span on:click={() => removeThis(index)} class="remove-btn" role="button" tabindex="0">&times;</span>
                                   </li>
                              {/each}
                         </ul>
                         {#if somedayList.length > 0}
                              <h2>SOMEDAY</h2>
                              <ul class="someday-list">
                                   {#each somedayList as item, index}
                                        <li>
                                             <input type="checkbox" bind:checked={item.done} on:change={updateList}> &nbsp;
                                             <span class:done={item.done}>{item.text}</span>
                                             <span on:click={() => removeThisSomeday(index)} class="remove-btn" role="button" tabindex="0">&times;</span>
                                        </li>
                                   {/each}
                              </ul>
                         {/if}
                 </div>
           </div>
           <div class="hr"></div>
          </div>
           <div class="carousel">
               <button on:click={clearDone} class="remove">
                    Remove Completed
               </button>
             <p style="font-size: 3vw;">&#8621;</p>
               <button on:click={clearAll} class="remove">
                    Remove All Tasks
               </button>
           </div>
         </div>
       </div>
</div>

<script>
     import '../style.css';
     import { writable } from 'svelte/store';
     let todoItem = '';
     let storedList, urgent, someday;
     let todoList = writable([]);

     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedList = localStorage.getItem('storedList');
          if(storedList) {
               $todoList = (JSON.parse(storedList));
          }
     }
     $: isDone = $todoList.filter(item => item.done);;
     $: somedayList = $todoList.filter(item => item.someday);
     console.log(somedayList);
     function updateList() {
          return storedList = localStorage.setItem('storedList', JSON.stringify($todoList));
     }

     function addToArray() {
          if (todoItem == '') {
               return;
          }
          $todoList = [...$todoList, {
               text: todoItem,
               done: false,
               urgent: urgent,
               someday: someday
          }];
          isDone = $todoList.filter(item => item.done);
          somedayList = $todoList.filter(item => item.someday);
          console.log(somedayList);
          //console.log($todoList);
          updateList();
          todoItem = '';
          urgent = false; 
          someday = false;
     }
     function removeThis(index) {
          $todoList.splice(index, 1);
          $todoList = $todoList;
          updateList();
     }
     function removeThisSomeday(index) {
          somedayList.splice(index, 1);
          somedayList = somedayList;
          updateList();
     }
     function clearDone() {
          $todoList = $todoList.filter(item => !item.done)
          updateList();
     }
     function clearAll() {
          $todoList = [];
          localStorage.clear();
     }
     
</script>

<style>


.marquee {
     color: #ffffff; 
     text-align: center;
     overflow: hidden;
     font-size: 3vw;
     letter-spacing: .9vw;
}

.marquee span {
     display: inline-block;
     animation: marquee 15s linear infinite;
}

@keyframes marquee {
     0% { transform: translateX(100%); }
     100% { transform: translateX(-100%); }
}

.col-1 .hero-img {
  width: 100%;
  height: 190px;
  border-radius: 50px;
  overflow: hidden;
}

img {
  width: 100%;
  height: 100%;
  object-fit: fill;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.form-container input[type="text"] {
  width: 300px;
  padding: 8px;
  margin-right: 10px;
  background-color: #ffffff; 
  border-radius: 50px;
  color: #1c1c1c;
  border: 2px solid #000000;
}

.checkbox-container {
  display: flex;
  align-items: center;
  margin-right: 10px;
  font-size: .8vw;
  
}

.checkbox-container label {
  margin-right: 10px;
  color: #000000;
}

ul {
     list-style-type: none;
     padding: 0;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.done {
  text-decoration: line-through;
  color: #888;
}

.remove-btn {
  margin-left: 10px;
  color: rgb(205, 0, 0);
  cursor: pointer;
}

.remove-btn:hover {
  text-decoration: underline;
}

p {
  text-transform: uppercase;
}

.container {
  width: 100%;
  height: 100vh;    
  display: flex;
  flex-direction: column;
  gap: 0.25em;
}

nav {
  width: 100%;
  flex: 1;
  display: flex;
  gap: 0.25em;
  padding: 0.25em 0 0 0;
  background: #000;
  font-size: .7rem;
}

nav > div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1em 2em;
  background: #eeeeee; 
  color: #000;
  border-radius: 30px;
}

.nav-1 {
  flex: 3;
}

.nav-2 {
  flex: 1.7;
}

.content-wrapper {
  flex: 12;
  width: 100%;
  height: 100%;
  display: flex;
  padding: 0 0 0.5em 0;
  gap: 0.25em;
  background: #000;
}
@media screen and (max-width: 1000px) {
     .content-wrapper {
          display: block;
     }
     .content-wrapper .col {
          margin-bottom: 3px;
     }    
}

.content-wrapper .col {
  border-radius: 50px;
  background:#eeeeee; 
  padding: 3em 2em;
}

.col-1 {
  flex: 5.5;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.col-2 {
  flex: 3;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.col-1 p {
  font-size: 12px;
  line-height: 1;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.col-1 h1 {
  font-family: "Mars Model";
  font-size: 10vw;
  text-transform: uppercase;
  line-height: 0.8;
}


.cta {
  width: 100%;
}

.cta .cta-btn {
  background: #000;
  color: #ffffff; 
  padding: 1.5em;
  border-radius: 50px;
  display: flex;
  justify-content: flex-end;
}

.cta-btn h3 {
  font-family: "Mars Model";
  font-size: 2.4vw;
  text-transform: uppercase;
  line-height: 1;
}

.img {
  display: flex;
  flex-direction: column;
}

.img-data {
  display: flex;
  justify-content: space-between;
}

.img-name p {
  font-size: 2vw;
}

.img .hr {
  width: 100%;
  height: 2px;
  margin: 1em 0;
  background: #000;
}


.carousel {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.remove {
  font-family: 'Times New Roman', Times, serif;
  font-size: 1vw;
  height: 4.6vw;
  padding: 1em;
  border-radius: 50px;
  background: #000;
  color: #ffffff; 
  transition: all .2s ease-in-out;
  text-transform: uppercase;
}

.submit {
     font-family: 'Times New Roman', Times, serif;
     font-size: .9vw;
     height: 2.5vw;
     width: 6vw;
     /* padding: 1em; */
     border-radius: 50px;
     background: #000;
     color: #ffffff; 
     transition: all .2s ease-in-out;
     text-transform: uppercase;
}

button:hover {
     cursor: pointer;
     background-color: #424242;
}

</style>

