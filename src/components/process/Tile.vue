<template lang="html">
  <v-list-tile>
    <v-list-tile-action>
      <v-checkbox hide-details v-model="selectedProcesses"
                  :value="process.uid"></v-checkbox>
    </v-list-tile-action>
    <v-list-tile-content>
      <v-list-tile-title>{{ process.name }}</v-list-tile-title>
    </v-list-tile-content>

    <v-list-tile-action>
      <v-flex justify-end>
        <ProcessState :state="process.statename"></ProcessState>
        <v-btn icon @click="restartProcess(process)">
          <v-icon color="green">
            <template v-if="process.running">autorenew</template>
            <template v-else>play_arrow</template>
          </v-icon>
        </v-btn>
        <v-btn icon @click="stopProcess(process)"
               :disabled="!process.running">
          <v-icon color="red">stop</v-icon>
        </v-btn>
                    <v-btn icon  @click="viewDetails(process)">
                      <v-icon color="blue">info</v-icon>
                    </v-btn>
                    <v-btn icon  @click="viewLog(process, 'out')"
                           v-if="process.logfile">
                      <v-icon color="blue">description</v-icon>
                    </v-btn>
                    <v-btn icon  @click="viewLog(process, 'err')"
                           v-if="process.stderr_logfile">
                      <v-icon color="orange">description</v-icon>
                    </v-btn>

      </v-flex>
    </v-list-tile-action>
  </v-list-tile>
</template>

<script>
import ProcessState from '../process/State'

export default {
  name: 'ProcessTitle',
  props: ['process'],
  components: {
    ProcessState
  },
  computed: {
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
