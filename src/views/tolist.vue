 <template>
  <div id="app">
    <header class="header">
      <div class="header__content container">
        <div class="logo">
          TodoList
        </div>

        <input v-model="inputText"
               type="text"
               class="input"
               placeholder="添加 todo"
               @keydown.enter="handleTitleInput">
      </div>
    </header>

    <main class="main">
      <div class="container">
        <div class="todo-group">
          <div class="todo-group__header">
            <h1 class="title">正在进行</h1>

            <div class="count">{{ doingList.length }}</div>
          </div>
          <div class="todo-group__body">
            <ul class="todo-list">
              <todo-item v-for="item in doingList"
                         :key="item.id"
                         :id="item.id"
                         :title="item.title"
                         :done="item.done"
                         @remove="id => deleteItem(id)"
                         @status-change="id => toggleItem(id)"
                         @title-change="({id, title}) => setItemTitle(id, title)"></todo-item>

            </ul>
          </div>
        </div>
        <div class="todo-group">
          <div class="todo-group__header">
            <h1 class="title">已完成</h1>

            <div class="count">{{ doneList.length }}</div>
          </div>
          <div class="todo-group__body">
            <ul class="todo-list">
              <todo-item v-for="item in doneList"
                         :key="item.id"
                         :id="item.id"
                         :title="item.title"
                         :done="item.done"
                         @remove="id => deleteItem(id)"
                         @status-change="id => toggleItem(id)"
                         @title-change="title => setItemTitle(title)"></todo-item>
            </ul>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>
 
 <script>
import shortid from 'shortid';
import TodoItem from '@/components/TodoItem';

export default {
  components: {
    TodoItem
  },

  data() {
    return {
      // 待添加事项的 title
      // --------------和内层组件的inputText啥关系
      inputText: '',

      dataList: [
        // {
        //   id: '001',
        //   title: 'tzz',
        //   done: false
        // }
      ]
    };
  },
  computed: {
    doneList() {
      return this.dataList.filter(item => item.done);
    },

    doingList() {
      return this.dataList.filter(item => !item.done);
    }
  },
  methods: {
    // -------------啥意思
    handleTitleInput() {
      this.addItem(this.inputText);
      this.inputText = '';
    },
    // 添加代办项
    addItem(title) {
      this.dataList.push({
        id: shortid.generate(),
        title: title,
        done: false
      });
    },
    // 删除代办项
    deleteItem(id) {
      let index = this.dataList.findIndex(item => item.id === id);

      if (index === -1) {
        throw new Error({
          message: '找不到待删除的代办项'
        });
      }
      this.dataList.splice(index, 1);
    },
    // 切换状态
    toggleItem(id) {
      let index = this.dataList.findIndex(item => item.id === id);

      if (index === -1) {
        throw new Error({
          message: '找不到要请求的待办事项'
        });
      }

      let target = this.dataList[index];

      target.done = !target.done;
    },
    // 编辑代办项
    setItemTitle(id, title) {
      let index = this.dataList.findIndex(item => item.id === id);
      if (index === -1) {
        throw new Error({
          message: '找不到要处理的代办项'
        });
      }
      let target = this.dataList[index];
      target.title = title;
    }
  }
};
</script>
 
 <style lang='scss' scoped>
* {
  box-sizing: border-box;
}

.container {
  width: 620px;
}

#app {
  display: flex;
  flex-direction: column;

  width: 100vw;
  height: 100vh;

  .header {
    display: flex;
    justify-content: center;

    height: 50px;

    background-color: rgba(47, 47, 47, 0.98);

    &__content {
      display: flex;
      align-items: center;
      justify-content: space-between;

      height: 100%;

      .logo {
        font-size: 24px;

        color: #ddd;
      }

      .input {
        width: 360px;
        height: 24px;
        padding: 0 12px;
        outline: 0;
        border: 0;
        border-radius: 5px;

        font-size: 12px;
        box-shadow: 0 1px 0 rgba(255, 255, 255, 0.24),
          0 1px 6px rgba(0, 0, 0, 0.45) inset;
      }
    }
  }

  .main {
    flex-grow: 1;
    background-color: #cdcdcd;

    .container {
      margin: 0 auto;

      .todo-group {
        padding-top: 20px;

        &__header {
          display: flex;
          align-items: center;
          justify-content: space-between;

          .title {
            margin: 0;

            font-size: 24px;
            font-weight: 700;
          }
        }

        &__body {
          .todo-list {
            padding: 0;
            list-style: none;
          }
        }
      }
    }
  }
}
</style>