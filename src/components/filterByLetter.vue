<template>
    <div>
        <ul class="filterLetters">
            <li
                v-for="letter in letterToFilter"
                @click="handleClick(letter)"
            >
                {{ letter }}
            </li>
            <li @click="handleViewFavs()">view favs</li>
            <li @click="handleResetFilter()">view all projects</li>
        </ul>
    </div>
</template>


<script>

    export default {
        name: 'FilterByLetter',
        props: ['projectsDY', 'handleFilter', 'handleResetFilter', 'handleViewFavs'],
        data () {
            return {
                letterToFilter: Object.keys(this.projectsDY).map(
                    (key, index) => { return this.projectsDY[key].client[0] }
                ).sort().reduce(
                    (arrayReduce, letter) => {
                        if (arrayReduce !== undefined && arrayReduce.length>0 && arrayReduce[letter] != letter){
                            arrayReduce[letter].push(letter)
                        }else{
                            arrayReduce[letter]=letter
                        }
                        return arrayReduce
                    }, {})
            }
        },
        methods: {
            handleClick: function(letter) {
                this.$parent.handleFilter(letter)
            }
        }
    }

</script>


<style lang="scss">

    .filterLetters {
        overflow: hidden;
        display: flex;
        margin: 40px 0;

        li {
            flex: auto;
            list-style-type: none;

            &:hover {
                color: blue;
                cursor: pointer;
                text-decoration: underline;
            }
        }
    }

</style>
