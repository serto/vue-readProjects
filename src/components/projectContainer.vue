<template>
    <div class="projectContainer">
        <h2>{{ client.client }}</h2>
        <div class="projectContainer_wrapper">
            <Project
                v-for="project in client.projects"
                :project="project"
                :client="client.client"
                :favAddDelete="favAddDelete"
            />
        </div>
    </div>
</template>


<script>

    import Project from './project'

    export default {
        name: 'ProjectContainer',
        props: ['client', 'favAddDelete'],
        data () {
            return {
            }
        },
        components: {
            Project
        }
    }

</script>

<style lang="scss">
    .projectContainer {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        margin-bottom: 24px;

        h2 {
            text-align: left;
            width: 100%;
        }

        &_wrapper {
            display: flex;
            width: 100%;
        }

    }

</style>
