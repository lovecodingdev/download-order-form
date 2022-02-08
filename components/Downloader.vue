<template>
  <div>
    <div id="table-wrapper">
      <div class="result">
        <span>Result&nbsp;</span>
        <button class="btn-reload" @click="refresh()">&#x27F3;</button>
        <span class="right">
          {{ totalSize() }}MB / {{ downloadedSize() }}GB in use
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
          <tr v-for="row in rows" :key="row.id">
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
            <td>{{ row.size }} MB</td>
            <td>{{ row.extractedAt }}</td>
            <td>
              <font-awesome-icon
                v-if="row.Status == 0"
                icon="exclamation-triangle"
                :style="{ color: '#e9d100' }"
              />
              <font-awesome-icon
                v-if="row.Status == 1"
                icon="cloud-download-alt"
                :style="{ color: '#708edc' }"
              />
              <font-awesome-icon
                v-if="row.Status == 2"
                icon="check"
                :style="{ color: '#26bc01' }"
              />
              {{ extractStatus[row.Status] }}
            </td>
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

interface DownloadItem {
  id: number
  name: String
  size: number
  extractedAt: Date
  Status: ExtractStatusType
}

export default defineComponent({
  props: {
    items: { type: Array, default: () => [] }
  },
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
      rows: [] as DownloadItem[],
      checkedRows: []
    }
  },
  watch: {
    items(newVal, _) {
      this.rows.push(...newVal)
    }
  },
  methods: {
    refresh() {
      console.log('refresh', this.items, this.rows)
    },
    deleteRows() {
      this.checkedRows.forEach((id) => {
        let checkedIndex = this.rows.findIndex((row) => row.id === id)
        if (checkedIndex >= 0) {
          this.rows.splice(checkedIndex, 1)
        }
      })
      this.checkedRows = []
    },
    downloadedSize() {
      return 10
    },
    totalSize() {
      let total = 0
      this.rows.forEach((item) => (total += item.size))
      return total
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
  border: 3px solid #dee9f7;
}

table th {
  text-align: left;
  background: #dee9f7;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
  border-right: 2px solid #fff;
}
table th:first-child {
  width: 50px;
}
table th:hover {
  background: #dee9f7;
}
table td {
  text-align: left;
  padding: 8px;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #dee9f7;
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
  background: #0085ff;
  border: solid 2px #0085ff;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}

.btn-delete {
  background: #ed4343;
  border: solid 2px #ed4343;
  border-radius: 4px;
  color: white;
  cursor: pointer;
}
</style>
