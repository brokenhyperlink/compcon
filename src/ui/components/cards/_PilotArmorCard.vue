<template>
  <equipment-card-base :item="item">
    <v-container slot="statblock" grid-list-md class="mt-0 pt-1">
      <v-row justify-space-between wrap fill-height>
        <cc-statblock-panel name="Armor" :value="item.Armor" />
        <cc-statblock-panel name="HP Bonus" :value="`+ ${item.HPBonus}`" />
        <cc-statblock-panel name="E-Defense" :value="item.EDefense" />
        <cc-statblock-panel name="Evasion" :value="item.Evasion" />
        <cc-statblock-panel name="Speed" :value="item.Speed" />
      </v-row>
    </v-container>
  </equipment-card-base>
</template>

<script lang="ts">
import Vue from 'vue'
import EquipmentCardBase from './_EquipmentCardBase.vue'

export default Vue.extend({
  name: 'cc-mech-weapon-card',
  components: { EquipmentCardBase },
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
})
</script>
