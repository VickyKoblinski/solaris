<template>
<div class="row bg-primary">
    <div class="col">
      <div class="table-responsive mb-0">
        <table class="table table-sm mb-1" v-if="research">
            <tbody>
                <tr>
                    <td class="row-icon"><i :class="getIcon('scanning')"></i></td>
                    <td>Scanning</td>
                    <td>Level {{research.scanning.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('scanning')">{{research.scanning.progress}} of {{getRequiredTotal('scanning')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('hyperspace')"></i></td>
                    <td>Hyperspace Range</td>
                    <td>Level {{research.hyperspace.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('hyperspace')">{{research.hyperspace.progress}} of {{getRequiredTotal('hyperspace')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('terraforming')"></i></td>
                    <td>Terraforming</td>
                    <td>Level {{research.terraforming.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('terraforming')">{{research.terraforming.progress}} of {{getRequiredTotal('terraforming')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('experimentation')"></i></td>
                    <td>Experimentation</td>
                    <td>Level {{research.experimentation.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('experimentation')">{{research.experimentation.progress}} of {{getRequiredTotal('experimentation')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('weapons')"></i></td>
                    <td>Weapons</td>
                    <td>Level {{research.weapons.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('weapons')">{{research.weapons.progress}} of {{getRequiredTotal('weapons')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('banking')"></i></td>
                    <td>Banking</td>
                    <td>Level {{research.banking.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('banking')">{{research.banking.progress}} of {{getRequiredTotal('banking')}}</span></td>
                </tr>
                <tr>
                    <td class="row-icon"><i :class="getIcon('manufacturing')"></i></td>
                    <td>Manufacturing</td>
                    <td>Level {{research.manufacturing.level}}</td>
                    <td class="text-right"><span v-if="isTechnologyEnabled('manufacturing')">{{research.manufacturing.progress}} of {{getRequiredTotal('manufacturing')}}</span></td>
                </tr>
            </tbody>
        </table>
      </div>
    </div>
</div>
</template>

<script>
import GameHelper from '../../../services/gameHelper'
import TechnologyHelper from '../../../services/technologyHelper'

export default {
  data: function () {
    return {
      research: null
    }
  },
  mounted () {
    this.research = GameHelper.getUserPlayer(this.$store.state.game).research
  },
  methods: {
    getRequiredTotal (technologyKey) {
      const game = this.$store.state.game

      const researchCostConfig = game.settings.technology.researchCosts[technologyKey]
      const expenseCostConfig = game.constants.star.infrastructureExpenseMultipliers[researchCostConfig]
      const progressMultiplierConfig = expenseCostConfig * game.constants.research.progressMultiplier

      return this.research[technologyKey].level * progressMultiplierConfig
    },
    isTechnologyEnabled (technologyKey) {
      return TechnologyHelper.isTechnologyEnabled(this.$store.state.game, technologyKey)
    },
    getIcon (technologyKey) {
      return 'fas fa-' + TechnologyHelper.getIcon(technologyKey)
    }
  }
}
</script>

<style scoped>
.row-icon {
    width: 1%;
}
</style>
