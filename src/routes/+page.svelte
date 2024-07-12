<script lang="ts">
    import { goto } from "$app/navigation";

    let img_level:number = 0;
    let student_code:string = "";
    let password:string = "";
    let name:string = "";
</script>
<div class="body">
    <div class="app_name">
        <img src="" alt="">
        <h1>Gojan 이름</h1>
    </div>
    <div class="content_box">
        <div class="ribon"></div>
        
        {#if img_level === 0}
            <div class="content">
                <div class="side_b no_button">
                    <button>&lt;</button>
                </div>
                <div class="img_box">
                    <img src="./이상해씨.webp" alt="로드 안될시 재시작해주세요">
                </div>
                <div class="side_b">
                    <button on:click={()=>{
                        img_level++
                    }}>&gt;</button>
                </div>
            </div>
        {:else if img_level === 1}
            <div class="content">
                <div class="side_b">
                    <button on:click={()=>{
                        img_level--
                    }}>&lt;</button>
                </div>
                <div class="img_box">
                    <img src="./파이리.jpg" alt="로드 안될시 재시작해주세요">
                </div>
                <div class="side_b">
                    <button on:click={()=>{
                        img_level++
                    }}>&gt;</button>
                </div>
            </div>
        {:else if img_level === 2}
            <div class="content">
                <div class="side_b">
                    <button on:click={()=>{
                        img_level--
                    }}>&lt;</button>
                </div>
                <div class="img_box">
                    <img src="./꼬부기.gif" alt="로드 안될시 재시작해주세요">
                </div>
                <div class="side_b">
                    <button on:click={()=>{
                        img_level++
                    }}>&gt;</button>
                </div>
            </div>
        {:else if img_level === 3}
            <div class="content">
                <div class="side_b">
                    <button on:click={()=>{
                        img_level--
                    }}>&lt;</button>
                </div>
                <div class="login_box">
                    <div class="login">
                        <div class="info">
                            <div>-로그인 안해도 가능은 함</div>
                        </div>
                        <button on:click={()=>{
                            
                        }}>로그인 없이 사용</button>
                    </div>
                    <div class="login">
                        <div class="info">
                            <div>-로그인 하면 좋은 일이 생겨요</div>
                            <div>-3분마다 자동저장 기능 있음</div>
                            <div>-필기 공유 이용 가능</div>
                            <div>-게시판 이용 가능</div>
                            <div>-스터디 플래너 이용 가능</div>
                        </div>
                        <button on:click={()=>{
                            img_level++
                        }}>로그인 후 사용</button>
                    </div>
                </div>
                <div class="side_b no_button">
                    <button>&gt;</button>
                </div>
            </div>
        {:else if img_level === 4}
        <div class="content">
            <div class="side_b">
                <button on:click={()=>{
                    img_level--
                }}>&lt;</button>
            </div>
            <div class="login_tap">
                <div class="logo">
                    <img src="./gojan.png" alt="이미지 로드 실패">
                </div>
                <input type="text" bind:value={student_code} placeholder="학생 코드를 입력">
                <input type="text" bind:value={password} placeholder="비밀 번호를 입력">
                <input type="text" bind:value={name} placeholder="닉네임을 입력, 나중에 수정 가능">
                <button on:click={()=>{
                    console.log(student_code, password, name)
                    if(student_code === ""){
                        alert("학생 코드를 입력하세요")
                    }else if(password === ""){
                        alert("비밀 번호를 입력하세요")
                    }else if(name === ""){
                        alert("닉네임을 입력하세요")
                    }else {
                        alert("문제 없음")
                        goto("/ability/main")
                    }
                }}>로그인 또는 등록하기</button>
            </div>
        </div>
        {/if}
        <div class="ribon"></div>
    </div> 
</div>

<style>
    .content_box {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .app_name {
        text-align: center;
    }

    .content {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        width: 60vw;
        height: 80vh;
    }

    .side_b {
        display: flex;
        align-items: center;
    }

    .no_button > button{
        visibility: hidden;
    }

    .img_box {
        display: flex;
        align-items: center;
        background-color: rgb(7, 179, 253);
    }

    .img_box>img {
        width: 40vw;
        height: 70vh;
    }

    .login_box {
        display: flex;
        flex-direction: row;
        width: 40vw;
    }

    .login {
        display: flex;
        flex-direction: column;
        
    }

    .login > div {
        width: 20vw;
        height: 65vh;
        margin-top: 0px;
        margin-bottom: 0px;
        padding-right: 0px;
    }

    .login > button {
        width: 20vw;
        height: 5vh;
        border: 1px solid rgba(7, 179, 253, 0.5);
    }

    .info {
        height: 65vh;
        width: 20vw;
        background-color: rgb(7, 54, 110);
        color: white;
        /*border: 1px solid rgba(7, 179, 253, 0.5);*/
    }

    .login > button:hover {
        transition-duration: 1s;
        cursor: pointer;
        background-color: rgba(7, 179, 253, 0.5);
        
        
    }

    .login_tap {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        height: 65vh;
        width: 40vw;
        background-color: rgb(7, 54, 110);
    }

    .login_tap > .logo {
        height: 40vh;
        border: 1px solid rgba(7, 179, 253, 0.5);
        border: none;
    }

    .logo > img {
        width: 30vw;
    }

    .login_tap > input {
        margin-bottom: 2vh;
    }
</style>
