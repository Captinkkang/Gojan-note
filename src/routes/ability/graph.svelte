<script lang="ts">
    import { onMount } from "svelte";
    import {evaluate, exp, or} from "mathjs"
  import { scale } from "svelte/transition";
    export let isdark = false
    export let graph_text = ""

    let canvas: HTMLCanvasElement;
    let ctx: CanvasRenderingContext2D | null = null;
    let isDarkMode = false;

    const GRID_SIZE = 20; // 격자 간격을 20으로 설정

    let expression = ""; //x값에 대한 식

    let graph_cor:{x:number, y:number}[][] = [];

    onMount(() => {
      ctx = canvas.getContext('2d');
      if (ctx) {
        ctx.fillStyle = '#fff';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        drawGrid(canvas.width / 100, '#ddd');
        drawAxes('#000');
        drawLabels('#000');
        /*ctx.beginPath();
        ctx.moveTo(0,300);
        ctx.lineTo(300,0);
        ctx.stroke();
        console.log("이건 되냐 씨발 새끼도노")*/
      }
    });

  const drawGrid = (scale: number, color: string) => {
    if (!ctx) return;

    ctx.strokeStyle = color;
    ctx.lineWidth = 0.5;

    const halfCanvasWidth = canvas.width / 2;
    const halfCanvasHeight = canvas.height / 2;

    // x 방향의 격자 그리기
    for (let x = -50; x <= 50; x+=5) {
      const xPos = halfCanvasWidth + x*scale; // x 좌표 계산
      ctx.beginPath();
      ctx.moveTo(xPos, 0);
      ctx.lineTo(xPos, canvas.height);
      ctx.stroke();
    }

    // y 방향의 격자 그리기
    for (let y = -50; y <= 50; y+=5) {
      const yPos = halfCanvasHeight - y*scale; // y 좌표 계산
      ctx.beginPath();
      ctx.moveTo(0, yPos);
      ctx.lineTo(canvas.width, yPos);
      ctx.stroke();
    }
  };  

  const drawAxes = (color: string) => {
    if (!ctx) return;

    ctx.strokeStyle = color;
    ctx.lineWidth = 1;

    // x축 그리기
    ctx.beginPath();
    ctx.moveTo(0, canvas.height / 2);
    ctx.lineTo(canvas.width, canvas.height / 2);
    ctx.stroke();

    // y축 그리기
    ctx.beginPath();
    ctx.moveTo(canvas.width / 2, 0);
    ctx.lineTo(canvas.width / 2, canvas.height);
    ctx.stroke();
  };  

    /*const drawGrid = (scale: number, color: string) => {
      if (!ctx) return;

      ctx.strokeStyle = color;
      ctx.lineWidth = 0.5;

      for (let x = -canvas.width / 2; x <= canvas.width / 2; x += scale) {
        const xPos = (canvas.width / 2) + x;
        ctx.beginPath();
        ctx.moveTo(xPos, 0);
        ctx.lineTo(xPos, canvas.height);
        ctx.stroke();
      }

      for (let y = -canvas.height / 2; y <= canvas.height / 2; y += scale) {
        const yPos = (canvas.height / 2) - y;
        ctx.beginPath();
        ctx.moveTo(0, yPos);
        ctx.lineTo(canvas.width, yPos);
        ctx.stroke();
      }
    };

    const drawAxes = (color: string) => {
      if (!ctx) return;

      ctx.strokeStyle = color;
      ctx.lineWidth = 0.5;

      ctx.beginPath();
      ctx.moveTo(0, canvas.height / 2);
      ctx.lineTo(canvas.width, canvas.height / 2);
      ctx.stroke();

      ctx.beginPath();
      ctx.moveTo(canvas.width / 2, 0);
      ctx.lineTo(canvas.width / 2, canvas.height);
      ctx.stroke();
    };*/

    /*const drawLabels = (color: string) => {
      if (!ctx) return;

      ctx.font = '10px Arial';
      ctx.fillStyle = color;

      for (let x = -10; x <= 10; x++) {
        const xPos = (canvas.width / 2) + x * GRID_SIZE;
        ctx.fillText(x.toString(), xPos - 5, canvas.height / 2 + 15);
      }

      for (let y = -10; y <= 10; y++) {
        const yPos = (canvas.height / 2) - y * GRID_SIZE;
        if (y !== 0) {
          ctx.fillText(y.toString(), canvas.width / 2 + 5, yPos + 5);
        }
      }
    };*/

  const drawLabels = (color: string) => {
    if (!ctx) return;

    const GRID_SIZE = canvas.width / 16; // 격자 간격
    const centerX = canvas.width / 2;   // 캔버스 중앙 x좌표
    const centerY = canvas.height / 2;  // 캔버스 중앙 y좌표

    ctx.font = '10px Arial';
    ctx.fillStyle = color;

    // x축 라벨
    for (let x = -50; x <= 50; x+=10) { // x축 라벨 개수 조정 (-8~8로 맞춤)
      const xPos = centerX + x * (canvas.width / 100);
      if (x !== 0) { // 0을 제외하고 라벨 표시
        ctx.fillText(x.toString(), xPos - 5, centerY + 15);
      }
    }

    // y축 라벨
    for (let y = -50; y <= 50; y+=10) { // y축 라벨 개수 조정 (-8~8로 맞춤)
      const yPos = centerY - y * (canvas.width / 100);
      if (y !== 0) { // 0을 제외하고 라벨 표시
        ctx.fillText(y.toString(), centerX + 5, yPos + 5);
      }
    }
  };

    const toggleMode = () => {
      if (ctx) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        if (isDarkMode) {
          ctx.fillStyle = '#000';
          ctx.fillRect(0, 0, canvas.width, canvas.height);
          drawGrid(canvas.width / 100, '#444');
          drawAxes('#fff');
          ctx.fillStyle = '#fff';
          drawLabels('#fff');
        } else {
          ctx.fillStyle = '#fff';
          ctx.fillRect(0, 0, canvas.width, canvas.height);
          drawGrid(canvas.width / 100, '#ddd');
          drawAxes('#000');
          ctx.fillStyle = '#000';
          drawLabels('#000');
        }
      }
    };

    //1. 받은 수식을 이용하기 알맞게 변환하는 함수
    const change_exp = (e:string) => {
      //1. 변수 처리, 만약 식에서 알파벳을 찾아서 그 종류가 2개 이상이라면 틀렸다고 반환
      //2. 해당 변수를 앞에 띄어쓰기가 있냐 없냐를 통해 "* 변수" 또는 " * 변수"로 대체
      //3. 해당 배열을 문자열로 고쳐서 expressgrion 변수에 대입
      let arr = e.split('')
      console.log(arr)
      let brr = []
      for(let i=2; i<arr.length; i++){
        brr.push(arr[i])
      }
      
      console.log(brr.join(""), "이게 니 측두엽이니?")
      expression = brr.join("");
      //-----------여기까지는 y = 없애는 코드
      let alp = expression.match(/[a-zA-Z]/g);
      if(alp === null)return

      let uniqu_arr:string[] = []; 
      alp.forEach((element)=>{
        if(!uniqu_arr.includes(element)){
          uniqu_arr.push(element);
        }
      })
      console.log(uniqu_arr, "일단 만든 배열임")
      let vname = uniqu_arr[0]
      if(uniqu_arr.length !== 1){
        console.log("잘못된 수식입니다.");
      }else {
        console.log("일단 1차 목표 달성");
        //변수/계수 곱하기 기호+괄호 앞에 곱하기 기호
        //console.log(e.split("").length);
        let origin_arr = e.split("")
        console.log(origin_arr)
        expression = e.replace(/(\d)([a-zA-Z])/g,'$1*$2').replace(/(\d)(\()/g,'$1*$2')
        expression = expression.replace(/([^\s])([\+\-\*/])/g,'$1 $2').replace(/([\+\-\*/])([^\s])/g,'$1 $2');
        console.log(expression,"중간 점검");
        //-50.000~50.000
        let cor = []
        for(let x=-50; x<=50; x+=0.001){
          x = Number(x.toFixed(3))

          const variables:{ [key:string]: number} = {};
          variables[vname] = x;
          const y = evaluate(expression, variables);
          cor.push({ x, y });
        }
        graph_cor.push(cor)
        console.log(cor, "좌표")
      }
    }

    const draw_garph = (arr:{x:number, y:number}[][]) =>{
      if(!ctx) return;
      
      ctx.strokeStyle = `#${Math.floor(Math.random() * 0xffffff).toString(16).padStart(6, '0')}`;
      ctx.fillStyle = '#fff';
      ctx.lineWidth = 1;
      console.log(ctx!.strokeStyle);

      const last_cor = arr[arr.length-1];
      let canvas_cor = []

      for(let i=0; i<last_cor.length; i++){
        let { x, y } = last_cor[i]
        canvas_cor.push({x:x*3+150, y:150-y*3})
      }
      console.log(canvas_cor, "빰빠빠빰 씨발")

      

      ctx!.beginPath();
      for(let i=0; i<last_cor.length; i++){
        const { x, y } = canvas_cor[i]
        //console.log(x,y)
        //0, 0=> 150, 150
        //-7,-14=>
        
        
        if (i === 0) {
          // 첫 번째 점으로 이동
          ctx!.moveTo(x, y);
        } else {
          // 나머지 점들을 연결
          ctx!.lineTo(x, y);
        } 
      }
      ctx!.stroke();
    }
    //=> 문제점, draw 클릭 시 한 번만 실행+ 중복 처리 과정 배열 잘못 만듦
    //=>문제점 해결은 모르겠다 솔직히 뭐가 문젠지 아직은 모르겠음, 
    //일단 현재 문제는 그래프 2번 못 그림, 그래프가 그려는 지는데 그게 정확한 그래프 인지는 모름

    //2. 수식 변환 함수
    //3. 변환 수식을 통해 값을 얻어내서 배열에 저장하는 함수
    //4. 배열에 저장된 값을 캔버스에 그리는 함수
    //=>다 만들긴 했음
    

    $: {
        console.log(graph_text)
        isDarkMode = isdark
        toggleMode()
        if(graph_text !== ""){
          change_exp(graph_text)
          draw_garph(graph_cor)
          graph_text = ""
        }
    } 
    
</script>
  
<canvas bind:this={canvas} width="300" height="300" on:click={(e)=>{
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  console.log(`캔버스 좌표: (${x}, ${y})`);
}}></canvas>