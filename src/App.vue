<template>
  <div class="app">
    <div class="header">
      <h1>Controle de Gastos</h1>
      <!--icone-->
      <div class="nav">
        <button class="small-btn" @click="filter = 'Tudo'">Tudo</button>
        <button class="small-btn" @click="filter = 'Comida'">Comida</button>
        <button class="small-btn" @click="filter = 'Transporte'">
          Transporte
        </button>
        <button class="small-btn" @click="filter = 'Outros'">Outros</button>
      </div>
    </div>



    


    <div class="layout" v-if="showAddPanel">
      <div class="modal">
        <button @click="closeAdd">voltar</button>
        <h2>Adicionar despesa</h2>
        <label for="title">Descrição</label>
        <input
          v-model="title"
          class="input"
          id="title"
          placeholder="Digite a descrição:"
        />
        <label for="value">Valor</label>
        <input
          v-model="value"
          class="input"
          id="value"
          placeholder="Digite o valor:"
        />
        <label for="category">Categoria</label>
        <div class="custom-select">
          <div class="selected" @click="toggleSelect">{{ category }}</div>
          <div v-if="openSelect" class="options">
            <div class="option" @click="selectOption('Comida')">Comida</div>
            <div class="option" @click="selectOption('Transporte')">
              Transporte
            </div>
            <div class="option" @click="selectOption('Outros')">Outros</div>
          </div>
        </div>
        <div class="row">
          <button class="small-btn" @click="addExpense">Add</button>
        </div>
      </div>
    </div>








    <div class="panel">
      <h2>Lista do dia</h2>
      <div class="table-container">
      <table class="table">
        <tbody>
          <tr v-for="item in filtered" :key="item.id">
            <div class="info">
              <td class="titulo">{{ item.title }}</td>
              <td class="cate">{{ item.category }}</td>
            </div>
            <td class="valor">R${{ item.value }}</td>
            <td>
              <button class="small-btn" @click="removeExpense(item.id)">
                X
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      </div>
      <div class="summary">Total do dia: R${{ total }}</div>
    </div>
    <div class="button"><button @click="add">Adicionar Gastos</button></div>






  </div>
</template>
<script setup>
import { computed, ref } from "vue";
const expenses = ref([ { id: 1, title: "Cafe", value: 6, category: "Comida" }, { id: 2, title: "Onibus", value: 4.5, category: "Transporte" }, { id: 3, title: "Lanche", value: 12, category: "Outros" }, ]);
const title = ref("");
const value = ref("");
const category = ref("");
const filter = ref("Tudo");
const filtered = computed(() => { if (filter.value === "Tudo") { return expenses.value; } return expenses.value.filter((item) => item.category === filter.value); });
const total = computed(() => { return expenses.value.reduce((sum, item) => sum + Number(item.value || 0), 0); });

function addExpense() { 
  if (!title.value.trim() || !value.value.trim()) 
  { alert("Preencha tudo"); return; } 
  expenses.value.push({ id: Date.now(), title: title.value, value: value.value, category: category.value || "other", }); 
  title.value = ""; value.value = ""; category.value = ""; 
  showAddPanel.value = false;
}
  


function removeExpense(id) { expenses.value = expenses.value.filter((item) => item.id !== id); }
function clearAll() { if (!confirm("Tem certeza?")) { return; } expenses.value = []; }
const showAddPanel = ref(false);
function add() { showAddPanel.value = true; }
function closeAdd() { showAddPanel.value = false; }
const openSelect = ref(false);
function toggleSelect() { openSelect.value = !openSelect.value };
function selectOption(option) { category.value = option; openSelect.value = false };
</script>
