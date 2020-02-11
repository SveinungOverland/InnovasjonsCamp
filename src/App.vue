<template>
    <div id="app">
        <div 
            class="header"
        >
            <div
                v-for="trail in trails"
                :key="trail.name"
                class="header-button"
                @click="changeRoute(trail.route)"
            >
                <h4>{{ trail.name }}</h4>
            </div>
        </div>
        <div class="grid" :text="Boolean(route.text)">
            <h1 v-if="route.text" style="font-size: 50px; text-align: center;">{{ route.text }}</h1>
            <div
                v-for="button in buttons"
                :key="button.route" 
                class="button"
                @click="handleClick(button.route)"
            >
                <h4>{{ button.name }}</h4>
            </div>
        </div>
    </div>
    
</template>

<script>

export default {
    name: 'App',
    components: {
        
    },
    mounted() {
    },
    watch: {
        trails(newTrail) {
            console.log(newTrail)
        }
    },
    computed: {
        buttons() {
            return this.route.buttons ? Object.entries(this.route.buttons).map(([key, value]) => ({
                name: value.name,
                route: key
            })) : false
        },
        route() {
            const route = this.paths
                .reduce((prev, curr) => prev.buttons[`/${curr}`], this.root)
            if (route.goto){
                this.nextTickRoute(route)
            }
            return route
        },
        trails() {
            return this.paths.length === 0 ? [] : this.paths
                .slice(0, this.paths.length - 1)
                .reduce((prev, curr) => [...prev, {
                    name: curr.replace(/^\w/, c => c.toUpperCase()),
                    route: [prev[prev.length-1].route, curr].join("/")
                }], [{ name: "Home", route: "/" }])
        },
        paths() {
            return this.$route.path.split("/").filter(it => it.length)
        }
    },
    methods: {
        nextTickRoute(route) {
            this.$nextTick(() => this.changeRoute(route.goto))
        },
        handleClick(route) {
            this.changeRoute(this.$route.path + route)
        },
        changeRoute(route) {
            this.show = false
            this.$router.push(route)
        }
    },
    data() { return {
        show: true,
        root: {
            buttons: {
                "/bank": {
                    name: "Bank",
                    buttons: {
                        "/saldo": {
                            name: "Se saldo",
                            text: "Saldoen din er 24289 kr",
                            buttons: {
                                '/ok': {
                                    name: "Ok",
                                    goto: "/"
                                }
                            }
                        },
                    },
                },
                "/helse": {
                    name: "Helse",
                    buttons: {
                        "/legetime": {
                            name: "Neste legetime",
                            text: "Din neste legetime er kommende tirsdag",
                            buttons: {
                                "/ok": {
                                    name: "Ok",
                                    goto: "/"
                                }
                            }
                        },
                    }
                },
                "/taxi": {
                    name: "Taxi",
                    buttons: {
                        "/bestill": {
                            name: "Bestill taxi",
                            text: "Taxien er bestillt og kommer om 15 min",
                            buttons: {
                                "/ok": {
                                    name: "Ok",
                                    goto: "/"
                                }
                            }
                        },
                        "/avbestill": {
                            name: "Avbestill taxi",
                            text: "Sikker på at du vil avbestille taxien",
                            buttons: {
                                "/ja": {
                                    name: "Ja",
                                    text: "Taxien er avbestillt",
                                    buttons: {
                                        '/ok': {
                                            name: "Ok",
                                            goto: "/"
                                        }
                                    }
                                },
                                "/nei": {
                                    name: "Nei",
                                    goto: "/"
                                }
                            }
                        }
                    }
                }
            },
        }
    }},
}
</script>

<style>

* {
    margin: 0;
    padding: 0;
}

body {
    background-image: url("./assets/spikes.png");
}

#app {
    font-family: 'Roboto', sans-serif;
}

.grid {
    display: grid;
    padding: 0 50px;
    min-height: 80vh;
    align-content: center;
    justify-content: center;
    grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
    grid-gap: 30px;
}

.grid[text="true"] {
    /* grid-column: 1 / -1; */
    grid-template-columns: 1fr 1fr;
}

.grid[text="true"] *:first-child, .grid[text="true"] div:only-of-type {
    grid-column: span 2;
}

.header {
    height: 67px;
    display: grid;
    padding: 10px;
    grid-template-columns: repeat(auto-fill, 200px);
    grid-gap: 30px;
}

.header-button {
    cursor: pointer;
    text-align: center;
    align-content: center;
    align-items: center;
    align-self: stretch;
    background-color: rgba(54, 54, 51, 0.85);
    color: white;
    text-justify: center;
    font-size: 24px;
    padding: 20px 0px;
    box-shadow: 0px 4px 15px 0px rgba(0,0,0,0.75);
}

.button {
    cursor: pointer;
    text-align: center;
    align-content: center;
    align-items: center;
    align-self: stretch;
    background-color: rgba(54, 54, 51, 0.85);
    color: white;
    text-justify: center;
    font-size: 50px;
    padding: 80px 0px;
    border-radius: 5px;
    box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.75);
}

.button:hover {
    filter: brightness(80%);
}

</style>
