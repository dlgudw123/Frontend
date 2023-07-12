<script lang="ts">
    let sel:number;
    let year = new Date().getFullYear();
    let month = new Date().getMonth() + 1;
    const yoil = ['일', '월', '화', '수', '목', '금', '토'];
</script>

<main>
    <div class="date {(sel + 1)? 'yes':''}">
        <div class="title">{year}년 {month}월{(sel)? ` ${sel}일 ${yoil[new Date(year,month - 1,sel).getDay()]}요일`:''}</div>
        <div class="bin" style="width: {new Date(year,month - 1,1).getDay() * 92}px;"></div>
        {#each Array(new Date(year,month,0).getDate()) as v, i}
        <label><div>{i + 1}</div><input type="radio" name="date" value="{i + 1}" on:input={() => {sel = i + 1}}></label>
        {/each}
    </div>
</main>

<style>
    main {
        width: 100vw;
        height: calc(100vh - 80px);
        padding-top: 80px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .date {
        display: flex;
        width: calc(92px * 7);
        flex-wrap: wrap;
        transition: 500ms cubic-bezier(.36,.01,0,1);
    }
    .date.yes {
        transform: translateX(-350px);
    }
    label > input {
        display: none;
    }
    label {
        width: 80px;
        height: 80px;
        background-color: #1D1B20;
        border: 4px solid #DFD5EC;
        border-radius: 20px;
        margin: 2px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #DFD5EC;
        transition: 200ms;
    }
    .bin {
        height: 80px;
    }
    label > div {
        font-size: 30px;
        font-weight: 700;
    }
    label:hover {
        cursor: pointer;
    }
    label:has(input:checked) {
        background-color: #DFD5EC;
        color: #1D1B20;
    }
    .title {
        color: #DFD5EC;
        font-size: 50px;
        width: 100%;
        font-weight: 700;
        text-align: center;
        margin-bottom: 20px;
    }
</style>