<template>
    <div class="container">
        <h1>Agendá tus tareas</h1>
        <v-textarea label="tarea" v-model="datos.tarea" rows="2" row-height="20"></v-textarea>
        <div class="guardar">
            <v-btn rounded  color="red darken-4" @click="guardar(datos)">
                Guardar
            </v-btn>
        </div>
        <div>
            <div v-if="control === true">
                <div>
                    <ul>
                        <a v-for="error in errores" :key="error">{{ error }}</a>
                    </ul>
                </div>
            </div>
        </div>
        <card-tarea class="mt-6"></card-tarea>
    </div>
</template>
<script>
import CardTarea from "../components/Card-Tarea.vue";

export default {
    name: "tareas-vue",
    components: { CardTarea, }
    ,

    data: function () {
        return {
            datos: {
                tarea: "",
                id: "",
            },
            errores: [],
            datosGuardados: [],
            control: false,
            date: new Date(),
        };
    },

    methods: {
        guardar: function (datos) {
            this.control = true;
            this.errores = [];
            if (!this.datos.tarea) {
                this.errores.push("No ingresaste ninguna tarea");
            }
            if (this.errores.length == 0) {
                datos = Object.assign({}, datos, { id: new Date().getTime() });
                console.log(datos);

                if (!localStorage.form) {
                    this.datosGuardados = [];
                } else {
                    this.datosGuardados = JSON.parse(localStorage.getItem("form"));
                }

                this.datosGuardados.push(datos);
                localStorage.setItem("form", JSON.stringify(this.datosGuardados));

                location.reload();
                this.$router.go("/tareas");
            }
        },
    },
};
</script>