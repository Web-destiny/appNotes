<template>
  <div>
    <div class="container pt-5 pb-5">
      <div class="card counter-card">
        <h1>{{ title }}:</h1>
        <div class="form-control">
          <input class="add-note-input"
                 type="text"
                 :placeholder="placeholder"
                 v-model="inputValue"
                 @keypress.enter="addNewNote"
          >
        </div>
<!--        <h2>InputValue: {{ inputValue }}</h2>-->
        <div style="display: flex;column-gap: 10px;">
          <button
              class="btn add-note-button"
              @click="addNewNote"
          >Добавить</button>
          <button
              class="btn add-note-button"
              @click="deleteLastNote"
          >Удалить последнюю</button>
          <button
              class="btn add-note-button"
              @click="deleteChosenNote"
              v-if="chosenNotes.length"
          >Удалить выбранную</button>
        </div>

        <hr>
        <ul
            class="list"
            v-if="notes.length !== 0">
          <li class="list-item"
              v-for="(myNote, index) in notes"
              :key="index"
              :noteIndex="index"
              @click="chooseNote"
              v-bind:selected="myNote.selected"
          >
            {{ myNote.text }}
            <div
                @click.stop="deleteNote(index)"
                class="delete-icon"
            >
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" width="16" height="16"><path d="M2.343 13.657A8 8 0 1 1 13.658 2.343 8 8 0 0 1 2.343 13.657ZM6.03 4.97a.751.751 0 0 0-1.042.018.751.751 0 0 0-.018 1.042L6.94 8 4.97 9.97a.749.749 0 0 0 .326 1.275.749.749 0 0 0 .734-.215L8 9.06l1.97 1.97a.749.749 0 0 0 1.275-.326.749.749 0 0 0-.215-.734L9.06 8l1.97-1.97a.749.749 0 0 0-.326-1.275.749.749 0 0 0-.734.215L8 6.94Z"></path></svg>
            </div>
          </li>
          <hr>
          <li>
            <strong>Общее количетво: {{ notes.length }}</strong> Удвоенное: {{ doubleCount }}
          </li>
        </ul>
        <div
            v-else
        >
          <h2>Заметок нет</h2>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      placeholder: 'Введите название новой заметки',
      title: 'Список заметок',
      inputValue: '',
      notes: [
        {text: 'Заметка 1', selected: false},
        {text: 'Заметка 2', selected: false}
      ]
    }
  },
  methods: {
    addNewNote(){
      if(this.inputValue !== ''){
        this.notes.push({text: this.inputValue, selected: false})
        this.inputValue = ''
      }
    },
    deleteNote(index){
      this.notes.splice(index, 1)
    },
    deleteLastNote(){
      this.notes.pop()
    },
    deleteChosenNote(){
      this.notes = this.notes.filter(note => !note.selected)
    },
    chooseNote(event){
      let note = event.target
      let index = note.getAttribute('noteIndex')
      this.notes[index].selected = !this.notes[index].selected
    },
  },
  computed: {
    doubleCount(){
      return this.notes.length * 2
    },
    chosenNotes(){
      return this.notes.filter(note => note.selected).map(note => note.text)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
li[selected='true'] {
  background-color: yellow;
  border: 1px solid black;
}
a {
  color: #42b983;
}
.counter-card{
  padding: 20px;
  background-color: #2c3e50;
  color: white;
  border-radius: 10px;
}
.add-note-input{
  width: 100%;
  padding: 10px 20px;
}
.add-note-button{
  background-color: white;
  margin-top: 10px;
  border-radius: 16px;
}
.list{
  display: flex;
  row-gap: 10px;
  border-radius: 10px;
  flex-direction: column;
}
.list-item{
  background-color: white;
  display: flex;
  color: #2c3e50;
  padding: 10px 12px;
  position: relative;
  cursor: pointer;
}

.list-item.chosen-list-item{
  background-color: orange;
}

.list-item.chosen-list-item .delete-icon svg{
  fill: #000000;
}

.list-item.chosen-list-item .delete-icon svg:hover{
  fill: #ffffff;
}

.delete-icon{
  position: absolute;
  right: 15px;
  cursor: pointer;
}

.delete-icon svg{
  fill: #41b883;
}
.delete-icon svg:hover{
  fill: #f51111;
}

</style>
