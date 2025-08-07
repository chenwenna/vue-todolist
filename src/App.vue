<template>
  <div class="app">
    <div class="container">
      <h1 class="title">📝 我的待办事项</h1>
      
      <!-- 添加新任务 -->
      <div class="add-todo">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo"
          placeholder="输入新的待办事项..."
          class="todo-input"
        />
        <button @click="addTodo" class="add-btn">添加</button>
      </div>

      <!-- 待办事项列表 -->
      <div class="todo-list" ref="todoList">
        <div 
          v-for="todo in todos" 
          :key="todo.id"
          class="todo-item"
          :class="{ completed: todo.completed }"
        >
          <div class="drag-handle">⋮⋮</div>
          <input 
            type="checkbox" 
            v-model="todo.completed"
            @change="saveTodos"
            class="todo-checkbox"
          />
          <span class="todo-text">{{ todo.text }}</span>
          <button @click="deleteTodo(todo.id)" class="delete-btn">🗑️</button>
        </div>
      </div>

      <!-- 统计信息 -->
      <div class="stats" v-if="todos.length > 0">
        <span>总计: {{ todos.length }}</span>
        <span>已完成: {{ completedCount }}</span>
        <span>待完成: {{ remainingCount }}</span>
      </div>

      <!-- 空状态 -->
      <div v-if="todos.length === 0" class="empty-state">
        <p>🎉 暂无待办事项，添加一个开始吧！</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, nextTick } from 'vue'
import Sortable from 'sortablejs'

export default {
  name: 'App',
  setup() {
    const newTodo = ref('')
    const todos = ref([])
    const todoList = ref(null)

    // 计算属性
    const completedCount = computed(() => 
      todos.value.filter(todo => todo.completed).length
    )
    
    const remainingCount = computed(() => 
      todos.value.filter(todo => !todo.completed).length
    )

    // 从localStorage加载数据
    const loadTodos = () => {
      const saved = localStorage.getItem('vue-todos')
      if (saved) {
        todos.value = JSON.parse(saved)
      }
    }

    // 保存到localStorage
    const saveTodos = () => {
      localStorage.setItem('vue-todos', JSON.stringify(todos.value))
    }

    // 添加新待办事项
    const addTodo = () => {
      if (newTodo.value.trim()) {
        const todo = {
          id: Date.now(),
          text: newTodo.value.trim(),
          completed: false,
          createdAt: new Date().toISOString()
        }
        todos.value.push(todo)
        newTodo.value = ''
        saveTodos()
      }
    }

    // 删除待办事项
    const deleteTodo = (id) => {
      todos.value = todos.value.filter(todo => todo.id !== id)
      saveTodos()
    }

    // 初始化拖拽功能
    const initSortable = () => {
      if (todoList.value) {
        new Sortable(todoList.value, {
          handle: '.drag-handle',
          animation: 150,
          ghostClass: 'sortable-ghost',
          chosenClass: 'sortable-chosen',
          dragClass: 'sortable-drag',
          onEnd: (evt) => {
            const { oldIndex, newIndex } = evt
            if (oldIndex !== newIndex) {
              const movedItem = todos.value.splice(oldIndex, 1)[0]
              todos.value.splice(newIndex, 0, movedItem)
              saveTodos()
            }
          }
        })
      }
    }

    // 组件挂载后初始化
    onMounted(() => {
      loadTodos()
      nextTick(() => {
        initSortable()
      })
    })

    return {
      newTodo,
      todos,
      todoList,
      completedCount,
      remainingCount,
      addTodo,
      deleteTodo,
      saveTodos
    }
  }
}
</script>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, 
    #ff6b6b 0%, 
    #ff8e53 12%, 
    #ff6b9d 25%, 
    #c44569 35%, 
    #4ecdc4 45%, 
    #45b7d1 55%, 
    #96ceb4 65%, 
    #6c5ce7 75%, 
    #a29bfe 85%, 
    #feca57 95%, 
    #ff9ff3 100%);
  background-size: 600% 600%;
  animation: gradientShift 12s ease infinite;
}

.container {
  background: linear-gradient(145deg, rgba(255, 255, 255, 0.95), rgba(255, 255, 255, 0.85));
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 25px;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(255, 255, 255, 0.1);
  padding: 40px;
  width: 100%;
  max-width: 600px;
  margin: 20px;
}

.title {
  text-align: center;
  background: linear-gradient(135deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4);
  background-size: 400% 400%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: gradientShift 6s ease infinite;
  margin-bottom: 30px;
  font-size: 2.5rem;
  font-weight: 700;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  25% { background-position: 100% 0%; }
  50% { background-position: 100% 100%; }
  75% { background-position: 0% 100%; }
  100% { background-position: 0% 50%; }
}

.add-todo {
  display: flex;
  gap: 12px;
  margin-bottom: 30px;
}

.todo-input {
  flex: 1;
  padding: 15px 20px;
  border: 2px solid #e1e5e9;
  border-radius: 12px;
  font-size: 16px;
  outline: none;
  transition: all 0.3s ease;
}

.todo-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.add-btn {
  padding: 15px 25px;
  background: linear-gradient(135deg, #ff6b6b 0%, #4ecdc4 50%, #45b7d1 100%);
  background-size: 200% 200%;
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  animation: buttonGradient 3s ease infinite;
}

@keyframes buttonGradient {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.add-btn:hover {
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 10px 30px rgba(255, 107, 107, 0.4), 0 0 20px rgba(78, 205, 196, 0.3);
  background: linear-gradient(135deg, #ff8a80 0%, #80cbc4 50%, #64b5f6 100%);
}

.todo-list {
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 18px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(248, 249, 250, 0.9));
  border-radius: 15px;
  margin-bottom: 12px;
  transition: all 0.3s ease;
  border: 2px solid transparent;
  backdrop-filter: blur(10px);
  position: relative;
  overflow: hidden;
}

.todo-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
  transition: left 0.5s;
}

.todo-item:hover::before {
  left: 100%;
}

.todo-item:hover {
  background: linear-gradient(135deg, rgba(255, 107, 107, 0.1), rgba(78, 205, 196, 0.1), rgba(69, 183, 209, 0.1));
  transform: translateY(-2px) scale(1.02);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15), 0 0 15px rgba(255, 107, 107, 0.2);
  border: 2px solid rgba(255, 107, 107, 0.3);
}

.todo-item.completed {
  opacity: 0.8;
  background: linear-gradient(135deg, rgba(150, 206, 180, 0.3), rgba(129, 199, 132, 0.4));
  border: 2px solid rgba(76, 175, 80, 0.3);
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #6c757d;
}

.drag-handle {
  cursor: grab;
  color: #6c757d;
  font-size: 18px;
  padding: 5px;
  user-select: none;
}

.drag-handle:active {
  cursor: grabbing;
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #667eea;
}

.todo-text {
  flex: 1;
  font-size: 16px;
  color: #333;
  word-break: break-word;
}

.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  padding: 8px;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.delete-btn:hover {
  background: #fee;
  transform: scale(1.1);
}

.stats {
  display: flex;
  justify-content: space-around;
  padding: 20px;
  background: linear-gradient(135deg, rgba(255, 107, 107, 0.1), rgba(78, 205, 196, 0.1), rgba(150, 206, 180, 0.1));
  border-radius: 15px;
  margin-top: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.stats span {
  font-weight: 600;
  background: linear-gradient(135deg, #ff6b6b, #4ecdc4, #45b7d1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-size: 16px;
}

.empty-state {
  text-align: center;
  padding: 60px 20px;
  color: #6c757d;
  font-size: 18px;
}

/* 拖拽样式 */
.sortable-ghost {
  opacity: 0.5;
  background: #667eea !important;
  color: white;
}

.sortable-chosen {
  transform: scale(1.02);
}

.sortable-drag {
  transform: rotate(5deg);
}

/* 响应式设计 */
@media (max-width: 768px) {
  .container {
    margin: 10px;
    padding: 20px;
  }
  
  .title {
    font-size: 2rem;
  }
  
  .add-todo {
    flex-direction: column;
  }
  
  .stats {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }
}
</style>