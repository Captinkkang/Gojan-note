<script lang="ts">
  import { goto } from "$app/navigation";

    interface notes {
        title:string;
        img:string;
        time:string
    }
    let user_note:notes[] = [
        {title:"1",img:"https://pbs.twimg.com/media/F2cRdzZaMAE8UCs.jpg",time:"24.7.13"},{title:"1",img:"https://pbs.twimg.com/media/F2cRdzZaMAE8UCs.jpg",time:"24.7.13"},{title:"1",img:"https://pbs.twimg.com/media/F2cRdzZaMAE8UCs.jpg",time:"24.7.13"}
    ]
</script>

<div class="content">
    <div class="note">
        <div class="thum">
        </div>
        <div class="note_info">
            <button on:click={()=>{
                goto('./drawing/')
            }}>새로 만들기</button>
        </div>
    </div>
    {#each user_note as i}
        <div class="note">
            <div class="thum">
                <img src="{i.img}" alt="섬네일 로드 실패">
            </div>
            <div class="note_info">{i.title}<br>{i.time}</div>
        </div>
    {/each}
</div>

<style>
    .content {
        height: 80vh;
        top: 20vh;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        overflow: auto;
    }

    .note {
        margin: 2.93%;
        width: 25vw;
        height: 40vh;
        background-color: rgb(31, 46, 101);
        padding: 1%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    
    .thum {
        background-color: white;
        width: 90%;
        height: 90%;
    }

    .thum > img {
        width: 100%;
        height: 100%;
    }

    .note_info {
        color: white;
        text-align: center;
    }

    .note_info:hover {
        cursor: pointer;
    }
</style>