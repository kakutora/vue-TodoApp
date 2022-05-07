<template>
  <div class="Todo">
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
      <button class="createBtn" type="button" @click="create"><span>Create</span></button>
      <button class="deleteAllBtn" type="button" @click="deleteAll"><span>deleteAll</span></button>
    </div>

    <button class="readBtn" type="button" @click="read"><span>Read</span></button>

    <ul class="TodoList" v-for="val in todos" :key="val.name">
      <!--create関数によってtodoの中身がtodosに挿入され、todosの中身をvalに出力-->
      <li>Todo名<br>{{ val.name }}</li>
      <li class="valDes">説明<br>{{ val.description }}</li>
      <li>時間<br>{{ val.deadline }}</li>
      <li><button class="deleteBtn" type="button" @click="deleteTodo(val)">delete</button></li>
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
    position: relative;
    display: block;
    background: none;
    font-size: 20px;
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
  }

  .createBtn,
  .deleteAllBtn,
  .readBtn,
  .deleteBtn {
    position: relative;

    &::before,
    &::after {
      position: absolute;
      width: 150%;
      height: 1px;
      background: #000000e6;
      top: 100%;
      left: -25%;
    }

    &::before {
      content: '';
    }

    &::before {
      height: 2px;
      border-radius: 3px;
      transform: scale3d(1, 1, 1);
      transition: transform 0.2s, opacity 0.2s;
      transition-timing-function: cubic-bezier(0.2, 0.57, 0.67, 1.53);
    }

    &:hover::before {
      transition-timing-function: cubic-bezier(0.8, 0, 0.1, 1);
      transition-duration: 0.4s;
      opacity: 1;
      transform: scale3d(1.2, 0.1, 1);
    }

    span {
      transform: translate3d(0, -4px, 0);
      display: inline-block;
      transition: transform 0.2s 0.05s cubic-bezier(0.2, 0.57, 0.67, 1.53);
    }

    &:hover span {
      transform: translate3d(0, 0, 0);
      transition-timing-function: cubic-bezier(0.8, 0, 0.1, 1);
      transition-duration: 0.4s;
      transition-delay: 0s;
    }
  }

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

  .readBtn {
    margin: 0 auto 100px;
  }

  .valDes {
    white-space: pre-line;
  }

  .TodoList {
    width: 80%;
    max-width: 600px;
    margin: 0 auto 50px;
    padding: 50px 10px 20px;
    border: 2px solid #000000;
    border-radius: 15px;

    li {
      margin-bottom: 45px;
      text-align: center;
      font-size: 16px;
      word-wrap: break-word;

      .deleteBtn {
        margin: 100px auto 0;
        font-size: 20px;
      }
    }
  }
</style>