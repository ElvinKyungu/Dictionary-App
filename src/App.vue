<script>
    export default{
        data(){
            return{
                isSearch: true,
                isSearching: false,
                isLoading: false,
                word: '',
                results: [],
                textInfo: true
            }
        },
        methods: {
            fetchData(){
                this.textInfo = false;
                this.isLoading = true;
                this.isSearching = true;
                console.log(this.word);
                console.log('fetchData');
                let url = `https://api.dictionaryapi.dev/api/v2/entries/en/${this.word}`;
                    fetch(url)
                    .then(response => response.json())
                    .then(result =>{
                        this.results = result;
                        this.isSearch = false;
                        this.isLoading = false;
                        console.log(result)
                    }).catch(() =>{

                });
            },
            closeSearch(){
                this.isSearching = false;
                this.textInfo = true;
                this.word = '';
            }
        },
    }
</script>

<template>
    <div class="wrapper">
        <header>Dictionary App</header>
        <div class="search">
            <input 
                type="text" 
                v-model="word"  
                @keyup.enter="fetchData" 
                placeholder="Search a word" 
                required spellcheck="false"
            >
            <i class="fas fa-search"></i>
            <span class="material-icons" @click="closeSearch">close</span>
        </div>
        <div class="logique-search-and-nosearch">
            <p class="info-text" v-if="textInfo">Type any existing word and hit enter to get meaning, example, synonyms, etc.</p>
            <p class="info-text" v-else></p>
        </div>
        <div class="content-result" v-if="isSearching">
            <p 
                class="info-text" 
                v-if="isLoading"
            >
                <b>{{ word }}</b> word search
            </p>
            <ul v-if="isLoading === false && results.length > 0">
                
                <div class="content">
                    <li class="word">
                        <div class="details">
                            <span >phonetic {{ results[0].phonetics[0].text}}</span>
                        </div>
                    </li>
                    <li class="meaning">
                        <div class="details">
                            <p>Meaning</p>
                            <span>{{ results[0].meanings[0].definitions[0].definition }}</span>
                        </div>
                    </li>
                    <li class="example" v-if="results[0].meanings[0].definitions[0].example">
                        <div class="details">
                        <p>Example</p>
                        <span>{{ results[0].meanings[0].definitions[0].example }}</span>
                        </div>
                    </li>
                    <li class="Synonyms" v-if="results[0].meanings[0].synonyms.length > 0">
                        <div class="details">
                            <p>Synonyms</p>
                            <span v-for="(item, index) in results[0].meanings[0].synonyms" :key="index">
                                {{ item +'  ' }}
                            </span>
                        </div>
                    </li>
                </div>
            </ul>
        </div>
    </div>
</template>

<style scoped>

</style>
