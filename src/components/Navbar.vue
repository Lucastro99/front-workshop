<template>

    <div class="navbar">
        <div class="logo">🤘</div>
        <div class="icons">
            <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30" fill="none">
                <circle cx="15" cy="15" r="15" fill="#009488"/>
                <path d="M12 20.2V9H14.08V18.44H19.936V20.2H12Z" fill="white"/>
            </svg>
        </div>
    </div>

</template>

<style>
.navbar{
    height: 70px;
    background: black;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.logo{
    font-size: 35px;
    padding-left: 20px;
}

.icons{
    padding-right: 20px;
}
</style>