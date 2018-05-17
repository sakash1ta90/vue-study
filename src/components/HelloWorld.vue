<template>
    <div class="hello" id="app">
        <h1>
            {{ title }}
            <span class="info">({{ uncheckedNumber.length }}/{{ todoList.length }})</span>
        </h1>
        <ul v-if="todoList.length">
            <li v-for="(todo, index) in todoList">
                <input type="checkbox" v-bind:checked="todo.done" @click="todo.done = !todo.done">
                <label :class="{done: todo.done}">{{todo.task}}</label>
                <span @click="deleteItem(index)" class="command">[x]</span>
            </li>
        </ul>
        <ul v-else>
            <li>Task is nothing ...</li>
        </ul>

        <v-form ref="form" v-model="valid" @submit.prevent="clear">
            <v-text-field
                    v-model="newItem"
                    :rules="taskRules"
                    :counter="15"
                    label="Task"
                    required
            ></v-text-field>
            <v-btn color="success" @click="addItem" :disabled="!valid" type="submit">Add task</v-btn>
            <v-btn color="error" @click="deleteChecked">Delete checked task</v-btn>
        </v-form>
        <v-alert :value="alert" outline color="success" icon="check_circle" :dismissible="true">
            This is a success alert.
        </v-alert>
    </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        valid: false,
        alert: false,
        newItem: '',
        title: 'My ToDo',
        todoList: [],
        taskRules: [
          v => !!v || 'Task is required',
          v => (v && v.length <= 15) || 'Task must be less than 15 characters'
        ],
      }
    },
    mounted() {
      this.todoList = JSON.parse(localStorage.getItem('todoList')) || []
    },
    methods: {
      // タスクの追加
      addItem() {
        if (0 == this.newItem.length) {
          return
        }
        this.todoList.push({
          task: this.newItem,
          done: false
        })

        this.alert = true
        setTimeout(() => {
          this.alert = false
        }, 5000)
      },
      clear() {
        this.$refs.form.reset()
      },

      // ☓ボタン処理
      deleteItem(idx) {
        this.todoList.splice(idx, 1)
      },
      deleteChecked() {
        this.todoList = this.uncheckedNumber
      }
    },
    // 未完のタスクの件数を返却する
    computed: {
      uncheckedNumber() {
        return this.todoList.filter((todo) => {
          return !todo.done
        })
      }
    },
    // TODOの変更を監視して、保存する
    watch: {
      todoList: {
        handler() {
          localStorage.setItem('todoList', JSON.stringify(this.todoList))
        },
        deep: true
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    body {
        font-size: 16px;
        font-family: Verdana, sans-serif;
    }

    #app h1 {
        font-size: 16px;
        border-bottom: 1px solid #ddd;
        padding: 16px 0;
    }

    #app li {
        line-height: 1.5;
    }

    #app input[type="text"] {
        padding: 2px;
        border-style: solid !important;
    }

    #app form input, #app form button {
        padding: 0px 2px;
    }

    .command {
        font-size: 12px;
        cursor: pointer;
        color: #08c;
    }

    #app ul {
        padding: 0;
        list-style: none;
    }

    #app li > label.done {
        text-decoration: line-through !important;
        color: #bbb;
    }

    .info {
        color: #bbb;
        font-size: 12px;
        font-weight: normal;
        background: transparent !important;
    }
</style>
