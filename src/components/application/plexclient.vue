<template>
  <v-list-tile avatar :style="styleObj" class="pa-1">
    <v-list-tile-avatar>
      <img class="clientLogo" :class="platformClass" :src="url">
    </v-list-tile-avatar>
    <v-list-tile-content>
      <v-list-tile-title>{{ object.name }}<v-chip v-for="label in object.labels" :key="label[0]" :color="label[1]" small label>{{ label[0] }}</v-chip></v-list-tile-title>
      <v-list-tile-sub-title>{{ object.product }} - last seen {{ lastSeenAgo }}</v-list-tile-sub-title>
    </v-list-tile-content>
  </v-list-tile>
</template>

<script>
const moment = require('moment');

export default {
  props: ['object', 'selected', 'startup', 'sidebar'],
  name: 'plexclient',
  methods: {},
  computed: {
    tooltipMsg() {
      return (`${this.object.name} running ${this.object.product} on ${this.object.device}`);
    },
    connection_success() {
      if (this.object.connectedstatus === 'connected') {
        return true;
      }
    },
    connection_wait() {
      if (this.object.connectedstatus === 'waiting') {
        return true;
      }
    },
    connection_failed() {
      if (this.object.connectedstatus === 'failed') {
        return true;
      }
    },
    connection_fresh() {
      if (this.object.connectedstatus === 'fresh') {
        return true;
      }
    },
    isTrunc() {
      if (this.sidebar) {
        return true;
      }
      return false;
    },
    lastSeenAgo() {
      const now = moment(new Date().getTime());
      const end = moment.unix(parseInt(this.object.lastSeenAt));
      const difference = moment.duration(now.diff(end));
      return `${difference.humanize()} ago`;
    },
    platform() {
      return this.platformMap[this.object.platform.toLowerCase()] || this.platformMap[this.object.product.toLowerCase()];
    },
    platformClass() {
      return [`platform-${this.platform}`];
    },
    url() {
      if (!this.platform) {
        return 'platforms/plex.svg';
      }
      if (this.platform === 'synclounge') {
        return 'platforms/synclounge.png';
      }
      return `platforms/${this.platform}.svg`;
    },
    styleObj() {
      if (this.selected) {
        return {
          'font-weight': '700',
          background: 'rgba(0,0,0,0.3)',
        };
      }
      return {
        opacity: '0.7',
      };
    },
  },
  data: () => ({
    platformMap: {
      android: 'android',
      'apple tv': 'atv',
      chrome: 'chrome',
      chromecast: 'chromecast',
      dlna: 'dlna',
      firefox: 'firefox',
      'internet explorer': 'ie',
      ios: 'ios',
      ipad: 'ios',
      iphone: 'ios',
      kodi: 'kodi',
      linux: 'linux',
      nexus: 'android',
      macos: 'macos',
      'microsoft edge': 'msedge',
      opera: 'opera',
      osx: 'macos',
      playstation: 'playstation',
      'plex home theater': 'plex',
      'plex media player': 'plex',
      plexamp: 'plexamp',
      plextogether: 'synclounge',
      roku: 'roku',
      safari: 'safari',
      samsung: 'samsung',
      synclounge: 'synclounge',
      tivo: 'tivo',
      tizen: 'samsung',
      tvos: 'atv',
      vizio: 'opera',
      wiiu: 'wiiu',
      windows: 'windows',
      'windows phone': 'wp',
      xbmc: 'xbmc',
      xbox: 'xbox',
    },
  }),
};
</script>
