<template>
  <q-card class="match-info">
    <q-card-section class="q-pa-xs">
      <q-item class="team-wrapper">
        <q-item-section avatar>
          <q-img :src="homeTeamLogo" alt="Logo" class="team-logo" />
        </q-item-section>
        <q-item-section class="team-details">
          <q-item-label
            :class="{ 'extra-bold-text': homeScore > awayScore }"
            class="team-name"
          >
            {{ homeTeam }}
          </q-item-label>
          <q-item-label caption class="home-away-label">Home</q-item-label>
        </q-item-section>
        <q-item-section side class="score-section">
          <q-item-label
            :class="{ 'extra-bold-text': homeScore > awayScore }"
            class="score"
          >
            {{ homeScore !== null ? homeScore : '' }}
          </q-item-label>
        </q-item-section>
      </q-item>
    </q-card-section>
    <q-card-section class="q-pa-xs">
      <q-item class="team-wrapper">
        <q-item-section avatar>
          <q-img :src="awayTeamLogo" alt="Logo" class="team-logo" />
        </q-item-section>
        <q-item-section class="team-details">
          <q-item-label
            :class="{ 'extra-bold-text': awayScore > homeScore }"
            class="team-name"
          >
            {{ awayTeam }}
          </q-item-label>
          <q-item-label caption class="home-away-label">Away</q-item-label>
        </q-item-section>
        <q-item-section side class="score-section">
          <q-item-label
            :class="{ 'extra-bold-text': awayScore > homeScore }"
            class="score"
          >
            {{ awayScore !== null ? awayScore : '' }}
          </q-item-label>
        </q-item-section>
      </q-item>
    </q-card-section>
    <q-card-section class="q-pa-xs">
      <q-item class="datetime-wrapper">
        <q-item-section class="datetime-details">
          <q-item-label>{{ date }}</q-item-label>
          <q-item-label>{{ time }}</q-item-label>
        </q-item-section>
        <q-item-section side class="rsvp-section">
          <q-btn v-if="!hasRsvped" size="sm" color="primary" @click="goToRsvp"
            >RSVP</q-btn
          >
          <q-icon
            v-else
            name="check_circle"
            color="green"
            size="md"
            class="checkmark"
          />
        </q-item-section>
      </q-item>
    </q-card-section>
  </q-card>
</template>

<script>
export default {
  props: {
    homeTeam: {
      type: String,
      default: 'The Homeys',
    },
    awayTeam: {
      type: String,
      default: 'A Way Good Team',
    },
    date: {
      type: String,
      default: 'Wed, Jan 25, 2024',
    },
    time: {
      type: String,
      default: '9:00 - 10:15 PM',
    },
    hasRsvped: {
      type: Boolean,
      default: false,
    },
    homeScore: {
      type: Number,
      default: 0,
    },
    awayScore: {
      type: Number,
      default: 0,
    },
    homeTeamLogo: {
      type: String,
      default: 'src/components/homeys.png',
    },
    awayTeamLogo: {
      type: String,
      default: 'src/components/awaygoodteam.png',
    },
  },
  methods: {
    goToRsvp() {
      this.$router.push({ path: '/rsvp' });
    },
  },
};
</script>

<style scoped>
.match-info {
  display: flex;
  flex-direction: column;
  padding-left: 2%;
  border-left: 5px solid #eb5031;
  margin: 2%;
}
.team-wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin-bottom: 0; /* Removed margin */
  padding: 2vw 0; /* Responsive padding */
}
.team-logo {
  width: 8vw; /* Responsive size */
  height: 8vw; /* Responsive size */
  border-radius: 10%;
}
.team-details {
  flex-grow: 1; /* Allow to take up remaining space */
  padding-left: 2vw; /* Responsive padding */
}
.team-name {
  font-size: 3.5vw; /* Responsive font size */
}
.home-away-label {
  font-size: 2.5vw; /* Responsive font size */
}
.extra-bold-text {
  font-weight: 900; /* Extra bold font weight */
  color: black; /* Ensure the text color is black */
}
.score-section {
  flex-shrink: 0; /* Prevent shrinking */
  margin-left: auto; /* Push to the right */
  text-align: right; /* Align score to the right */
  padding-right: 2vw; /* Responsive padding */
}
.score {
  text-align: right; /* Align score to the right */
  color: black; /* Ensure the text color is black */
  font-size: 3.5vw; /* Responsive font size */
}
.datetime-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2vw 0; /* Responsive padding */
}
.datetime-details {
  flex-grow: 1; /* Allow to take up remaining space */
}
.rsvp-section {
  flex-shrink: 0; /* Prevent shrinking */
  margin-left: auto; /* Push to the right */
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-right: 2vw; /* Responsive padding */
}
.checkmark {
  font-size: 4vw; /* Responsive font size */
}

@media (min-width: 768px) {
  .team-logo {
    width: 4vw; /* Smaller size on larger screens */
    height: 4vw;
  }
  .team-name {
    font-size: 2vw; /* Adjust font size */
  }
  .home-away-label {
    font-size: 1.5vw; /* Adjust font size */
  }
  .score {
    font-size: 2vw; /* Adjust font size */
  }
  .checkmark {
    font-size: 2.5vw; /* Adjust font size */
  }
}
</style>
