<template>
  <v-dialog v-model="data.dialogOpen" activator="parent" width="50%">
    <template v-slot:activator="{ props }">
      <v-btn class="dp" icon v-bind="props">
        <v-icon :color="'#ee732f'">mdi-plus-box-multiple</v-icon>
        <v-tooltip activator="parent" location="bottom">Add Tale</v-tooltip>
      </v-btn>
      <!-- <v-btn
          color="primary"
          v-bind="props"
        >
          Open Dialog
        </v-btn> -->
    </template>
    <v-card>
      <v-card-title>Add Tale</v-card-title>
      <v-card-text>
        <v-text-field v-model="data.title" label="Title" required hide-details></v-text-field>
        <v-text-field
          v-model="data.description"
          label="Description"
          required
          hide-details
        ></v-text-field>
        <v-file-input accept="image/*" label="File input" v-model="thumbnail"></v-file-input>
        <v-select
          v-model="data.category"
          :items="Object.keys(data.categories)"
          label="Category"
        ></v-select>
        <v-text-field
          v-model="data.expectedStoryCount"
          label="Expected Story Count"
          required
          hide-details
        ></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="closeDialog">Cancel</v-btn>
        <v-btn color="#ee732f" variant="elevated" @click="submitDialog">Submit</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
import { HomeService } from '@/api/HomeService'
import category from '@/test-data/category'
import { onMounted } from 'vue'
import { ref } from 'vue'
import { reactive } from 'vue'

const categoryService = new HomeService()

const data = reactive({
  title: '',
  description: '',
  category: undefined,
  expectedStoryCount: 0,
  tags: [],
  categories: {},
  loading: true,
  dialogOpen: false
})

const thumbnail = ref(undefined)

const closeDialog = () => {
  data.dialogOpen = false
}

const submitDialog = () => {
  console.log('func called')
}

onMounted(async () => {
  try {
    let res: any = await categoryService.fetchCategories()
    if (res['success']) {
      let cat: any = category
      const val: any = {}
      let a = (cat || res['data']).map((item: any) => {
        val[item.name] = item.entityId
      })

      data.categories = val
    }
    data.loading = false
  } catch (e) {
    console.log(e)
    data.loading = false
  }
})
</script>

<style lang="scss" scoped>
.dp {
  margin-top: 10px;
}

.v-card-text {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.v-card-actions {
  justify-content: center;
}
</style>
