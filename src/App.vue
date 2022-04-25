<template>

  <img v-show="darkmode" src="./assets/images/bg-desktop-dark.jpg" alt="" srcset="" class="hero desktop">
  <img v-show="!darkmode" src="./assets/images/bg-desktop-light.jpg" alt="" srcset="" class="hero desktop">

  <img v-show="darkmode" src="./assets/images/bg-mobile-dark.jpg" alt="" srcset="" class="hero mobile">
  <img v-show="!darkmode" src="./assets/images/bg-mobile-light.jpg" alt="" srcset="" class="hero mobile">


  <div id="center">
    <header>
      <h1 id="header-title">Todo</h1>
      <img v-show="darkmode" @click="changeTheme" class="theme-logo" src="./assets/images/icon-sun.svg" alt="" >
      <img v-show="!darkmode" @click="changeTheme" class="theme-logo" src="./assets/images/icon-moon.svg" alt="" >
    </header>

    <span class="input">
      <div class="first-one" :class="{ light: !darkmode }"> </div>
      <input type="text" :class="{ light: !darkmode }" v-model="task" @keyup.enter.exact="addTask" placeholder="Create a new todo...">
    </span>
    
    <div id="tasks" :class="{ light: !darkmode }">
      <ul>
        <draggable v-model="tasks" tag="transition-group" :component-data="{name:'flip'}" item-key="id" ghost-class="g-class">
          <template #item="{element}">            
            <li v-show="showAll" ref="li" :class="{ light: !darkmode }" @mouseenter="showCancel(element)" @mouseleave="removeCancel" @mousedown="drag=true" @mouseup="removeborder" @end="drag=false"> 
              <span>
                <div class="check" @click="addTick(element), checker" :class="{ colored: element.check, light: !darkmode }"> 
                  <img v-if="element.check" class="img-check" src=".\assets\images\icon-check.svg" alt=""> 
                </div> 
                <p @click="addTick(element), checker" class="task" :class="{ checked: element.check, light: !darkmode }" >{{ element.text }}</p>
              </span>
              <img v-show="element.hover" class="cancel desktop" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
              <img class="cancel mobile" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
            </li>
          </template>
        </draggable>

        <div> {{ completed }} </div>

        <draggable v-model="tasks" tag="transition-group" item-key="id" @start="drag=true" @end="drag=false">
          <template #item="{element}">            
            <li v-show="showActive && element.check == false" ref="li" :class="{ light: !darkmode }" @mouseenter="showCancel(element)" @mouseleave="removeCancel"> 
              <span>
                <div class="check" @click="addTick(element), checker" :class="{ colored: element.check, light: !darkmode }"> 
                  <img v-if="element.check" class="img-check" src=".\assets\images\icon-check.svg" alt=""> 
                </div>
                <p @click="addTick(element), checker" class="task" :class="{ checked: element.check, light: !darkmode }" >{{ element.text }}</p>
              </span>
              <img v-show="element.hover" class="desktop" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
              <img class="mobile" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
            </li>
          </template>
        </draggable>        


        <draggable v-model="tasks" tag="transition-group" item-key="id">
          <template #item="{element}">            
            <li v-show="showComp && element.check == true" ref="li" :class="{ light: !darkmode }" @mouseenter="showCancel(element)" @mouseleave="removeCancel"> 
              <span>
                <div class="check" @click="addTick(element), checker" :class="{ colored: element.check, light: !darkmode }"> 
                  <img v-if="element.check" class="img-check" src=".\assets\images\icon-check.svg" alt=""> 
                </div>
                <p @click="addTick(element), checker" class="task" :class="{ checked: element.check, light: !darkmode }" >{{ element.text }}</p>
              </span>
              <img v-show="element.hover" class="desktop" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
              <img class="mobile" src= "./assets/images/icon-cross.svg" alt="" @click="deleteTask(element)">
            </li>
          </template>
        </draggable>        

      </ul>

      <div id="info">
        <span> {{ unchecked }} items left</span>
        
        <span id="actions" >
          <p @click="returnAll" :class="{ active: showAll, light: !darkmode }">All</p>
          <p @click="returnActive" :class="{ active: showActive, light: !darkmode }">Active</p>
          <p @click="returnComp" :class="{ active: showComp, light: !darkmode }">Completed</p>
        </span>

        <span id="clear" :class="{ light: !darkmode }" @click="clear">Clear completed</span>
      </div>
    </div>

    <div class="mobile" id="mobile-actions" :class="{ light: !darkmode }">
      <span id="actions" >
        <p @click="returnAll" :class="{ active: showAll, light: !darkmode }">All</p>
        <p @click="returnActive" :class="{ active: showActive, light: !darkmode }">Active</p>
        <p @click="returnComp" :class="{ active: showComp, light: !darkmode }">Completed</p>
      </span>
    </div>

    <small class="bgdark" :class="{bgtext: !darkmode }">Drag and drop to reorder list</small>

    <div class="attribution" :class="{ light: !darkmode }">
      Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>
      Coded by <a href="https://www.linkedin.com/in/dapo-ola-olatunji-5128b0160" target="_blank">Dapo</a>
    </div>
    
  </div>
    
   
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'App',
  components: {
    draggable,
  },
  data() {
    return {
      drag: false,

      task: "",
      tasks: [
        {id: 1, text:"Empty the dustbin", hover: false, check: false},
        {id: 2, text:"Go to the toilet", hover: false, check: false},
        {id: 3, text:"Preach the gospel", hover: false, check: false},
        {id: 4, text:"Listen to Apostle Joshua Selman's message", hover: false, check: false},
        {id: 5, text:"Call El Shakar", hover: false, check: false}
      ],
      textArray: [], /* an array to check if the new task to be inputted into the todo list already exists */
      unchecked: 0,
      showAll: true,
      showActive: false,
      showComp: false,
      darkmode: true
    }
  },

  watch: {
    task() {
      if (typeof(Storage) !== 'undefined') {
        localStorage.setItem('storedTask', this.task)
      }
    },

    // tasks() {
    //   if (typeof(Storage) !== 'undefined') {
    //     localStorage.setItem('storedTasks', JSON.stringify(this.tasks))
    //   }
    //   deep: true
    // }
  },
  mounted() {
    for (const value of this.tasks) {
      this.textArray.push(value.text)
    }
    this.countTask() 

    this.task = localStorage.getItem('storedTask')
    // this.tasks = JSON.parse(localStorage.getItem('storedTasks')) || []
  },

  methods: {
    countTask() {
      this.tasks.forEach(task => {
        if (task.check == false) {
          this.unchecked += 1
        }
      })
    },
    checker() {
      let j = 0
      for (const i of this.tasks) {
        if (i.check != true) {
          j++
        }
      }
      this.unchecked = j
    },
    changeTheme() {
      this.darkmode = !this.darkmode
      
      if (this.darkmode == false) {
        document.querySelector('body').style.background = "rgb(240, 240, 240)"
      } else {
        document.querySelector('body').style.background = "hsl(235, 21%, 11%)"
      }
    },
    addTask() {
      if (!this.textArray.includes(this.task)  && this.task != '') {
        this.tasks.push({id: this.tasks.length + 1, text: this.task, hover: false, check: false })
        this.textArray.push(this.task)
      }

      this.task = ''

      this.checker()
    },
    deleteTask(task) {
      this.tasks = this.tasks.filter((tsk) => {
        return task !== tsk
      })
      this.textArray = this.textArray.filter(tsk => tsk != task.text)

      this.unchecked -= 1
      
      this.checker()
    },
    showCancel(task) {
      for (const i of this.tasks) {
        if (i == task) {
          i.hover = !i.hover
        }else {
          i.hover = false
        }
      }
    },
    removeCancel() {
      for (const i of this.tasks) {
        i.hover = false
      }
    },
    addTick(task) {
      let j = 0
      for (const i of this.tasks) {
        if (i == task) {
          i.check = !i.check
        }
        if (i.check != true) {
          j++
        }
      }
      this.unchecked = j
    },
    returnAll() {
      this.showActive = false
      this.showComp = false
      this.showAll = true
    },
    returnActive() {
      this.showAll = false
      this.showComp = false
      this.showActive = true
    },
    returnComp() {
      this.showAll = false
      this.showActive = false
      this.showComp = true
    },
    clear() {
      this.tasks = this.tasks.filter(task => {
        return task.check != true
      })
      let j = []
      this.tasks.forEach(element => {
        if (element.check != true) {
          j.push(element.text)
        }
      })
      console.log(j)

      this.textArray = j
    },
    border() {
      // console.log(this)
      this.$refs.li.classList = 'border'
      // console.log(this.$refs.li)
      // console.log(typeof(Storage))
    },
    draggable() {
      // this.$refs.li.classList = 'sortable-chosen sortable-ghost'
      this.$refs.li.setAttribute('draggable', 'true')
      console.log(this.$refs.li)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@400;700&display=swap');

* {
  margin: 0;
  padding: 0;
  list-style-type: none;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-family: 'Josefin Sans', sans-serif;
}
body {
  background: hsl(235, 21%, 11%);
  color: #fff;
  transition: ease-out 0.7s background;
}
.hero {
  width: 100%;
  user-select: none;
}
.mobile {
  display: none;
}
#center {
  position: absolute;
  top: 11vh;
  width: 40%;
  padding: 5% auto;
  margin: auto;
  display: grid;
  justify-self: center;
  transform: translateX(30vw);
}
header {
  display: flex;
  justify-content: space-between;
}
#header-title {
  text-transform: uppercase;
  letter-spacing: 1rem;
  font-size: 2.2rem
}
.theme-logo {
  align-self: center;
  cursor: pointer;
}
.input {
  position: relative;
}
.first-one {
  display: inline-block;
  position: relative;
  top: 4.75rem;
  left: 1.4rem;
}
input, #mobile-actions {
  width: 100%;
  box-sizing: border-box;
  margin: 2rem 0;
  border: none;
  outline: none;
  padding: 1rem 4rem;
  color: hsl(236, 33%, 92%);
}
input, #tasks, #mobile-actions {
  background: hsl(235, 24%, 19%);
  border-radius: 0.3rem;
  font-size: 18px;
  /* box-shadow: 5px 10px 8px rgba(27, 26, 26, 0.2); */
}
#tasks, #mobile-actions {
  box-shadow: -5px 10px 20px 10px rgba(15, 15, 15, 0.4);
}
input.light, #tasks.light, #mobile-actions.light {
  background: hsl(0, 0%, 98%);
}
#tasks.light, #mobile-actions.light {
  box-shadow: -5px 10px 20px 10px rgb(230, 230, 230), -5px 10px 20px 10px rgb(230, 230, 230);
}
ul {
  min-height: 2rem;
}
input, li {
  font-size: 1.2rem;
}
input.light, li.light {
  color:hsl(235, 19%, 35%);
}
li {
  border-bottom: hsl(237, 14%, 26%) solid 1px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.4rem;
  color: hsl(234, 39%, 85%);
  cursor: move;
}
li span {
  display: flex;
  place-items: center;
}
li span p {
  margin: 0 1rem;
}
.sortable-drag {
  opacity: 0;
}
.flip-move {
  transition: transform 0.5s;
}
.border {
  border-left: rgb(59, 59, 185) solid 2px;
}
.g-class {
  box-shadow: 10px 10px 5px -1px rgba(0, 0, 0, 0.14);
  opacity: 0.7;
}
.check, .first-one {
  padding: calc(1.5rem/2);
  border: rgba(255, 255, 255, 0.2) solid 1px;
  border-radius: 50%;
  box-sizing: content-box;
  cursor: pointer;
  outline: none !important;
}
.check:hover, .check.light:hover {
  /* border-image: linear-gradient(150deg, #57ddff, #c058f3); */

  border-color: rgb(59, 59, 185);

}
.check.light, .first-one.light, li.light {
  border-color: hsl(233, 11%, 84%);
}
.colored {
  background: linear-gradient(150deg, #57ddff, #c058f3);
  display: flex;
  place-items: center;
  justify-content: center;
  padding: 0.5rem;
  transition: ease 0.4s;
}
.img-check {
  width: 0.6rem;
  height: 0.6rem;
}
.checked {
  text-decoration: line-through;
  color: hsl(233, 14%, 35%);
}
.checked.light {
  color: hsl(233, 11%, 84%);
}
.cancel {
  cursor: pointer;
}
.task {
  cursor: pointer;
}
.task:hover {
  color: hsl(236, 33%, 92%);
}
.task.light:hover {
  color: hsl(237, 14%, 26%);
}
#info {
  display: flex;
  justify-content: space-between;
  padding: 1rem 1.4rem;
  text-align: center;
  box-sizing: border-box;
}
#info span {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: hsl(234, 11%, 52%);
}
#actions p {
  margin: 0 0.6rem;
  cursor: pointer;
  font-weight: 700;
}
#actions p:hover, #clear:hover {
  color: hsl(234, 39%, 85%);
}
#actions p.light:hover, #clear.light:hover, #mobile-actions p.light:hover {
  color: hsl(235, 19%, 35%);
}
.active, #actions .active:hover, #actions .active.light:hover, #mobile-actions span p.active, #mobile-actions span p.active:hover, #mobile-actions span p.light.active {
  color: hsl(220, 98%, 61%);
}
#clear {
  cursor: pointer;
}
#mobile-actions {
  padding: 0;
  padding: 1rem 20%;
  font-size: 1rem;
  /* margin: 0; */
}
#mobile-actions span {
  display: flex;
  justify-content: space-around;
}
#mobile-actions span p.light {
  color: hsl(234, 11%, 52%);
}
.bgdark {
  color: hsl(233, 14%, 35%);
}
.bgtext {
  color: hsl(236, 9%, 61%);
}
small {
  text-align: center;
  margin: 3rem 0;
}
.attribution { 
  font-size: 11px; 
  color: hsl(234, 39%, 85%);
  text-align: center; 
  margin-bottom: 1rem;
}
.attribution.light {
  color:hsl(235, 19%, 35%);
}
.attribution a { 
  color: hsl(228, 45%, 44%); 
  text-decoration: none;
}
.checked:hover {
  color: hsl(233, 14%, 35%);
}
.checked.light:hover {
  color: hsl(233, 11%, 84%);
}

@media (max-width:865px) {
  #center {
    width: 55%;
    transform: translateX( calc(45vw/2) );
  }
}
@media (max-width:630px) {
  #center {
    width: 70%;
    transform: none;
    position: relative;
    top: -11vh;
  }
}
@media (max-width:500px) {
  #center {
    width: 80%;
    transform: none;
    position: relative;
    top: -23vh;
    box-sizing:content-box;
  }
  .first-one {
    top: 4.55rem;
  }
  input, li {
    font-size: 0.9rem;
  }
  .mobile {
    display: block;
  }
  .desktop, #info span#actions {
    display: none;
  }
  #info span{
    font-size: 0.8rem;
  }
  
}
@media (max-width:400px) {
  #center {
    width: 80vw;
    position: relative;
    top: -18vh;
    box-sizing: border-box;
  }
  .mobile {
    display: block;
  }
  .desktop {
    display: none;
  }
}
</style>


// border-color
