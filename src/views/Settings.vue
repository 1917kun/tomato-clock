<template>
  <div id="settings">
    <b-container>
      <b-row class="justify-content-center align-items-center">
        <b-table :items="items" :fields="fields" @row-clicked="selectAlarm">
          <template v-slot:cell(preview)="data">
            <!-- {{data}} 用來檢查 -->
            <audio controls :src="'./alarms/'+data.item.file"></audio>
          </template>
          <template v-slot:cell(select)="data">
            <font-awesome-icon v-if="data.item.file == alarm" :icon="['fas','check']"></font-awesome-icon>
          </template>
        </b-table>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: [
        {
          name: '鬧鈴',
          file: 'alarm1.mp3'
        },
        {
          name: 'alarm2',
          file: 'alarm2.mp3'
        },
        {
          name: 'ya',
          file: 'ya.mp3'
        }
      ],
      fields: [
        {
          key: 'name',
          label: '名稱'
        },
        {
          key: 'preview',
          label: '預覽'
        },
        {
          key: 'select',
          label: '選擇'
        }
      ]
    }
  },
  computed: {
    alarm () {
      return this.$store.getters.alarm
    }
  },
  methods: {
    selectAlarm (item) {
      this.$store.commit('selectAlarm', item.file)
    }
  }
}
</script>
