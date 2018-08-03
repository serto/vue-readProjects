<template>
    <div id='app' class="wrapper">
        <h1>{{ titleP }}</h1>

        <!-- <Search /> -->
        <FilterByLetter
            :projectsDY='allProjects'
            :handleFilter='handleFilter'
            :handleResetFilter='resetFilter'
            :handleViewFavs='viewFavs' />
        <Projects
            :projectsDY='listProjects'
            :favAddDelete='handleFavs'
        />
    </div>
</template>

<script>
    import Projects from './components/projects'
    import Search from './components/search'
    import FilterByLetter from './components/filterByLetter'

    import projects_dy from './assets/data'

    export default {
        data() {
            return {
                titleP: 'DoubleYou URL Projects',
                letter: '',
                listProjects: '',
                allProjects: '',
                favProjects: ''
            }
        },
        created() {
            this.listProjects = (this.existsFavProjects()) ? this.getFavsFromLocalStorage() : this.getAllProjects()
            this.allProjects = (this.existsFavProjects()) ? this.getAllProjectsWithFavs() : this.getAllProjects()
            this.favProjects = (this.existsFavProjects()) ? this.getFavsFromLocalStorage() : this.getAllProjects()
        },
        methods: {
            getAllProjects: function() {
                const listProjects = projects_dy.projects_dy.map( (project) => {
                    const name = project.titleProject.split('_');
                    const element = {
                        client: name[0],
                        projects: [
                            {
                                "name": name[1],
                                "urlInterna": project.urlInterna,
                                "urlCliente": project.urlCliente,
                                "favProject": false
                            }]
                        };
                    return element
                }).reduce((arrayReduce, project)=>{
                    if ((arrayReduce !== undefined || arrayReduce.length > 0) &&
                        (arrayReduce[project.client] != undefined && arrayReduce[project.client].client == project.client)) {
                        //el 0 es para coger el primer objeto del array, por ahora solo tiene un elemento - cuidado !
                        arrayReduce[project.client].projects.push(project.projects[0]);
                    } else {
                        arrayReduce[project.client] = project
                    }
                    return arrayReduce
                }, {});
                return listProjects;
            },

            handleFilter: function(letter) {
                this.letter = letter
                this.filterClients()
            },
            filterClients: function() {
                let filteredProjects = {};
                for (let client in this.allProjects) {
                    if (this.allProjects[client].client[0] == this.letter) filteredProjects[client] = this.allProjects[client];
                }
                this.listProjects = filteredProjects
            },
            resetFilter: function() {
                this.listProjects = this.allProjects
            },
            existsFavProjects: function(){
                return (localStorage.getItem('favProjects')) ? true : false
            },
            handleFavs: function(clientSelect, projectSelect, stateFav) {

                for (let client in this.allProjects) {
                    if (this.allProjects[client].client === clientSelect) {
                        for (let project in this.allProjects[client].projects ) {
                            if (this.allProjects[client].projects[project].name === projectSelect) {
                                //this.favProjects[client].projects[project].favProject = stateFav
                                this.allProjects[client].projects[project].favProject = stateFav
                            }
                        }
                    }
                }

                localStorage.setItem('favProjects', JSON.stringify(this.allProjects));
            },
            getFavsFromLocalStorage: function() {
                let takeFavs = JSON.parse(localStorage.getItem('favProjects'));
                let favsProjects = new Object();

                if (takeFavs) {
                    for (let client in takeFavs) {

                        for (let project in takeFavs[client].projects ) {

                            if (takeFavs[client].projects[project].favProject == true) {

                                const element = {
                                    client: takeFavs[client].client,
                                    projects: [
                                        {
                                            "name": takeFavs[client].projects[project].name,
                                            "urlInterna": takeFavs[client].projects[project].urlInterna,
                                            "urlCliente": takeFavs[client].projects[project].urlCliente,
                                            "favProject": takeFavs[client].projects[project].favProject
                                        }]
                                    };

                                (favsProjects[client]) ?
                                    favsProjects[client].projects.push(element.projects[0])
                                :
                                    favsProjects[client] = element
                            }
                        }
                    }
                }

                return favsProjects

            },
            viewFavs: function() {
                this.listProjects = this.getFavsFromLocalStorage()
            },
            getAllProjectsWithFavs: function() {

                const allProjectsNew = this.getAllProjects()
                let favProjects = JSON.parse(localStorage.getItem('favProjects'))

                const numAllProjects = Object.keys(allProjectsNew).length
                const numAllProjectsWithFavs = Object.keys(favProjects).length

                //mirar 3 casos
                // 1 - si hi han nous projectes a la llista
                // 2 - si hi ha menys projectes a la llista
                // 3 - si no hi han modificacions
                // Per ara sempre son iguals, es fa el 3 !!!!

                let allProjectsWithFavs = favProjects // cas 3

                /*
                if (numAllProjectsWithFavs != numAllProjects) {
                    if (numAllProjectsWithFavs < numAllProjects) { // cas 1

                        for (let client in allProjectsNew) {

                            console.log(allProjectsNew[client])
                            console.log(favProjects[client])
                            console.log(allProjectsNew[client] === favProjects[client]) sempre dona fals ja que son dues instancies diferents

                        }

                    } else { // cas 2

                    }
                }
                */

                //console.log(allProjectsWithFavs)

                return allProjectsWithFavs;
            }
        },
        components: {
            Projects,
            Search,
            FilterByLetter
        }

    }
</script>

<style lang="scss">
    body {
        color: #3c3c3c;
    }
    .wrapper {
        width: 90%;
        margin: 15px auto;
        text-align: center;
    }
    h1 {
        color: #3c3c3c;
    }

</style>
