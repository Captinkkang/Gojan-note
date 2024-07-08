<script>
    import { goto } from "$app/navigation";
    let spot = "main"
</script>
<div id="top_bar">
    <div class="title">blue board</div>
    <a class="thum" href="https://kojan-h.goeas.kr/kojan-h/main.do">
        <img src="../gojan.png" alt="이미지 로드 실패">
    </a>
    <button class="menu">
        <img src="../menu.png" alt="이미지 로드 실패">
    </button>
</div>
<div class="content_box">
    <slot />
</div>
<div id="bottom_bar">
    <div class="bottom_b">
        <button on:click={()=>{
            spot = "main"
            goto("/ability/main")
        }}>
            {#if spot==="main"}
                <div>
                    <img class="icon" src="../bnote.png" alt="노트 이미지">
                </div>
                <div style="color: blue;">필기</div>
            {:else}
                <div>
                    <img class="icon" src="../note.png" alt="노트 이미지">
                </div>
                <div style="color: white;">필기</div>
            {/if}
        </button>
        
    </div>
    <div class="bottom_b">
        <button on:click={()=>{
            spot = "calendar"
            goto("/ability/calendar")
        }}>
            {#if spot==="calendar"}
                <div>
                    <img class="icon" src="../bcalendar.png" alt="노트 이미지">
                </div>
                <div style="color: blue;">달력</div>
            {:else}
                <div>
                    <img class="icon" src="../calendar.png" alt="노트 이미지">
                </div>
                <div style="color: white;">달력</div>
            {/if}
        </button>
    </div>
    <div class="bottom_b">
        <button on:click={()=>{
            spot="share"
            goto("/ability/share")
        }}>
            {#if spot==="share"}
                <div>
                    <img class="icon" src="../bshare.png" alt="노트 이미지">
                </div>
                <div style="color: blue;">공유</div> 
            {:else}
                <div>
                    <img class="icon" src="../share.png" alt="노트 이미지">
                </div>
                <div style="color: white;">공유</div>
            {/if}
        </button>
    </div>
    <div class="bottom_b">
        <button on:click={()=>{
            spot="social"
            goto("/ability/social")
        }}>
            {#if spot==="social"}
                <div>
                    <img class="icon" src="../bsocial.png" alt="노트 이미지">
                </div>
                <div style="color: blue;">소셜</div>
            {:else}
                <div>
                    <img class="icon" src="../social.png" alt="노트 이미지">
                </div>
                <div style="color: white;">소셜</div>
            {/if}
        </button>
    </div>
</div>

<style>

    #top_bar {
        background-color: rgb(7, 179, 253);
        width: 100vw;
        height: 10vh;
        display: flex;
    }

    #top_bar > button {
        flex-grow: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: rgb(7, 179, 253);
        border: none;
    }

    .menu > img {
        width: 80%;
        height: 80%;
    }

    .menu > img:hover {
        cursor: pointer;
    }

    #top_bar > .title {
        flex-grow: 50;
        display: flex;
        text-align: center;
        align-items: center;
        justify-content: center;
        font-style: italic;
        font-weight: bold;
        color: white;
    }

    #top_bar > .thum {
        flex-grow: 2;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .thum > img {
        width: 80%;
        height: 80%;
    }

    .thum > img:hover {
        cursor: pointer;
    }

    #bottom_bar {
        display: flex;
        justify-content: space-between;
        background-color: rgb(7, 179, 253);
        width: 100vw;
        height: 10vh;
    }

    #bottom_bar>.bottom_b {
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 25%;
    }

    .bottom_b>button {
        width: 50%;
        height: 100%;
        overflow: hidden;
        background-color: rgb(7, 179, 253);
        border: none;
    }

    .content_box {
        height: 80vh;
    }

    .bottom_b>button:hover {
        cursor: pointer;
    }

    .icon {
        width: 100%;
        height: 6vh;
    }

    .icon:hover {
        cursor: pointer;
    }
</style>