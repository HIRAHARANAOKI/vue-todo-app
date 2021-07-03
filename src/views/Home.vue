<template>
<el-container>
  <el-main>
	<h1>Vue3 Todo App</h1>
	
	<el-form @submit.prevent="addTodo">
		<el-input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
      placeholder="todoを入力..."
		>
    </el-input>
		<el-button @click="addTodo" type="danger">追加</el-button>
	</el-form>

	<h2>ToDo List</h2>
	<ul>
		<li
			v-for="(todo, index) in todos"
			:key="index"
		>
			<span
				:class="{ done: todo.done }"
				@click="doneTodo(todo)"
			>{{ todo.content }}</span>

		<!-- 編集用のフォーム -->
		<form @submit.prevent="updateTodo(todo, index)" v-show="todo.show">
			<el-input v-model="newUpdateTodo"></el-input>
			<el-button @click="updateTodo(todo, index)">変更する</el-button>
		</form>
		<!-- 普段は非表示 -->

      <div>
				<el-button @click="toggleTodo(todo)" type="primary">完了</el-button>
				<el-button @click="showForm(todo)" type="warning">編集</el-button>
				<el-button @click="removeTodo(index)" type="info">削除</el-button>
      </div>
		</li>
	</ul>


  </el-main>
</el-container>
</template>

<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
		setup () {
			const newTodo = ref('');
			const newUpdateTodo = ref('');
			const defaultData = [{
				done: false,
				show: false,
				content: '',
			}]
			const show = ref(false)
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			const todos = ref(todosData);

			// Create
			const addTodo = () => {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}

			// Update
			const updateTodo = (todo, index) => {
				console.log(todos.value, 'todos.value')
				console.log(index, 'index')

				if (newUpdateTodo.value) {
					todos.value.splice(index, 1, {
						content: newUpdateTodo.value
					})
					newUpdateTodo.value = '';
					saveData()
				}
				newUpdateTodo.value = ''
			}

			// Delete
			const removeTodo = (index) => {
				todos.value.splice(index, 1);
				saveData();
			}

			// liタグのtex-decorationの切り替え
			const toggleTodo = (todo) => {
				todo.done = !todo.done
				saveData();
			}

			// 編集フォームの表示切り替え
			const showForm = (todo) => {
				todo.show = !todo.show
				saveData();
			}

			// storageDataに保存
			const saveData = () => {
				const storageData = JSON.stringify(todos.value);
				localStorage.setItem('todos', storageData);
			}

			return {
				todos,
				show,
				newTodo,
				newUpdateTodo,
				addTodo,
				showForm,
				toggleTodo,
				updateTodo,
				removeTodo,
				saveData
			}
		}
	}
</script>

<style lang="scss">

form {
  display: flex;
	align-items: center;
}

li {
  list-style: none;
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;

	// 完了ボタンをクリック時のクラス
  .done {
   text-decoration: line-through;
	}

}

</style>
