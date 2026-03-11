<template>
    <div class="app">
        <div class="header">
            <h1>Controle de Gastos</h1>
        <!--icone-->

            <div class="nav">
                <button class="small-btn" @click="filter = 'all'">Tudo</button>
                <button class="small-btn" @click="filter = 'food'">Comida</button>
                <button class="small-btn" @click="filter = 'transport'">Transporte</button>
                <button class="small-btn" @click="filter = 'other'">Outros</button>
            </div>
        </div>
            

        <div class="layout" v-if="false">
            <div class="panel">
                <h2>Nova despesa</h2>
                <input v-model="title" class="input" placeholder="Descricao" />
                <input v-model="value" class="input" placeholder="Valor" />
                <input v-model="category" class="input" placeholder="Categoria" />
                <div class="row">
                    <button class="small-btn" @click="addExpense">Add</button>
                    <button class="small-btn" @click="clearAll">Limpar tudo</button>
                </div>
            </div>

        </div>
        <div class="panel">
                <h2>Lista do dia</h2>
                <table class="table">
                    <tbody>
                        <tr v-for="item in filtered" :key="item.id">
                        <div class="info">
                            <td class="titulo">{{ item.title }}</td>
                            <td class="cate">{{ item.category }}</td>
                        </div>
                            <td class="valor">R${{ item.value }}</td>
                            
                            <td>
                                <button class="small-btn" @click="removeExpense(item.id)">X</button>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <div class="summary">
                    Total do dia: R${{ total }}
                </div>
            </div>
            <div class="button">
            <button @click="add">Adicionar Gastos</button>
            </div>


            <div class="panel" v-if="showAddPanel">
                <div class="modal-content">
                <h2>Adicionar despesa</h2>
                <input v-model="title" class="input" placeholder="Descricao" />
                <input v-model="value" class="input" placeholder="Valor" />
                <input v-model="category" class="input" placeholder="Categoria" />
                <div class="row">
                    <button class="small-btn" @click="addExpense">Add</button>
                    <button class="small-btn" @click="clearAll">Limpar tudo</button>
                </div>
                </div>
            </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const expenses = ref([
    { id: 1, title: 'Cafe', value: 6, category: 'food' },
    { id: 2, title: 'Onibus', value: 4.5, category: 'transport' },
    { id: 3, title: 'Lanche', value: 12, category: 'food' },
]);

const title = ref('');
const value = ref('');
const category = ref('');
const filter = ref('all');

const filtered = computed(() => {
    if (filter.value === 'all') {
        return expenses.value;
    }
    return expenses.value.filter((item) => item.category === filter.value);
});

const total = computed(() => {
    return expenses.value.reduce((sum, item) => sum + Number(item.value || 0), 0);
});

function addExpense() {
    if (!title.value.trim() || !value.value.trim()) {
        alert('Preencha tudo');
        return;
    }
    expenses.value.push({
        id: Date.now(),
        title: title.value,
        value: value.value,
        category: category.value || 'other',
    });
    title.value = '';
    value.value = '';
    category.value = '';
}

function removeExpense(id) {
    expenses.value = expenses.value.filter((item) => item.id !== id);
}

function clearAll() {
    if (!confirm('Tem certeza?')) {
        return;
    }
    expenses.value = [];
}
const showAddPanel = ref(false);
function add() {
    showAddPanel.value = true;
}

function closeAdd(){
  showAdd.value = false
}
</script>
