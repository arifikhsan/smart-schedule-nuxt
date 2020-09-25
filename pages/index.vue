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
          <tr v-if="currentInputScheduleTimeLength() < 60">
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
                >
                  {{ option.text }}
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
            v-if="currentInputScheduleTimeLength() < 60"
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
          <button
            class="px-4 py-2 font-bold text-white bg-yellow-500 rounded hover:bg-yellow-700"
            @click="populate"
          >
            Buat data demo
          </button>
        </div>
      </div>
    </div>

    <!-- next week -->
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-2xl font-medium text-green-500">Jadwal Minggu Depan</h2>
      </div>
      <table class="w-full table-auto">
        <thead class="text-gray-800">
          <tr>
            <th class="px-4 py-2 border">Waktu</th>
            <th class="px-4 py-2 border">Senin</th>
            <th class="px-4 py-2 border">Selasa</th>
            <th class="px-4 py-2 border">Rabu</th>
            <th class="px-4 py-2 border">Kamis</th>
            <th class="px-4 py-2 border">Jumat</th>
            <th class="px-4 py-2 border">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="schedule in nextWeekSchedulesInTable" :key="schedule.id">
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.monday }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.tuesday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.wednesday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.thursday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.friday }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.saturday }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="flex items-start justify-start mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: {{ nextWeekHourCount() }} Jam</p>
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
            <th class="px-4 py-2 border">Waktu</th>
            <th class="px-4 py-2 border">Senin</th>
            <th class="px-4 py-2 border">Selasa</th>
            <th class="px-4 py-2 border">Rabu</th>
            <th class="px-4 py-2 border">Kamis</th>
            <th class="px-4 py-2 border">Jumat</th>
            <th class="px-4 py-2 border">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr
            v-for="schedule in nextWeekPostponedSchedulesInTable"
            :key="schedule.id"
          >
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.monday }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.tuesday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.wednesday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.thursday }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.friday }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.saturday }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="flex items-start justify-start mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: {{ nextWeekPostponedHourCount() }} Jam</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import faker from 'faker'
/* eslint-disable no-unused-vars */

export default {
  data() {
    return {
      totalHour: 61,
      done: false,
      hourOptions: [
        { text: '1 Jam', value: 1 },
        { text: '2 Jam', value: 2 },
        { text: '3 Jam', value: 3 },
      ],
      dayOptions: [
        { text: 'Ya', value: true },
        { text: 'Tidak', value: false },
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
          saturday: false,
        },
      },
      newSchedule: {},
      inputSchedules: [],
      nextWeekSchedules: [],
      nextWeekPostponedSchedules: [],
      nextWeekSchedulesInTable: [],
      nextWeekPostponedSchedulesInTable: [],
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
        saturday: false,
      },
    }
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
          saturday: false,
        },
      }
      this.inputSchedules.push(copyNewSchedule)
    },
    populate() {
      this.inputSchedules = []
      let index = 0
      while (this.currentInputScheduleTimeLength() < 60) {
        this.inputSchedules.push({
          id: index,
          name: faker.name.findName(),
          hour: this.randomHour(),
          days: {
            monday: this.randomBoolean(),
            tuesday: this.randomBoolean(),
            wednesday: this.randomBoolean(),
            thursday: this.randomBoolean(),
            friday: this.randomBoolean(),
            saturday: this.randomBoolean(),
          },
        })
        index++
      }
    },
    randomHour() {
      return Math.floor(Math.random() * 3) + 1 // 1 ~ 3
    },
    randomBoolean() {
      return Math.random() >= 0.5 // true || false
    },
    currentInputScheduleTimeLength() {
      let count = 0
      this.inputSchedules.map((e) => (count += e.hour))
      return count
    },
    nextWeekHourCount() {
      let count = 0
      this.nextWeekSchedules.map((e) => (count += e.hour))
      return count
    },
    nextWeekPostponedHourCount() {
      let count = 0
      this.nextWeekPostponedSchedules.map((e) => (count += e.hour))
      return count
    },
    countHourInSchedule(arr) {
      let count = 0
      arr.map((e) => (count += e.hour))
      return count
    },
    makeSchedule() {
      // pilih random array
      // sampai ketemu >= 48 jam
      // yang sudah 48, keluarkan dari array

      const maxHour = 48
      let nextWeekHour = 0
      const nextWeekSchedules = []
      const currentSchedules = this.inputSchedules
      // console.log(currentSchedules)

      // cari jadwal sampai angka >= 48 jam

      for (const schedule of currentSchedules) {
        nextWeekHour += schedule.hour
        nextWeekSchedules.push(schedule)
        if (nextWeekHour >= maxHour) {
          break
        }
      }

      if (nextWeekHour === maxHour) {
        this.nextWeekSchedules = nextWeekSchedules
      } else {
        // kurangi satu jika ada 49
        const offsetSchedules = nextWeekSchedules.filter((e) => e.hour === 1)
        // cari yang terakhir
        const offsetSchedule = offsetSchedules[offsetSchedules.length - 1]
        // console.log(nextWeekHour)
        // console.log(offsetSchedules)
        // console.log(offsetSchedule)

        nextWeekSchedules.splice(nextWeekSchedules.indexOf(offsetSchedule, 1))
        this.nextWeekSchedules = nextWeekSchedules
      }

      // console.log(this.nextWeekSchedules.length)

      // cari jadwal minggu tertunda

      const remainingSchedules = this.inputSchedules.filter(
        (e) => !this.nextWeekSchedules.includes(e)
      )
      // console.log(remainingSchedules)
      this.nextWeekPostponedSchedules = remainingSchedules

      // console.log(this.nextWeekSchedules)
      // populate table

      // const copyNextWeekSchedules = [...this.nextWeekSchedules]

      // const mondays = []
      // const tuesdays = []
      // const wednesdays = []
      // const thursdays = []
      // const fridays = []
      // const saturdays = []

      // while (copyNextWeekSchedules.length > 0) {
      //   const schedule = copyNextWeekSchedules.shift()

      //   if (schedule.days.monday) mondays.push(schedule)
      //   if (schedule.days.tuesday) tuesdays.push(schedule)
      //   if (schedule.days.wednesday) wednesdays.push(schedule)
      //   if (schedule.days.thursday) thursdays.push(schedule)
      //   if (schedule.days.friday) fridays.push(schedule)
      //   if (schedule.days.saturday) saturdays.push(schedule)
      // }

      // make table content
      // const dailySchedules = []

      // let c = 0
      // thursdays.map((e) => (c += e.hour))
      // console.log(thursdays)
      // console.log(c)

      //* cara menaruh jadwalnya gimana

      // const mondaySchedule = []
      // for (let index = 0; index < 16; index++) {
      //   const monday = mondays.shift()
      //   const tuesday = tuesdays.shift()
      //   const wednesday = wednesdays.shift()
      //   const thursday = thursdays.shift()
      //   const friday = fridays.shift()
      //   const saturday = saturdays.shift()

      //   dailySchedules.push({
      //     range: 'time range',
      //     monday: mondays.shift(),
      //     tuesday: tuesdays.shift(),
      //     wednesday: wednesdays.shift(),
      //     thursday: thursdays.shift(),
      //     friday: fridays.shift(),
      //     saturday: saturdays.shift()
      //   })
      // }

      // console.log(mondays)
      // console.log(dailySchedules)
      // this.nextWeekSchedulesInTable = dailySchedules

      // timeRange: `${this.moment('08:00', 'H:mm').format(
      //       'H:mm'
      //     )} - ${this.moment('09:00', 'H:mm').format('H:mm')}`,
      //     monday: schedule.days.monday ? schedule.name : '',
      //     tuesday: schedule.days.tuesday ? schedule.name : '',
      //     wednesday: schedule.days.wednesday ? schedule.name : '',
      //     thursday: schedule.days.thursday ? schedule.name : '',
      //     friday: schedule.days.friday ? schedule.name : '',
      //     saturday: schedule.days.saturday ? schedule.name : ''

      const initTime = this.$moment('08:00', 'H:mm')
      for (let index = 0; index < 8; index++) {
        const dailySchedule = {
          range: `${initTime.format('H:mm')} - ${initTime
            .add(1, 'hours')
            .format('H:mm')}`,
          monday: 'Not Implemented Error',
          tuesday: 'Not Implemented Error',
          wednesday: 'Not Implemented Error',
          thursday: 'Not Implemented Error',
          friday: 'Not Implemented Error',
          saturday: 'Not Implemented Error',
        }
        this.nextWeekSchedulesInTable.push(dailySchedule)
        this.nextWeekPostponedSchedulesInTable.push(dailySchedule)
      }
    },
  },
  head: {
    title: 'Smart Scheduling System (Unfinished)',
    description: 'Penjadwalan otomatis dengan JavaScript.',
  },
}
</script>
