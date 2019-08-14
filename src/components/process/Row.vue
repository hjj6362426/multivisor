<template>
  <tr>
    <td class="px-0"
        style="height:30px;">
      <v-checkbox primary hide-details v-model="selectedProcesses"
                  :value="process.uid" >
      </v-checkbox>
    </td>
    <td class="px-0" style="height:30px;">
      {{ process.name }}
    </td>
    <td v-if="showGroup" class="hidden-xs-only px-0" style="height:30px;">
        {{ row.item.group }}
    </td>
    <td v-if="showSupervisor"
        class="hidden-xs-only px-0" style="height:30px;">
      {{ process.supervisor }}
    </td>
    <td class="px-0" style="height:30px;">
      <v-chip disabled label :color="stateColorMap[process.statename]"
              text-color="white" small>
              {{ process.statename }}
      </v-chip>
    </td>

    <td class="layout px-0" style="height:30px;">
      <v-btn icon small @click="restartProcess(process)"  class="mx-0 my-1">
        <v-icon color="green">
          <template v-if="row.item.running">autorenew</template>
          <template v-else>play_arrow</template>
        </v-icon>
      </v-btn>
      <v-btn icon small @click="stopProcess(process)"
             :disabled="!row.item.running" class="mx-0 my-1">
        <v-icon color="red">stop</v-icon>
      </v-btn>
      <v-btn icon small @click="viewDetails(process)" class="mx-0 my-1">
        <v-icon color="blue">info</v-icon>
      </v-btn>
       <v-btn icon small @click="viewLog(process, 'out')" v-if="process.logfile" class="mx-0 my-1">
        <v-icon color="blue">description</v-icon>
      </v-btn>
        <v-btn icon small @click="viewLog(process, 'err')" v-if="process.stderr_logfile" class="mx-0 my-1">
        <v-icon color="orange">description</v-icon>
      </v-btn>

    </td>
  </tr>
</template>

<script>
import { stateColorMap } from '../../multivisor'

export default {
  name: 'ProcessRow',
  props: [ 'row', 'show-supervisor', 'show-group' ],
  data () { return { stateColorMap: stateColorMap } },
  computed: {
    process () { return this.row.item },
    selectedProcesses: {
      get () { return this.$store.state.selectedProcesses },
      set (v) { this.$store.commit('setSelectedProcesses', v) }
    }
  },
  methods: {
    restartProcess (process) {
      this.$store.dispatch('restartProcesses', [process.uid])
    },
    stopProcess (process) {
      this.$store.dispatch('stopProcesses', [process.uid])
    },
    viewLog (process, stream) {
      this.$store.commit('setLog', {
        process,
        stream,
        visible: true
      })
    },
    viewDetails (process) {
      this.$store.commit('setProcessDetails', {
        process,
        visible: true
      })
    }
  }
}
</script>
