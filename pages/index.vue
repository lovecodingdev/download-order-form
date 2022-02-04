<template>
  <div>
    <div id="table-wrapper">
      <span>Result</span>
      <span class="right">{{ downloadedSize }}MB/{{ totalSize }}GB in use</span>
      <table>
        <thead>
          <tr>
            <th class="text-align-center"><button>Delete</button></th>
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
  el: '#sixthTable',
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
          name: '202112251215.zip',
          size: 100,
          extractedAt: '2021/12/25 12:15:09',
          Status: 0
        },
        {
          name: '202112260947.zip',
          size: 98,
          extractedAt: '2021/12/26 09:47:08',
          Status: 0
        },
        {
          name: '202112271409.zip',
          size: 1004,
          extractedAt: '2021/12/27 14:09:45',
          Status: 1
        },
        {
          name: '202112281205.zip',
          size: 890,
          extractedAt: '2021/12/28 12:05:18',
          Status: 1
        },
        {
          name: '202112291110.zip',
          size: 750,
          extractedAt: '2021/12/29 11:10:32',
          Status: 1
        },
        {
          name: '202112301815.zip',
          size: 1280,
          extractedAt: '2021/12/30 18:15:12',
          Status: 2
        },
        {
          name: '202112311805.zip',
          size: 780,
          extractedAt: '2021/12/31 18:05:00',
          Status: 2
        },
        {
          name: '202201021004.zip1',
          size: 430,
          extractedAt: '2022/01/02 10:04:19',
          Status: 2
        },
        {
          name: '202201022315.zip',
          size: 1320,
          extractedAt: '2022/01/02 23:15:57',
          Status: 2
        }
      ]
    }
  },
  computed: {
    columns: function columns() {
      if (this.rows.length === 0) {
        return []
      }
      return Object.keys(this.rows[0])
    },
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
    sortTable: function sortTable(col) {
      if (this.sortColumn === col) {
        this.ascending = !this.ascending
      } else {
        this.ascending = true
        this.sortColumn = col
      }

      let ascending = this.ascending

      this.rows.sort(function (a, b) {
        if (a[col] > b[col]) {
          return ascending ? 1 : -1
        } else if (a[col] < b[col]) {
          return ascending ? -1 : 1
        }
        return 0
      })
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
  border: 3px solid #44475c;
}

table th {
  text-align: left;
  background: #44475c;
  color: #fff;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
}
table th:first-child {
  width: 50px;
}
table th:hover {
  background: #717699;
}
table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7d82a8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #d4d8f9;
}

.pagination {
  font-family: 'Open Sans', sans-serif;
  text-align: right;
  width: 750px;
  padding: 8px;
}

.arrow_down {
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB8AAAAaCAYAAABPY4eKAAAAAXNSR0IArs4c6QAAAvlJREFUSA29Vk1PGlEUHQaiiewslpUJiyYs2yb9AyRuJGm7c0VJoFXSX9A0sSZN04ULF12YEBQDhMCuSZOm1FhTiLY2Rky0QPlQBLRUsICoIN/0PCsGyox26NC3eTNn3r3n3TvnvvsE1PkwGo3yUqkkEQqFgw2Mz7lWqwng7ztN06mxsTEv8U0Aam5u7r5EInkplUol/f391wAJCc7nEAgE9Uwmkzo4OPiJMa1Wq6cFs7Ozt0H6RqlUDmJXfPIx+qrX69Ti4mIyHA5r6Wq1egND+j+IyW6QAUoul18XiUTDNHaSyGazKcZtdgk8wqhUKh9o/OMvsVgsfHJy0iWqVrcQNRUMBnd6enqc9MjISAmRP3e73T9al3XnbWNjIw2+KY1Gc3imsNHR0YV4PP5+d3e32h3K316TySQFoX2WyWR2glzIO5fLTSD6IElLNwbqnFpbWyO/96lCoai0cZjN5kfYQAYi5H34fL6cxWIZbya9iJyAhULBHAqFVlMpfsV/fHxMeb3er+Vy+VUzeduzwWC45XA4dlD/vEXvdDrj8DvURsYEWK3WF4FA4JQP9mg0WrHZbEYmnpa0NxYgPVObm5teiLABdTQT8a6vrwdRWhOcHMzMzCiXlpb2/yV6qDttMpkeshEzRk4Wo/bfoe4X9vb2amzGl+HoXNT29vZqsVi0sK1jJScG+Xx+HGkL4Tew2TPi5zUdQQt9otPpuBk3e0TaHmMDh1zS7/f780S0zX6Yni+NnBj09fUZUfvudDrNZN+GkQbl8Xi8RLRtHzsB9Hr9nfn5+SjSeWUCXC7XPq5kw53wsNogjZNohYXL2EljstvtrAL70/mVaW8Y4OidRO1/gwgbUMvcqGmcDc9aPvD1gnTeQ+0nmaInokRj0nHh+uvIiVOtVvt2a2vLv7Ky0tL3cRTXIcpPAwMDpq6R4/JXE4vFQ5FI5CN+QTaRSFCYc8vLy1l0rge4ARe5kJ/d27kYkLXoy2Jo4C7K8CZOsEBvb+9rlUp1xNXPL7v3IDwxvPD6AAAAAElFTkSuQmCC');
}
.arrow_up {
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAaCAYAAACgoey0AAAAAXNSR0IArs4c6QAAAwpJREFUSA21Vt1PUmEYP4dvkQ8JFMwtBRocWAkDbiqXrUWXzU1rrTt0bdVqXbb1tbW16C9IBUSmm27cODdneoXjputa6069qwuW6IIBIdLvdaF4OAcOiGeDc87zPs/vd57P96WpFq7p6enbGo1mjKZpeTabjU1MTCRagGnOZHFxcXxtbe1XKpUq7+zslJeXl//Mz8+Hy+Uy3RxSE9qTk5M3otFooVQqgef4Wl9f343FYoEmoISrxuNxFX5f9vb2jhn/PxUKhfLS0tIPfFifUESRUMV8Pv/M6XReRm5rTGQyGeXxeGxYe1ezeBpBOBx2rKysbO7v79d4Wy3Y2Nj4GQqFbgnhaugxwiuGJx99Pp9FLBbXxYTXvTqd7v3MzIy6riIWGxJnMpl7AwMD14xGYyMsSq1WUyQdUqn0eSPlusQIsbGrq+vl4OCgvhFQZd1utyv1en0gEolcqsi47nWJlUrlG5fLZVcoFFy2nDKSDpIWlUoVTCQSEk4lCHmJMZ2GTCbTiMVikfIZ88l7enoos9l8dXt7+z6fDicxSJUokqDX6xXcl2wCROoc0vQCWL3sNfLOSdzR0fHY4XC4tVotl40gmVwup9xuN4OQv+UyqCFGH9rg7SOGYVRcBs3IEG4J0nVnamrqOtvuBDGGgQg9+wHFcVEi4a0LNkbdd6TrPKo8ODc311mteIIYjT/a398/jK+s1jnVM0kXoufCFvq0GuiIGEVgQIhfoygM1QrteEa9dAL7ITiYCt4RMabOK5AyKKzKWtvupLcRciu8D5J0EuDDPyT/Snd39yh6VtY2NhYQSR9G79Ds7OxdskRjEyAufvb7/cPoO5Z6e1+xtVKrq6vfcFzyi/A3ZrPZ3GdNSlwgo5ekE4X2RIQGf2C1WlufFE0GBeGWYQ8YERWLxQtnUVB830MKLZfL9RHir8lkssCn2G751tZWEWe03zTKm15YWPiEiXXTYDB0Ig/t7yd8PRws4EicwWHxO4jHD8/C5HiTTqd1BwcHFozKU89origB+y/kmzgYpgOBQP4fGmUiZmJ+WNgAAAAASUVORK5CYII=');
}
.arrow {
  float: right;
  width: 12px;
  height: 15px;
  background-repeat: no-repeat;
  background-size: contain;
  background-position-y: bottom;
}

.number {
  display: inline-block;
  padding: 4px 10px;
  color: #fff;
  border-radius: 4px;
  background: #44475c;
  margin: 0px 5px;
  cursor: pointer;
}
.number:hover,
.number.active {
  background: #717699;
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
</style>
