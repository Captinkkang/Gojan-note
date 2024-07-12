<script>
    import { isMenuOpen } from "./store";
    import { fly } from "svelte/transition";

    let love = 1;
    let sub = 114;
    let writing = 4;
    let user_name = "강연우"
    let user_id = "rkddusdn"
</script>

{#if $isMenuOpen === true}
    <div class="side_bar" transition:fly={{x : 300, duration:500}}>
        <div class="top">
            <div class="exit_bar">
                <div class="user_info">
                    <div>&ensp;이름 : {user_name} 님</div>
                    <div>&ensp;id : {user_id}</div>
                </div>
                <button class="close" on:click={()=>{
                    $isMenuOpen = false
                }}>&gt;</button>
            </div>
            <div class="info_bar">
                <div class="profile">
                    <img src="../페드로 라쿤.gif" alt="이밎 로드 실패">
                </div>
                <div class="info_container">
                    <div>
                        <div>{love}</div>
                    <div>좋아요</div>
                    </div>
                    <div>
                        <div>{writing}</div>
                        <div>게시글</div>
                    </div>
                    <div>
                        <div>{sub}</div>
                        <div>구독자</div>
                    </div>
                </div>
            </div>
        </div>
        <div class="mid"></div>
        <div class="bot">
            <div>
                <div class="text">&ensp;개인 정보 설정</div>
                <div class="icon">&gt;</div>
            </div>
            <div>
                <div class="text">&ensp;환경 설정</div>
                <div class="icon">&gt;</div>
            </div>
            
        </div>
    </div>
{/if}

<style>
    .side_bar {
        z-index: 3;
        position: fixed;
        height: 100vh;
        width: 60vw;
        right: 0%;
        background-color: rgb(7,54,110);
    }

    .close {
        background-color: white;
        font-weight: bold;
        font-size: 20px;
        width: 50px;
        height: 50px;
        border-radius: 70%;
        transition: 0.5s;
        transform: scale(1.0);
    }

    .close:hover {
        cursor: pointer;
        transition: 0.5s ease-in-out;
        transform: scale(0.9);
        background-color: aliceblue;
    }

    .info_bar {
        display: flex;
        flex-direction: row;

    }

    .profile {
        border-radius: 70%;
        width: 70px;
        height: 70px;
        margin: 5px;
        overflow: hidden;
    }

    .profile > img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .info_container {
        display: flex;
        flex-direction: row;
        color: white;
        text-align: center;
        align-items: center;
    }

    .info_container > div {
        margin-right: 5px;
        font-size: 14px;
    }

    .exit_bar {
        display: flex;
        flex-direction: row;
        margin-top: 5px;
        margin-bottom: 5px;
        overflow: hidden;
    }

    .user_info {
        width: 70%;
        color: white;
    }

    .mid {
        width: 100%;
        height: 5px;
        background-color: white;
        margin-top: 5px;
        margin-bottom: 5px;
    }

    .bot {
        display: flex;
        flex-direction: column;
    }

    .bot > div {
        display: flex;
        flex-direction: row;
        margin-top: 5px;
        width: 95%;
        background-color: white;
        transition: 0.5s;
        transform: translateX(0%);
    }

    .bot > div:hover {
        cursor: pointer;
        transition: 0.5s;
        transform: translateX(5%);
        background-color: aliceblue;
    }

    .text {
        width: 90%;
    } 

    .icon {
        width: 10%;
    }
</style>