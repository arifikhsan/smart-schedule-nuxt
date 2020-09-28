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
    <div v-for="(dailySchedule, iw) in weeklySchedules" :key="iw">
      <div class="py-4">
        <div class="py-4">
          <h2 class="text-2xl font-medium text-green-500">
            Jadwal Minggu Depan #{{ iw + 1 }}
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
            <tr v-for="(s, id) in dailySchedule" :key="id">
              <td class="px-4 py-2 border">
                {{ 'schedule.range' }}
              </td>
              <td class="px-4 py-2 text-green-500 border">{{ s[0] }}</td>
              <td class="px-4 py-2 text-green-500 border">
                {{ s[1] }}
              </td>
              <td class="px-4 py-2 text-green-500 border">
                {{ s[2] }}
              </td>
              <td class="px-4 py-2 text-green-500 border">
                {{ s[3] }}
              </td>
              <td class="px-4 py-2 text-green-500 border">{{ s[4] }}</td>
              <td class="px-4 py-2 text-green-500 border">
                {{ s[5] }}
              </td>
            </tr>
          </tbody>
        </table>
        <div class="flex items-start justify-start mt-4">
          <div class="flex flex-col justify-start">
            <p>Total jam: {{ nextWeekHourCount(dailySchedule) }} Jam</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import faker from 'faker'

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
          name: `i ${index}`,
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
    nextWeekHourCount(dailySchedule) {
      let hour = 0
      const a = dailySchedule.filter((e) => !!e)
      a.forEach((s) => {
        const b = s.filter((e) => !!e)
        if (b.length > 1) {
          hour += b.length
        }
      })
      return hour
    },
    makeSchedule() {
      const currentSchedule = [...this.inputSchedules]
      this.weeklySchedules = []

      const bl = []
      let week = 0
      while (bl.length < this.inputSchedules.length) {
        this.weeklySchedules[week] = []

        for (let id = 0; id < 6; id++) {
          this.weeklySchedules[week][id] = []
          currentSchedule.forEach((c, ci) => {
            if (!c.days[id]) return
            if (bl.includes(c)) return

            if (this.weeklySchedules[week][id].length + c.hour <= 8) {
              for (let ih = 0; ih < c.hour; ih++) {
                this.weeklySchedules[week][id].push(c.name)
              }
              currentSchedule.splice(ci, 1)
              bl.push(c)
            }
          })
        }

        week++
        if (week > 1) {
          console.log('infinite loop aaaaaaaaaa')
          console.log(week)
          console.log('bl', bl.length)
          console.log('currentSchedule', currentSchedule.length)
          console.log('this.inputSchedules', this.inputSchedules.length)
          break
        }
      }


      const transpose = (matrix) => {
        const [row] = matrix
        return row.map((value, column) => matrix.map((row) => row[column]))
      }

      // console.log(this.weeklySchedules)
      this.weeklySchedules = this.weeklySchedules.map((w) => transpose(w))
      // console.log(this.weeklySchedules)
    },
  },

  head: {
    title: 'Smart Schedule System',
    description: 'Penjadwalan otomatis dengan JavaScript.',
  },
}
</script>
