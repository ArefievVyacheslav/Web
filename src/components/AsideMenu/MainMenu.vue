<template>
  <InspectorLimit
    v-if="showFreeModal"
    @cancel="showFreeModal = false"
  />
  <InspectorModalBox
    v-model="showInspector"
    button="warning"
    has-button
    has-cancel
    button-label="Delete"
  />
  <aside
    id="aside"
    style="overflow-x:hidden; scrollbar-width: none;"
    class="w-[292px] fixed top-0 z-30 h-screen transition-position bg-[#f4f5f7] font-SfProDisplayNormal text-sm xl:left-0"
    :class="isAsideMobileExpanded ? 'left-0' : '-left-full'"
  >
    <div class="px-[16px] pt-[15px]">
      <AsideMenuListItem
        class="mb-[16px]"
        :selected="lastTab === 'settings'"
        :title="user?.current_user_name"
        @click="gotoSettings"
      >
        <img
          class="rounded-[8px] h-[30px] w-[30px] border-2 border-white"
          :src="user?.foto_link"
        >
      </AsideMenuListItem>
      <EventAlert
        v-if="user?.tarif === 'trial' || user?.tarif === 'free'"
        :bg-color="'#FFCA86'"
        :message-text="user?.tarif === 'trial' ? 'Пробная версия' : 'У Вас истекла лицензия, пожалуйста, обновите тариф'"
        :text-color="'white'"
      />
      <AsideMenuListItem
        :selected="lastTab === 'doitnow'"
        title="Очередь"
        @click="gotoDoitnow"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M5 6.89471C5 6.65742 5.08975 6.42985 5.2495 6.26206C5.40925 6.09426 5.62593 6 5.85185 6H22.5926C22.8185 6 23.0352 6.09426 23.1949 6.26206C23.3547 6.42985 23.4444 6.65742 23.4444 6.89471C23.4444 7.13201 23.3547 7.35958 23.1949 7.52737C23.0352 7.69517 22.8185 7.78943 22.5926 7.78943H5.85185C5.62593 7.78943 5.40925 7.69517 5.2495 7.52737C5.08975 7.35958 5 7.13201 5 6.89471ZM14.0741 13.1577H5.85185C5.62593 13.1577 5.40925 13.252 5.2495 13.4198C5.08975 13.5876 5 13.8151 5 14.0524C5 14.2897 5.08975 14.5173 5.2495 14.6851C5.40925 14.8529 5.62593 14.9471 5.85185 14.9471H14.0741C14.3 14.9471 14.5167 14.8529 14.6764 14.6851C14.8362 14.5173 14.9259 14.2897 14.9259 14.0524C14.9259 13.8151 14.8362 13.5876 14.6764 13.4198C14.5167 13.252 14.3 13.1577 14.0741 13.1577ZM14.0741 20.3154H5.85185C5.62593 20.3154 5.40925 20.4097 5.2495 20.5775C5.08975 20.7453 5 20.9729 5 21.2102C5 21.4474 5.08975 21.675 5.2495 21.8428C5.40925 22.0106 5.62593 22.1049 5.85185 22.1049H14.0741C14.3 22.1049 14.5167 22.0106 14.6764 21.8428C14.8362 21.675 14.9259 21.4474 14.9259 21.2102C14.9259 20.9729 14.8362 20.7453 14.6764 20.5775C14.5167 20.4097 14.3 20.3154 14.0741 20.3154ZM26 17.6313C26.0009 17.784 25.964 17.9343 25.8931 18.0676C25.8222 18.2009 25.7196 18.3126 25.5954 18.3918L18.7805 22.8654C18.6517 22.9494 18.5038 22.9958 18.3522 22.9997C18.2006 23.0037 18.0507 22.965 17.918 22.8877C17.7862 22.8091 17.6764 22.6953 17.5997 22.5578C17.5231 22.4204 17.4823 22.2642 17.4815 22.1049V13.1577C17.4816 12.9985 17.5222 12.8423 17.5991 12.7051C17.6759 12.5679 17.7862 12.4547 17.9186 12.3772C18.051 12.2997 18.2006 12.2607 18.3522 12.2642C18.5037 12.2677 18.6516 12.3136 18.7805 12.3972L25.5954 16.8708C25.7196 16.95 25.8222 17.0617 25.8931 17.195C25.964 17.3283 26.0009 17.4786 26 17.6313ZM23.5403 17.6313L19.1852 14.7682V20.4944L23.5403 17.6313Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="lastTab === 'tasks'"
        title="Задачи"
        @click="gotoTasks"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M6.44542 5.83331H12.8532C13.1941 5.83331 13.4651 6.10431 13.4651 6.44524V12.853C13.4651 13.1939 13.1941 13.4649 12.8532 13.4649H6.44542C6.10449 13.4649 5.8335 13.1939 5.8335 12.853V6.44524C5.8335 6.10431 6.10449 5.83331 6.44542 5.83331ZM7.34583 7.34565V11.9526H11.9528V7.34565H7.34583ZM6.44542 16.035H12.8532C13.1941 16.035 13.4651 16.306 13.4651 16.647V23.0547C13.4651 23.3957 13.1941 23.6667 12.8532 23.6667H6.44542C6.10449 23.6667 5.8335 23.3957 5.8335 23.0547V16.647C5.8335 16.306 6.10449 16.035 6.44542 16.035ZM7.34583 17.5474V22.1543H11.9528V17.5474H7.34583ZM20.6071 16.035H19.0948V19.0947H16.0352V20.607H19.0948V23.6667H20.6071V20.607H23.6668V19.0947H20.6071V16.035ZM16.6471 5.83331H23.0548C23.3958 5.83331 23.6668 6.10431 23.6668 6.44524V12.853C23.6668 13.1939 23.3958 13.4649 23.0548 13.4649H16.6471C16.3062 13.4649 16.0352 13.1939 16.0352 12.853V6.44524C16.0352 6.10431 16.3062 5.83331 16.6471 5.83331ZM17.5475 7.34565V11.9526H22.1544V7.34565H17.5475Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="lastTab === 'directory'"
        title="Справочники"
        @click="gotoDirectory"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M26.2803 18.4697L24.0303 16.2197C23.8896 16.0791 23.6989 16.0002 23.5 16.0002C23.3011 16.0002 23.1104 16.0791 22.9697 16.2197L16 23.1895V26.5H19.3105L26.2803 19.5303C26.4209 19.3896 26.4998 19.1989 26.4998 19C26.4998 18.8011 26.4209 18.6104 26.2803 18.4697ZM18.6895 25H17.5V23.8105L21.25 20.0605L22.4395 21.25L18.6895 25ZM23.5 20.1895L22.3105 19L23.5 17.8105L24.6895 19L23.5 20.1895ZM10 16H17.5V17.5H10V16ZM10 11.5H19V13H10V11.5Z"
            fill="currentColor"
          />
          <path
            d="M23.5 7C23.4996 6.6023 23.3414 6.221 23.0602 5.93978C22.779 5.65856 22.3977 5.5004 22 5.5H7.00001C6.6023 5.5004 6.221 5.65856 5.93978 5.93978C5.65856 6.221 5.5004 6.6023 5.50001 7V16.75C5.49833 18.2458 5.90404 19.7137 6.67359 20.9964C7.44313 22.279 8.54745 23.3278 9.868 24.0302L13.75 26.1002V24.4L10.5745 22.7065C9.49402 22.1318 8.59042 21.2737 7.96069 20.2243C7.33095 19.1749 6.99884 17.9738 7.00001 16.75V7H22V13.75H23.5V7Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="lastTab === 'new_private_boards'"
        title="Доски"
        @click="gotoBoards"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M6.5 24.9694C5.67347 24.9694 5 24.3061 5 23.4898V8.85714C5 8.04082 5.67347 7.37755 6.5 7.37755H10.102V6.32653C10.102 5.96939 10.3878 5.68367 10.7449 5.68367H12.7347L12.7857 5.55102C13.1122 4.62245 14.0102 4 15 4C15.9898 4 16.8878 4.62245 17.2143 5.55102L17.2653 5.68367H19.2551C19.6122 5.68367 19.898 5.96939 19.898 6.32653V7.37755H23.5C24.3265 7.37755 25 8.04082 25 8.85714V23.4898C25 24.3061 24.3265 24.9694 23.5 24.9694H6.5ZM6.29592 23.6939H23.7143V8.65306H19.898V9.70408C19.898 10.0612 19.6122 10.3469 19.2551 10.3469H10.7449C10.3878 10.3469 10.102 10.0612 10.102 9.70408V8.65306H6.29592V23.6939ZM11.398 9.06122H18.6122V6.96939H16.7041C16.3469 6.96939 16.0612 6.68367 16.0612 6.32653C16.0612 5.7449 15.5918 5.27551 15.0102 5.27551C14.4286 5.27551 13.949 5.7449 13.949 6.32653C13.949 6.68367 13.6633 6.96939 13.3061 6.96939H11.398V9.06122ZM19.1327 20.3571C18.7041 20.3571 18.3673 20.0102 18.3673 19.5918V18.6224C18.3673 18.1939 18.7143 17.8571 19.1327 17.8571H21.3163C21.7449 17.8571 22.0816 18.2041 22.0816 18.6224V19.5918C22.0816 20.0204 21.7347 20.3571 21.3163 20.3571H19.1327ZM14.0306 20.3571C13.6122 20.3571 13.2653 20.0102 13.2653 19.5918V18.6224C13.2653 18.1939 13.6122 17.8571 14.0306 17.8571H16.2143C16.6429 17.8571 16.9796 18.2041 16.9796 18.6224V19.5918C16.9796 20.0204 16.6327 20.3571 16.2143 20.3571H14.0306ZM8.68367 20.3571C8.2551 20.3571 7.91837 20.0102 7.91837 19.5918V18.6224C7.91837 18.1939 8.26531 17.8571 8.68367 17.8571H10.8673C11.2857 17.8571 11.6327 18.2041 11.6327 18.6224V19.5918C11.6327 20.0204 11.2857 20.3571 10.8673 20.3571H8.68367ZM19.1327 15.5C18.7041 15.5 18.3673 15.1531 18.3673 14.7347V13.7551C18.3673 13.3367 18.7143 12.9898 19.1327 12.9898H21.3163C21.7449 12.9898 22.0816 13.3367 22.0816 13.7551V14.7245C22.0816 15.1429 21.7347 15.4898 21.3163 15.4898H19.1327V15.5ZM14.0306 15.5C13.6122 15.5 13.2653 15.1531 13.2653 14.7347V13.7551C13.2653 13.3367 13.6122 12.9898 14.0306 12.9898H16.2143C16.6429 12.9898 16.9796 13.3367 16.9796 13.7551V14.7245C16.9796 15.1429 16.6327 15.4898 16.2143 15.4898H14.0306V15.5ZM8.68367 15.5C8.2551 15.5 7.91837 15.1531 7.91837 14.7347V13.7551C7.91837 13.3367 8.26531 12.9898 8.68367 12.9898H10.8673C11.2857 12.9898 11.6327 13.3367 11.6327 13.7551V14.7245C11.6327 15.1429 11.2857 15.4898 10.8673 15.4898H8.68367V15.5Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="lastTab === 'new_private_projects'"
        title="Проекты"
        @click="gotoProjects"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M23.3813 9.4868V8.39736C23.3813 7.3868 22.5646 6.56356 21.562 6.56356H14.0645L13.8396 6.18891C13.7026 5.97201 13.463 5.83398 13.2087 5.83398H7.65284C6.65024 5.83398 5.8335 6.65722 5.8335 7.66779V21.5002C5.8335 22.5107 6.65024 23.334 7.65284 23.334H23.1808C24.1834 23.334 25.0001 22.5107 25.0001 21.5002V11.3107C25.005 10.3643 24.291 9.58539 23.3813 9.4868ZM7.31049 7.66779C7.31049 7.47553 7.46699 7.31779 7.65773 7.31779H12.7978L14.7933 10.6009C14.9253 10.8178 15.1649 10.9558 15.4242 10.9558H23.1906C23.3813 10.9558 23.5378 11.1136 23.5378 11.3058V21.5002C23.5378 21.6924 23.3813 21.8502 23.1906 21.8502H7.65284C7.4621 21.8502 7.3056 21.6924 7.3056 21.5002V7.66779H7.31049ZM21.9043 9.47201H15.8301L14.9644 8.04736H21.5571C21.7478 8.04736 21.9043 8.20511 21.9043 8.39736V9.47201Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListButton
        class="px-[12px] my-[16px]"
        title="Добавить задачу"
        @click="shouldShowInspector"
      />
    </div>
  </aside>
</template>
<script>
import { SWITCH_TAB } from '@/store/actions/tabs.js'
import * as TASK from '@/store/actions/tasks.js'

import InspectorModalBox from '@/components/Inspector/InspectorModalBox.vue'
import InspectorLimit from '@/components/TasksList/InspectorLimit.vue'
import EventAlert from '@/components/EventAlert.vue'
import AsideMenuListItem from '@/components/AsideMenu/AsideMenuListItem.vue'
import AsideMenuListButton from '@/components/AsideMenu/AsideMenuListButton.vue'

export default {
  components: {
    InspectorLimit,
    EventAlert,
    InspectorModalBox,
    AsideMenuListItem,
    AsideMenuListButton
  },
  data: () => ({
    showFreeModal: false,
    showInspector: false
  }),
  computed: {
    lastTab () {
      return this.$store.state.navigator.lastTab
    },
    menu () {
      return this.$store.state.navigator.menu
    },
    user () {
      return this.$store.state.user.user
    },
    navStack () {
      return this.$store.state.navbar.navStack
    },
    storeNavigator () {
      return this.$store.state.navigator.navigator
    },
    isAsideMobileExpanded () {
      return this.$store.state.isAsideMobileExpanded
    }
  },
  methods: {
    gotoProjects () {
      const elemToPush = {
        name: 'Проекты',
        uid: '7e345ae4-0408-416c-b1af-6bd8a15addec',
        type: 'greed',
        code: 'new_private_projects'
      }
      this.changeTab(elemToPush)
    },
    gotoBoards () {
      const elemToPush = {
        name: 'Доски',
        uid: '2e8dddd0-125a-49ef-a87c-0ea17b1b7f56',
        type: 'greed',
        code: 'new_private_boards'
      }
      this.changeTab(elemToPush)
    },
    gotoDirectory () {
      const elemToPush = {
        code: 'directory',
        name: 'Справочники',
        items: []
      }
      this.changeTab(elemToPush)
    },
    gotoTasks () {
      this.$store.dispatch(TASK.DAYS_WITH_TASKS)
      const elemToPush = {
        code: 'tasks',
        name: 'Задачи',
        items: [
          [{
            label: 'В работе',
            uid: '2a5cae4b-e877-4339-8ca1-bd61426864ec',
            bold: false,
            icon: 'M8.46116 22.6199L21.893 14.865L8.46116 7.1101L8.46116 22.6199ZM22.9465 16.6897C24.3512 15.8787 24.3512 13.8513 22.9465 13.0403L9.51464 5.28542C8.11 4.47445 6.3542 5.48817 6.3542 7.1101V22.6199C6.3542 24.2418 8.11 25.2555 9.51464 24.4446L22.9465 16.6897Z',
            iconBox: '0 0 30 30',
            width: 30,
            height: 30,
            type: 'uid',
            iconBackgroundClass: ''
          }],
          [{
            label: 'Задачи в фокусе',
            uid: '6fc44cc6-9d45-4052-917e-25b1189ab141',
            bold: false,
            icon: 'M21.0096 5.83331H8.99077C8.62927 5.83331 8.3335 6.1487 8.3335 6.53418V23.4658C8.3335 23.7578 8.50877 24.0206 8.7662 24.1258C8.83741 24.155 8.91409 24.1666 8.99077 24.1666C9.18248 24.1666 9.36871 24.0732 9.49469 23.9155L15.0002 18.9771L20.5056 23.9155C20.6316 24.0732 20.8124 24.1666 21.0096 24.1666C21.0862 24.1666 21.1629 24.155 21.2341 24.1258C21.4916 24.0265 21.6668 23.7578 21.6668 23.4658V6.53418C21.6668 6.1487 21.3711 5.83331 21.0096 5.83331ZM14.4962 17.3958L9.79183 21.7285V7.38279L20.2085 7.37273V21.7184L15.5041 17.3958C15.3781 17.2381 15.1973 17.1446 15.0002 17.1446C14.803 17.1446 14.6222 17.2381 14.4962 17.3958Z',
            iconBox: '0 0 30 30',
            width: 30,
            height: 30,
            type: 'uid',
            iconBackgroundClass: ''
          }],
          [{
            label: 'Готово к сдаче',
            uid: 'd35fe0bc-1747-4eb1-a1b2-3411e07a92a0',
            bold: false,
            icon: 'M22.781 8.88555C23.1009 9.20546 23.1009 9.71404 22.781 10.034L13.479 19.336C13.4039 19.412 13.3145 19.4723 13.2159 19.5135C13.1173 19.5546 13.0116 19.5758 12.9048 19.5758C12.7979 19.5758 12.6922 19.5546 12.5936 19.5135C12.495 19.4723 12.4056 19.412 12.3306 19.336L8.88537 15.8908C8.80938 15.8157 8.74905 15.7263 8.70787 15.6277C8.66669 15.5292 8.64549 15.4234 8.64549 15.3166C8.64549 15.2098 8.66669 15.104 8.70787 15.0054C8.74905 14.9069 8.80938 14.8175 8.88537 14.7424C8.96045 14.6664 9.04986 14.6061 9.14842 14.5649C9.24699 14.5237 9.35275 14.5025 9.45957 14.5025C9.56639 14.5025 9.67215 14.5237 9.77071 14.5649C9.86928 14.6061 9.95869 14.6664 10.0338 14.7424L12.9048 17.6134L21.6326 8.88555C21.7076 8.80956 21.7971 8.74923 21.8956 8.70805C21.9942 8.66687 22.1 8.64567 22.2068 8.64567C22.3136 8.64567 22.4194 8.66687 22.5179 8.70805C22.6165 8.74923 22.7059 8.80956 22.781 8.88555ZM21.0502 7.14655L12.9048 15.292L10.6162 13.0034C9.97635 12.3636 8.93459 12.3636 8.29477 13.0034L7.14637 14.1518C6.50655 14.7916 6.50655 15.8334 7.14637 16.4732L11.74 21.0668C12.3798 21.7066 13.4215 21.7066 14.0614 21.0668L24.52 10.6164C25.1598 9.97653 25.1598 8.93477 24.52 8.29495L23.3716 7.14655C22.7236 6.50673 21.69 6.50673 21.0502 7.14655Z',
            iconBox: '0 0 30 30',
            width: 30,
            height: 30,
            type: 'uid',
            iconBackgroundClass: ''
          }]
        ]
      }
      this.changeTab(elemToPush)
    },
    gotoDoitnow () {
      const elemToPush = {
        code: 'doitnow',
        name: 'Очередь',
        items: [{}]
      }
      window.ym(89796698, 'reachGoal', 'doitnow')
      this.changeTab(elemToPush)
    },
    gotoSettings () {
      const elemToPush = {
        code: 'settings',
        name: 'Настройки',
        items: []
      }
      this.changeTab(elemToPush)
    },
    shouldShowInspector () {
      window.ym(89796698, 'reachGoal', 'inspector')
      if (this.user.tarif !== 'alpha' && this.user.tarif !== 'trial') {
        this.showFreeModal = true
        return
      }
      this.showInspector = true
    },
    changeTab (tab) {
      if (tab.code === 'doitnow') {
        this.$store.state.navigator.submenu.status = false
      } else {
        this.$store.state.navigator.submenu.status = true
      }
      if (this.lastTab !== tab.code) {
        this.switchTab(tab)
      }
    },
    switchTab (tab) {
      this.$store.dispatch(SWITCH_TAB, tab)
        .then(() => {
          if (tab?.code === 'doitnow') {
            this.$router.push(tab.code)
          }
        })
    },
    checkOnWhichTab (item) {
      const lastNavStack = this.navStack[this.navStack.length - 1]
      if (lastNavStack?.value?.uid === item.uid ||
        lastNavStack?.uid === item.uid ||
        lastNavStack?.name === item.label ||
        (lastNavStack.name && item.name && lastNavStack?.name === item.name)) {
        return true
      }
    }
  }
}
</script>
