<script lang="ts">
    
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
            <a href="/exercise" class="goto">
                <svg width="30" height="30" viewBox="0 0 25 24" fill="none">
                    <path d="M12.625 20L11.2 18.6L16.8 13H4.625V11H16.8L11.2 5.4L12.625 4L20.625 12L12.625 20Z" fill="#DFD5EC"/>
                </svg>                    
            </a>
        </div>
        <div class="info">
            <div class="title2 botm">추천 음식</div>
            <div class="text1 infoT">Bruschetta Style Pork & Pasta</div>
            <a href="/food" class="goto">
                <svg width="30" height="30" viewBox="0 0 25 24" fill="none">
                    <path d="M12.625 20L11.2 18.6L16.8 13H4.625V11H16.8L11.2 5.4L12.625 4L20.625 12L12.625 20Z" fill="#DFD5EC"/>
                </svg>                    
            </a>
        </div>
        <div class="info">
            <div class="title2 botm">현재 상태</div>
            <div class="text1 infoT">거북목이 우려됨.</div>
            <a href="/info" class="goto">
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
        /* min-height: 300px; */
        display: flex;
        align-items: center;
        justify-content: start;
        flex-direction: column;
    }
    .title {
        color: #DFD5EC;
        font-size: 40px;
        width: 100%;
        font-weight: 900;
        text-align: start;
        /* text-wrap: balance; */
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
        width: 1150px;
        min-height: 200px;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: row;
    }
    @media (max-width: 1150px) {
        .infos {
            flex-wrap: wrap;
            width: 570px;
        }
        .feed {
            width: 700px;
        }
        .feed > .title {
            font-size: 30px;
        }
    }
    @media (max-width: 770px) {
        .infos {
            flex-wrap: wrap;
            width: 280px;
        }
        .feed {
            width: 90%;
        }
        .feed > .title {
            font-size: 20px;
        }
    }
    /* .infos:has(.info:hover) > .info:not(:hover) {
        opacity: 20%;
        user-select: none;
    } */
    .info {
        width: 220px;
        height: 150px;
        margin: 10px;
        background-color: #1D1B20;
        border: 4px solid #DFD5EC;
        border-radius: 20px;
        padding: 15px;
        position: relative;
        /* transition: 200ms; */
    }
    /* .info:hover {
        transform: scale(120%, 120%);
        z-index: 4;
    } */
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
        background-color: #1d1b207c;
        box-shadow: #1d1b207c 0px 0px 5px 3px;
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
    svg {
        pointer-events: none;
    }
</style>