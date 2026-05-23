<script lang="ts">
	import { onMount } from 'svelte';
	interface Todo {
		id: string;
		text: string;
		done: boolean;
	}

	const todos: Todo[] = $state([]);
	// Sample: todos
	// [
	//     {id: crypto.randomUUID(), text: "ミーティング資料を作る", done: false},
	//     {id: crypto.randomUUID(), text: "プルリクエストをレビューする", done: true},
	//     {id: crypto.randomUUID(), text: "本番リリースする", done: false},
	// ]

	let isInitialized = $state(false);

	$effect(() => {
		if (isInitialized && typeof window !== 'undefined') {
			saveTodos();
		}
	});

	onMount(() => {
		if (!isInitialized && typeof window !== 'undefined') {
			loadTodos();
			isInitialized = true;
		}
	});

	let newTodoText: string = $state('');
	function addTodo() {
		if (newTodoText.trim() === '') {
			return;
		}

		const newTodo: Todo = {
			id: crypto.randomUUID(),
			text: newTodoText,
			done: false
		};
		todos.push(newTodo);
		newTodoText = '';
	}

	function deleteTodo(index: number) {
		todos.splice(index, 1);
	}

	function saveTodos() {
		try {
			localStorage.setItem('todos', JSON.stringify(todos));
		} catch (e) {
			console.error('Failed to save todos to localStorage:', e);
		}
	}

	function loadTodos() {
		try {
			const saved = localStorage.getItem('todos');
			const parsed: Todo[] = saved ? JSON.parse(saved) : [];
			if (parsed) {
				todos.push(...parsed);
			}
		} catch (e) {
			console.error('Failed to load todos from localStorage:', e);
			return;
		}
	}
</script>

<main class="min-h-screen bg-gray-100 flex flex-col items-center py-10">
	<h1 class="text-4xl font-bold to-gray-800 mb-8">Todo App</h1>
    <!-- 入力 -->
    <div class="flex items-center gap-4 mb-6">
        <input
            type="text"
            placeholder="新しいtodo..."
            bind:value={newTodoText}
            onkeydown={(e) => e.key === 'Enter' && addTodo()}
            class="w-64 p-2 border bg-white border-gray-300 rounded-lg
                focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button 
            onclick={addTodo} 
            disabled={newTodoText.trim() === ''}
            class="px-4 py-2 bg-blue-500 text-white font-semibold rounded-lg shadow-md cursor-pointer
                hover:bg-blue-600 disabled:bg-gray-300 disabled:cursor-not-allowed"
        >
            追加
        </button>
    </div>

    <!-- リスト -->
	<ul class="w-full max-w-md">
		{#each todos as todo, i (todo.id)}
			<li class="flex items-center justify-between bg-white p-4 rounded-lg shadow-sm mb-2">
				<div class="flex items-center gap-3">
                    <input 
                        type="checkbox" 
                        bind:checked={todo.done} 
                        class="h-5 w-5 cursor-pointer"
                    />
                    <span 
                        class={`text-lg ${todo.done ? 'line-through text-gray-400' : 'text-gray-700'}   `}
                    >
                        {todo.text}
                    </span>
                </div>
				<button 
                    onclick={() => deleteTodo(i)}
                    class="text-red-500 font-semibold cursor-pointer 
                        hover:underline hover:opacity-80"
                >
                    削除
                </button>
			</li>
		{/each}
	</ul>
</main>
