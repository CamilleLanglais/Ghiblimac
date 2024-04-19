<template>
    <div class="sort">
        <button @click="sortByTitle()" :class="{ 'active': sortCriteria === 'title' }" >Sort by title</button>
        <button @click="sortByDate()" :class="{ 'active': sortCriteria === 'date' }">Sort by release date</button>
        <button @click="sortByDirector()" :class="{ 'active': sortCriteria === 'director' }">Sort by director</button>
        <button @click="sortByTime()" :class="{ 'active': sortCriteria === 'time' }">Sort by movie length</button>
    </div>
</template>

<script>
    export default {
        name: 'sortGhibli',
        props: {
            ghibliData: {
                type: Array,
                required: true
            }
        },
        data() {
            return {
                sortCriteria: null,
                sortOrder: 'asc',
            }
        },
        methods: {
            sortByTitle(){
            this.ghibliData.sort((a, b) => {
                    if (this.sortOrder === 'asc') {
                        return a['title'].localeCompare(b['title']);
                    } else {
                        return b['title'].localeCompare(a['title']);
                    }
                });

                this.sortCriteria = 'title';
                localStorage.sort = 'title';
        },
        sortByDirector(){
            this.ghibliData.sort((a, b) => {
                if (a.director && b.director) {
                    return a.director.localeCompare(b.director);
                } else {
                    return 0;
                }
                });
                
                
                this.sortCriteria = 'director';
                localStorage.sort = 'director';

        },
        sortByDate(){
            this.ghibliData.sort((a, b) => {
                if (a.release_date && b.release_date) {
                    return new Date(a.release_date) - new Date(b.release_date);
                } else {
                    return 0;
                }
                });
                
                this.sortCriteria = 'date';
                localStorage.sort = 'date';
        },
        sortByTime(){
            this.ghibliData.sort((a, b) => {
                    if (a.running_time && b.running_time) {
                    const runningTimeA = a.running_time.includes('min') ? parseInt(a.running_time) : parseInt(a.running_time) * 60;
                    const runningTimeB = b.running_time.includes('min') ? parseInt(b.running_time) : parseInt(b.running_time) * 60;
                    return runningTimeA - runningTimeB;
                } else {
                    return 0;
                }
                });

            this.sortCriteria = 'time';
            localStorage.sort = 'time';
        }
    }
};
</script>
