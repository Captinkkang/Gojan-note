<script>
    import { goto } from "$app/navigation";
    let spot = "main"
</script>
<div id="top_bar"></div>
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