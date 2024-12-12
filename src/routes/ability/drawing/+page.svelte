<script lang="ts">
    import { onMount, tick } from "svelte";
    import Canvas from "../canvas.svelte";
    import Graph from "../graph.svelte";
    import { fly } from "svelte/transition";


    let color:string = "#000000";
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
    let clear_all = false
    let event_res: string | boolean | null;
    let choose_text = false//텍스트 사이드바 펼치냐 마냐
    let wanted_text = ""
    let wanted_size = 10
    let wanted_weight = 1
    let fill_text = false
    let wanted_font = "기본"
    let text_chance:[boolean, string, number, number, boolean] = [false, wanted_text, wanted_size, wanted_weight, fill_text]
    let choose_graph = false
    //아래는 지오지브라 api 변수
    //응 직접 만들꺼야 ㅗ
    let change_light = false
    let g_graph = ""
    let send_t = ""
    let graphs:string[] = []

    const draw_shape = (n:number, c:boolean) => {
        click_chance = [n,true,c]
    }

    const draw_line = (n:number, a:number, b:string) => {
        line_chance = [n, a, b]
    }

    const rgbToHex = (r:number, g:number, b:number) => {
        return (
          "#" +
          [r, g, b]
            .map((x) => {
              const hex = x.toString(16);
              return hex.length === 1 ? "0" + hex : hex;
            })
            .join("")
        );
    }

    const handle_custom_event = (event:CustomEvent<{value:string}>) => {
        //console.log(event.detail.value, "이벤트 받았다")
        event_res = event.detail.value
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
        if(event_res === false){
            clear_all = false
            event_res = null
        }
        //console.log(change_light, g_graph)
    }
</script>

<div class="content">
    <div class="top_b">
        <div class="main_t">
            <input type="color" bind:value={color} on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
            }}>
            <button on:click={()=>{
                let answer = prompt("위 동작은 현재 작성하는 문서가 저장 되지 않습니다.\n새로고침 하시겠습니까? y/n")
                if(answer === "y"){
                    if(bright === '#fff'){bright = '#00000'}
                    else {bright = '#fff'}
                }
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
            }}>색반전</button>
            <input type="range" min="0.1" max="10.0" step="0.5" bind:value={linewidth} on:click={()=>{
                choose_text = false
                r_pallet_open = false
                choose_polygon = false
                choose_graph = false
            }}>
            <button on:click={()=>{
                r_pallet_open = false
                choose_text = false
                choose_graph = false
                if(choose_polygon === true){
                    choose_polygon = false
                }else{
                    choose_polygon = true
                }
            }}>도형</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                if(choose_graph === true){
                    choose_graph = false
                }else{
                    choose_graph = true
                }
            }}>그래프</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
            }}>이미지</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_graph = false
                if(choose_text === true)choose_text = false
                else choose_text = true
            }}>텍스트</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                clear_all = true
                choose_graph = false
            }}>전체 지우기</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
            }}>되돌리기</button>
            <button on:click={()=>{
                if(r_pallet_open===false){r_pallet_open = true}
                else{r_pallet_open = false}
                choose_polygon = false
                choose_text = false
                choose_graph = false
            }}>최근 색상</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
                let rr = Math.floor(Math.random()*255)
                let rg = Math.floor(Math.random()*255)
                let rb = Math.floor(Math.random()*255)
                color = rgbToHex(rr, rg, rb)
            }}>랜덤 색상</button>
            <button on:click={()=>{
                r_pallet_open = false
                choose_polygon = false
                choose_text = false
                choose_graph = false
                color = "#FFFFFF"
            }}>지우개</button>
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
        {clear_all}
        draw_text = {text_chance}
        on:cclear={handle_custom_event}
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

                }}>직선</button>             
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
    {#if choose_text === true}
        <div class="c_polygon c_text" transition:fly={{ x: 300, duration: 500 }}>
            <div>
                <button on:click={()=>{
                    choose_text = false
                    text_chance = [true, wanted_text, wanted_size, wanted_weight, fill_text]
                    console.log(text_chance,"4달 동안")
                }}>글씨 작성</button>
                <input type="checkbox" bind:value={fill_text}>
            </div>
            <div class="text_input">
                <input type="text" bind:value={wanted_text}>
            </div>
            <div class="size_input">
                <input type="number" min="1" bind:value={wanted_size}>
                <input type="number" min="1" bind:value={wanted_weight}>
            </div>
            <div>
                <select name="font" bind:value={wanted_font}>
                    <option value="default">기본</option>
                    <option value="굴림">굴림</option>
                    <option value="굴림체">굴림체</option>
                    <option value="궁서">궁서</option>
                    <option value="궁서체">궁서체</option>
                    <option value="돋움">돋움</option>
                    <option value="돋움체">돋움체</option>
                    <option value="바탕">바탕</option>
                    <option value="바탕체">바탕체</option>
                    <option value="휴먼엽서체">휴먼엽서체</option>
                    <option value="Viner Hand ITC">Viner Hand ITC</option>
                    <option value="Times New Roman">Times New Roman</option>
                    <option value="Symbol">Symbol</option>
                    <option value="Poor Richard">Poor Richard</option>
                    <option value="Papyrus">Papyrus</option>
                    <option value="MS Serif">MS Serif</option>
                    <option value="MS Gothic">MS Gothic</option>
                </select>
            </div>
            <div>
                <input type="color" bind:value={color}>
            </div>
            <div>
                <button>미리보기</button>
            </div>
            <div class="preview" style="color: {color};font-weight: {wanted_weight};font-size: {wanted_size}">{wanted_text}</div>
        </div>
    {/if}
    {#if choose_graph === true}
        <div class="g_pallet" transition:fly={{ x: 300, duration: 500 }}>
            <div>그래프</div>
            <div>
                <input type="text" bind:value={g_graph}>
                <button on:click={()=>{
                    if (g_graph.trim() !== "") { // 빈 값이 아닐 때만 추가
                        send_t = g_graph;
                        graphs = [...graphs, send_t]; // 배열 업데이트
                        g_graph = ""; // 입력 필드 초기화
                        console.log(send_t);
                    }
                }}>draw</button>
            </div>
            <div>
                <Graph 
                    isdark = {change_light}
                    graph_text = {send_t}
                ></Graph>
            </div>
            <div>
                <button on:click={()=>{
                    if(change_light === true){
                        change_light = false
                    }else change_light = true
                    console.log("영역전개")
                    
                }}>다크모드</button>
            </div>
            {#each graphs as i (i)}
                <div>{i}</div>
            {/each}
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

    /*.c_polygon > div > input:hover {
        cursor: pointer;
    }*/

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

    .text_input > input {
        width: 90%;
    }

    .size_input > input {
        width: 45%;
    }

    .preview {
        overflow: auto;
    }

    .g_pallet {
        position: absolute;
        right: 0;
        display: flex;
        flex-direction: column;
        height: 70vh;
        width: 50vw;
        background-color: aqua;
        align-items: flex-start;
        text-align: center;
        top: 20vh;
        left: 50vw;
    }

    .g_pallet >  div{
        background-color: inherit;
        width: 100%;
        overflow: auto;
    }
</style>