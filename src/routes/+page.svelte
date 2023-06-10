<script lang="ts">
    // import { onMount } from "svelte";

    // onMount(() => {
    //     const idxedDB = window.indexedDB;
    //     if (!idxedDB) window.alert('해당 브라우저에서는 indexedDB를 지원하지 않습니다.')
    //     else {
    //         let db;
    //         const request = idxedDB.open('SampleDB');
    //         request.onerror = (e) => alert('failed');
    //         request.onsuccess = (e) => db = request.result;
    //         request.onupgradeneeded = (e) => {                
    //             db = e.target.result;
    //             let objectStore = db.createObjectStore("memo", { keyPath: "id" });
    //             objectStore.createIndex("name", "name", { unique: false });
    //         }
    //     }
    // })
</script>

<main>
    <div class="feed">
        <div class="text1">AI의 피드백</div>
        <div class="title">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec id est luctus, vehicula nisl ac, volutpat nisl. Nullam facilisis efficitur mauris, et volutpat nisl pulvinar eget. Morbi erat ex, pellentesque varius odio quis, aliquam feugiat ipsum. Aliquam ante ex, volutpat id dolor sit amet, blandit luctus elit.</div>
    </div>
    <div class="infos">
        <div class="info">
            <div class="title2 botm">추천 운동</div>
            <div class="text1 infoT">Seated Head Harness Neck Resistance</div>
            <a href="/" class="goto">
                <svg width="30" height="30" viewBox="0 0 25 24" fill="none">
                    <path d="M12.625 20L11.2 18.6L16.8 13H4.625V11H16.8L11.2 5.4L12.625 4L20.625 12L12.625 20Z" fill="#DFD5EC"/>
                </svg>                    
            </a>
        </div>
        <div class="info">
            <div class="title2 botm">추천 음식</div>
            <div class="text1 infoT">Bruschetta Style Pork & Pasta</div>
            <a href="/" class="goto">
                <svg width="30" height="30" viewBox="0 0 25 24" fill="none">
                    <path d="M12.625 20L11.2 18.6L16.8 13H4.625V11H16.8L11.2 5.4L12.625 4L20.625 12L12.625 20Z" fill="#DFD5EC"/>
                </svg>                    
            </a>
        </div>
        <div class="info">
            <div class="title2 botm">현재 상태</div>
            <div class="text1 infoT">거북목이 우려됨.</div>
            <a href="/" class="goto">
                <svg width="30" height="30" viewBox="0 0 25 24" fill="none">
                    <path d="M12.625 20L11.2 18.6L16.8 13H4.625V11H16.8L11.2 5.4L12.625 4L20.625 12L12.625 20Z" fill="#DFD5EC"/>
                </svg>                    
            </a>
        </div>
        <div class="info">
            <div class="title2 botm">오늘 점수</div>
            <div class="infoT2">94점
                <div class="text1">어제보다 8% 좋음</div>
            </div>
        </div>
    </div>
</main>

<style>
    main {
        padding-top: 80px;
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .feed {
        margin: 20px;
        margin-top: 40px;
        width: 90%;
        min-height: 300px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }
    .title {
        color: #DFD5EC;
        font-size: 40px;
        width: 100%;
        font-weight: 900;
        text-align: start;
        text-wrap: balance;
    }
    .title2 {
        color: #DFD5EC;
        font-size: 30px;
        font-weight: 700;
    }
    .text1 {
        color: #DFD5EC;
        font-size: 20px;
        width: 100%;
        font-weight: 500;
        text-align: start;
        text-wrap: balance;
    }
    .infos {
        margin: 20px;
        width: 90%;
        min-height: 200px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: row;
    }
    .infos:has(.info:hover) > .info:not(:hover) {
        filter: blur(4px) brightness(50%);
        user-select: none;
    }
    .info {
        width: 220px;
        height: 150px;
        margin: 10px;
        background-color: #1D1B20;;
        border: 4px solid #DFD5EC;
        border-radius: 20px;
        padding: 15px;
        position: relative;
        transition: 200ms;
    }
    .info:hover {
        transform: scale(120%, 120%);
        z-index: 4;
        /* margin-left: 25px;
        margin-right: 25px; */
    }
    .infoT {
        width: 220px;
        height: 110px;
        overflow: hidden;
        line-height: 27px;
    }
    .infoT2 {
        font-size: 60px;
        font-weight: 600;
        color: #DFD5EC;
        width: 220px;
        height: 110px;
        overflow: hidden;
    }
    .info:hover > .infoT {
        overflow: auto;
    }
    .info > .infoT::-webkit-scrollbar {
        display: none;
    }
    
    .botm {
        margin-bottom: 5px;
    }
    .goto {
        width: 50px;
        height: 50px;
        margin: 10px;
        border-radius: 50%;
        display: block;
        position: absolute;
        right: 5px;
        bottom: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: 200ms;
        z-index: 5;
    }
    .goto > svg {
        border-radius: 50%;
        background-color: rgba(0, 0, 0, 0.5);
        box-shadow: rgba(0, 0, 0, 0.5) 0px 0px 5px 3px;
        transition: 200ms;
    }
    .goto:hover > svg {
        border-radius: 50%;
        background-color: #00000000;
        box-shadow: none;
    }
    .goto:hover {
        background-color: rgba(208, 188, 255, 0.12);
    }
</style>