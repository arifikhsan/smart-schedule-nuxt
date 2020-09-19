<template>
  <div class="max-w-6xl mx-auto p-4">
    <div class="text-center pt-10 pb-5">
      <h1 class="text-green-500 font-bold text-5xl">Smart Schedule</h1>
    </div>
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">Form Isian</h2>
      </div>
      <table class="table-auto overflow-auto w-full">
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
          <!-- display rows -->
          <template v-if="inputSchedules.length > 0">
            <tr v-for="row in inputSchedules" :key="row.id">
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
          <tr>
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
          <!-- <tr v-for="item in inputSchedules" v-else :key="item.name">
            <td class="border px-4 py-2">{{ item.name }}</td>
            <td class="border px-4 py-2">{{ item.hour }}</td>
            <td v-for="day in item.days" :key="day" class="border px-4 py-2">
              {{ day }}
            </td>
          </tr> -->
        </tbody>
      </table>
      <div class="mt-4 flex justify-end space-x-4">
        <button
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
          @click="addRow"
        >
          Selesai ini pengajar dan tambah lagi
        </button>
        <button
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
          @click="makeSchedule"
        >
          Buat jadwal
        </button>
      </div>
    </div>

    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">Form Hasil</h2>
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
        name: 'Tes Nama',
        hour: 2,
        days: {
          monday: false,
          tuesday: true,
          wednesday: false,
          thursday: true,
          friday: false,
          saturday: true
        }
      },
      newSchedule: {},
      inputSchedules: []
    }
  },
  created() {
    this.newSchedule = {
      id: 0,
      name: 'Tes Nama',
      hour: 2,
      days: {
        monday: false,
        tuesday: true,
        wednesday: false,
        thursday: true,
        friday: false,
        saturday: true
      }
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
        name: 'Tes Nama',
        hour: 2,
        days: {
          monday: false,
          tuesday: true,
          wednesday: false,
          thursday: true,
          friday: false,
          saturday: true
        }
      }
      this.inputSchedules.push(copyNewSchedule)
    },
    makeSchedule() {
      console.log('makeSchedule')
    }
  },
  head: {
    title: 'Smart Schedule',
    description: 'Penjadwalan otomatis dengan JavaScript.'
  }
}
</script>
