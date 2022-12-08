<template>
  <table class="table">
    <thead>
      <tr>
        <th @click="sortItems('city')">City</th>
        <th @click="sortItems('min')">Min <span>&#8451;</span></th>
        <th @click="sortItems('max')">Max <span>&#8451;</span></th>
        <th class="day">
          Day
          <select v-model="day">
            <option value="0">0 (today)</option>
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
            <option value="4">4</option>
            <option value="5">5</option>
            <option value="6">6</option>
          </select>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in data" :key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.daily.temperature_2m_min[day] }}</td>
        <td>{{ item.daily.temperature_2m_max[day] }}</td>
        <td class="remove-btn" @click="removeCity(item.name)"><span>Remove</span></td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import { onMounted, ref } from 'vue'
export default {
  setup() {
    const cities = ref([
      {
        name: 'Kyiv',
        latitude: 50.45,
        longitude: 30.52
      },
      {
        name: 'New York',
        latitude: 40.71,
        longitude: -74.01
      }, 
      {
        name: 'Geneva',
        latitude: 46.20,
        longitude: 6.15
      }, 
      {
        name: 'Tokyo',
        latitude: 35.69,
        longitude: 139.69
      }, 
      {
        name: 'Honolulu',
        latitude: 21.31,
        longitude: -157.86
      }
    ])
    const data = ref([])
    const day = ref(0)

    const load = () => {
      try {
        cities.value.map(async city => {
          const res = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${city.latitude}&longitude=${city.longitude}&daily=temperature_2m_max,temperature_2m_min&current_weather=true&timezone=Africa%2FCairo`)
          data.value.push({name: city.name, ...await res.json()})
        })
      } catch (e) {}
    }

    const removeCity = city => {
      data.value = data.value.filter(i => i.name !== city)
    }

    const sortItems = sortBy => {
      data.value.sort((a, b) => {
        if (sortBy === 'city') { 
          return a.name.localeCompare(b.name)
        } else if (sortBy === 'min') {
          return a.daily.temperature_2m_min[0] - b.daily.temperature_2m_min[0]
        } else {
          return b.daily.temperature_2m_max[0] - a.daily.temperature_2m_max[0]
        }
      })
    }

    onMounted(() => {
      load()
    })
    
    return {
      data, day, removeCity, sortItems
    }
  }
}
</script>

<style>
.table {
  width: 700px;
  margin: 30px auto;
}
thead {
  background-color: rgba(76, 136, 32, 0.429);
  color: #363636;
  cursor: pointer;
}
th, td {
  padding: 20px;
  border-radius: 5px;
}
th span {
  font-size: 20px;
}
.remove-btn span {
  cursor: pointer;
  border: 1px solid #363636;
  border-radius: 50px;
  padding: 10px;
  font-size: 20px;
  transition: .3s;
}
.remove-btn span:hover {
  cursor: pointer;
  background-color: #ba4b4b;
  border-color: #ba4b4b;
  color: bisque;
  transition: .3s;
}
th.day {
  display: flex;
}
select {
  background-color: rgba(76, 136, 32, 0.429);
  padding: 5px;
  border: 1px solid #363636;
  border-radius: 20px;
  color: bisque;
  outline: none;
  margin-left: 10px;
  font-size: 16px;
  align-self: flex-end;
}
option {
  color: #363636;
}
</style>