<template>
  <v-container fluid class="px-3 mt-n6">
    <h1 class="heading margin-nav pl-2">Pilot Roster</h1>
    <v-btn-toggle v-model="profile.RosterView" mandatory>
      <v-btn small icon value="list">
        <v-icon color="accent">mdi-view-list</v-icon>
      </v-btn>
      <v-btn small icon value="table">
        <v-icon color="accent">mdi-format-align-justify</v-icon>
      </v-btn>
      <v-btn small icon value="cards">
        <v-icon color="accent">mdi-view-grid</v-icon>
      </v-btn>
      <v-btn small icon value="small-cards">
        <v-icon color="accent">mdi-grid</v-icon>
      </v-btn>
    </v-btn-toggle>
    <roster-sort :pilots="pilots" @sort="onSort" />
    <v-slide-x-transition mode="out-in">
      <v-container fluid>
        <v-row v-if="profile.RosterView === 'list'" dense>
          <v-col v-for="p in pilots" :key="p.ID" cols="12">
            <pilot-list-item :pilot="p" />
          </v-col>
        </v-row>
        <v-row v-else-if="profile.RosterView === 'cards'">
          <pilot-card v-for="p in pilots" :key="p.ID" :pilot="p" />
        </v-row>
        <v-row v-else-if="profile.RosterView === 'small-cards'">
          <pilot-card v-for="p in pilots" :key="p.ID" :pilot="p" small />
        </v-row>
        <pilot-table v-else :pilots="pilots" class="mx-4" />
        <add-pilot />
      </v-container>
    </v-slide-x-transition>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import RosterSort from './components/RosterSort.vue'
import PilotTable from './components/PilotTable.vue'
import PilotCard from './components/PilotCard.vue'
import PilotListItem from './components/PilotListItem.vue'
import AddPilot from './components/AddPilot.vue'
import { getModule } from 'vuex-module-decorators'
import { CompendiumStore, PilotManagementStore } from '@/store'
import { UserProfile } from '@/io/User'

export default Vue.extend({
  name: 'roster-view',
  components: { RosterSort, PilotTable, AddPilot, PilotListItem, PilotCard },
  data: () => ({
    sortParams: null,
  }),
  computed: {
    profile(): UserProfile {
      const store = getModule(CompendiumStore, this.$store)
      return store.UserProfile
    },
    pilotsUnsorted() {
      const store = getModule(PilotManagementStore, this.$store)
      return store.Pilots
    },
    pilots() {
      if (!this.sortParams) return this.pilotsUnsorted
      return this.$_.orderBy(this.pilotsUnsorted, ...this.sortParams)
    },
  },
  methods: {
    onSort(sortParams: any[]) {
      this.sortParams = sortParams
    },
  },
})
</script>
