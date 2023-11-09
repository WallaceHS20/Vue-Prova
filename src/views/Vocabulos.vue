<template>
    <div class="prova">
        <div class="top-prova">
            <h1>VOCÁBULOS</h1>
        </div>
        <div class="body-prova">
            <h4>TODOS OS VOCÁBULOS</h4>
            <div class="todos">
                <table>
                    <thead>
                        <td>ID</td>
                        <td>TERMO</td>
                        <td>SIGNIFICADO</td>
                    </thead>
                    <tbody>
                        <tr v-for="voc in vocabulos">
                            <td>{{ voc.id }}</td>
                            <td>{{ voc.termo }}</td>
                            <td>{{ voc.significado }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <button @click="atualizar()">test</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import '../assets/voc.css'
import { onMounted, ref } from 'vue';
import axios from 'axios';

const vocabulos = ref();

async function atualizar() {
    vocabulos.value = (await axios.get("https://8080-wallacehs20-springboots-ija7j0ufl4t.ws-us106.gitpod.io/vocabulo")).data;
}

onMounted(() => {
    atualizar();
});


</script>
