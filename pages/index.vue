<!-- eslint-disable vue/no-side-effects-in-computed-properties -->
<template>
  <div class="container">
    <div
      class="title border-bottom d-flex align-items-center justify-content-between
py-2"
    >
      <h5>Task</h5>
      <div class="d-flex align-items-center ms-auto">
        <input v-model="searchQuery" type="text" class="form-control me-4" placeholder="Search">

        <select v-model="searchCategory" class="form-control">
          <option disabled value="">
            Search by Category
          </option>
          <option v-for="(category, i) in listCategory" :key="i" :value="category">
            {{ category }}
          </option>
        </select>

        <div class="d-flex align-items-center justify-content-end w-100">
          <span class="me-2 text-center">
            View As
          </span>
          <button class="btn btn-outline-secondary py-1 px-3" @click="isGrid = !isGrid">
            {{ isGrid ? 'Grid' : 'List' }}
          </button>
        </div>
      </div>
    </div>
    <div class="list-task row">
      <div v-if="searchQuery !== ''">
        <div v-if="resultQuery.length">
          <CardItem
            v-for="(task, i) in resultQuery"
            :key="i"
            :task="task"
            :is-grid="isGrid"
          />
        </div>
        <h3 v-else class="text-center mt-5">
          Catatan tidak ada
        </h3>
      </div>
      <div v-else-if="searchCategory !== ''">
        <div v-if="resultCategory.length">
          <CardItem
            v-for="(task, i) in resultCategory"
            :key="i"
            :task="task"
            :is-grid="isGrid"
          />
        </div>
        <h3 v-else class="text-center mt-5">
          Catatan tidak ada
        </h3>
      </div>
      <div v-else>
        <CardItem
          v-for="(task, i) in tasks"
          :key="i"
          :task="task"
          :is-grid="isGrid"
        />
      </div>
    </div>

    <div class="action py-2">
      <a v-if="!isCreating" href="#" class="add-button" @click="isCreating = !isCreating">Add Task</a>
      <div v-else class="add-card">
        <div class="card mb-2">
          <div class="card-body d-flex flex-column p-0">
            <input class="form-control border-0 mb-2" placeholder="Title" type="text">
            <textarea class="form-control border-0 small" placeholder="Description" rows="3" />
          </div>
        </div>
        <div class="button-wrapper d-flex">
          <button class="btn btn-primary me-2">
            Save
          </button>
          <button class="btn btn-outline-secondary" @click="isCreating = !isCreating">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardItem from '~/components/CardItem.vue'

export default {
  components: { CardItem },
  data () {
    return {
      // Daftar task
      tasks: [
        {
          title: 'Task 1',
          description: 'ini deskripsi',
          isDone: false,
          category: 'Web'
        },
        {
          title: 'Task 2',
          description: 'ini deskripsi 2',
          isDone: false,
          category: 'Backend'
        },
        {
          title: 'Task 3',
          description: ' ini deskripsi 3',
          isDone: false,
          category: 'Frontend'
        }
      ],
      listCategory: [
        'Web', 'Backend', 'FrontEnd', 'Javascript', 'All'
      ],
      isCreating: false,
      isGrid: false,
      searchQuery: '',
      searchCategory: '',
      resultTask: []
    }
  },
  computed: {
    resultQuery () {
      if (this.searchQuery) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.searchCategory = ''
        return this.tasks.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(' ')
            .every(v => item.title.toLowerCase().includes(v))
        })
      } else {
        return this.tasks
      }
    },

    // eslint-disable-next-line vue/return-in-computed-property
    resultCategory () {
      if (this.searchCategory.toLowerCase() === 'all') {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.searchQuery = ''
        return this.tasks
      } else if (this.searchCategory && this.searchCategory.toLowerCase() !== 'all') {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.searchQuery = ''
        return this.tasks.filter(item => item.category.toLowerCase() === this.searchCategory.toLowerCase())
      }
    }
  }
}
</script>
<style></style>
