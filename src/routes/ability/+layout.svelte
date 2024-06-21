<script>

  import { goto } from "$app/navigation";

</script>
<div id="top_bar"></div>
<slot />
<div id="bottom_bar">
    <button on:click={()=>{
        goto("/ability/main")
    }}>필기</button>
    <button on:click={()=>{
        goto("/ability/calendar")
    }}>달력</button>
    <button on:click={()=>{
        goto("/ability/share")
    }}>공유</button>
    <button on:click={()=>{
        goto("/ability/social")
    }}>소셜</button>
</div>

<style>
    #top_bar {
        background-color: rgb(7, 179, 253);
        width: 100vw;
        height: 10vh;
    }

    #bottom_bar {
        background-color: rgb(7, 179, 253);
        width: 100vw;
        height: 10vh;
    }
</style>