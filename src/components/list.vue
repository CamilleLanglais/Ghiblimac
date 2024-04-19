<template>
    <div id="listGhibli">
     <div v-if="test">
        <div class="detail" v-if="showDetails">
            <div class="back">
                <button @click="showList()">Return to list</button>
            </div>
            <Detail :ghibli="selectedGhibli" />
        </div>
        <div class="list" v-else>
            <div class="filters">
                <searchGhibli :ghibliData="ghibliData" :allGhibliData="allGhibliData" @filtered="updateGhibliData" ref="search"/>
                <sortGhibli :ghibliData="ghibliData" ref='sort' />
            </div>
                <div class="allCards">
                    <div v-for="ghibli in ghibliData" class="CardContent">
                        <cardGhibli
                        :ghibli="ghibli"
                        @click="showDetail(ghibli)"
                        />
                    </div>
                </div>
            </div>

        
    </div>
        <p v-else>Chargement...</p>
    </div>
</template>


<script>
import { getGhibliData } from '@/api/ghibliAPI.js';
import cardGhibli from './card.vue';
import Detail from './detail.vue';
import sortGhibli from './sort.vue';
import searchGhibli from './search.vue';

export default {
    name: 'listGhibli',
    data() {
        return {
            ghibliData: [],
            allGhibliData : [],
            test: false,
            searchQuery: '',
            sortCriteria: null,
            sortOrder: 'asc',
            showDetails: false,
            selectedGhibli: null,

        }
    },
    methods: {
        async retrieveGhibliData() {
            this.ghibliData = await getGhibliData();
            this.allGhibliData = this.ghibliData; 
            
            this.test = true;
            
            
            this.$nextTick(() => { 
                this.$refs.search.searchMovies();
                this.sortbylocalhost();
                
            });
        },
        updateGhibliData(filteredData) {
                this.ghibliData = filteredData;
                this.$nextTick(() => {
                    console.log('filter');
                    this.sortbylocalhost(); 
                });
                
        },
        sortbylocalhost(){
            if(localStorage.sort){
                    console.log(localStorage.sort);
                 switch (localStorage.sort) {
                     case 'title':
                         this.$refs.sort.sortByTitle();
                         break;
                     case 'director':
                         this.$refs.sort.sortByDirector();
                         break;
                     case 'time':
                    this.$refs.sort.sortByTime();
                         break
                     default:
                         this.$refs.sort.sortByDate();
                         break;
                     }
                } else{
                    this.$refs.sort.sortByDate();
                }
        },

        //Detail
        showDetail(ghibli) {
        this.selectedGhibli = ghibli;
        this.showDetails = true;
        },
        showList(){
            this.showDetails = false;
        }
    },
    
    mounted() {
        this.retrieveGhibliData();
        if(localStorage.searchQuery){
            this.searchQuery = localStorage.searchQuery;
        }
    },
    components : {cardGhibli, Detail, sortGhibli, searchGhibli}
}


</script>



