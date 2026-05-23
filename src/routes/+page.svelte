<script lang="ts">
    interface Todo {
        id: number;
        text: string;
        done: boolean;
    }

    const todos: Todo[] = $state([
        {id: 1, text: "ミーティング資料を作る", done: false},
        {id: 2, text: "プルリクエストをレビューする", done: true},
        {id: 3, text: "本番リリースする", done: false},
    ]);

</script>

<main>
    <h1>Todo App</h1>
    <input type="text" placeholder="新しいtodo..."/>
    <button>追加</button>

    <ul>
        {#each todos as todo (todo.id)}
            <li>
                <input type="checkbox" bind:checked={todo.done}/>
                <span class={todo.done ? "done" : ""}>{todo.text}</span>
                <button>削除</button>
            </li>
        {/each}
    </ul>
</main>

<style>
    .done {
        text-decoration: line-through;
    }
</style>