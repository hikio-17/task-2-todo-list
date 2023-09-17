<!-- eslint-disable vue/no-side-effects-in-computed-properties -->
<template>
  <div class="container-fluid">
    <header>
      <HeaderWeb />
    </header>

    <main class="container mt-4">
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

      <div v-if="!isCreating" class="list-task row">
        <div v-if="searchQuery !== ''">
          <div v-if="resultQuery.length">
            <CardItem v-for="(task, i) in resultQuery" :key="i" :task="task" :is-grid="isGrid" />
          </div>
          <h3 v-else class="text-center mt-5">
            Catatan tidak ada
          </h3>
        </div>
        <div v-else-if="searchCategory !== ''">
          <div v-if="resultCategory.length">
            <CardItem v-for="(task, i) in resultCategory" :key="i" :task="task" :is-grid="isGrid" />
          </div>
          <h3 v-else class="text-center mt-5">
            Catatan tidak ada
          </h3>
        </div>
        <div v-else>
          <CardItem v-for="(task, i) in tasks" :key="i" :task="task" :is-grid="isGrid" />
        </div>
      </div>

      <button v-if="!isCreating" href="#" class="btn btn-primary mt-5" @click="isCreating = !isCreating">
        Add Task
      </button>
      <div v-else class="action py-2">
        <div>
          <FormInput :categories="listCategory" @on-cancel-form="onCancelFormInputHandler" @on-submit-form="onSubmitFormHandler" />
        </div>
      </div>
    </main>

    <footer>
      <FooterWeb />
    </footer>
  </div>
</template>
<script>

export default {
  data () {
    return {
      // Daftar task
      tasks: [
        {
          id: 1,
          title: 'Learn Machine Learning',
          description: 'Memulai belajar machine learning dengan bahasa pemrograman python',
          isDone: false,
          category: 'Backend'
        },
        {
          id: 2,
          title: 'Learn NuxtJs',
          description: 'Memulai belajar NuxtJs dengan Kelas Beta Intensif',
          isDone: true,
          category: 'FrontEnd'
        },
        {
          id: 3,
          title: 'Learn ReactJs',
          description: 'Memulai belajar ReactJs',
          isDone: false,
          category: 'Frontend'
        },
        {
          id: 4,
          title: 'Learn Fundamental Javascript',
          description: 'Memulai belajar Fundamental Javascript',
          isDone: false,
          category: 'Javascript'
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
  },
  methods: {
    onCancelFormInputHandler () {
      this.isCreating = !this.isCreating
    },
    onSubmitFormHandler ({ title, description, category }) {
      const newTask = {
        id: +new Date(),
        title,
        description,
        isDone: false,
        category
      }

      this.tasks.unshift(newTask)
      this.isCreating = !this.isCreating
    }
  }
}
</script>
<style></style>
