<template>
  <div class="max-w-6xl p-4 pb-20 mx-auto">
    <div class="pt-10 pb-5 text-center">
      <h1 class="text-5xl font-bold text-green-500">Smart Schedule System</h1>
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
              <td
                v-for="(day, indexDay) in row.days"
                :key="indexDay"
                class="px-4 py-2 border"
              >
                <p class="block w-full mt-1">
                  {{ day ? 'Ya' : 'Tidak' }}
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
          <p>Total instruktur: {{ instructorCount() }}</p>
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
    <!-- <div v-for="schedule in weeklySchedules" class="py-4">
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
          <tr v-for="schedule in nextWeekSchedules" :key="schedule.id">
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-green-500 border">{{ schedule.mo }}</td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.tu }}
            </td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.we }}
            </td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.th }}
            </td>
            <td class="px-4 py-2 text-green-500 border">{{ schedule.fr }}</td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.sa }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="flex items-start justify-start mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: {{ nextWeekHourCount() }} Jam</p>
        </div>
      </div>
    </div> -->

    <!-- next week postponed -->
    <!-- <div class="py-4">
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
          <tr v-for="schedule in nextWeekPostponedSchedules" :key="schedule.id">
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-green-500 border">{{ schedule.mo }}</td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.tu }}
            </td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.we }}
            </td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.th }}
            </td>
            <td class="px-4 py-2 text-green-500 border">{{ schedule.fr }}</td>
            <td class="px-4 py-2 text-green-500 border">
              {{ schedule.sa }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="flex items-start justify-start mt-4">
        <div class="flex flex-col justify-start">
          <p>Total jam: {{ nextWeekPostponedHourCount() }} Jam</p>
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
/* eslint-disable prefer-const */
/* eslint-disable no-unused-vars */

import faker from 'faker'

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
        days: [false, false, false, false, false, false],
      },
      newSchedule: {},
      selectedInstructors: [],
      inputSchedules: [],
      outputSchedules: [],
      weeklySchedules: [],
      nextWeekSchedules: [],
      nextWeekPostponedSchedules: [],
      nextWeekHour: 0,
      nextWeekPostponedHour: 0,
    }
  },
  created() {
    this.newSchedule = { ...this.defaultSchedule }
  },
  methods: {
    addRow() {
      const copyNewSchedule = { ...this.newSchedule }
      // cek jamnya
      if (
        this.currentInputScheduleTimeLength() +
          this.scheduleHourLength(copyNewSchedule) >
        62
      ) {
        alert('Jumlah jam atau hari terlalu banyak, mohon dikurangi')
      } else {
        let lastId = this.inputSchedules.length + 1
        copyNewSchedule.id = lastId++
        copyNewSchedule.hour = +copyNewSchedule.hour
        this.newSchedule = { ...this.defaultSchedule }
        this.inputSchedules.push(copyNewSchedule)
      }
    },
    populate() {
      this.inputSchedules = []
      let index = 1
      while (this.currentInputScheduleTimeLength() < 60) {
        this.inputSchedules.push({
          id: index,
          // name: faker.name.findName(),
          name: `instructor ${index}`,
          hour: this.randomHour(),
          days: [
            this.randomBoolean(),
            this.randomBoolean(),
            this.randomBoolean(),
            this.randomBoolean(),
            this.randomBoolean(),
            this.randomBoolean(),
          ],
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
      this.inputSchedules.map((schedule) => {
        // const hourPerPerson =
        //   schedule.hour * schedule.days.filter((day) => day === true).length
        // count += hourPerPerson
        count += schedule.hour
      })
      return count
    },
    scheduleHourLength(schedule) {
      return schedule.hour * schedule.days.filter((day) => day === true).length
    },
    instructorCount() {
      return this.inputSchedules.length
    },
    nextWeekHourCount() {
      let hour = 0
      this.nextWeekSchedules.forEach((s) => {
        if (s.mo !== undefined) hour++
        if (s.tu !== undefined) hour++
        if (s.we !== undefined) hour++
        if (s.th !== undefined) hour++
        if (s.fr !== undefined) hour++
        if (s.sa !== undefined) hour++
      })
      return hour
    },
    nextWeekPostponedHourCount() {
      let hour = 0
      // this.nextWeekPostponedSchedules.map((e) => (count += e.hour))
      this.nextWeekPostponedSchedules.forEach((s) => {
        if (s.mo !== undefined) hour++
        if (s.tu !== undefined) hour++
        if (s.we !== undefined) hour++
        if (s.th !== undefined) hour++
        if (s.fr !== undefined) hour++
        if (s.sa !== undefined) hour++
      })
      return hour
    },
    countHourInSchedule(arr) {
      let count = 0
      arr.map((e) => (count += e.hour))
      return count
    },
    anySchedulesLeft() {
      let count = 0

      this.weeklySchedules.forEach((e) => {
        count += e.length
      })

      if (count > 0) {
        return true
      } else {
        return false
      }
    },
    makeSchedule() {
      this.nextWeekSchedules = []
      this.nextWeekPostponedSchedules = []
      this.nextWeekHour = 0
      this.nextWeekPostponedHour = 0

      const currentSchedules = [...this.inputSchedules]
      const outputSchedules = []
      const weekArray = [[], [], [], [], [], []]

      const dailySchedules = [...weekArray]

      for (let index = 0; index < dailySchedules.length; index++) {
        dailySchedules[index] = this.inputSchedules.filter((schedule) => {
          // console.log(schedule.days[index])
          if (schedule.days[index]) return schedule
        })
      }

      // console.log(dailySchedules)

      // [[mo, mo], [], [], [], [], []]
      dailySchedules.forEach((dailySchedule, sixDayIndex) => {
        let currentHour = 0
        let weekNum = 0
        const maxHour = 8
        // this.weeklySchedules[weekNum] = []
        // this.weeklySchedules[weekNum][sixDayIndex] = []
        this.weeklySchedules[sixDayIndex] = []

        // while (dailySchedule.length > 0) {
        // [mo, mo]
        for (let index = 0; index < dailySchedule.length; index++) {
          const schedule = dailySchedule[index]

          if (currentHour + schedule.hour <= 8) {
            console.log(schedule.name)
            // if (!this.selectedInstructors.includes(schedule.id)) {
            for (let i = 0; i < schedule.hour; i++) {
              this.weeklySchedules[sixDayIndex].push(schedule.name)
            }
            currentHour += schedule.hour
            this.selectedInstructors.push(schedule.id)
            dailySchedule.splice(dailySchedule.indexOf(schedule), 1)
            // }
          }

          if (currentHour === 8) {
            continue
            // break
          }

          // if (dailySchedule.length === 0) {
          //   break
          // }
        }

        //   weekNum++
        // }
      })
      // console.log(this.selectedInstructors)
      // console.log(dailySchedules)

      console.log(this.weeklySchedules)
    },
  },
  head: {
    title: 'Smart Schedule System',
    description: 'Penjadwalan otomatis dengan JavaScript.',
  },
}
</script>
