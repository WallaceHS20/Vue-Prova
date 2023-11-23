<template>
    <div class="prova">
        <div class="body-prova">
            <h4>CADASTRAR NOVO VOCÁBULO</h4>
            <div class="formulario">
                <div class="input-box">
                    <label for="termo">Nome</label>
                    <input type="text" name="termo" v-model="nome">
                </div>
                <div class="input-box">
                    <label for="datahora">Data/Hora</label>
                    <input type="datetime-local" name="datahora" v-model="data">
                </div>
                <div class="input-box">
                    <label for="significado">Duracao</label>
                    <input type="number" name="significado" v-model="duracao">
                </div>
                <div class="input-box">
                    <label for="significado">Cep</label>
                    <input type="number" name="significado" v-model="cep">
                </div>
                <div class="input-box">
                    <label for="significado">Num</label>
                    <input type="number" name="significado" v-model="num">
                </div>
                <div class="formulario-action">
                    <button @click="cadastrar()">Cadastrar</button>
                </div>
            </div>
            <h4>BUSCAR VOCÁBULO</h4>
            <div class="formulario">
                <div class="input-box">
                    <label for="significado">Nome</label>
                    <input type="text" name="significado" v-model="nome">
                </div>
                <div class="input-box">
                    <label for="significado">Versão</label>
                    <input type="datetime-local" name="significado" v-model="data">
                </div>
                <div class="formulario-action">
                    <button @click="buscarTermo()">Buscar</button>
                </div>
            </div>
            <h4>TODOS OS VOCÁBULOS</h4>
            <div v-if="!erro" class="todos">
                <table>
                    <thead>
                        <td>ID</td>
                        <td>NOME</td>
                        <td>DATA/HORA</td>
                        <td>TIPO</td>
                    </thead>
                    <tbody>
                        <tr v-for="voc in vocabulos">
                            <td>{{ voc.id }}</td>
                            <td>{{ voc.nome }}</td>
                            <td>{{ voc.dataHora }}</td>
                            <td v-if="voc.cep">Presencial</td>
                            <td v-else>Online</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div v-else="erro" class="todos">
                <p>{{ erro }}</p>
                <div class="formulario-action">
                    <button @click="retornar()">Retornar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import '../assets/voc.css'
import { onMounted, ref } from 'vue';
import axios from 'axios';
const erro = ref();



//variável para receber o resultado do get
const vocabulos = ref();

const nome = ref()
const data = ref()
const duracao = ref()
const cep = ref()
const num = ref()

//função de captura de dados
async function atualizar() {
    try {                                  
        vocabulos.value = (await axios.get("https://8080-pavaodogsi-springboot3l-e9tclh5yhb8.ws-us106.gitpod.io/compromisso")).data;
        console.log(vocabulos.value);
        
    }
    catch (ex) {
        alert('DEU RUIM!!')
        erro.value = (ex as Error).message;
    }
}

//função de captura de dados POR TERMO E VERSAO

async function buscarTermo() {
    try {                                  
        vocabulos.value = (await axios.get(`https://8080-pavaodogsi-springboot3l-e9tclh5yhb8.ws-us106.gitpod.io/compromisso/${nome.value}/${data.value}`)).data;
        
        //SE RETORNAR VAZIO
        if(vocabulos.value == ''){
            erro.value = 'Nenhum dado retornado'
        }
    }
    catch (ex) {
        alert('DEU RUIM!!')
        erro.value = (ex as Error).message;
    }
}

function retornar(){
    location.reload ()
}

//CADASTRAR NOVO VOCABULO
async function cadastrar() {
    try {
        await axios.post("https://8080-pavaodogsi-springboot3l-e9tclh5yhb8.ws-us106.gitpod.io/compromisso",
            {
                nome: nome.value,
                dataHora: data.value,
                duracaoPrevista: duracao.value,
                cep: cep.value,
                numero: num.value,
            });
    
            alert('Cadastrado')

            // LIMPAR OS CAMPOS
            nome.value = ''
            cep.value = ''
            data.value = ''
            num.value = ''
            duracao.value = ''
    }
    catch (ex) {
        erro.value = (ex as Error).message;
    }
    atualizar();
}

//PUXR DADOS AO SISTEMA ABRIR A PAG
onMounted(() => {
    atualizar();
});


</script>
