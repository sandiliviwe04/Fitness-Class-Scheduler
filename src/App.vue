<template>
  <div id="app">
    <h1>Book A Fitness Session</h1>
    <input v-model="searchQuery" placeholder="Search sessions..." />
    <form @submit.prevent="addSession">
      <input v-model="newSession.name" placeholder="Class Name" />
      <input v-model="newSession.coach" placeholder="Coach" />
      <input v-model="newSession.date" type="date" />
      <input v-model="newSession.time" type="time" />
      <input v-model.number="newSession.capacity" type="number" placeholder="Capacity" />
      <button type="submit">Add Session</button>
    </form>
    <ul>
      <li v-for="(session, index) in filteredSessions" :key="index">
        {{ session.name }} with {{ session.coach }} on {{ session.date }} at {{ session.time }} (Capacity: {{ session.capacity }})
        <button @click="deleteSession(index)">Delete</button>
      </li>
    </ul>
    <p v-if="sessions.length === 0">No sessions scheduled</p>
    <p>Total Sessions: {{ sessions.length }}</p>
    <div v-if="nextSession" class="next-session">
      <h2>Next Session:</h2>
      <p>{{ nextSession.name }} with {{ nextSession.coach }} on {{ nextSession.date }} at {{ nextSession.time }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newSession: {
        name: '',
        coach: '',
        date: '',
        time: '',
        capacity: null
      },
      sessions: [],
      searchQuery: ''
    }
  },
  computed: {
    filteredSessions() {
      return this.sessions.filter(session => {
        const query = this.searchQuery.toLowerCase();
        return (
          session.name.toLowerCase().includes(query) ||
          session.coach.toLowerCase().includes(query) ||
          session.date.includes(query)
        );
      });
    },
    nextSession() {
      const today = new Date();
      const upcomingSessions = this.sessions.filter(session => {
        const sessionDate = new Date(session.date);
        return sessionDate >= today;
      });
      upcomingSessions.sort((a, b) => new Date(a.date) - new Date(b.date));
      return upcomingSessions[0];
    }
  },
  methods: {
    addSession() {
      if (this.validateSession()) {
        this.sessions.push({ ...this.newSession });
        this.newSession = {
          name: '',
          coach: '',
          date: '',
          time: '',
          capacity: null
        };
      }
    },
    deleteSession(index) {
      this.sessions.splice(index, 1);
    },
    validateSession() {
      // same validation logic as before
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 800px;
  margin: 0 auto;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  margin: 10px;
  padding: 10px;
  width: 200px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  margin: 10px;
  padding: 10px 20px;
  background-color: #4CAF50;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #3e8e41;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  margin: 10px;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

li:last-child {
  border-bottom: none;
}

.next-session {
  background-color: #f0f0f0;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 20px;
}
</style>