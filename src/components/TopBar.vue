<script>

</script>

<template>
    <div>
        <div id="homeTop" class="grid-col-span-full">
            <hr>
            <br>
            <a href="index.html">
                <img id="logo" src="../assets/MMDB Logo Gold.png" alt="">
            </a>
            <br>
            <hr>
        </div>
    </div>
</template>

<style scoped>
* {
    background-color: #0f0d06;
    box-sizing: border-box;
    color: #c2870c;
    padding: 0%;
    margin: 0;
    font-family: "Lilita One","Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    margin: 0%;
    display: grid;
}
#homeTop {
    background-color: #0f0d06;
    padding: 1%;
}

#logo {
    background-color: #0f0d06;
    padding: 0.5%;
    display: block;
    margin-left: auto;
    margin-right: auto;
    align-items: center;
    width: 10%;
    height: auto;
}

#logo:hover {
    transform: scale(1.1);
    transition: all linear;
}
.grid-col-span-full {
    grid-column: span 5;
}
.grid-col-span-2 {
    grid-column: span 2;
}
.grid-col-span-3 {
    grid-column: span 3;
}
</style>