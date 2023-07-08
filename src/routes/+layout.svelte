<script lang="ts">
    import { onMount } from "svelte";
    
    let load = false;
    let sidebar = false;
    let popup = false;
    
    let writeIdxedDB = async (names:any[]) => {console.log('추가 로딩 안됨')};
    let deleteIdxedDBValue = (key:number) => {console.log('삭제 로딩 안됨')};
    let clearIdxedDBValue = async () => {console.log('초기화 로딩 안됨')};
    let getIdxedDBValue = async (key:number) => {console.log('초기화 로딩 안됨')};
    let getIdxedDBFirstValue = async () => {console.log('첫번째 로딩 안됨')};
    let getIdxedDBLength = async () => {console.log('길이 로딩 안됨');return 0};

    const add = (things:any[]) => {writeIdxedDB(things)};
    const del = (key:number) => {deleteIdxedDBValue(key)};
    const clear = () => {clearIdxedDBValue()};
    const find = (key:number) => getIdxedDBValue(key);
    const first = () => getIdxedDBFirstValue();
    const getLength = () => getIdxedDBLength();

    let lists:{id:number, name:string, time:number[]}[] = [];
    let can = true;
    let yoil = ['일', '월', '화', '수', '목', '금', '토'];
    let focusinfo:{id:number, name:string, time:number[]};

    const pop = (focus:{id:number, name:string, time:number[]}) => {
        focusinfo = focus;
        popup = true;
    }

    const DBName = "list";

    const update = async () => {
        const length = await getLength();
        const preLength = (lists.length)? lists[0].id: -1;
        const fir = await first();
        if (length > preLength) {
            for (let i = preLength + fir.id + 1; i < length + fir.id; i++) {
                let res = await find(i);
                lists = [res, ...lists];
            }
            can = !(lists.length >= length);
        }
    }
    const reload = async () => {
        lists = [];
        const leng = await getLength();
        if (!lists.length && leng) {
            const fir = await first();
            const last = fir.id + leng - 1;
            for (let i = last; i > Math.max(last - 10, fir.id - 1); i--) {
                let now = await find(i);
                lists = [...lists, now];
            }
            can = !(last - 10 <= fir.id - 1);
        }
    }

    onMount(async () => {
        const idxedDB = window.indexedDB;
        if (!idxedDB) window.alert('해당 브라우저에서는 indexedDB를 지원하지 않습니다.')
        else {
            let db;
            const request = idxedDB.open('SampleDB');
            request.onerror = (e) => alert('failed');
            request.onsuccess = (e) => db = e.target.result;
            request.onupgradeneeded = (e) => {                
                db = e.target.result;
                let objectStore = db.createObjectStore(DBName, { keyPath: "id" });
            }}
        writeIdxedDB = async (names:any[]) => {
            const request = window.indexedDB.open('SampleDB');
            request.onsuccess = async (e) => {
                const db = request.result;
                const transaction = db.transaction([DBName], 'readwrite');  
                //person 객체 저장소에 읽기&쓰기 권한으로 transaction 생성
    
                // 완료, 실패 이벤트 처리
                transaction.oncomplete =(e)=> {
                    console.log('추가 성공');
                }
                transaction.onerror =(e)=> {
                    if(!(e.target instanceof IDBRequest)) return;
                    console.log('추가 실패', e.target.error);
                };
    
                // transaction으로 
                const objStore = transaction.objectStore(DBName);
                for (const name of names) {
                    const request = objStore.add(name);   // 저장
                }

                await update();
            }}
        deleteIdxedDBValue = (key:number) => {
            const request = window.indexedDB.open('SampleDB');     // 1. db 열기

            request.onsuccess =(e)=> {
                const db = request.result;
                const transaction = db.transaction(DBName, 'readwrite');
                transaction.onerror =(e)=> {
                    if(!(e.target instanceof IDBRequest)) return;
                    console.log('삭제 실패', e.target.error);
                };
                transaction.oncomplete =(e)=> console.log('삭제 성공');

                const objStore = transaction.objectStore(DBName);   // 2. name 저장소 접근
                const objStoreRequest = objStore.delete(key);       // 3. 삭제하기
            }}
        clearIdxedDBValue = async () => {
            const request = window.indexedDB.open('SampleDB');     // 1. db 열기

            request.onsuccess = async (e) => {
                const db = request.result;
                const transaction = db.transaction(DBName, 'readwrite');
                transaction.onerror =(e)=> {
                    if(!(e.target instanceof IDBRequest)) return;
                    console.log('초기화실패', e.target.error);
                };
                transaction.oncomplete =(e)=> console.log('초기화성공');

                const objStore = transaction.objectStore(DBName);   // 2. name 저장소 접근
                const objStoreRequest = objStore.clear();           // 3. 전체 삭제
                objStoreRequest.onsuccess =(e)=> {
                    console.log('초기화됨.');
                }

                await reload();
            }}
        getIdxedDBValue = async (key:number) => {
            const request = window.indexedDB.open('SampleDB');  // 1. DB 열기
            const e = await new Promise<Event>((res, rej) => {
                request.onsuccess = res;
                request.onerror = rej;
            });
            const db = request.result;
            const transaction = db.transaction(DBName);
            transaction.onerror =(e)=> {
                if(!(e.target instanceof IDBRequest)) return;
                console.log('조회실패', e.target.error);
            };
            transaction.oncomplete =(e)=> console.log('조회성공');

            const objStore = transaction.objectStore(DBName);
            const objStoreRequest = objStore.get(key);        // 2. get으로 데이터 접근
            const res = await new Promise((res, rej) => {
                objStoreRequest.onsuccess = res;
                objStoreRequest.onerror = rej;
            }).then(val => objStoreRequest.result)
            .catch(err => {throw err});
            return res;}
        getIdxedDBFirstValue = async () => {
            const request = window.indexedDB.open('SampleDB');      // 1. DB 열기
            const e = await new Promise<Event>((res, rej) => {
                request.onsuccess = res;
                request.onerror = rej;
            });
            const db = request.result;
            const transaction = db.transaction(DBName);
            transaction.onerror =(e)=> {
                if(!(e.target instanceof IDBRequest)) return;
                console.log('첫번째 조회 실패', e.target.error);
            };
            transaction.oncomplete =(e)=> console.log('첫번째 조회 성공');

            const objStore = transaction.objectStore(DBName);    // 2. name 저장소 접근
            const cursorRequest = objStore.openCursor();
            const res = await new Promise((res, rej) => {
                cursorRequest.onsuccess = res;
                cursorRequest.onerror = rej;
            }).then(val => cursorRequest.result)
            .catch(err => {throw err});
            return res?.value;}
        getIdxedDBLength = async () => {
            const request = window.indexedDB.open('SampleDB');  // 1. DB 열기
            const e = await new Promise<Event>((res, rej) => {
                request.onsuccess = res;
                request.onerror = rej;
            });
            const db = request.result;
            const transaction = db.transaction(DBName);
            transaction.onerror =(e)=> {
                if(!(e.target instanceof IDBRequest)) return;
                console.log('길이 조회 실패', e.target.error);
            };
            transaction.oncomplete =(e)=> console.log('길이 조회 성공');

            const objStore = transaction.objectStore(DBName);
            const objStoreRequest = objStore.count();
            const res = await new Promise((res, rej) => {
                objStoreRequest.onsuccess = res;
                objStoreRequest.onerror = rej;
            }).then(val => objStoreRequest.result)
            .catch(err => {throw err});
            return res;
        }
        const leng = await getLength();
        console.log(leng);
        if (!lists.length && leng) {
            const fir = await first();
            if (fir.time) {
                const last = fir.id + leng - 1;
                for (let i = last; i > Math.max(last - 10, fir.id - 1); i--) {
                    let now = await find(i);
                    lists = [...lists, now];
                }
                can = !(last - 10 <= fir.id - 1);
            }
            else {
                clear();
            }
        }
        load = true;
    });

    const more = async () => {
        const fir = await first();
        const last = lists[lists.length - 1].id;
        for (let i = last - 1; i > Math.max(last - 11, fir.id - 1); i--) {
            let now = await find(i);
            lists = [...lists, now];
        }
        can = !(last - 11 <= fir.id - 1);
    }
</script>

<main class="bc2" style="{(load)? '':'display: none;'}">
    <div class="pop {popup? '':'no'}">
        <div class="popup" style="z-index: 2;">
            {#if focusinfo}
            <div class="popupup">
                <div class="popuptitle">{focusinfo.id}번째 {focusinfo.name}</div>
                <div class="popupclose" on:click={() => {popup = false}}><div class="closein">close</div></div>
            </div>
            <div class="popupdown">{focusinfo.time[0]}년 {focusinfo.time[1] + 1}월 {focusinfo.time[2]}일 {yoil[focusinfo.time[7]]}요일 {focusinfo.time[3]}시 {focusinfo.time[4]}분 {focusinfo.time[5]}.{focusinfo.time[6]}초</div>
            {/if}
        </div>
        <div style="width: 100vw; height: 100vh; position: absolute;" on:click={() => {popup = false}}></div>
    </div>
    <header class="bc3">
        <button class="pan" on:click={() => {sidebar = true}}>
            <svg width="50" height="50" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
            <g clip-path="url(#clip0_53997_18030)">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M11 16V14H29V16H11ZM11 21H29V19H11V21ZM11 26H29V24H11V26Z" fill="#CAC4D0"/>
            </g>
            <defs>
            <clipPath id="clip0_53997_18030">
            <rect width="40" height="40" rx="20" fill="white"/>
            </clipPath>
            </defs>
            </svg>
        </button>
        <a href="/" class="home">home</a>
        <h1>모션인식 건강관리 프로그램</h1>
        <button style="margin: 5px;" on:click={async () => {const time = new Date(); await add([{id: await getLength(), name: 'dsfasdf', time: [time.getFullYear(), time.getMonth(), time.getDate(), time.getHours(), time.getMinutes(), time.getSeconds(), time.getMilliseconds(), time.getDay()]}])}}>값 1개 추가</button>
        <button style="margin: 5px;" on:click={async () => {await clear()}}>모두 삭제</button>
    </header>
    <div class="side {sidebar? 'on':''}">
        <div class="up">
            <div class="listname">기록</div>
            <div class="reload" on:click={async () => {await reload()}}>refresh</div>
            <button class="pan" on:click={() => {sidebar = false}}>
                <svg width="50" height="50" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g clip-path="url(#clip0_53997_17377)">
                <path d="M23.089 12.0734L21.9265 10.9108L17.3175 15.5198L12.7085 10.9108L11.5459 12.0734L16.1549 16.6824L11.5459 21.2914L12.7085 22.4539L17.3175 17.8449L21.9265 22.4539L23.089 21.2914L18.48 16.6824L23.089 12.0734Z" fill="#CAC4D0"/>
                </g>
                <defs>
                <clipPath id="clip0_53997_17377">
                <rect x="0.827148" y="0.192169" width="32.9804" height="32.9804" rx="16.4902" fill="white"/>
                </clipPath>
                </defs>
                </svg>
            </button>
        </div>
        <div class="lists">
            {#each lists as {id, name, time}, i}
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div class="list" on:click={() => {pop({id, name, time})}}>
                <div class="listTitle">{i + 1}. {id}번째 {name}</div>
                <div class="listTime"><div>{time[1] + 1}월 {time[2]}일 {time[3]}:{time[4]}:{time[5]}</div>
                <div class="infoBut">info</div></div>
                <!-- <div class="listTime">{new Date(Date.now()).toISOString()}</div> -->
            </div>
            {/each}
            {#if can && lists.length}
            <div class="graph" style="position: static;" on:click={more}>
                <div class="graphText">더보기</div>
            </div>
            {:else if !lists.length}
            <div class="nothing">기록이 없습니다</div>
            {/if}
            <a class="graph" href="/graph">
                <div class="graphText">눈 깜빡임 그래프 보기</div>
            </a>
            <div class="dark"></div>
        </div>
    </div>
    <slot></slot>
</main>
<style>
    .bc1 {
        background-color: black;
    }
    .bc2 {
        background-color: #141218;
    }
    .bc3 {
        background-color: #2B2930;
    }
    h1 {
        font-weight: 700;
        font-size: 22px;
        color: #E6E0E9;
        text-align: center;
    }
    main {
        width: 100vw;
        min-height: 100vh;
    }
    header {
        position: fixed;
        width: 100vw;
        height: 80px;
        z-index: 101;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .pan{
        position: absolute;
        background-color: rgba(0, 0, 0, 0);
        border: 0;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 10px;
        padding: 0;
        transition: 200ms;
    }
    header > .pan {
        left: 0px;
    }
    .pan:hover {
        background-color: rgba(208, 188, 255, 0.12);
        cursor: pointer;
    }
    .side {
        position: fixed;
        width: 350px;
        height: 100vh;
        background-color: #1D1B20;
        z-index: 102;
        transform: translateX(-350px);
        transition: 200ms;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .side.on {
        transform: translateX(0px);
    }
    .up {
        width: 100%;
        height: 80px;
        display: flex;
        align-items: center;
        flex-shrink: 0;
    }
    .lists {
        width: 100%;
        height: 100%;
        overflow-x: hidden;
        overflow-y: scroll;
        padding-bottom: 100px;
    }
    .lists::-webkit-scrollbar {
        width: 10px;
    }
    .lists::-webkit-scrollbar-thumb {
        border-radius: 5px;
        background-color: rgba(54, 52, 59, 1);
    }
    .lists::-webkit-scrollbar-track {
        background-color: rgba(0, 0, 0, 0);
    }
    .list {
        width: calc(100% - 4px);
        height: 80px;
        margin-bottom: 2px;
        background-color: rgba(20, 18, 24, 1);
        color: rgba(230, 224, 233, 1);
        padding-left: 20px;
        padding-right: 20px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        flex-shrink: 2;
    }
    .up > .pan {
        right: 0;
    }
    .listname {
        font-size: 19px;
        margin: 20px;
        color: #CAC4D0;
        font-weight: 800;
        margin-right: 0px;
    }
    .list > div {
        width: 300px;
        overflow: hidden;
        display: inline-block; 
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
    }
    .list > div:hover {
        cursor: pointer;
    }
    .listTitle {
        font-size: 20px;
        color: #E6E0E9;
        font-weight: 600;
    }
    div.listTime {
        font-size: 16px;
        color: #CAC4D0;
        font-weight: 300;
        display: flex;
        align-items: center;
        flex-direction: row;
    }
    .graph {
        position: fixed;
        width: calc(100% - 40px);
        height: 60px;
        margin: 20px;
        display: flex;
        align-items: center;
        flex-shrink: 0;
        bottom: 0;
        border-radius: 20px;
        border: 4px solid #DFD5EC;
        margin-left: 10px;
        background-color: #1D1B20;
        user-select: none;
        color: #E6E0E9;
        text-decoration: none;
        justify-content: center;
        z-index: 2;
    }
    .graph:hover {
        cursor: pointer;
    }
    .graphText {
        font-weight: 400;
        font-size: 19px;
        margin: 20px;
    }
    .nothing {
        color: #E6E0E9;
        font-size: 18px;
        text-align: center;
        margin: 20px;
        font-weight: 500;
    }
    .dark {
        position: absolute;
        width: 100px;
        height: 150px;
        background-color: black;
        bottom: 0px;
        width: calc(100% - 10px);
        z-index: 1;
        background: linear-gradient(180deg, rgba(255,255,255,0) 0%, rgba(0,0,0,1) 100%);
        pointer-events: none;
    }
    .reload {
        font-family: "Material Symbols Outlined";
        font-size: 23px;
        font-weight: 500;
        text-align: center;
        color: #CAC4D0;
        margin: 4px;
        transition: 200ms;
        user-select: none;
    }
    .reload:hover {
        cursor: pointer;
        transform: scale(110%, 110%) rotate(180deg);
    }
    .home {
        font-family: "Material Symbols Outlined";
        font-size: 28px;
        color: #E6E0E9;
        margin: 3px;
        user-select: none;
        transition: 200ms;
        text-decoration: none;
    }
    .home:hover {
        cursor: pointer;
        transform: scale(110%, 110%);
    }
    .infoBut {
        font-family: "Material Symbols Outlined";
        font-size: 18px;
        user-select: none;
        margin: 0;
        width: min-content;
        height: min-content;
        text-align: center;
        margin-left: 3px;
        font-weight: 400;
    }
    .pop {
        width: 100vw;
        height: 100vh;
        position: fixed;
        background-color: rgba(0, 0, 0, 0.4);
        z-index: 10000;
        display: flex;
        justify-content: center;
        align-items: center;
        transition: 200ms;
    }
    .pop.no {
        pointer-events: none;
        user-select: none;
        opacity: 0%;
    }
    .popup {
        width: 30vw;
        height: 45vh;
        background-color: #1D1B20;
        border: 4px solid #DFD5EC;
        border-radius: 20px;
        padding: 30px;
        color: #DFD5EC;
        transition: 300ms cubic-bezier(.36,.01,0,1);
    }
    .pop.no > .popup {
        transform: scale(95%, 95%);
        opacity: 0%;
    }
    .popupup {
        width: 100%;
        display: flex;
        justify-content: space-between;
    }
    .popuptitle {
        font-size: 40px;
        font-weight: 700;
        width: calc(100% - 60px);
    }
    .popupclose {
        width: 50px;
        height: 50px;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: 200ms;
        border-radius: 50%;
    }
    .popupclose:hover {
        background-color: rgba(208, 188, 255, 0.12);
        cursor: pointer;
    }
    .closein {
        font-family: "Material Symbols Outlined";
        font-size: 30px;
        user-select: none;
    }
    .popupdown {
        margin-top: 5px;
        width: 100%;
        font-size: 23px;
    }
</style>