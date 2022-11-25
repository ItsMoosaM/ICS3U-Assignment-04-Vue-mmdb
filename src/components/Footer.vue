<script>

</script>

<template>
    <div>
        <div id="footer" class="grid-col-span-full">
            <footer>
                <br>
                <p id="left">MMDB | Moosa Movie Database</p>
                <p id="right">Created Using The <a target="_blank" href="https://developers.themoviedb.org/3/getting-started/introduction">TMDB API</a></p>
                <br>
            </footer>
        </div>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Rubik+Bubbles&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Lilita+One&display=swap');


#footer {
    display: block;
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    text-align: center;
    padding-bottom: 5px;
    clear: both;
}

#left {
    text-align: left;
    justify-content: left;
    float: left;
    padding-left: 2%;

}
#right {
    text-align: right;
    justify-content: right;
    float: right;
    padding-right: 2%;
}

</style>