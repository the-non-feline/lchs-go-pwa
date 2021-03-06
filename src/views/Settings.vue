<template>
  <div class="settings">
    <h2>App Settings</h2>
    <div class="settings-rows">
      <div class='settings-row'> 
        <div class='sr-head'>
          <span class="sr-badge-new">NEW</span>
          <b class='sr-title'>Grade Level</b> 
          <span class='sr-desc'>Change which grade is used for calculating schedules</span> 
        </div> 
        <div class='sr-option'>
          <select v-model="grade" @change="updateGrade()" class = "grade-select">
            <option v-for="grade in allGrades" :key="grade" :value="grade" class = "grade-select-item">{{strGrade(grade)}}</option> 
          </select>
        </div> 
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <b class="sr-title">Desktop Notifications</b>
          <span class="sr-desc">Enable or disable desktop notifications</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="notifyMe();"
              :class="{selected: this.$store.state.settings.notificationsOn}">Enable</div>
            <div class="ex-selector-option" @click="updateOptionBL('notificationsOn', false)"
              :class="{selected: !(this.$store.state.settings.notificationsOn)}">Disable</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <b class="sr-title">Time Display</b>
          <span class="sr-desc">Change between 12h and 24h time</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="updateOptionBL('useMilitaryTime', false)"
              :class="{selected: !this.$store.state.settings.useMilitaryTime}">12h</div>
            <div class="ex-selector-option" @click="updateOptionBL('useMilitaryTime', true)"
              :class="{selected: this.$store.state.settings.useMilitaryTime}">24h</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <span class="sr-badge-new">NEW</span>
          <b class="sr-title">Enable Theme Animations</b>
          <span class="sr-desc">Toggle fancy color fading</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="updateOptionBL('enableThemeAnimations', true)"
              :class="{selected: this.$store.state.settings.enableThemeAnimations}">Enable</div>
            <div class="ex-selector-option" @click="updateOptionBL('enableThemeAnimations', false)"
              :class="{selected: !this.$store.state.settings.enableThemeAnimations}">Disable</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <span class="sr-badge-new">NEW</span>
          <b class="sr-title">Gradient Type</b>
          <span class="sr-desc">Switch between linear and radial color blend</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="updateOptionBL('enableRadialGradient', true)"
              :class="{selected: this.$store.state.settings.enableRadialGradient}">Linear</div>
            <div class="ex-selector-option" @click="updateOptionBL('enableRadialGradient', false)"
              :class="{selected: !this.$store.state.settings.enableRadialGradient}">Radial</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <b class="sr-title">Enable Blinking</b>
          <span class="sr-desc">Toggle fancy blinking animations</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="updateOptionBL('enableAnimations', true)"
              :class="{selected: this.$store.state.settings.enableAnimations}">Enable</div>
            <div class="ex-selector-option" @click="updateOptionBL('enableAnimations', false)"
              :class="{selected: !this.$store.state.settings.enableAnimations}">Disable</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <b class="sr-title">Show Hidden Periods</b>
          <span class="sr-desc">Show passing periods and times outside school on bell schedule</span>
        </div>
        <div class="sr-option">
          <div class="ex-selector">
            <div class="ex-selector-option" @click="updateOptionBL('showExtraPeriods', true)"
              :class="{selected: this.$store.state.settings.showExtraPeriods}">Show</div>
            <div class="ex-selector-option" @click="updateOptionBL('showExtraPeriods', false)"
              :class="{selected: !this.$store.state.settings.showExtraPeriods}">Hide</div>
          </div>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <span class="sr-badge-new">NEW</span>
          <b class="sr-title">Color Theme (beta)</b>
          <span class="sr-desc">Change app color scheme. Send an email to team@lciteam.club for suggestions.</span>
        </div>
        <div class="sr-option">
          <select v-model="colorThemeId" @change="updateTheme()" class="grade-select">
            <option v-for="theme in allThemes" :key="theme.id" :value="theme.id" class="grade-select-item">{{theme.name}}</option>
          </select>
        </div>
      </div>
      <div class="settings-row">
        <div class="sr-head">
          <b class="sr-title">App Version</b>
          <span class="sr-desc">Please include the full version in bug reports</span>
        </div>
        <div class="sr-option" style="cursor: text;">{{appVersion}}</div>
      </div>
    </div>
    
    <h5>This web app was produced by iTeam, a technology service club at LCHS.</h5>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { Themes } from '../themes';
import { allGrades } from '@/schedule';

@Component({})
export default class Home extends Vue {
  public appVersion = `v${process.env.VUE_APP_VERSION} (b${process.env.VUE_APP_COMMIT_COUNT.trim()}#${process.env.VUE_APP_COMMIT_SHASH.trim()})`;
  colorThemeId = this.$store.state.settings.colorTheme;
  notificationsStatus = this.$store.state.settings.notificationsOn;
  grade = allGrades[2];
  allGrades = allGrades;
  allThemes: any[] = [];

  strGrade(grade: any){
  if (grade < 13 && grade > 3) {
    grade = String(grade);
    grade = grade.concat('th Grade');
  } else if (grade === 0) {
    grade = 'K/Pre K';
  } else if (grade === 1) {
    grade = '1st Grade';
  } else if (grade === 2) {
    grade = '2nd Grade';
  } else if (grade === 3) {
    grade = '3rd Grade';
  } else if (grade === 13) {
    grade = 'Event';
  }
  return grade;
  }

  getNotifStatus() {
    // A way to refrence this boolean expression as a single variable
    // if (!(Notification.permission === "granted") && (this.$store.state.settings.notificationsOn)) {this.notifyMe()}
    // console.log('test');
    return (Notification.permission === 'granted') && (this.$store.state.settings.notificationsOn);
  }

  updateOptionBL(name: string, value: any): void {
    this.$store.commit('UPDATE_SETTING', { name, value });
  }

  updateTheme() {
    this.updateOptionBL('colorTheme', this.colorThemeId);
  }

  updateGrade() {
    this.updateOptionBL('grade', this.grade);
  }

  notifyMe() {
    // Let's check if the browser supports notifications
    let temp = this;
    if (!('Notification' in window)) {
      alert('This browser does not support desktop notification');
    }

    else if (Notification.permission === 'denied') {
      alert('You have blocked notifications for this website. In order to enable popup notifications, you must click on the "i" next to your address bar and set notifications to "allow"');
    }

    // Let's check whether notification permissions have already been granted
    else if (Notification.permission === 'granted') {
      // If it's okay let's create a notification
      let notification = new Notification('LCHS Go', {
        body: 'Notifications are now on!',
        badge: 'https://go.lciteam.club/favicon.ico',
        icon: 'https://go.lciteam.club/favicon.ico',
        vibrate: [200, 100, 200],
        silent: false,
        tag: String(this.$store.state.settings.numberOfClicks),
      });
      temp.notificationsStatus = true;
      temp.updateOptionBL('notificationsOn', true);
    }

    // Otherwise, we need to ask the user for permission
    else {
      Notification.requestPermission().then((permission) => {
        // If the user accepts, let's create a notification
        if (permission === 'granted') {
          let notification = new Notification('LCHS Go', {
            body: 'Notifications are now on!',
            badge: 'https://go.lciteam.club/favicon.ico',
            icon: 'https://go.lciteam.club/favicon.ico',
            vibrate: [200, 100, 200],
            silent: false,
            tag: String(temp.$store.state.settings.numberOfClicks),
          });
          temp.notificationsStatus = true;
          // console.log('test');
          temp.updateOptionBL('notificationsOn', true);
        }
        else {
          alert('In order to enable popup notifications, you must click allow. \n(If you don\'t want notifications, you can disable them in settings to avoid this popup)');
          temp.notificationsStatus = false;
        }
      });

    // At last, if the user has denied notifications, and you
    // want to be respectful there is no need to bother them any more.
    }
  }

  updateStats() {
    console.log(this.$store.state.settings.numberOfClicks);
    this.$store.state.settings.numberOfClicks = this.$store.state.settings.numberOfClicks + 1;
  }

  mounted() {
    // this part is to prevent invalid grade values
    this.grade = this.$store.state.settings.grade;

    if (allGrades.indexOf(this.grade) === -1) {
      this.grade = allGrades[2];

      this.updateGrade();
    }

    setInterval(this.updateStats, 5000);
    this.updateStats();

    this.allThemes = Themes;
  }
}
</script>

<style lang="scss">
.settings-rows {
  text-align: left;
}

.settings-row {
  padding: 10px 16px;
  border-radius: 4px;
  margin: 5px 0;
  cursor: default;
  
  &:hover {
    background-color: rgba(0, 0, 0, .04);
  }

  .sr-head {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
  }

  .sr-title {
    color: rgba(255, 255, 255, 0.8);
    display: inline;
    font-size: 1.2em;
    margin-right: 8px;
  }

  .sr-badge-new {
    background-color: rgba(18, 15, 43, .3);
    color: rgba(255, 255, 255, 0.8);
    border-radius: 2px;
    display: inline-block;
    font-size: 10px;
    padding: 2px 6px;
    margin-right: 8px;
  }

  .sr-desc {
    font-size: 0.9em;
    display: inline;
    color: rgba(255, 255, 255, 0.7);
  }

  .sr-option {
    font-weight: 600;
    margin: 8px 0;
  }
}

.ex-selector {
  background: rgba(0, 0, 0, .08);
  display: inline-block;
  padding: 4px 4px;
  border-radius: 2px;

  .ex-selector-option {
    // background: rgba(0, 0, 0, .1);
    display: inline-block;
    padding: 4px 12px;
    margin: 0 2px;
    min-width: 80px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;
    transition: 100ms ease;
    color: rgba(255, 255, 255, 0.7);
    box-sizing: border-box;

    &:hover {
      background: rgba(0, 0, 0, .05);
    }
    &.selected {
      background: rgba(0, 0, 0, .2);
      cursor: default;
      color: rgba(255, 255, 255, 1);
    }
  }
}

select.grade-select {
  color: #ffffff;
  background: rgba(0,0,0,.2);
  padding: 5px;
  text-decoration-color: white;
  font-weight: 600;
  font-family: Niramit,Avenir,sans-serif;
  border-color:rgba(0,0,0,0);
  border-width: 1px;
  border-radius: 3px;
} 

option.grade-select-item  {
color: rgba(255, 255, 255, 0.6);

  background: var(--button-menu-color, #42b983);
  padding: 5px;
  text-decoration-color: white;
  font-weight: 600;
  font-family: Niramit,Avenir,sans-serif;
  border-color:rgba(0,0,0,0);
  border-width: 1px;
  border-radius: 3px;

}

</style>
