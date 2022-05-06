<template>
  <div class="Todo">
    <h2>Todo App</h2>

    <p>Todo名</p>
    <input type="text" class="inputTodo" v-model="todo.name">
    <!--入力した内容をtodo.nameに挿入-->

    <p>説明</p>
    <textarea class="inputDes" v-model="todo.description"></textarea>
    <!--入力した内容をtodo.descriptionに挿入-->

    <p>時間</p>
    <input type="text" class="inputDead" v-model="todo.deadline">
    <!--入力した内容をtodo.deadlineに挿入-->

    <div class="flexBtn">
      <button class="createBtn" type="button" @click="create">Create</button>
      <button class="deleteAllBtn" type="button" @click="deleteAll">deleteAll</button>
    </div>

    <button class="readBtn" type="button" @click="read">Read</button>

    <ul v-for="val in todos" :key="val.name">
      <!--create関数によってtodoの中身がtodosに挿入され、todosの中身をvalに出力-->
      <li>Todo名: {{ val.name }}</li>
      <li class="valDes">説明: {{ val.description }}</li>
      <li>時間: {{ val.deadline }}</li>
      <li><button type="button" @click="deleteTodo(val)">delete</button></li>
    </ul>

  </div>
</template>

<script>
  export default {
    name: 'TodoApp',
    data() {
      return {
        todos: [],
        //ここに入力された情報が配列の中にオブジェクトとして保存される

        todo: { //todoの中身を初期化
          name: '',
          description: '',
          deadline: ''
        },
        name: 'TODOS'
        //セッションストレージの名前
      }
    },
    methods: {
      create() {
        this.todos.push(this.todo)
        //todosの中に入力されたtodoの中身を挿入

        sessionStorage.setItem(this.name, JSON.stringify(this.todos))
        //入力された情報をセッションストレージに保存

        this.todo = { //上でpushできたらtodoの中身を初期化
          name: '',
          description: '',
          deadline: ''
        }
      },
      read() {
        const data = sessionStorage.getItem(this.name)
        //セッションストレージに保存されている情報を変数dataにセット

        if (data) {
          this.todos = JSON.parse(data)
          //JSONのあれこれ
        }
      },
      deleteTodo(val) { //
        this.todos = this.todos.filter((item) => item.name !== val.name)
        //valに入ってるモノとtodosに入っている全てのモノを1つずつ比べ、一致していたら除外する→結果ボタンを押したところだけ削除される

        sessionStorage.setItem(this.name, JSON.stringify(this.todos))
        //除外されたもの以外が入っているtodosをセッションストレージへ保存
      },
      deleteAll() {
        sessionStorage.clear()
        //セッションストレージの中身削除

        this.todos = []
        //todosの中身削除
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  #Todo {
    width: 100%;
  }

  h2 {
    margin: 50px 0 25px;
    text-align: center;
    font-size: 24px;
  }

  p {
    margin-bottom: 5px;
    text-align: center;
    font-size: 18px;
  }

  input {
    display: block;
    width: 80%;
    height: 35px;
    max-width: 600px;
    margin: 0 auto 25px;
    padding: 12px 8px;
  }

  textarea {
    display: block;
    width: 80%;
    height: 100px;
    max-width: 600px;
    margin: 0 auto 25px;
    padding: 4px 6px;
    resize: vertical;
  }

  .inputDead {
    margin-bottom: 50px;
  }

  button {
    display: block;
    background: none;
    border: none;
    outline: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
  }

  .flexBtn {
    display: flex;
    justify-content: center;
    gap: 100px;
    margin-bottom: 50px;

    .createBtn {
      @media not all and (min-resolution:.001dpcm) {
        @supports (not (translate: none)) {
          margin-right: 50px;
        }
      }
    }

    .deleteAllBtn {
      @media not all and (min-resolution:.001dpcm) {
        @supports (not (translate: none)) {
          margin-left: 50px;
        }
      }
    }
  }

  .readBtn {
    margin: 0 auto 50px;
  }

  .valDes {
    white-space: pre-line;
  }
</style>