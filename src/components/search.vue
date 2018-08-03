<template>
    <div class="searchBox">
        <form>
            <input
                type="text"
                v-model="search_project"
                placeholder="que estas buscando ?"
                @input="handleChange"
            />
            <!--
                @input per cada cop que sescriu algo nou
                @change per cada cop que escribim i sortim del input
            -->
        </form>
        <p>Valor Input: {{ search_project }}</p>

    </div>
</template>


<script>

    export default {
        name: 'Search',
        data () {
            return {
                search_project: ""
            }
        },
        methods: {
            handleChange: function() {
                console.log('update');
            }
        }
    }

</script>


<style lang="scss">
    .searchBox {
        width: 90%;
        margin: 12px auto 24px;

        input {
            width: 96%;
            padding: 8px 2%;
        }

    }

</style>
