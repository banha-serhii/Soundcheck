<script setup>

import {ref, computed} from "vue";

const header = ref('Звукова накладна')
const characterCountMaxLength = 64
const characterCount = computed(() => {
    return newItem.value.length
})
const editing = ref(false)
const countItems = [1, 2, 3, 4, 5, 6, 7, 8, '9+'];
const items = ref([

    {
        id: 13,
        label: 'Стіл',
        purchased: false,
        highPriority: false
    },
    {
        id: 12,
        label: 'Проектор (стояка + кабелі)',
        purchased: false,
        highPriority: false
    },
    {
        id: 11,
        label: 'Стійки під колонки',
        purchased: false,
        highPriority: false
    },
    {
        id: 10,
        label: 'Мікрофони',
        purchased: false,
        highPriority: false
    },
    {
        id: 9,
        label: 'Світло',
        purchased: false,
        highPriority: false
    },
    {
        id: 8,
        label: 'Світло',
        purchased: false,
        highPriority: false
    },
    {
        id: 7,
        label: 'Кабелі (живлення)',
        purchased: false,
        highPriority: false
    },
    {
        id: 6,
        label: 'Кабелі (спікони)',
        purchased: false,
        highPriority: false
    },

    {
        id: 5,
        label: 'Підсилювач',
        purchased: false,
        highPriority: false
    },
    {
        id: 4,
        label: 'Мікшерний пульт',
        purchased: false,
        highPriority: false
    },
    {
        id: 3,
        label: 'Саббуфер',
        purchased: false,
        highPriority: false
    },
    {
        id: 2,
        label: 'Колонки',
        purchased: false,
        highPriority: false
    },
    {
        id: 1,
        label: 'Ноутбук',
        purchased: false,
        highPriority: false
    }

])
const reversedItems = computed(() => [...items.value].reverse())
const newItem = ref('')
const newItemHighPriority = ref(false)
const saveItem = () => {
    items.value.push({
        id: items.value.length + 1,
        label: newItem.value,
        highPriority: newItemHighPriority.value
    })
    newItem.value = ""
    newItemHighPriority.value = ""
}
const doEdit = (e) => {
    editing.value = e;
    newItem.value = ""
}

const togglePurchased = (item) => {
    item.purchased = !item.purchased
}
</script>

<template>
    <header class="header">
        <h1>{{ header }}</h1>
        <button v-if="editing"
                class="btn"
                @click="doEdit(false)">
            Відміна
        </button>
        <button v-else
                class="btn btn-primary"
                @click="doEdit(true)">
            Додати
        </button>
    </header>
    <main>
        <form class="add-item-form"
              v-if="editing "
              @submit.prevent="saveItem">
            <div style="display: flex; flex-direction: column">
                <input v-model.trim="newItem" type="text" placeholder="Додати в накладну" :maxlength="characterCountMaxLength">

                <label for="high-priority">
                    <input
                        type="checkbox"
                        name="high-priority"
                        id="high-priority"
                        v-model="newItemHighPriority">
                    Важливе
                </label>
            </div>
            <button
                    :disabled="newItem.length < 2"
                    class="btn btn-primary">
               Зберегти
            </button>
        </form>
        <p v-if="editing" class="counter">
            {{ characterCount }}/{{ characterCountMaxLength }}
        </p>
        <ul>
            <li v-for="item in reversedItems"
                :key="item.id"
            >
               <span    @click="togglePurchased(item)"
                        :class="{ strikeout: item.purchased, priority: item.highPriority }">
                   {{ item.label }}
               </span>

                <select>
                    <option v-for="countItem in countItems">{{ countItem }} шт.</option>
                </select>
            </li>

        </ul>
        <p v-if="!items.length">
            Nothing to see here
        </p>
    </main>
</template>


