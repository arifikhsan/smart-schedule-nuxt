<template>
  <div class="max-w-6xl p-4 pb-20 mx-auto">
    <div class="pt-10 pb-5 text-center">
      <h1 class="text-5xl font-bold text-green-500">
        Smart Scheduling System <span class="text-red-500">(Unfinished)</span>
      </h1>
    </div>
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-2xl font-medium text-green-500">Form Isian</h2>
      </div>
      <table class="w-full overflow-auto table-auto">
        <thead class="text-gray-800">
          <tr>
            <th class="px-4 py-2 border">No.</th>
            <th class="px-4 py-2 border">Nama Instruktur</th>
            <th class="px-4 py-2 border">Jam Pelatihan</th>
            <th class="px-4 py-2 border">Senin</th>
            <th class="px-4 py-2 border">Selasa</th>
            <th class="px-4 py-2 border">Rabu</th>
            <th class="px-4 py-2 border">Kamis</th>
            <th class="px-4 py-2 border">Jumat</th>
            <th class="px-4 py-2 border">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <!-- display rows -->
          <template v-if="inputSchedules.length > 0">
            <tr v-for="(row, index) in inputSchedules" :key="row.id">
              <td class="px-4 py-2 border">
                <p class="w-full mt-1">{{ index + 1 }}</p>
              </td>
              <td class="px-4 py-2 border">
                <p class="w-full mt-1">{{ row.name }}</p>
              </td>
              <td class="px-4 py-2 border">
                <p class="w-full mt-1">{{ row.hour }}</p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.monday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.tuesday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.wednesday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.thursday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.friday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
              <td class="px-4 py-2 border">
                <p class="block w-full mt-1">
                  {{ row.days.saturday ? 'Ya' : 'Tidak' }}
                </p>
              </td>
            </tr>
          </template>

          <!-- input row -->
          <tr v-if="currentInputScheduleTimeLength() <= 60">
            <td class="px-4 py-2 border"></td>
            <td class="px-4 py-2 border">
              <input
                v-model="newSchedule.name"
                class="block w-full mt-1 form-input"
                placeholder="Nama"
                type="text"
              />
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.hour"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in hourOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.monday"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.tuesday"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.wednesday"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.thursday"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.friday"
                class="block w-full mt-1 form-select"
              >
                <option
                  v-for="option in dayOptions"
                  :key="option.text"
                  :value="option.value"
                  >{{ option.text }}
                </option>
              </select>
            </td>
            <td class="px-4 py-2 border">
              <select
                v-model="newSchedule.days.saturday"
                class="block w-full mt-1 form-select"
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
        </tbody>
      </table>
      <div class="flex items-start justify-between mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: {{ currentInputScheduleTimeLength() }}</p>
          <p>Maksimal jam: 60/61/62</p>
        </div>
        <div class="flex justify-end space-x-4">
          <button
            v-if="currentInputScheduleTimeLength() <= 60"
            class="px-4 py-2 font-bold text-white bg-green-500 rounded hover:bg-green-700"
            @click="addRow"
          >
            Selesai dan tambah jadwal
          </button>
          <button
            class="px-4 py-2 font-bold text-white bg-green-500 rounded hover:bg-green-700"
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
        <h2 class="text-2xl font-medium text-green-500">
          Jadwal Minggu Depan
        </h2>
      </div>
      <table class="w-full table-auto">
        <thead class="text-gray-800">
          <tr>
            <th class="px-4 py-2 border">Nama Instruktur</th>
            <th class="px-4 py-2 border">Jam Pelatihan</th>
            <th class="px-4 py-2 border">Senin</th>
            <th class="px-4 py-2 border">Selasa</th>
            <th class="px-4 py-2 border">Rabu</th>
            <th class="px-4 py-2 border">Kamis</th>
            <th class="px-4 py-2 border">Jumat</th>
            <th class="px-4 py-2 border">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="row in inputSchedules" :key="row.id">
            <td class="px-4 py-2 border">{{ row.name }}</td>
            <td class="px-4 py-2 border">{{ row.hour }}</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
          </tr>
        </tbody>
      </table>
      <div class="flex items-start justify-start mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: 48 Jam</p>
        </div>
      </div>
    </div>

    <!-- next week postponed -->
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-2xl font-medium text-green-500">
          Jadwal Minggu Depan (Tertunda)
        </h2>
      </div>
      <table class="w-full table-auto">
        <thead class="text-gray-800">
          <tr>
            <th class="px-4 py-2 border">Nama Instruktur</th>
            <th class="px-4 py-2 border">Jam Pelatihan</th>
            <th class="px-4 py-2 border">Senin</th>
            <th class="px-4 py-2 border">Selasa</th>
            <th class="px-4 py-2 border">Rabu</th>
            <th class="px-4 py-2 border">Kamis</th>
            <th class="px-4 py-2 border">Jumat</th>
            <th class="px-4 py-2 border">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="row in inputSchedules" :key="row.id">
            <td class="px-4 py-2 border">{{ row.name }}</td>
            <td class="px-4 py-2 border">{{ row.hour }}</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
            <td class="px-4 py-2 border">?</td>
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
      hourOptions: [
        { text: '1 Jam', value: 1 },
        { text: '2 Jam', value: 2 },
        { text: '3 Jam', value: 3 }
      ],
      dayOptions: [
        { text: 'Ya', value: true },
        { text: 'Tidak', value: false }
      ],
      defaultSchedule: {
        id: 0,
        name: '',
        hour: 1,
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
      hour: 1,
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
        hour: 1,
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
    title: 'Smart Scheduling System (Unfinished)',
    description: 'Penjadwalan otomatis dengan JavaScript.'
  }
}
</script>
