<script setup>
import { ref } from "vue";
import axios from 'axios'

// api_key= ba4adcc4706ed37650e0a813de11a08f;

let placeholderRemoved = false;

const movieSelected = ref('')
let genreTitle = ref('')
const movie = ref(null)

const getData = async (url, params) => {
    try {
        return await axios.get(url, params);
    } catch (error) {
        console.log(error);
    }
};

const getSelected = () => {
    const selectedMovie = movieSelected.value;
    console.log(selectedMovie);
    GetMovieData(selectedMovie)
}


const GetMovieData = async (selectedMovie) => {
    const movieDataID = await getData("https://api.themoviedb.org/3/search/movie", {
        params: {
            api_key: "ba4adcc4706ed37650e0a813de11a08f",
            query: selectedMovie,
        }
    });

    let movieID = movieDataID.data.results.at(0).id;
    console.log(movieID)

    movie.value = (await getData(`https://api.themoviedb.org/3/movie/${movieID}`, {
        params: {
            api_key: "ba4adcc4706ed37650e0a813de11a08f",
            append_to_response: "videos",
        }
    })).data;

    console.log(movie.value)

    const genresElement = document.getElementById('genres')

    //Removes Place Holder Text
    if (placeholderRemoved == false) {
        const placeholder = document.getElementById("selectAMoviePlaceholder");
        placeholder.remove();
        placeholderRemoved = true
    } else {
        //Do Nothing
    }
    
    genreTitle = "Genres";
    
    genresElement.innerHTML = '';
    
    let genresInfo = movie.value.genres.forEach(element => {
        let genresNames = element.name;
        console.log(genresNames)
        genresElement.innerHTML += `<li> ${genresNames} </li>`
    });
    
    console.log(genresInfo)

}
</script>

<template>
    <div>
        <div id="movieSelectionContainer" class="grid-col-span-full">
            <label id="label">Select A Movie From Below or <a href="search.html">Search</a></label>
            <br>
            <div id="selectionContainer">
                <select name="movies" id="movies" v-model="movieSelected">
                    <option value="" disabled selected id="selectAMoviePlaceholder">Select a Movie</option>
                    ,<optgroup label="Kung Fu Panda">
                        <option value="Kung Fu Panda">Kung Fu Panda</option>
                        <option value="Kung Fu Panda 2">Kung Fu Panda 2</option>
                        <option value="Kung Fu Panda 3">Kung Fu Panda 3</option>
                    </optgroup>
                    ,<optgroup label="Angry Birds">
                        <option value="Angry Birds">Angry Birds</option>
                        <option value="Angry Birds 2">Angry Birds 2</option>
                    </optgroup>
                    ,<optgroup label="Cars">
                        <option value="Cars">Cars</option>
                        <option value="Cars 2">Cars 2</option>
                        <option value="Cars 3">Cars 3</option>
                    </optgroup>
                    ,<optgroup label="Batman">
                        <option value="Batman Begins">Batman Begins</option>
                        <option value="The Dark Knight Rises">The Dark Knight Rises</option>
                    </optgroup>
                </select>
            </div>
            <br>
            <button id="getInfo" @click="getSelected()">Get Info</button>

            <br>
            <hr>
            <br>
            <br>
        </div>
        <div v-if="movie">
            <h1 id="movie_title" class="grid-col-span-full">{{ movie.title }}</h1>
            <h3 id="tagline" class="grid-col-span-full">{{ movie.tagline }}</h3>
            <img :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`" id="poster" alt=""
                class="grid-col-span-2">
            <iframe :src="`https://www.youtube.com/embed/${movie.videos.results.filter((video) => video.type === 'Trailer').at(0).key}`" frameborder="0" id="trailer" class="grid-col-span-3"></iframe>
            <br>
            <br>
            <hr class="grid-col-span-full">
            <h4 id="overview" class="grid-col-span-full">{{ movie.overview }}</h4>
            <hr class="grid-col-span-full">
            <div class="grid-col-span-2 otherInfo ">
            <br>
                <div id="genresContainer" class="grid-col-span-2">
                    <h2 id="genreTitle">{{ genreTitle }}</h2>
                    <br>
                    <ul id="genres">
                    </ul>
                    <br>
                </div>
                <br>
                <br>
                <h4 id="releaseDate" class="otherInfoNoGenre">Release Date: {{ movie.release_date }}</h4>
                <br>
                <h4 id="productionCompany" class="otherInfoNoGenre">Production Company: {{
                        movie.production_companies[0].name
                }}</h4>
                <br>
                <h4 id="movieCollection" class="otherInfoNoGenre">Collection: {{ movie.belongs_to_collection.name }}
                </h4>
            </div>
            <div class="grid-col-span-3 ">
                <br>
                <a :href="`${movie.homepage}`" id="backdropImageLink" target="_blank"><img
                        :src="`https://image.tmdb.org/t/p/w500${movie.backdrop_path}`" id="backdropImage" alt=""></a>
                <a :href="`${movie.homepage}`" id="movieWebsite" target="_blank">{{ movie.homepage }}</a>
            </div>
            <br>
            <br>
            <br>
            <br>
        </div>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Rubik+Bubbles&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Lilita+One&display=swap');
* {
    background-color: #0f0d06;
    box-sizing: border-box;
    color: #c2870c;
    display: grid;
    padding: 0%;
    margin: 0;
    font-family: "Lilita One","Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
    margin: 0%;
}

hr{
    border-color: #c2870c;
    border-width: 4px;
}

.allContainer {
    display: grid;
    gap: 1.5rem;
    grid-template-columns: repeat(5, 1fr);
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



#label {
    font-size: x-large;
    text-align: center;
    display: block;
    margin-left: auto;
    margin-right: auto;
    /* background-color: #222e2c; */
}

/* #movieSelectionContainer {
    background-color: #222e2c;
}
#selectionContainer {
    background-color: #222e2c;
} */

#getInfo {
    display: block;
    margin-left: auto;
    margin-right: auto;
}
#getInfo:hover {
    background-color: rgb(161, 115, 17);
    border: rgb(161, 115, 17);
    display: block;
    margin-left: auto;
    margin-right: auto;
    cursor: pointer;
}
#getInfo:active {
    background-color: rgb(255, 180, 18);
    border: rgb(255, 180, 18);
    display: block;
    margin-left: auto;
    margin-right: auto;
}

#movie_title {
    /* padding-bottom: 1.5rem; */
    text-align: center;
    font-size: 500px;
    grid-column: span 5;
    /* background-color: #a5a299; */
}

#tagline {
    text-align: center;
    font-size: 140%;
    padding-bottom: 1%;
}

@keyframes glowEffect {
    from {
        box-shadow: 0px 0px 28px #c2870c;
    }
    to {
        box-shadow: 0px 0px 10px #c2870c;
    }
}@keyframes glowEffectHover {
    from {
        box-shadow: 0px 0px 24px #ffbc2d;
    }
    to {
        box-shadow: 0px 0px 10px #ffbc2d;
    }
}

optgroup {
    text-align: left;
    font-weight: bolder;
    color: #0f0d06;
    background-color: #c2870c;
}
option {
    background-color: #0f0d06;
    text-align: center;
}
#movies {
    display: block;
    margin: 1% auto;
    box-shadow: 0px 0px 15px #c2870c;
    text-align: center;
    width: 45%;
    height: 100%;
    min-height: 35px;
    /* background-color: #222e2c; */
    font-size: 150%;
    border: 2px solid #c2870c;
    border-radius: 0.9rem;
    outline: none;
    appearance: none;
    grid-column: span 5;
    padding: 1%;
    /* Animation */
    animation-name: glowEffect;
    animation-duration: 1s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}
#movies:hover{
    border-color: #ffbc2d;
    color: #ffbc2d;
    animation-name: glowEffectHover;
    animation-duration: 0.2s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}

.backGrey {
    background-color: #222e2c;
}

/* #selectionContainer::after{
    content: '';
    border-width: 5px;
    border-style: solid;
    border-color: transparent;
    border-top-color: #C2870C;
    display: inline-block;
    position: relative;
    bottom: 40px;
    left: 550px;
} */

#getInfo {
    width: 30%;
    height: 5rem;
    min-height: 35px;
    color: #141006;
    background-color: #c2870c;
    border-radius: 0.2rem;
    font-size: x-large;
}

#movie_title {
    text-align: center;
    font-size: 300%;
    text-decoration: underline overline;
}

#poster {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 50%;
    height: auto;
    grid-column: span 2;
}

#trailer {
    display: block;
    margin: auto;
    min-height: 100px;
    width: 90%;
    height: 90%;
}

#overview {
    padding: 5% 5%;
    font-size: 140%;
    text-align: center;
}

#genreTitle {
    text-align: center;
    text-decoration: underline;
    font-size: 200%;
}

/* #genresContainer {
    background-color: #222E2C;
} */

#genres {
    display: block;
    padding-left: 20%;
    margin-left: auto;
    margin-right: auto;
    /* background-color: #222E2C; */
}

#genres li{
    margin: 5% 0px;
    font-size: 150%;
}

.otherInfo {
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding-left: 15%;
    /* background-color: #222E2C; */
}
.otherInfoNoGenre{
    font-size: 140%;
}

/* #releaseDate{
    background-color: #222E2C;
}
#productionCompany{
    background-color: #222E2C;
}
#releaseDate{
    background-color: #222E2C;
} */

#backdropImage {
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding-top: 5%;
    width: 75%;
    height: auto;
}

#backdropImage:hover {
    transform: scale(1.05);
}

#movieWebsite {
    display: block;
    margin-left: auto;
    margin-right: auto;
    padding-top: 5%;
    font-size: 120%;
    text-align: center;
}

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