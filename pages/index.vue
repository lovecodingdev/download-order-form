<template>
  <div>
    <div id="table-wrapper">
      <div class="result">
        <span>Result&nbsp;</span>
        <button class="btn-reload" @click="refresh()">&#x27F3;</button>
        <span class="right">
          {{ downloadedSize }}MB/{{ totalSize }}GB in use
        </span>
      </div>
      <table>
        <thead>
          <tr>
            <th class="text-align-center">
              <button class="btn-delete" @click="deleteRows()">Delete</button>
            </th>
            <th>File Name</th>
            <th>File Size</th>
            <th>Extraction date time</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="row in rows">
            <td class="text-align-center">
              <input
                v-model="checkedRows"
                :value="row.id"
                type="checkbox"
                :class="[row.Status == 0 ? 'disabled' : '']"
              />
            </td>
            <td>
              <a :href="row.name" :class="[row.Status == 0 ? 'disabled' : '']">
                {{ row.name }}
              </a>
            </td>
            <td>{{ row.size }}</td>
            <td>{{ row.extractedAt }}</td>
            <td>{{ extractStatus[row.Status] }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'

const extractStatus = {
  0: 'In process',
  1: 'Downloadable',
  2: 'Downloaded'
} as const

type ExtractStatusType = typeof extractStatus[keyof typeof extractStatus]

interface downloadItem {
  Id: number
  Name: String
  Size: number
  ExtractedAt: Date
  Status: ExtractStatusType
}

export default defineComponent({
  components: {},
  setup() {},
  data() {
    return {
      ascending: false,
      sortColumn: '',
      extractStatus,
      columns: [
        {
          key: 'name',
          fieldName: 'Name',
          linkable: true
        },
        {
          key: 'size',
          fieldName: 'Size'
        },
        {
          key: 'extractedAt',
          fieldName: 'extractedAt'
        },
        {
          key: 'Status',
          fieldName: 'Status'
        }
      ],
      rows: [
        {
          id: 1,
          name: '202112251215.zip',
          size: 100,
          extractedAt: '2021/12/25 12:15:09',
          Status: 0
        },
        {
          id: 2,
          name: '202112260947.zip',
          size: 98,
          extractedAt: '2021/12/26 09:47:08',
          Status: 0
        },
        {
          id: 3,
          name: '202112271409.zip',
          size: 1004,
          extractedAt: '2021/12/27 14:09:45',
          Status: 1
        },
        {
          id: 4,
          name: '202112281205.zip',
          size: 890,
          extractedAt: '2021/12/28 12:05:18',
          Status: 1
        },
        {
          id: 5,
          name: '202112291110.zip',
          size: 750,
          extractedAt: '2021/12/29 11:10:32',
          Status: 1
        },
        {
          id: 6,
          name: '202112301815.zip',
          size: 1280,
          extractedAt: '2021/12/30 18:15:12',
          Status: 2
        },
        {
          id: 7,
          name: '202112311805.zip',
          size: 780,
          extractedAt: '2021/12/31 18:05:00',
          Status: 2
        },
        {
          id: 8,
          name: '202201021004.zip1',
          size: 430,
          extractedAt: '2022/01/02 10:04:19',
          Status: 2
        },
        {
          id: 9,
          name: '202201022315.zip',
          size: 1320,
          extractedAt: '2022/01/02 23:15:57',
          Status: 2
        }
      ],
      checkedRows: []
    }
  },
  computed: {
    downloadedSize() {
      return 10
    },
    totalSize() {
      let total = 0
      this.rows.forEach((item) => (total += item.size))
      return total
    }
  },
  methods: {
    refresh() {
      console.log('refresh')
    },
    deleteRows() {
      console.log(this.checkedRows)
      this.checkedRows.forEach((id) => {
        let checkedIndex = this.rows.findIndex((row) => row.id === id)
        if (checkedIndex >= 0) {
          this.rows.splice(checkedIndex, 1)
        }
      })
      this.checkedRows = []
    }
  }
})
</script>

<style scoped>
#table-wrapper {
  margin: 32px;
}
table {
  font-family: 'Open Sans', sans-serif;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid #DEE9F7;
}

table th {
  text-align: left;
  background: #DEE9F7;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
  border-right: 2px solid #fff;
}
table th:first-child {
  width: 50px;
}
table th:hover {
  background: #DEE9FF;
}
table td {
  text-align: left;
  padding: 8px;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #DEE9F7;
}

table td input[type='checkbox'] {
  width: 20px;
  height: 20px;
}

.right {
  float: right;
}

.text-align-center {
  text-align: center;
}

.disabled {
  pointer-events: none;
  color: gray;
}

.result {
  margin: 4px;
}

.btn-reload {
  font-size: 16px;
  font-weight: bold;
  padding: 2px;
  width: 24px;
  height: 24px;
  background: #0085FF;
  border: solid 2px #0085FF;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}

.btn-delete {
  background: #ED4343;
  border: solid 2px #ED4343;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}
</style>
