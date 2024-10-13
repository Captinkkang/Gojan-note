<script lang="ts">
    import { onMount } from "svelte";
    import Canvas from "../canvas.svelte";

    let color:string;
    let bright = '#fff'
    let linewidth = 3;
    let recent_pallet = ["#000000","#ff0000","#ffae00","#fff700","#ffffff","#11ff00","#00eeff","#8000ff"]
    let r_pallet_open = false
    let choose_polygon = false
    let click_chance:[number, boolean] = [0,false]
    const draw_shape = (n:number) =>{
        click_chance = [n,true]
    }
    console.log(choose_polygon)
    $:{
        if(recent_pallet.includes(color) === false){
            recent_pallet.pop()
            recent_pallet.unshift(color)
            if(recent_pallet[0] === undefined){
                recent_pallet.shift()
                recent_pallet.push("#00000")
            } 
        }
    }
</script>

<div class="content">
    <div class="top_b">
        <div class="main_t">
            <input type="color" bind:value={color} on:click={()=>{r_pallet_open = false}}>
            <button on:click={()=>{
                let answer = prompt("위 동작은 현재 작성하는 문서가 저장 되지 않습니다.\n새로고침 하시겠습니까? y/n")
                if(answer === "y"){
                    if(bright === '#fff'){bright = '#00000'}
                    else {bright = '#fff'}
                }
                r_pallet_open = false
                choose_polygon = false
            }}>색반전</button>
            <input type="range" min="0.1" max="10.0" step="0.5" bind:value={linewidth} on:click={()=>{r_pallet_open = false}}>
            <button on:click={()=>{
                r_pallet_open = false
                if(choose_polygon === true){
                    choose_polygon = false
                }else{
                    choose_polygon = true
                }
            }}>도형</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
            }}>그래프</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
            }}>이미지</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
            }}>텍스트</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
            }}>전체 지우기</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
            }}>되돌리기</button>
            <button on:click={()=>{
                if(r_pallet_open===false){r_pallet_open = true}
                else{r_pallet_open = false}
                choose_polygon = false
            }}>최근 색상</button>
        </div>
        <div class="sub_t"></div>
        {#if r_pallet_open === true}
            <div class="r_pallet">
                {#each recent_pallet as i}
                    <button style="background-color: {i};" on:click={()=>{
                        color = i
                        r_pallet_open = false
                    }}></button>
                {/each}
            </div>
        {/if}
        {#if choose_polygon === true}
            <div class="c_polygon">
                <button on:click={()=>{
                    console.log("make triangle")
                    choose_polygon = false
                    draw_shape(3)
                }}>삼각형</button>
                <button>정삼각형</button>
                <button>직사각형</button>
                <button>정사각형</button>
                <button>평행사변형</button>
                <button>정오각형</button>
                <button>다각형</button>
                <button>선</button>
            </div>
        {/if}
    </div>
    <Canvas
        {color}
        backcolor = {bright}
        line = {linewidth}
        draw = {click_chance}
    />
</div>

<style>
    .content {
        width: 100%;
    }

    .top_b {
        background-color: gray;
    }

    .r_pallet {
        left: 12%;
        height: 50px;
        width: 60px;
        position: absolute;
        border: 1px solid black;
        background-color: white;
    }

    .r_pallet > button{
        width: 10px;
        height: 10px;
        border-collapse: 10px;
        border: 1px solid black;
        border-radius: 50%;
        margin-left: 1px;
        transition: 0.5s;
        scale: 1;
        opacity: 1;
    }

    .r_pallet > button:hover {
        cursor: pointer;
        transition: 0.5s;
        scale: 1.2;
        opacity: 0.5;
    }

</style>