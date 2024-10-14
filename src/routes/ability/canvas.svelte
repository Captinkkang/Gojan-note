<script lang="ts">
    import { onDestroy, onMount } from "svelte";

    export let width = 500
    export let height = 500
    export let color = '#333'
    export let backcolor = '#fff'
    export let line:number = 3
    export let draw:[number, boolean, boolean] = [0, false, false]

    let canvas:HTMLCanvasElement;
    let ctx:CanvasRenderingContext2D|null;
    let drawing = false;
    let start:{ x:number, y:number};

    let t:number, l:number
    let canvasWidth = 0
    let canvasHeight = 0

    onMount(()=>{
        ctx = canvas.getContext('2d')
        ctx!.lineWidth = line

        handleSize()
        resizeCanvas()
        
        const resizeObserver = new ResizeObserver(()=>{
            resizeCanvas()
            handleSize()
        })

        resizeObserver.observe(document.body)

        /*onDestroy(()=>{
            resizeObserver.disconnect()
        })*/
    })
    //아래는 변수
    let coor_arr:[number[]] = [[0,0]]

    //아래는 함수
    const resizeCanvas = () => {
        const availableHeight = window.innerHeight * 0.7; // 100vh - (탑바 10vh + 바텀바 10vh) = 80vh
        const availableWidth = window.innerWidth;

        canvasWidth = availableWidth;
        canvasHeight = availableHeight;

        canvas.width = canvasWidth;
        canvas.height = canvasHeight;

        ctx!.lineWidth = line;
        ctx!.strokeStyle = color;
        ctx!.fillStyle = backcolor;
        ctx!.clearRect(0, 0, canvas.width, canvas.height);  // 캔버스 초기화
    };

    const handleStart = (({ offsetX: x, offsetY: y}: { offsetX: number, offsetY: number}) => {
        if(color === backcolor) {
            ctx!.clearRect(0, 0, width, height)
        } else {
            drawing = true
            start = { x, y }
        } 
    })

    const handleEnd = () => {
        drawing = false
    }

    const handleMove = (({ offsetX: x1, offsetY: y1 }: { offsetX: number, offsetY:number}) => {
        if(!drawing) return

        const { x, y } = start
        ctx?.beginPath()
        ctx?.moveTo(x, y)
        ctx?.lineTo(x1, y1)
        ctx?.closePath()
        ctx?.stroke()

        start = { x : x1, y: y1}
    })

    const handleSize = () => {
        const { top, left } = canvas.getBoundingClientRect()
        t = top
        l = left
    }

    const get_coordinate = (e:MouseEvent) =>{
        const coordinateX = e.clientX - ctx!.canvas.offsetLeft
        const coordinateY = e.clientY - ctx!.canvas.offsetTop
        if(draw[0] !== 0){
           coor_arr.push([coordinateX, coordinateY]) 
        }
        //console.log(coor_arr)

        if(draw[0] !== 0&&coor_arr.length > draw[0]){
            let urr = []
            for(let i=0; i<draw[0]; i++){
                urr.push(coor_arr[coor_arr.length - (i+1)])
            } 
            ctx?.beginPath()
            ctx?.moveTo(urr[0][0], urr[0][1])
            for(let i=1; i<draw[0]; i++){
                ctx?.lineTo(urr[i][0], urr[i][1])
            }
            ctx!.fillStyle = color
            
            ctx?.fill()
            //뒷정리
            urr = []
            coor_arr = [[0,0]]
            draw[0] = 0
            console.log("end rect")
        }
    }

    //마우스 커서 아래에 원 하나 그리기, 크기 조절에 실시간 영향을 반영
    let mousecircle:HTMLDivElement;
    function handleMouseMove(event:MouseEvent) {
        const rect = canvas.getBoundingClientRect();
        const mouseX = event.clientX - rect.left;
        const mouseY = event.clientY - rect.top;

        // ctx.lineWidth는 선의 두께와 동일하게 원의 크기를 설정
        const circleDiameter = ctx!.lineWidth;

        // 마우스 위치에서 원의 중심이 마우스 커서 위치에 맞도록 조정
        mousecircle.style.left = `${mouseX - circleDiameter / 2}px`;
        mousecircle.style.top = `${(mouseY - circleDiameter / 2)+130}px`;
        mousecircle.style.width = `${circleDiameter}px`;
        mousecircle.style.height = `${circleDiameter}px`;
    }
    
    $:{ 
        if(ctx) {
            ctx.strokeStyle = color
            ctx!.lineWidth = line
        }
        
        //mousecircle.style.borderColor = color

    }

    //8월 30 문제 상황, 위 주석 코드 문제 생김, style객체를 못읽는 에러, 
    //그 외 기능구현: 누르면 커서 아래 원 채우기, 위 주석 코드에 색 바꾸면 커서 아래 원 테두리 색 바꾸는 기능
    //9월 말까지 필수 구현 기능: 도형 삽입, 이미지 삽입, 그래프 삽입?-보류, 텍스트 삽입, 확대 기능...(진짜 모르겠음..)
    //그래프 기능은 지오지브라 api 사용, 아직 실현가능까지는 모름, 유료일 수도 있어서

    //10월 말까지 필수 구현 기능: 로그인 DB 및 로그인, 파이어베이스까지는 필요 없음, 대충 관리하셈...
    //11월 말까지 필수 구현 기능: DB 구현 더 하기(노트 DB 중점, 파일 구조)
    //12월 초까지 : 구현 안된거 어떻게든 마무리, 앱 배포가 되면 좋겠지만 안되면 내년에 출시할 것 preview로 올려,
    //애초에 퀄리티 따질거였음 이따구로 안했음

    //도형 삽입 기능 구현
    //도형 삽입을 누르면 동그라미가 네모나게 바뀜
    //->드래그 시작 좌표와 끝나는 지점을 찾아서 그림을 그린다.

    //10/13 응 중간도 ㅈ됐고 동아리 활동도 ㅈ됐어 ㅋㅋ
    //일단 그림판 기능 완성하고 낙서게시판으로 사용하든 뭘로 하든 일단 그림판은 완성해야함 ㅇ

</script>

<canvas
    {width}
    {height}
    style="background-color: {backcolor}"
    bind:this={canvas}
    on:mousedown={(e)=>{
        handleStart(e)
        if(draw[1] === true){
            for(let i = 0; i < draw[0]; i++){
                
            }
        }
    }}
    on:touchstart={e => {
        const { clientX, clientY } = e.touches[0]
        handleStart({
            offsetX: clientX - l,
            offsetY: clientY - t
        })
    }}
    on:mouseup={handleEnd}
    on:touchend={handleEnd}
    on:mouseleave={handleEnd}
    on:mousemove={(e)=>{
        handleMove(e)
        handleMouseMove(e)
        }}
    on:touchmove={e => {
        const { clientX, clientY } = e.touches[0]
        handleMove({
            offsetX: clientX - l,
            offsetY: clientY - t
        })
    }}
    on:click={(e)=>{
        get_coordinate(e)
    }}
/>
<div class="mousecircle" bind:this={mousecircle}></div>

<style>
    canvas {
        cursor: none;
    }

    .mousecircle {
        position: absolute;
        width: 20px;
        height: 20px;
        background-color: transparent;
        border: 2px solid black;
        border-radius: 50%;
        pointer-events: none;
    }
</style>