<script setup>
// MUDANÇA PARA IONIC:
// além do ref/onMounted/computed do Vue,
// importamos os componentes visuais do Ionic
import { ref, onMounted, computed } from 'vue'
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent
} from '@ionic/vue'

const modal_aberto = ref(false)
const descricao = ref('')
const urgencia = ref('')
const data = ref('')
const idEditando = ref(null)
const usuarios = ref([])

const tarefas = ref([
  {
    id: 1,
    descricao: 'Compras da semana',
    urgencia: 'Média',
    data: '2026-04-08'
  },
  {
    id: 2,
    descricao: 'Atividade Física',
    urgencia: 'Baixa',
    data: '2026-04-05'
  },
  {
    id: 3,
    descricao: 'Limpeza da Casa',
    urgencia: 'Alta',
    data: '2026-04-06'
  }
])

const num_tarefas = computed(() => tarefas.value.length)

onMounted(() => {
  const dados = localStorage.getItem('tarefas')
  if (dados) {
    tarefas.value = JSON.parse(dados)
  }
  buscarUsuarios()
})

function salvar_tarefas() {
  localStorage.setItem('tarefas', JSON.stringify(tarefas.value))
}

function abrirModalNovaTarefa() {
  // limpa os campos para criar nova tarefa
  descricao.value = ''
  urgencia.value = ''
  data.value = ''
  idEditando.value = null
  modal_aberto.value = true
}

function salvarTarefa() {
  if (idEditando.value === null) {
    tarefas.value.push({
      id: Date.now(),
      descricao: descricao.value,
      urgencia: urgencia.value,
      data: data.value
    })
  } else {
    const tarefa = tarefas.value.find(
      tarefa => tarefa.id === idEditando.value
    )

    if (tarefa) {
      tarefa.descricao = descricao.value
      tarefa.urgencia = urgencia.value
      tarefa.data = data.value
    }
  }

  modal_aberto.value = false
  salvar_tarefas()
}

function edt_tarefa(tarefa) {
  descricao.value = tarefa.descricao
  urgencia.value = tarefa.urgencia
  data.value = tarefa.data

  idEditando.value = tarefa.id
  modal_aberto.value = true
}

function exc_tarefa(id) {
  tarefas.value = tarefas.value.filter(tarefa => tarefa.id !== id)
  salvar_tarefas()
}

async function buscarUsuarios() {
  try {
    const resposta = await fetch('https://jsonplaceholder.typicode.com/users')
    const dados = await resposta.json()
    usuarios.value = dados
  } catch (erro) {
    console.error('Erro ao buscar usuários:', erro)
  }
}
</script>

<template>
  <!-- MUDANÇA PARA IONIC:
       no Vue puro tu usava só <main>.
       No Ionic a tela precisa ficar dentro de IonPage -->
  <IonPage>
    <!-- MUDANÇA PARA IONIC:
         topo da tela no padrão mobile -->
    <IonHeader>
      <IonToolbar>
        <IonTitle>Lista de Tarefas com VUE</IonTitle>
      </IonToolbar>
    </IonHeader>

    <!-- MUDANÇA PARA IONIC:
         conteúdo principal da tela -->
    <IonContent>
      <!-- Mantive teu HTML praticamente igual -->
      <main>
        <h1>Lista de Tarefas com VUE</h1>
        <p>Simples código utilizando VUE para criar uma lista de tarefas que permite:</p>

        <ul>
          <li>Criar</li>
          <li>Editar</li>
          <li>Excluir</li>
        </ul>

        <h4>Tarefas: {{ num_tarefas }}</h4>

        <div class="options">
          <!-- AJUSTE:
               teu código original tinha @click="adc_tarefa, modal_aberto=true"
               mas essa função não existia.
               Aqui deixei um abrirModalNovaTarefa() -->
          <button class="btn_adicionar" @click="abrirModalNovaTarefa">
            <span class="transition"></span>
            <span class="gradient"></span>
            <span class="label">ADICIONAR</span>
          </button>
        </div>

        <div v-if="modal_aberto" class="modal">
          <div class="conteudo_modal">
            <h3>
              {{ idEditando === null ? 'Insira as informações da Nova Tarefa' : 'Editar Tarefa' }}
            </h3>

            <div class="campo">
              <label for="descricao">Descrição</label>
              <input v-model="descricao" type="text" id="descricao">
            </div>

            <div class="campo">
              <label for="urgencia">Informe a prioridade</label>
              <select v-model="urgencia" name="urgencia" id="urgencia">
                <option value="Baixa">Baixa</option>
                <option value="Média">Média</option>
                <option value="Alta">Alta</option>
              </select>
            </div>

            <div class="campo">
              <label for="data">Data</label>
              <input v-model="data" type="date" name="data_tarefa" id="data">
            </div>

            <div class="botoes_modal">
              <button @click="salvarTarefa()">Salvar</button>
              <button @click="modal_aberto = false">Cancelar</button>
            </div>
          </div>
        </div>

        <div>
          <table id="tabela">
            <thead>
              <tr>
                <th>ID</th>
                <th>DESCRIÇÃO</th>
                <th>URGÊNCIA</th>
                <th>DATA</th>
                <th>AÇÕES</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="tarefa in tarefas" :key="tarefa.id">
                <td>{{ tarefa.id }}</td>
                <td>{{ tarefa.descricao }}</td>
                <td>{{ tarefa.urgencia }}</td>
                <td>{{ tarefa.data.split('-')[2] }}/{{ tarefa.data.split('-')[1] }}</td>
                <td>
                  <button @click="edt_tarefa(tarefa)">EDITAR</button>
                  <button @click="exc_tarefa(tarefa.id)">EXCLUIR</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </main>
      <IonList class="lista-usuarios">
        <h3 class="titulo-usuarios">Lista de Usuários - API Pública</h3>
        <p class="descricao-api">Esta é uma lista de usuários obtida através da API pública JSONPlaceholder.</p>
        <IonItem v-for="usuario in usuarios" :key="usuario.id" class="item-usuario">
          <IonLabel>
            <h2 class="nome-usuario">Nome: {{ usuario.name }}</h2>
            <p class="email-usuario">Email: {{ usuario.email }}</p>
          </IonLabel>
        </IonItem>
      </IonList>
    </IonContent>
  </IonPage>
</template>

<style scoped>
/* Mantive teu estilo praticamente igual */

body {
  background-color: #ffff99;
}

main {
  max-width: 700px;
  margin: 40px auto; /* ajuste: no original estava com vírgula */
  padding: 24px;
  font-family: Arial, Helvetica, sans-serif;
  background-color: #0e162c;
  border-radius: 25px;
}

h1 {
  margin-bottom: 5px;
  text-align: center;
  padding-bottom: 20px;
}

.options {
  align-items: center;
  text-align: center;
}

button {
  margin-right: 20px;
}

table {
  margin-top: 40px;
  width: 100%;
  border-collapse: collapse;
  font-family: Arial, sans-serif;
}

th,
td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
  background-color: lightslategrey;
}

th {
  background-color: #4CAF50;
  color: white;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #ddd;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.conteudo_modal {
  color: white;
  background-color: white;
  padding: 24px;
  border-radius: 12px;
  width: 420px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  gap: 16px;
}

h3 {
  margin: 0;
  color: #333;
}

.campo {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.campo label {
  font-weight: bold;
  font-size: 14px;
  color: #333;
}

.campo input,
.campo select {
  padding: 10px 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 14px;
  outline: none;
  transition: 0.2s;
}

.campo input:focus,
.campo select:focus {
  border-color: #4CAF50;
  box-shadow: 0 0 6px rgba(76, 175, 80, 0.3);
}

.botoes_modal {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 10px;
}

.botoes_modal button {
  padding: 10px 16px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}


.btn_adicionar {
  font-size: 17px;
  padding: 1em 2.7em;
  font-weight: 500;
  background: #1f2937;
  color: white;
  border: none;
  position: relative;
  overflow: hidden;
  border-radius: 0.6em;
  cursor: pointer;
}

.gradient {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  border-radius: 0.6em;
  margin-top: -0.25em;
  background-image: linear-gradient(
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 0.3)
  );
}

.label {
  position: relative;
  top: -1px;
}

.transition {
  transition-timing-function: cubic-bezier(0, 0, 0.2, 1);
  transition-duration: 500ms;
  background-color: rgba(16, 185, 129, 0.6);
  border-radius: 9999px;
  width: 0;
  height: 0;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

button:hover .transition {
  width: 14em;
  height: 14em;
}

button:active {
  transform: scale(0.97);
}

/* Estilos para a lista de usuários */
.lista-usuarios {
  margin-top: 24px;
  background: transparent;
  padding: 0 4px;
}

.item-usuario {
  --background: #ffffff;
  --color: #1f2937;
  --border-color: transparent;
  --inner-border-width: 0;
  --padding-start: 16px;
  --inner-padding-end: 16px;
  --min-height: 76px;

  margin-bottom: 14px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
}

.nome-usuario {
  color: white;
  font-size: 16px;
  font-weight: 700;
  margin: 0 0 6px 0;
  margin-left: 20%;
}

.email-usuario {
  color: #6b7280;
  font-size: 14px;
  margin: 0;
  margin-left: 20%;
  margin-bottom: 20px;
}

.titulo-usuarios{
  text-align: center;
  color: white;
  margin-bottom: 20px;
}
.descricao-api {
  text-align: center;
  color: #afafaf;
  margin-bottom: 50px;
}
</style>