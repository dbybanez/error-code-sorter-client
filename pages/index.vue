<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col">
        <h1>Not sorted</h1>
        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>Code</th>
              <th>Whole</th>
              <th>Decimal</th>
              <th>Alpha</th>
              <th>Description</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="elem in data" :key="elem.id">
              <th>{{ elem.id }}</th>
              <th>{{ elem.code }}</th>
              <th>{{ elem.code.toString().split('.')[0] }}</th>
              <th>{{ elem.code.toString().split('.')[1].replace(/[^0-9]/g, '') }}</th>
              <th>{{ elem.code.toString().split('.')[1].replace(/[^a-z]/gi, '') }}</th>
              <th>{{ elem.description }}</th>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="col">
        <h1>Sorted</h1>
        <table class="table table-bordered table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>Code</th>
              <th>Whole</th>
              <th>Decimal</th>
              <th>Alpha</th>
              <th>Description</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="elem in sorted" :key="elem.id">
              <th>{{ elem.id }}</th>
              <th>{{ elem.code }}</th>
              <th>{{ elem.code.toString().split('.')[0] }}</th>
              <th>{{ elem.code.toString().split('.')[1].replace(/[^0-9]/g, '') }}</th>
              <th>{{ elem.code.toString().split('.')[1].replace(/[^a-z]/gi, '') }}</th>
              <th>{{ elem.description }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      data: '',
      sorted: ''
    }
  },
  beforeMount () {
    this.getData()
  },
  methods: {
    async getData () {
      const response = await axios.get('http://localhost:5000/api/data')
      this.data = response.data
      const sortCodes = (a, b) => {
        // let tempACode = a.code
        // let tempBCode = b.code
        // if (typeof tempACode === 'string') {
        //   a = a.toLowerCase()
        // } else {
        //   a = a.toString()
        // }
        a = typeof a.code === 'string' ? a.toLowerCase() : a.toString()
        b = typeof b.code === 'string' ? b.toLowerCase() : b.toString()
        return a.localeCompare(b)
      }
      this.sorted = response.data.sort(sortCodes)
    }
  }
}
</script>

<style></style>
