<script lang="ts">
    import { onMount } from "svelte";

    onMount(() => {
        const idxedDB = window.indexedDB;
        if (!idxedDB) window.alert('해당 브라우저에서는 indexedDB를 지원하지 않습니다.')
        else {
            let db;
            const request = idxedDB.open('SampleDB');
            request.onerror = (e) => alert('failed');
            request.onsuccess = (e) => db = request.result;
            request.onupgradeneeded = (e) => {                
                db = e.target.result;
                let objectStore = db.createObjectStore("memo", { keyPath: "id" });
                objectStore.createIndex("name", "name", { unique: false });
            }
        }
    })
</script>
<h1>프론트엔드</h1>