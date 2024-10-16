<script lang="ts">
    import { onMount } from "svelte";
    import Canvas from "../canvas.svelte";
    import { fly } from "svelte/transition";

    let color:string;
    let bright = '#fff'
    let linewidth = 3;
    let recent_pallet = ["#000000","#ff0000","#ffae00","#fff700","#ffffff","#11ff00","#00eeff","#8000ff"]
    let r_pallet_open = false
    let choose_polygon = false//사이드바 펼치냐 마냐
    let vtx_num = 0
    let ischecked = false
    let click_chance:[number, boolean, boolean] = [0,false, false]
    let bt_info = ""
    let infoVisible = false;
    let line_chance:[number, number, string] = [0, 0, "nl"]
    let point_line = 0
    let s_line = "nl"

    const draw_shape = (n:number, c:boolean) =>{
        click_chance = [n,true,c]
    }

    const draw_line = (n:number, a:number, b:string) => {
        line_chance = [n, a, b]
    }

    $: if (bt_info) {
        infoVisible = true;  // bt_info가 바뀌면 보이도록 설정
        setTimeout(() => {
            infoVisible = false;  // 3초 후에 사라지도록 설정
        }, 3000); // 3초 후에 사라짐
    }

    $:{
        if(recent_pallet.includes(color) === false){
            recent_pallet.pop()
            recent_pallet.unshift(color)
            if(recent_pallet[0] === undefined){
                recent_pallet.shift()
                recent_pallet.push("#00000")
            } 
        }
        console.log(line_chance)
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
        
    </div>
    <Canvas
        {color}
        backcolor = {bright}
        line = {linewidth}
        draw = {click_chance}
        draw_line = {line_chance}
    />
    {#if choose_polygon === true}
        <div class="c_polygon" transition:fly={{ x: 300, duration: 500 }}>
            <div class="vtx">
                <button on:click={()=>{
                    //console.log("start rect")
                    choose_polygon = false
                    if(vtx_num < 3||Number.isInteger(vtx_num) === false){
                        alert("3이상 정수가 필요합니다.")
                        return
                    }
                    draw_shape(Number(vtx_num), ischecked)
                }}>다각형</button>
                <input type="checkbox" bind:checked={ischecked} 
                on:mouseover={()=>{
                    bt_info = "도형 색상 채우기 선택"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "도형 색상 채우기 선택"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
                <input type="number" class="vtx_in" bind:value={vtx_num} min="3" 
                on:mouseover={()=>{
                    bt_info = "각이 몇 개인지 선택"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "도형 색상 채우기 선택"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
            </div>
            <div>
                <button>정삼각형</button>
            </div>
            <div>
                <button>직사각형</button>
            </div>
            <div>
                <button>정사각형</button>
            </div>
            <div>
                <button>평행사변형</button>
            </div>
            <div>
                <button>정오각형</button>
            </div>
            <div class="ltx">
                <button on:click={()=>{
                    console.log("start line")
                    choose_polygon = false
                    draw_line(2, point_line, s_line)

                }}>선</button>             
                <input type="radio" name="pen_end" checked bind:group={s_line} value="nl"
                on:mouseover={()=>{
                    bt_info = "선 끝 네모나게"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "선 끝 네모나게"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
                <input type="radio" name="pen_end" bind:group={s_line} value="ml"
                on:mouseover={()=>{
                    bt_info = "선 끝 두께만큼 늘리기"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "선 끝 두께만큼 늘리기"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
                <input type="radio" name="pen_end" bind:group={s_line} value="rl"
                on:mouseover={()=>{
                    bt_info = "선 끝 동그랗게"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "선 끝 동그랗게"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
                <input type="number" bind:value={point_line} class="p_in"
                on:mouseover={()=>{
                    bt_info = "점선간격 설정"
                    infoVisible = true
                }}
                on:focus={()=>{
                    bt_info = "점선간격 설정"
                    infoVisible = true
                }}
                on:mouseleave={()=>{
                    bt_info = ""
                    infoVisible = false
                }}>
            </div>
            <div class="button_info" style="opacity: {infoVisible ? 1 : 0};">{bt_info}</div>
            
        </div>
    {/if}
</div>

<style>
    button {
        cursor: pointer;
    }

    .content {
        width: 100%;
    }

    .top_b {
        width: 100vw;
        background-color: gray;
        height: 10vh;
    }

    .r_pallet {
        left: 35%;
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

    .c_polygon {
        position: absolute;
        right: 0;
        display: flex;
        flex-direction: column;
        height: 70vh;
        width: 30vw;
        background-color: aqua;
        align-items: flex-start;
        text-align: center;
        top: 20vh;
        left: 70vw;
    }

    .c_polygon > div {
        display: flex;
        align-items: center;
        width: 100%;
        margin-top: 3px;
        height: 40px;
        background: linear-gradient(25deg, aqua 20%, rgb(0, 192, 192));
    }

    .c_polygon > div > button {
        height: 60%;
        margin-left: 3px;
        border: none;
        background: none;
        color: white;
        transition: 0.5s;
    }

    .c_polygon > div > button:hover {
        transition: 0.5s;
        scale: 1.05;
        color: rgb(0, 104, 104);
    }
    
    .c_polygon > div > input {
        height: 60%;
    }

    .c_polygon > div > input:hover {
        cursor: pointer;
    }

    .vtx {
        display: flex;
        flex-direction: row;
        height: 20px;
    }

    .vtx_in {
        width: 40px;
    }

    .c_polygon > .button_info {
        color: white;
        width: 100%;
        height: 40px;
        font-size: 13px;
        background: linear-gradient(25deg, rgb(0, 192, 192) 80%, aqua);
        box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        transition: opacity 0.5s ease-in-out;
        max-width: 200px;
        align-items: center;
        position: sticky;
        bottom: 0;
        margin-top: auto;
    }

    .ltx > .p_in {
        width: 40px;
    }
</style>