<template>
    <div class="project">
        <div class="project_title">
            <!-- p>{{ project.favProject }}</p><br />
            <p>{{ isChecked }}</p><br / -->
            <h3>{{ project.name }}</h3>
            <input type="checkbox" @click="handleClickFav()" v-model="isChecked"/>
        </div>
        <p>Url Interna: <a :href="project.urlInterna" target="_blank">{{ project.urlInterna }}</a></p>
        <p>Url Cliente: <a :href="project.urlCliente" target="_blank">{{ project.urlCliente }}</a></p>
    </div>
</template>


<script>

    export default {
        name: 'Project',
        props: ['project', 'client', 'favAddDelete'],
        data () {
            return {
                isChecked: this.project.favProject
            }
        },
        methods: {
            handleClickFav: function() {
                //console.log('pasa !!')
                this.isChecked = !this.isChecked
                this.favAddDelete(this.client, this.project.name, this.isChecked);
            }
        }
    }

</script>

<style lang="scss">
    .project {
        width: 23%;
        padding: 1%;
        text-align: left;
        @media screen and (max-width: 750px) {
            width: 48%;
        }

        &_title {
            display: flex;

            input {
                margin: 3px 0 0 8px;
            }
        }
    }
    h3 {
        text-align: left;
        margin-bottom: 12px;
    }
    p {
        margin-bottom: 4px;
    }
</style>
