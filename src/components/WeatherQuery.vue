<template>
  <div>
    <h1>Weather Query</h1>

    <form @submit.prevent="submitForm">
      <div>
        <label for="location">Location:</label>
        <input list="locations" v-model="location" />
        <datalist id="locations">
          <option v-for="loc in locationsList" :key="loc" :value="loc" />
        </datalist>
      </div>

      <div>
        <label for="date">Date:</label>
        <input type="date" v-model="date" />
      </div>

      <div>
        <label for="hour">Hour:</label>
        <input type="number" v-model="hour" />
      </div>

      <button type="submit">Query</button>
    </form>

    <div v-if="data.length">
      <h2>Results:</h2>
      <table>
        <thead>
          <tr>
            <th>Date</th>
            <th>Hour</th>
            <th>Temperature</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in data" :key="item.date + item.hour">
            <td>{{ item.date }}</td>
            <td>{{ item.hour }}</td>
            <td>{{ item.temperature }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-else>
      <p>No results found.</p>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data() {
    return {
      location: '',
      date: '',
      hour: '',
      locationsList: [], // populate this list from your API
      data: [] // populate this with the query results from your API
    };
  },
  methods: {
    async submitForm() {
        try {
            // Define the URL of your Django backend endpoint
            const url = 'http://127.0.0.1:8000/weather/query/';
            
            // Define the data to be sent in the POST request
            const postData = {
                location: this.location,
                date: this.date,
                hour: this.hour
            };
            
            // Make a POST request to the Django backend
            const response = await axios.post(url, postData);
            
            // Check if the response indicates success
            if (response.data.success) {
                // Update the data in the Vue component with the received data
                this.data = response.data.data;
            } else {
                // Handle the case where the backend response indicates an error
                console.error(response.data.message);
            }
        } catch (error) {
            // Handle network errors or other issues with the request
            console.error(error);
        }
    }
}
};
</script>

<style scoped>
/* Add your CSS styles here */
</style>
