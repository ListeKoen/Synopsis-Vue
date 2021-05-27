<template>
  <div>
    <h1>Vue Todo List</h1>
    <input type="text" placeholder="Add a task..." v-model="word">
    <button class="addButton" @click="addWord">Add</button>

    <b-list-group class="listGroup">
      <b-list-group-item
        class="listGroupItem"
        v-for="(item, index) in filteredArray"
        v-bind:key="index"
        :class="{ active: item.done === true }"
      >
        <span>{{ index + 1 }} -</span>
        {{ item.text }}
        <button class="removeButton" @click="removeItem(index);">X</button>
        <font-awesome-icon class="checkIcon" icon="check" @click="changeStatus(item);"/>
      </b-list-group-item>
    </b-list-group>

    <ul class="filter">
      <li class="pointy" @click="filterArray('all')">All</li>
      <li class="pointy" @click="filterArray('active')">Active</li>
      <li class="pointy" @click="filterArray('complete')">Completed</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  created: function(){
      this.filteredArray = [...this.todoArray];
  },
  data() {
    return {
      count: 0,
      word: "",
      todoArray: [
        { text: "Learn JavaScript", done: false },
        { text: "Learn Vue", done: false },
        { text: "Play around in JSFiddle", done: false },
        { text: "Build something awesome", done: false }
      ],
      filteredArray: []
    };
  },
  methods: {
    addWord: function() {
      if (this.word !== "") {
        this.todoArray.push({ text: this.word, done: false });
        this.word = "";
        this.filterArray();
      }
    },
    removeItem: function(idx) {
      let prevWordArray = [...this.todoArray];
      prevWordArray.splice(idx, 1);
      this.todoArray = prevWordArray;
      this.filterArray();
    },
    changeStatus: function(itm) {
      let clickedElem = this.todoArray.find(item => {
        return item.text === itm.text;
      });
      clickedElem.done = clickedElem.done === false;
      this.filterArray();
    },
    filterArray: function(type) {
        if (type === "active") {
            this.filteredArray = this.todoArray.filter(item => {
                return item.done === false;
            })
        } else if (type === 'complete'){
            this.filteredArray = this.todoArray.filter(item => {
                return item.done === true;
            })
        } else {
            this.filteredArray = this.todoArray;
        }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.listGroup {
  width: 30%;
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
}

span {
  color: purple;
  font-weight: bold;
}

input[type="text"] {
  margin-top: 20px;
  padding: 8px 8px 8px 14px;
  border-radius: 5px;
  border: 3px solid lightblue;
}

.listGroupItem {
  margin-top: 10px;
  text-align: left;
}

.addButton {
  margin-left: 10px;
  padding: 5px 10px;
  background-color: lightgreen;
  border: 1px solid lightgreen;
  color: white;
  border-radius: 3px;
}

.removeButton {
  color: white;
  font-weight: bold;
  background-color: red;
  border: 1px solid red;
  font-size: 14px;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  float: right;
  transition: background-color 0.5s, color 0.5s;
}

.checkIcon {
  float: right;
  font-size: 28px;
  margin-right: 10px;
  color: green;
}

.checkIcon:hover {
  cursor: pointer;
}

.removeButton:hover {
  cursor: pointer;
  background-color: white;
  color: red;
}

.active {
  text-decoration-line: line-through;
  text-decoration-color: black;
  background-color: gainsboro;
  border-color: gainsboro;
  color: gray;
}

.filter {
  margin-top: 15px;
}

.filter li {
  display: inline;
  margin-right: 15px;
}

.pointy {
  cursor: pointer;
}
</style>
