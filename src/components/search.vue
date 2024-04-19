<template>
    <form @submit.prevent="searchMovies">
        <input type="text" v-model="searchQuery" placeholder="Search for a movie">
        <button type="submit">Research</button>
    </form>
</template>

<script>
    export default {
        name: 'searchGhibli',
        props: {
            allGhibliData : {
                type: Array,
                required: true,
            },
        },
        data() {
            return {
                filteredData : [],
                searchQuery: '',
                
            }
        },
        methods: {
            searchMovies() {
            const query = this.searchQuery.trim().toLowerCase();
            this.filteredData = this.allGhibliData.filter(ghibli =>
                ghibli.title.toLowerCase().includes(query) ||
                ghibli.director.toLowerCase().includes(query) ||
                ghibli.producer.toLowerCase().includes(query)
            );

            localStorage.searchQuery = this.searchQuery;
            this.$emit('filtered', this.filteredData);
        },
        }

        

}
</script>