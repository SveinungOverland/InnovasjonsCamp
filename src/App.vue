<template>
    <div id="app">
        <div id="grid" :text="Boolean(route.text)">
            <h1 v-if="route.text" style="font-size: 50px; text-align: center;">{{ route.text }}</h1>
            <div
                v-for="button in buttons"
                :key="button.route"
                class="button"
                @click="handleClick(button.route)"
            >
                {{ button.name }}
            </div>
        </div>
    </div>
    
</template>

<script>

export default {
    name: 'App',
    components: {
        
    },
    computed: {
        buttons() {
            return this.route.buttons ? Object.entries(this.route.buttons).map(([key, value]) => ({
                name: value.name,
                route: key
            })) : false
        },
        route() {
            const route = this.$route.path
                .split("/")
                .filter(it => it.length)
                .reduce((prev, curr) => prev.buttons[`/${curr}`], this.root)
            if (route.goto){
                this.$nextTick(() => this.changeRoute(route.goto))
            }
            return route
        }
    },
    methods: {
        handleClick(route) {
            this.changeRoute(this.$route.path + route)
        },
        changeRoute(route) {
            this.$router.push(route)
        }
    },
    data() { return {
        root: {
            buttons: {
                "/bank": {
                    name: "Vil du ha bank",
                    buttons: {
                        "/saldo": {
                            name: "Se saldo",
                            text: "Saldoen din er -23 kr! Lykke til",
                            buttons: {
                                '/ok': {
                                    name: "Ok",
                                    goto: "/"
                                }
                            }
                        },
                        "/regninger": {
                            name: "Regninger",
                        }
                    },
                },
                "/helse": {
                    name: "Helse, hva med helikkese",
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
                        "/bestill": {
                            name: "Bestill legetime",
                        }
                    }
                },
                "/taxi": {
                    name: "Taxi, fikk du fullt hus",
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

#app {

}

#grid {
    display: grid;
    padding: 0 50px;
    min-height: 100vh;
    align-content: center;
    grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
    grid-gap: 30px;
}

#grid[text="true"] {
    grid-template-columns: 1fr;
}

.button {
    cursor: pointer;
    text-align: center;
    align-content: center;
    background-color: gray;
    text-justify: center;
    font-size: 50px;
    padding: 80px 0px;
}

.button:hover {
    filter: brightness(80%);
}

</style>
