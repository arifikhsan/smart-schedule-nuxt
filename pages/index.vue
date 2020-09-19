<template>
  <div class="max-w-6xl mx-auto p-4">
    <div class="text-center pt-10 pb-5">
      <h1 class="text-green-500 font-bold text-5xl">Smart Schedule System</h1>
    </div>
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">Form Isian</h2>
      </div>
      <table class="table-auto overflow-auto w-full">
        <thead class="text-gray-800">
          <tr>
            <th class="border px-4 py-2">No.</th>
            <th class="border px-4 py-2">Nama Instruktur</th>
            <th class="border px-4 py-2">Jam Pelatihan</th>
            <th class="border px-4 py-2">Senin</th>
            <th class="border px-4 py-2">Selasa</th>
            <th class="border px-4 py-2">Rabu</th>
            <th class="border px-4 py-2">Kamis</th>
            <th class="border px-4 py-2">Jumat</th>
            <th class="border px-4 py-2">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <!-- display rows -->
          <template v-if="inputSchedules.length > 0">
            <tr v-for="(row, index) in inputSchedules" :key="row.id">
              <td class="border px-4 py-2">
                <p class="mt-1 w-full">{{ index + 1 }}</p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 w-full">{{ row.name }}</p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 w-full">{{ row.hour }}</p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.monday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.tuesday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.wednesday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.thursday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.friday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="border px-4 py-2">
                <p class="mt-1 block w-full">
                  {{ row.days.saturday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
            </tr>
          </template>

          <!-- input row -->
          <tr v-if="currentInputScheduleTimeLength() <= 60">
            <td class="border px-4 py-2"></td>
            <td class="border px-4 py-2">
              <input
                v-model="newSchedule.name"
                class="form-input mt-1 block w-full"
                placeholder="Nama"
                type="text"
              />
            </td>
            <td class="border px-4 py-2">
              <input
                v-model="newSchedule.hour"
                class="form-input mt-1 block w-full"
                placeholder="Jam"
                type="number"
              />
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.monday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.tuesday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.wednesday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.thursday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.friday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select
                v-model="newSchedule.days.saturday"
                class="form-select mt-1 block w-full"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
          </tr>

          <tr v-else>
            <p>
              Total jam sudah lebih dari 60 jam
            </p>
          </tr>
        </tbody>
      </table>
      <div class="mt-4 flex justify-between items-start">
        <div class="flex justify-start flex-col">
          <p>Total jam: {{ currentInputScheduleTimeLength() }}</p>
          <p>Maksimal jam: 60/61/62</p>
        </div>
        <div class="flex justify-end space-x-4">
          <button
            v-if="currentInputScheduleTimeLength() <= 60"
            class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
            @click="addRow"
          >
            Selesai dan tambah jadwal
          </button>
          <button
            class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
            @click="makeSchedule"
          >
            Buat jadwal minggu depan
          </button>
        </div>
      </div>
    </div>

    <!-- next week -->
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">
          Form Jadwal Minggu Depan
        </h2>
      </div>
      <table class="table-auto w-full">
        <thead class="text-gray-800">
          <tr>
            <th class="border px-4 py-2">Nama Instruktur</th>
            <th class="border px-4 py-2">Jam Pelatihan</th>
            <th class="border px-4 py-2">Senin</th>
            <th class="border px-4 py-2">Selasa</th>
            <th class="border px-4 py-2">Rabu</th>
            <th class="border px-4 py-2">Kamis</th>
            <th class="border px-4 py-2">Jumat</th>
            <th class="border px-4 py-2">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="row in inputSchedules" :key="row.id">
            <td class="border px-4 py-2">{{ row.name }}</td>
            <td class="border px-4 py-2">{{ row.hour }}</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- next week postponed -->
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">
          Form Jadwal Minggu Depan (Tertunda)
        </h2>
      </div>
      <table class="table-auto w-full">
        <thead class="text-gray-800">
          <tr>
            <th class="border px-4 py-2">Nama Instruktur</th>
            <th class="border px-4 py-2">Jam Pelatihan</th>
            <th class="border px-4 py-2">Senin</th>
            <th class="border px-4 py-2">Selasa</th>
            <th class="border px-4 py-2">Rabu</th>
            <th class="border px-4 py-2">Kamis</th>
            <th class="border px-4 py-2">Jumat</th>
            <th class="border px-4 py-2">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="row in inputSchedules" :key="row.id">
            <td class="border px-4 py-2">{{ row.name }}</td>
            <td class="border px-4 py-2">{{ row.hour }}</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
            <td class="border px-4 py-2">?</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */

export default {
  data() {
    return {
      totalHour: 61,
      done: false,
      dayOptions: [
        { text: 'Ya', value: true },
        { text: 'Tidak', value: false }
      ],
      defaultSchedule: {
        id: 0,
        name: '',
        hour: 0,
        days: {
          monday: false,
          tuesday: false,
          wednesday: false,
          thursday: false,
          friday: false,
          saturday: false
        }
      },
      newSchedule: {},
      inputSchedules: [],
      nextWeekSchedules: [],
      nextWeekPostponedSchedules: []
    }
  },
  created() {
    this.newSchedule = {
      id: 0,
      name: '',
      hour: 0,
      days: {
        monday: false,
        tuesday: false,
        wednesday: false,
        thursday: false,
        friday: false,
        saturday: false
      }
    }

    // this.populate()
  },
  methods: {
    addRow() {
      const copyNewSchedule = { ...this.newSchedule }
      let lastId = this.inputSchedules.length + 1
      copyNewSchedule.id = lastId++
      copyNewSchedule.hour = +copyNewSchedule.hour
      this.newSchedule = {
        id: 0,
        name: '',
        hour: 0,
        days: {
          monday: false,
          tuesday: false,
          wednesday: false,
          thursday: false,
          friday: false,
          saturday: false
        }
      }
      this.inputSchedules.push(copyNewSchedule)
    },
    populate() {
      for (let index = 1; index <= 40; index++) {
        this.inputSchedules.push({
          id: index,
          name: `Instruktur ${index}`,
          hour: this.randomHour(),
          days: {
            monday: this.randomBoolean(),
            tuesday: this.randomBoolean(),
            wednesday: this.randomBoolean(),
            thursday: this.randomBoolean(),
            friday: this.randomBoolean(),
            saturday: this.randomBoolean()
          }
        })
      }
    },
    randomHour() {
      return Math.floor(Math.random() * 3) + 1
    },
    randomBoolean() {
      return Math.random() >= 0.5
    },
    currentInputScheduleTimeLength() {
      let count = 0
      this.inputSchedules.map((e) => (count += e.hour))
      return count
    },
    countHourInSchedule(arr) {
      let count = 0
      arr.map((e) => (count += e.hour))
      return count
    },
    makeSchedule() {
      // pilih random array
      // sampai ketemu 48 jam
      // yang sudah 48, keluarkan dari array
      // const maxHour = 48
      // let currentHour = 0
      // const currentSchedules = { ...this.inputSchedules }
      // const randomSchedules = []
      // while (currentHour !== maxHour) {
      //   for (let index = 0; index < currentSchedules.length; index++) {
      //     const randomIndex = Math.floor(
      //       Math.random() * currentSchedules.length
      //     )
      //     const element = this.inputSchedules[randomIndex]
      //     currentSchedules.splice(currentSchedules, 1)
      //     randomSchedules.push(element)
      //     console.log(randomIndex)
      //   }
      //   currentHour = this.countHourInSchedule(randomSchedules)
      //   console.log(currentHour)
      // }
      // console.log('makeSchedule')
      // console.log(randomSchedules)
    }
  },
  head: {
    title: 'Smart Schedule System',
    description: 'Penjadwalan otomatis dengan JavaScript.'
  }
}
</script>
