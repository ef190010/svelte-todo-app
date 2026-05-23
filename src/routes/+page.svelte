<main>
    <h1>Todo App</h1>
    <input type="text" placeholder="新しいtodo..."/>
    <button>追加</button>

    <ul>
        <li>
            <input type="checkbox" />
            <span>ミーティング資料を作る</span>
            <button>削除</button>
        </li>
        <li>
            <input type="checkbox" />
            <span class="done">プルリクエストをレビューする</span>
            <button>削除</button>
        </li>
        <li>
            <input type="checkbox" />
            <span>本番リリースする</span>
            <button>削除</button>
        </li>
    </ul>
</main>

<style>
    .done {
        text-decoration: line-through;
    }
</style>