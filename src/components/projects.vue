<template>
    <div class="projects">

        <div class="projects-wrapper">

            <h4 v-if="numFavs == 0">NO TIENES PROYECTOS FAVORITOS</h4>

            <ProjectContainer
                v-for="client in projectsDY"
                :client="client"
                :favAddDelete="favAddDelete"
            />
        </div>
    </div>
</template>


<script>

    import ProjectContainer from './projectContainer'

    export default {
        name: 'Projects',
        props: ['projectsDY', 'favAddDelete'],
        data () {
            return {
            }
        },
        components: {
            ProjectContainer
        },
        computed: {
            numFavs: function() {
                return Object.keys(this.projectsDY).length
            }
        }
    }

</script>

<style lang="scss">
    .projects {
        padding: 12px;

        &-wrapper {
            display: flex;
            flex-direction: column;
            flex-wrap: wrap;
        }
    }

</style>
