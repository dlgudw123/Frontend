<script>
    let sidebar = false;
</script>

<main class="bc2">
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
        <h1>모션인식 건강관리 프로그램</h1>
    </header>
    <div class="side {sidebar? 'on':''}">
        <div class="up">
            <div class="listname">기록</div>
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
            {#each Array(30).fill('b') as v, i}
            <div class="list">
                <div class="listTitle">{i + 1}번째</div>
                <div class="listTime">0월 0일 0:00</div>
            </div>
            {/each}
            <div class="graph">
                <div class="graphText">눈 깜빡임 그래프 보기</div>
            </div>
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
        position: absolute;
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
    .listTime {
        font-size: 16px;
        color: #CAC4D0;
        font-weight: 300;
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
    }
    .graph:hover {
        cursor: pointer;
    }
    .graphText {
        font-weight: 400;
        font-size: 19px;
        margin: 20px;
    }
</style>