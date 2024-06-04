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
  <q-card class="q-mb-md">
    <q-card-section>
      <div><strong>Scorekeeper:</strong> Ur mom</div>
      <div><strong>Referee:</strong> yo momma</div>
      <div><strong>Referee:</strong> your mother</div>
    </q-card-section>
  </q-card>
  <q-card class="q-mb-md">
    <q-card-section>
      <strong>RSVP</strong>
    </q-card-section>
    <q-card-section>
      <div class="q-pa-lg">
        <q-option-group
          v-model="selectedItems"
          :options="options"
          color="red"
          left-label
        />
      </div>
    </q-card-section>
  </q-card>
  <q-card class="q-mb-md">
    <q-expansion-item label="Team RSVP" expand-separator>
      <q-card-section>
        <q-icon name="check_circle" color="green" size="32px" />
        <strong>Going</strong>
        <div
          v-for="player in playersGoing"
          :key="player.name"
          class="q-ml-md q-mt-sm"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </div>
      </q-card-section>
      <q-card-section>
        <strong>Subs</strong>
        <div
          v-for="(sub, index) in subs"
          :key="index"
          class="q-mt-sm q-ml-md q-row no-wrap items-center"
        >
          <q-select
            v-model="sub.selectedUser"
            :options="availableUsers"
            label="Select User"
            dense
            class="q-mr-sm"
          />
          <q-btn dense flat icon="close" @click="removeSub(index)" />
        </div>
        <q-btn
          label="Add Sub"
          @click="addSub"
          color="primary"
          class="q-mt-sm q-ml-md"
        />
      </q-card-section>
      <q-card-section>
        <q-icon name="cancel" color="red" size="32px" />
        <strong>Not Going</strong>
        <div
          v-for="player in playersNotGoing"
          :key="player.name"
          class="q-ml-md q-mt-sm"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </div>
      </q-card-section>
      <q-card-section>
        <q-icon :name="maybeIcon" color="yellow" size="32px" />
        <strong>Maybe</strong>
        <div
          v-for="player in playersMaybe"
          :key="player.name"
          class="q-ml-md q-mt-sm"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </div>
      </q-card-section>
      <q-card-section>
        <q-icon name="radio_button_unchecked" color="gray" size="32px" />
        <strong>No Response</strong>
        <div
          v-for="player in playersNoResponse"
          :key="player.name"
          class="q-ml-md q-mt-sm"
        >
          <q-avatar size="32px" class="q-mr-sm">
            <img :src="player.image" alt="player profile" />
          </q-avatar>
          {{ player.name }}
        </div>
      </q-card-section>
    </q-expansion-item>
  </q-card>
  <q-card>
    <q-expansion-item label="Officiator RSVP" expand-separator>
      <q-card-section>
        <strong>Officiators</strong>
        <div
          v-for="(official, index) in officials"
          :key="index"
          class="q-mt-sm q-ml-md q-row no-wrap items-center"
        >
          <q-select
            v-model="official.selectedUser"
            :options="availableUsers"
            label="Select Official"
            dense
            class="q-mr-sm"
          />
          <q-btn dense flat icon="close" @click="removeOfficial(index)" />
        </div>
        <q-btn
          label="Add Official"
          @click="addOfficial"
          color="primary"
          class="q-mt-sm q-ml-md"
        />
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
      selectedItems: 'four', // Setting default value to 'four' which corresponds to 'No response'
      options: [
        { label: 'I am going', value: 'one' },
        { label: 'I am not going', value: 'two' },
        { label: 'Maybe', value: 'three' },
        { label: 'No response', value: 'four' },
      ],
      maybeIcon: 'img:src/assets/yellow-dot-icon.svg', // Use custom SVG icon
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
.q-row {
  display: flex;
  align-items: center;
}
</style>
