<script setup>
  import { computed, ref } from 'vue'
  const header = ref('Shopping list app')

  const items = ref([])
  const newItem = ref('')
  const reversedItems = computed(() =>[... items.value].reverse())
  const newItemHighPriority = ref(false)
  const editing = ref(false)

  // const characterCount = computed(() => {
  //   return newItem.value.length
  // })

  // add
  const saveItem = () => {
    items.value.push({
      id: items.value.length + 1, 
      label: newItem.value,
      highPriority: newItemHighPriority.value
    })
    newItem.value = ""
    newItemHighPriority.value = ""
  }

  // edit
  const doEdit = (e) => {
    editing.value = e
    newItem.value = ""
    newItemHighPriority.value = ""
  }

  const togglePurchased = (item) => {
    item.purchased = !item.purchased
  }
</script>

<template>
  <div class="header">
    <h1> {{ header.toUpperCase() }} </h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">
      close
    </button>
    <button v-else class="btn  btn-primary" @click="doEdit(true)">
      Add item
    </button>
    
  </div>
  <!-- <a :href="newItem">Dynamic link</a> -->
  <br/>
  <form
    class="add-item-form"
    v-if="editing"
    @submit.prevent="saveItem"
  >
    <input 
      v-model.trim="newItem" 
      type="text" 
      placeholder="Add an item"
    />

    <label>
      <input
        v-model="newItemHighPriority"
        type="checkbox"
      />
      High Priority
    </label>
    <button
      :disabled="newItem.length < 3" 
      class="btn btn-primary"
    >
      Save item
    </button>
  </form>
  <!-- <br> -->
  <!-- <p class="count">{{ characterCount }} / 20</p> -->
  <br>
  <ul>
    <li 
      v-for="(item, index) in reversedItems"
      @click="togglePurchased(item)" 
      :key="item.id"
      class="static-class"
      :class="{ 
        strikeout: item.purchased, 
        priority: item.highPriority 
        }"
      >
      {{item.label }} 
    </li>
  </ul>
  <p v-if="!items.length">
    No items available
  </p>
</template>

