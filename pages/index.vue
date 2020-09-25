<template>
  <div class="max-w-6xl p-4 pb-20 mx-auto">
    <div class="pt-10 pb-5 text-center">
      <h1 class="text-5xl font-bold text-green-500">Smart Scheduling System</h1>
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
          <tr v-for="schedule in nextWeekSchedules" :key="schedule.id">
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.mo }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.tu }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.we }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.th }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.fr }}</td>
            <td class="px-4 py-2 text-red-500 border">
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
          <tr v-for="schedule in nextWeekPostponedSchedules" :key="schedule.id">
            <td class="px-4 py-2 border">
              {{ schedule.range }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.mo }}</td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.tu }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.we }}
            </td>
            <td class="px-4 py-2 text-red-500 border">
              {{ schedule.th }}
            </td>
            <td class="px-4 py-2 text-red-500 border">{{ schedule.fr }}</td>
            <td class="px-4 py-2 text-red-500 border">
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
    </div>
  </div>
</template>

<script>
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
      inputSchedules: [],
      nextWeekSchedules: [],
      nextWeekPostponedSchedules: [],
    }
  },
  created() {
    this.newSchedule = {
      id: 0,
      name: '',
      hour: 1,
      days: [false, false, false, false, false, false],
    }
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
        this.newSchedule = {
          id: 0,
          name: '',
          hour: 1,
          days: [false, false, false, false, false, false],
        }
        this.inputSchedules.push(copyNewSchedule)
      }
    },
    populate() {
      this.inputSchedules = []
      let index = 0
      while (this.currentInputScheduleTimeLength() < 60) {
        this.inputSchedules.push({
          id: index,
          name: faker.name.findName(),
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
        const hourPerPerson =
          schedule.hour * schedule.days.filter((day) => day === true).length
        count += hourPerPerson
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
      // this.nextWeekSchedules.map((e) => (hour += e.name))
      return hour
    },
    nextWeekPostponedHourCount() {
      let count = 0
      // this.nextWeekPostponedSchedules.map((e) => (count += e.hour))
      return count
    },
    countHourInSchedule(arr) {
      let count = 0
      arr.map((e) => (count += e.hour))
      return count
    },
    makeSchedule() {
      const currentSchedules = [...this.inputSchedules]
      const outputSchedules = []
      // let totalTeachingHours = 0

      // console.log(currentSchedules)
      const mo = []
      const tu = []
      const we = []
      const th = []
      const fr = []
      const sa = []

      const moNext = []
      const tuNext = []
      const weNext = []
      const thNext = []
      const frNext = []
      const saNext = []

      let hours = 0
      currentSchedules.forEach((s) => {
        // currentSchedules.forEach((s) => {
        // const o = {}
        // do {

        if (hours < 48) {
          if (s.days[0]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              mo.push(s.name)
            }
          }
        } else if (s.days[0]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            moNext.push(s.name)
          }
        }

        if (hours < 48) {
          if (s.days[1]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              tu.push(s.name)
            }
          }
        } else if (s.days[1]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            tuNext.push(s.name)
          }
        }

        if (hours < 48) {
          if (s.days[2]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              we.push(s.name)
            }
          }
        } else if (s.days[2]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            weNext.push(s.name)
          }
        }

        if (hours < 48) {
          if (s.days[3]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              th.push(s.name)
            }
          }
        } else if (s.days[3]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            thNext.push(s.name)
          }
        }

        if (hours < 48) {
          if (s.days[4]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              fr.push(s.name)
            }
          }
        } else if (s.days[4]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            frNext.push(s.name)
          }
        }

        if (hours < 48) {
          if (s.days[5]) {
            hours += s.hour
            for (let i = 0; i < s.hour; i++) {
              sa.push(s.name)
            }
          }
        } else if (s.days[5]) {
          hours += s.hour
          for (let i = 0; i < s.hour; i++) {
            saNext.push(s.name)
          }
        }

        // } while (hours < )

        // if (s.days[1]) o.t = s.name
        // if (s.days[2]) o.w = s.name
        // if (Object.keys(o).length > 0) {
        //   m.push(o)
        // }
        // })

        // console.log(...m)
        // for (let indexDay = 0; indexDay < 6; indexDay++) {

        // }
      })
      // console.log(mo)
      console.log(hours)
      const max = [
        mo.length,
        tu.length,
        we.length,
        th.length,
        fr.length,
        sa.length,
      ]
        .sort()
        .pop()
      const maxNext = [
        moNext.length,
        tuNext.length,
        weNext.length,
        thNext.length,
        frNext.length,
        saNext.length,
      ]
        .sort()
        .pop()

      // for (let index = 0; index < 8; index++) {
      //   const dailySchedule = {
      //     range: `${initTime.format('H:mm')} - ${initTime
      //       .add(1, 'hours')
      //       .format('H:mm')}`,

      const initTime = this.$moment('08:00', 'H:mm')
      for (let index = 0; index < max; index++) {
        this.nextWeekSchedules.push({
          id: index,
          range: `${initTime.format('H:mm')} - ${initTime
            .add(1, 'hours')
            .format('H:mm')}`,
          mo: mo.shift(),
          tu: tu.shift(),
          we: we.shift(),
          th: th.shift(),
          fr: fr.shift(),
          sa: sa.shift(),
        })
      }

      const initTimeNext = this.$moment('08:00', 'H:mm')
      for (let index = 0; index < maxNext; index++) {
        this.nextWeekPostponedSchedules.push({
          id: index,
          range: `${initTimeNext.format('H:mm')} - ${initTimeNext
            .add(1, 'hours')
            .format('H:mm')}`,
          mo: moNext.shift(),
          tu: tuNext.shift(),
          we: weNext.shift(),
          th: thNext.shift(),
          fr: frNext.shift(),
          sa: saNext.shift(),
        })
      }

      console.log(this.nextWeekSchedules)
      console.log(this.nextWeekPostponedSchedules)

      console.log(this.nextWeekSchedules)

      // this.nextWeekSchedules = outputSchedules
      // this.nextWeekPostponedSchedules =

      // const nextWeek = []
      // outputSchedules.forEach(s => {

      // })
      // console.log(outputSchedules)

      // console.log(nextWeekSchedulesPerPerson)
      // console.log(totalTeachingHours)

      // let what = 0
      // nextWeekSchedules.forEach((schedule) => {
      //   schedule.forEach((day) => {})
      // })
      // pilih random array
      // sampai ketemu >= 48 jam
      // yang sudah 48, keluarkan dari array

      // const maxHour = 48
      // let nextWeekHour = 0
      // const nextWeekSchedules = []
      // const currentSchedules = this.inputSchedules
      // console.log(currentSchedules)

      // cari jadwal sampai angka >= 48 jam

      // for (const schedule of currentSchedules) {
      //   nextWeekHour += schedule.hour
      //   nextWeekSchedules.push(schedule)
      //   if (nextWeekHour >= maxHour) {
      //     break
      //   }
      // }

      // if (nextWeekHour === maxHour) {
      //   this.nextWeekSchedules = nextWeekSchedules
      // } else {
      //   // kurangi satu jika ada 49
      //   const offsetSchedules = nextWeekSchedules.filter((e) => e.hour === 1)
      //   // cari yang terakhir
      //   const offsetSchedule = offsetSchedules[offsetSchedules.length - 1]
      //   // console.log(nextWeekHour)
      //   // console.log(offsetSchedules)
      //   // console.log(offsetSchedule)

      //   nextWeekSchedules.splice(nextWeekSchedules.indexOf(offsetSchedule, 1))
      //   this.nextWeekSchedules = nextWeekSchedules
      // }

      // console.log(this.nextWeekSchedules.length)

      // cari jadwal minggu tertunda

      // const remainingSchedules = this.inputSchedules.filter(
      //   (e) => !this.nextWeekSchedules.includes(e)
      // )
      // // console.log(remainingSchedules)
      // this.nextWeekPostponedSchedules = remainingSchedules

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

      // const initTime = this.$moment('08:00', 'H:mm')
      // for (let index = 0; index < 8; index++) {
      //   const dailySchedule = {
      //     range: `${initTime.format('H:mm')} - ${initTime
      //       .add(1, 'hours')
      //       .format('H:mm')}`,
      //     monday: 'Not Implemented Error',
      //     tuesday: 'Not Implemented Error',
      //     wednesday: 'Not Implemented Error',
      //     thursday: 'Not Implemented Error',
      //     friday: 'Not Implemented Error',
      //     saturday: 'Not Implemented Error',
      //   }
      //   this.nextWeekSchedulesInTable.push(dailySchedule)
      //   this.nextWeekPostponedSchedulesInTable.push(dailySchedule)
      // }
    },
  },
  head: {
    title: 'Smart Scheduling System',
    description: 'Penjadwalan otomatis dengan JavaScript.',
  },
}
</script>
