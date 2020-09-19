<template>
  <div class="max-w-6xl mx-auto">
    <div class="text-center pt-10 pb-5">
      <h1 class="text-green-500 font-bold text-5xl">Smart Schedule</h1>
    </div>
    <div class="py-4">
      <div class="py-4">
        <h2 class="text-green-500 font-medium text-2xl">Form Isian</h2>
      </div>
      <table class="table-auto w-full">
        <thead class="text-gray-800">
          <tr>
            <th class="border px-4 py-2">Nama Instruktur</th>
            <th class="border px-4 py-2">Jumlah Jam Pelatihan</th>
            <th class="border px-4 py-2">Senin</th>
            <th class="border px-4 py-2">Selasa</th>
            <th class="border px-4 py-2">Rabu</th>
            <th class="border px-4 py-2">Kamis</th>
            <th class="border px-4 py-2">Jumat</th>
            <th class="border px-4 py-2">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-if="inputForm.length === 0">
            <td class="border px-4 py-2">
              <input
                class="form-input mt-1 block w-full"
                placeholder="Nama"
                type="text"
              />
            </td>
            <td class="border px-4 py-2">
              <input
                class="form-input mt-1 block w-full"
                placeholder="Jam"
                type="number"
              />
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
            <td class="border px-4 py-2">
              <select class="form-select mt-1 block w-full">
                <option :value="false">Tidak</option>
                <option :value="true">Ya</option>
              </select>
            </td>
          </tr>
          <tr v-for="item in inputForm" v-else :key="item.name">
            <td class="border px-4 py-2">{{ item.name }}</td>
            <td class="border px-4 py-2">{{ item.hour }}</td>
            <td v-for="day in item.days" :key="day" class="border px-4 py-2">
              {{ day }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="mt-4 flex justify-end">
        <button
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
        >
          Tambah baris
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
            <th class="border px-4 py-2">Jumlah Jam Pelatihan</th>
            <th class="border px-4 py-2">Senin</th>
            <th class="border px-4 py-2">Selasa</th>
            <th class="border px-4 py-2">Rabu</th>
            <th class="border px-4 py-2">Kamis</th>
            <th class="border px-4 py-2">Jumat</th>
            <th class="border px-4 py-2">Sabtu</th>
          </tr>
        </thead>
        <tbody class="text-gray-700">
          <tr v-for="item in form" :key="item.name">
            <td class="border px-4 py-2">{{ item.name }}</td>
            <td class="border px-4 py-2">{{ item.hour }}</td>
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
const senin = 'senin'
const selasa = 'selasa'
const rabu = 'rabu'
const kamis = 'kamis'
const jumat = 'jumat'
const sabtu = 'sabtu'

export default {
  data() {
    return {
      totalHour: 61,
      inputForm: [],
      form: [
        {
          name: 'Adi',
          hour: 3,
          days: [senin, selasa, rabu]
        },
        {
          name: 'Beni',
          hour: 2,
          days: [rabu, kamis]
        }
      ]
    }
  }
}
</script>
