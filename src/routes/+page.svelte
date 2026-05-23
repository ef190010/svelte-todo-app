<script lang="ts">
    interface Todo {
        id: string;
        text: string;
        done: boolean;
    }

    const todos: Todo[] = $state([
        {id: crypto.randomUUID(), text: "ミーティング資料を作る", done: false},
        {id: crypto.randomUUID(), text: "プルリクエストをレビューする", done: true},
        {id: crypto.randomUUID(), text: "本番リリースする", done: false},
    ]);

    let newTodoText: string = $state("");
    function addTodo() {
        if (newTodoText.trim() === "") {
            return;
        }

        const newTodo: Todo = {
            id: crypto.randomUUID(),
            text: newTodoText,
            done: false
        };
        todos.push(newTodo);
        newTodoText = "";
    }

    function deleteTodo(index: number) {
        todos.splice(index, 1);
    }

</script>

<main>
    <h1>Todo App</h1>
    <input
      type="text" 
      placeholder="新しいtodo..." 
      bind:value={newTodoText}
      onkeydown={(e) => e.key === "Enter" && addTodo()}
    />
    <button onclick={addTodo} disabled={newTodoText.trim() === ""}>追加</button>

    <ul>
        {#each todos as todo, i (todo.id)}
            <li>
                <input type="checkbox" bind:checked={todo.done}/>
                <span class={todo.done ? "done" : ""}>{todo.text}</span>
                <button onclick={() => deleteTodo(i)}>削除</button>
            </li>
        {/each}
    </ul>
</main>

<style>
    .done {
        text-decoration: line-through;
    }

    button[disabled] {
        opacity: 0.5;
        cursor: not-allowed;
    }
</style>