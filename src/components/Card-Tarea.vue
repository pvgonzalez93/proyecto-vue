<template>
    <div class="contenedor">
        <div class="centrado">
                     <template>
                <v-row>
                    <v-col cols="12">
                        <v-card v-for="item in local" :key="item.id" class="mx-auto my-12" max-width="600"
                            :loading="loading" tile>
                            <template slot="progress">
                                <v-progress-linear color="red darken-4" height="14"></v-progress-linear>
                            </template>
                            <v-card-title>
                                {{ item.tarea }}
                            </v-card-title>
                            <v-divider class="mx-4"></v-divider>
                            <v-card-actions>
                                <v-btn depressed class="space--text m-3" color="red" :key="item.id"
                                    @click="borrar(item)">Borrar
                                </v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-col>
                </v-row>
            </template>

            <div v-if="this.local.length == 0">
                <v-alert prominent type="error">
                    <v-row align="center">
                        <v-col class="grow">
                            {{mensaje}}
                        </v-col>
                        </v-row>
                </v-alert>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    name: "Card-Tarea",

    data: function () {
        return {
            local: [],
            mensaje: "",
            loading: false,
            };
    },

    mounted() {
        this.tareaLocal();
    },

    methods: {
        tareaLocal() {
            if (localStorage.form) {
                this.local = JSON.parse(localStorage.getItem("form"));
                this.loading = true;

                setTimeout(() => (this.loading = false), 2000);
                this.loading = true;
            }
            if (this.local.length == 0) {
                this.mensaje = "Aquí se mostraran tus tareas!";
            }
        },

        borrar(item) {
            this.local = JSON.parse(localStorage.getItem("form"));

            for (let i = 0; i < this.local.length; i++) {
                if (this.local[i].id == item.id) {
                    let respuesta = confirm(
                        "¡Ojo! Tu tarea se borrará..."
                    );
                    if (respuesta == true) {
                        this.local.splice(i, 1);
                    }
                }
            }

            localStorage.setItem("form", JSON.stringify(this.local));

            this.tareaLocal();
        },
    },
};
</script>
  
  