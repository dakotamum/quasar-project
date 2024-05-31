<template>
  <GameInfo
    :homeTeam="homeTeam"
    :awayTeam="awayTeam"
    :date="date"
    :time="time"
    :hasRsvped="hasRsvped"
    :homeScore="homeScore"
    :awayScore="awayScore"
    :homeTeamLogo="homeTeamLogo"
    :awayTeamLogo="awayTeamLogo"
  />
  <q-card class="scoreKeeperRefereeSection">
    <q-card-section>
      <span class="label-bold">Scorekeeper:</span> Ur mom
    </q-card-section>
    <q-card-section>
      <span class="label-bold">Referee:</span> yo momma
    </q-card-section>
    <q-card-section>
      <span class="label-bold">Referee:</span> your mother
    </q-card-section>
  </q-card>
  <q-card class="rsvpInfo">
    <q-card-section>
      <span class="label-bold">RSVP</span>
    </q-card-section>
    <div class="q-pa-md column q-col-gutter-sm">
      <q-btn-group vertical>
        <q-btn-toggle
          v-model="selectedItems"
          no-caps
          toggle-color="green"
          color="white"
          text-color="black"
          :options="options"
          unelevated
        />
      </q-btn-group>
    </div>
  </q-card>
  <q-card>
    <q-expansion-item
      label="Team RSVP"
      expand-separator
      header-class="team-rsvp-header"
    >
      <q-card-section class="goingSection">
        <q-card-section class="section-header">
          <q-icon name="check_circle" color="green" size="32px" />
          <span class="going-label">Going</span>
        </q-card-section>
        <q-card-section
          v-for="player in playersGoing"
          :key="player.name"
          class="player-item"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </q-card-section>
      </q-card-section>
      <q-card-section class="subsSection">
        <q-card-section>
          <span class="subs-label">Subs</span>
        </q-card-section>
        <q-card-section
          v-for="(sub, index) in subs"
          :key="index"
          class="sub-item"
        >
          <q-select
            v-model="sub.selectedUser"
            :options="availableUsers"
            label="Select User"
            class="q-mr-sm sub-dropdown"
          />
          <q-btn dense flat icon="close" @click="removeSub(index)" />
        </q-card-section>
        <q-card-section>
          <q-btn
            label="Add Sub"
            @click="addSub"
            color="primary"
            class="q-mt-sm"
          />
        </q-card-section>
      </q-card-section>
      <q-card-section class="notGoingSection">
        <q-card-section class="section-header">
          <q-icon name="cancel" color="red" size="32px" />
          <span class="not-going-label">Not Going</span>
        </q-card-section>
        <q-card-section
          v-for="player in playersNotGoing"
          :key="player.name"
          class="player-item"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </q-card-section>
      </q-card-section>
      <q-card-section class="maybeSection">
        <q-card-section class="section-header">
          <q-icon name="radio_button_unchecked" color="yellow" size="32px" />
          <span class="maybe-label">Maybe</span>
        </q-card-section>
        <q-card-section
          v-for="player in playersMaybe"
          :key="player.name"
          class="player-item"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </q-card-section>
      </q-card-section>
      <q-card-section class="noResponseSection">
        <q-card-section class="section-header">
          <q-icon name="radio_button_unchecked" color="gray" size="32px" />
          <span class="no-response-label">No Response</span>
        </q-card-section>
        <q-card-section
          v-for="player in playersNoResponse"
          :key="player.name"
          class="player-item"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </q-card-section>
      </q-card-section>
    </q-expansion-item>
  </q-card>
  <q-card>
    <q-expansion-item
      label="Officiator RSVP"
      expand-separator
      header-class="officiator-rsvp-header"
    >
      <q-card-section class="officiatorsSection">
        <q-card-section>
          <span class="officiators-label">Officiators</span>
        </q-card-section>
        <q-card-section
          v-for="(official, index) in officials"
          :key="index"
          class="official-item"
        >
          <q-select
            v-model="official.selectedUser"
            :options="availableUsers"
            label="Select Official"
            class="q-mr-sm official-dropdown"
          />
          <q-btn dense flat icon="close" @click="removeOfficial(index)" />
        </q-card-section>
        <q-card-section>
          <q-btn
            label="Add Official"
            @click="addOfficial"
            color="primary"
            class="q-mt-sm"
          />
        </q-card-section>
      </q-card-section>
    </q-expansion-item>
  </q-card>
</template>

<script>
import GameInfo from 'components/GameInfo.vue';

export default {
  components: {
    GameInfo,
  },
  data() {
    return {
      selectedItems: [],
      options: [
        { label: 'I am going', value: 'one' },
        { label: 'I am not going', value: 'two' },
        { label: 'Maybe', value: 'three' },
        { label: 'No response', value: 'four' },
      ],
      playersGoing: [
        { name: 'Player 1', image: 'https://via.placeholder.com/32' },
        { name: 'Player 2', image: 'https://via.placeholder.com/32' },
        { name: 'Player 3', image: 'https://via.placeholder.com/32' },
        { name: 'Player 4', image: 'https://via.placeholder.com/32' },
      ],
      playersNotGoing: [
        { name: 'Player 5', image: 'https://via.placeholder.com/32' },
        { name: 'Player 6', image: 'https://via.placeholder.com/32' },
        { name: 'Player 7', image: 'https://via.placeholder.com/32' },
      ],
      playersMaybe: [
        { name: 'Player 8', image: 'https://via.placeholder.com/32' },
        { name: 'Player 9', image: 'https://via.placeholder.com/32' },
      ],
      playersNoResponse: [
        { name: 'Player 10', image: 'https://via.placeholder.com/32' },
        { name: 'Player 11', image: 'https://via.placeholder.com/32' },
      ],
      subs: [],
      officials: [],
      availableUsers: [
        { label: 'User 1', value: 'user1' },
        { label: 'User 2', value: 'user2' },
        { label: 'User 3', value: 'user3' },
      ],
    };
  },
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
      default: 'src/assets/homeys.png',
    },
    awayTeamLogo: {
      type: String,
      default: 'src/assets/awaygoodteam.png',
    },
  },
  methods: {
    addSub() {
      this.subs.push({ selectedUser: null });
    },
    removeSub(index) {
      this.subs.splice(index, 1);
    },
    addOfficial() {
      this.officials.push({ selectedUser: null });
    },
    removeOfficial(index) {
      this.officials.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.q-btn-toggle__content {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.q-btn-toggle__content .q-btn-toggle__button {
  margin-bottom: 8px;
}
.label-bold {
  font-weight: bold;
}
.section-header {
  display: flex;
  align-items: center;
}
.goingSection .q-icon,
.notGoingSection .q-icon,
.maybeSection .q-icon,
.noResponseSection .q-icon {
  vertical-align: middle;
  margin-right: 8px;
}
.going-label,
.not-going-label,
.maybe-label,
.no-response-label {
  font-weight: bold;
  font-size: 1.2em;
}
.subs-label,
.officiators-label {
  font-weight: bold;
  font-size: 1.2em;
}
.player-item,
.sub-item,
.official-item {
  display: flex;
  align-items: center;
  margin-left: 24px;
}
.q-avatar {
  margin-right: 8px;
}
.q-mt-sm {
  margin-top: 8px;
}
.sub-dropdown,
.official-dropdown {
  width: 200px;
}
.team-rsvp-header .q-item__label,
.officiator-rsvp-header .q-item__label {
  background-color: #ccdcfd !important;
  font-size: 1.2em;
  font-weight: bold;
}
</style>
