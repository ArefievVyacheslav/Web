<template>
  <div class="px-[16px] pt-[15px]">
    <AsideMenuSkeleton v-if="status == 'loading'" />
    <div v-if="status == 'success'">
      <TasksSubmenuCalendar
        class="pl-[6px] mb-[10px]"
        @dayclick="onDayClick"
      />
      <AsideMenuListItem
        :selected="isTodaySelected"
        title="Сегодня"
        @click="gotoToday"
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
            d="M8.69793 4.46611C8.69793 3.91238 9.14681 3.4635 9.70054 3.4635C10.2543 3.4635 10.7031 3.91238 10.7031 4.46611V5.18226H19.2969V4.46611C19.2969 3.91238 19.7458 3.4635 20.2995 3.4635C20.8533 3.4635 21.3021 3.91238 21.3021 4.46611V5.18226H23.0209C24.2865 5.18226 25.3126 6.20827 25.3126 7.47393V22.3698C25.3126 23.6355 24.2865 24.6615 23.0209 24.6615H6.97917C5.71352 24.6615 4.6875 23.6355 4.6875 22.3698V7.47393C4.6875 6.20827 5.71352 5.18226 6.97917 5.18226H8.69793V4.46611ZM19.2969 6.75778V7.90362C19.2969 8.45734 19.7458 8.90622 20.2995 8.90622C20.8533 8.90622 21.3021 8.45734 21.3021 7.90362V6.75778H23.0209C23.4164 6.75778 23.737 7.07841 23.737 7.47393V22.3698C23.737 22.7653 23.4164 23.086 23.0209 23.086H6.97917C6.58366 23.086 6.26303 22.7653 6.26303 22.3698V7.47393C6.26303 7.07841 6.58366 6.75778 6.97917 6.75778H8.69793V7.90362C8.69793 8.45734 9.14681 8.90622 9.70054 8.90622C10.2543 8.90622 10.7031 8.45734 10.7031 7.90362V6.75778H19.2969ZM9.62005 10.8333C8.9095 10.8333 8.3335 11.4224 8.3335 12.1491C8.3335 12.8758 8.9095 13.4649 9.62005 13.4649C10.3306 13.4649 10.9066 12.8758 10.9066 12.1491C10.9066 11.4224 10.3306 10.8333 9.62005 10.8333ZM15.4914 12.1491C15.4914 11.4224 16.0674 10.8333 16.7779 10.8333C17.4885 10.8333 18.0645 11.4224 18.0645 12.1491C18.0645 12.8758 17.4885 13.4649 16.7779 13.4649C16.0674 13.4649 15.4914 12.8758 15.4914 12.1491ZM15.4914 15.8333C15.4914 15.1066 16.0674 14.5175 16.7779 14.5175C17.4885 14.5175 18.0645 15.1066 18.0645 15.8333C18.0645 16.56 17.4885 17.1491 16.7779 17.1491C16.0674 17.1491 15.4914 16.56 15.4914 15.8333ZM15.4914 19.5175C15.4914 18.7908 16.0674 18.2017 16.7779 18.2017C17.4885 18.2017 18.0645 18.7908 18.0645 19.5175C18.0645 20.2442 17.4885 20.8333 16.7779 20.8333C16.0674 20.8333 15.4914 20.2442 15.4914 19.5175ZM19.0937 12.1491C19.0937 11.4224 19.6697 10.8333 20.3803 10.8333C21.0908 10.8333 21.6668 11.4224 21.6668 12.1491C21.6668 12.8758 21.0908 13.4649 20.3803 13.4649C19.6697 13.4649 19.0937 12.8758 19.0937 12.1491ZM19.0937 15.8333C19.0937 15.1066 19.6697 14.5175 20.3803 14.5175C21.0908 14.5175 21.6668 15.1066 21.6668 15.8333C21.6668 16.56 21.0908 17.1491 20.3803 17.1491C19.6697 17.1491 19.0937 16.56 19.0937 15.8333ZM19.0937 19.5175C19.0937 18.7908 19.6697 18.2017 20.3803 18.2017C21.0908 18.2017 21.6668 18.7908 21.6668 19.5175C21.6668 20.2442 21.0908 20.8333 20.3803 20.8333C19.6697 20.8333 19.0937 20.2442 19.0937 19.5175ZM8.3335 15.8333C8.3335 15.1066 8.9095 14.5175 9.62005 14.5175C10.3306 14.5175 10.9066 15.1066 10.9066 15.8333C10.9066 16.56 10.3306 17.1491 9.62005 17.1491C8.9095 17.1491 8.3335 16.56 8.3335 15.8333ZM8.3335 19.5175C8.3335 18.7908 8.9095 18.2017 9.62005 18.2017C10.3306 18.2017 10.9066 18.7908 10.9066 19.5175C10.9066 20.2442 10.3306 20.8333 9.62005 20.8333C8.9095 20.8333 8.3335 20.2442 8.3335 19.5175ZM11.9358 12.1491C11.9358 11.4224 12.5119 10.8333 13.2224 10.8333C13.9329 10.8333 14.5089 11.4224 14.5089 12.1491C14.5089 12.8758 13.9329 13.4649 13.2224 13.4649C12.5119 13.4649 11.9358 12.8758 11.9358 12.1491ZM11.9358 15.8333C11.9358 15.1066 12.5119 14.5175 13.2224 14.5175C13.9329 14.5175 14.5089 15.1066 14.5089 15.8333C14.5089 16.56 13.9329 17.1491 13.2224 17.1491C12.5119 17.1491 11.9358 16.56 11.9358 15.8333ZM11.9358 19.5175C11.9358 18.7908 12.5119 18.2017 13.2224 18.2017C13.9329 18.2017 14.5089 18.7908 14.5089 19.5175C14.5089 20.2442 13.9329 20.8333 13.2224 20.8333C12.5119 20.8333 11.9358 20.2442 11.9358 19.5175Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="isOverdueSelected"
        title="Просроченные"
        @click="gotoOverdue"
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
            d="M18.283 22.2558L18.3026 22.2846C18.6498 22.7855 19.1424 23.178 19.7195 23.4009C19.7331 23.4061 19.7467 23.4113 19.7604 23.4163C20.0915 23.5387 20.4495 23.6056 20.8231 23.6056C22.5154 23.6056 23.8873 22.2337 23.8873 20.5413C23.8873 20.1175 23.8013 19.7138 23.6458 19.3468C23.6224 19.2917 23.5975 19.2374 23.5711 19.1841C23.3132 18.663 22.9115 18.2258 22.4179 17.9243C22.3893 17.9068 22.3603 17.8898 22.3311 17.8732C21.8859 17.621 21.3713 17.4771 20.8231 17.4771C19.1307 17.4771 17.7588 18.849 17.7588 20.5413C17.7588 21.1765 17.9521 21.7665 18.283 22.2558ZM22.9036 16.4417C22.2789 16.124 21.5719 15.9449 20.8231 15.9449C18.2846 15.9449 16.2267 18.0028 16.2267 20.5413C16.2267 21.3666 16.4441 22.141 16.8249 22.8104C16.0862 23.0166 15.3075 23.1268 14.503 23.1268C9.7433 23.1268 5.88478 19.2683 5.88478 14.5086C5.88478 9.74885 9.7433 5.89033 14.503 5.89033C19.2628 5.89033 23.1213 9.74885 23.1213 14.5086C23.1213 15.1732 23.046 15.8203 22.9036 16.4417ZM24.3696 17.6172C24.6784 16.636 24.8449 15.5918 24.8449 14.5086C24.8449 8.79691 20.2147 4.16669 14.503 4.16669C8.79136 4.16669 4.16113 8.79691 4.16113 14.5086C4.16113 20.2202 8.79136 24.8505 14.503 24.8505C15.7554 24.8505 16.9559 24.6278 18.0669 24.22C18.8348 24.7963 19.7891 25.1377 20.8231 25.1377C23.3616 25.1377 25.4195 23.0799 25.4195 20.5413C25.4195 19.4306 25.0254 18.4118 24.3696 17.6172ZM15.2683 10.4868C14.8048 10.4868 14.4316 10.8638 14.4316 11.3234V15.2326H12.7542C12.2907 15.2326 11.9175 15.6097 11.9175 16.0692C11.9175 16.5287 12.2907 16.9058 12.7542 16.9058H15.2683C15.7319 16.9058 16.1051 16.5287 16.1051 16.0692V11.3234C16.1051 10.8638 15.7319 10.4868 15.2683 10.4868ZM20.8949 21.3987C21.2391 21.3987 21.5161 21.1195 21.5161 20.7726V18.8693C21.5161 18.5224 21.2391 18.2432 20.8949 18.2432C20.5507 18.2432 20.2737 18.5224 20.2737 18.8693V20.781C20.2737 21.1195 20.5507 21.3987 20.8949 21.3987ZM20.8949 21.7879C20.5423 21.7879 20.2485 22.0841 20.2485 22.4394C20.2485 22.7948 20.5423 23.0909 20.8949 23.0909C21.2475 23.0909 21.5413 22.7948 21.5413 22.4394C21.5413 22.0841 21.2475 21.7879 20.8949 21.7879Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="isUnsortedSelected"
        title="Неразобранные"
        @click="gotoUnsorted"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M26 12.3422C26 11.6962 25.4763 11.1725 24.8303 11.1725H5.16971C4.5237 11.1725 4 11.6962 4 12.3422C4 12.9882 4.5237 13.5119 5.16971 13.5119H24.8303C25.4763 13.5119 26 12.9882 26 12.3422Z"
            fill="currentColor"
          />
          <path
            d="M20.3657 6.16971C20.3657 5.5237 19.842 5 19.196 5H5.16971C4.5237 5 4 5.5237 4 6.16971C4 6.81573 4.5237 7.33942 5.16971 7.33942H19.196C19.842 7.33942 20.3657 6.81573 20.3657 6.16971Z"
            fill="currentColor"
          />
          <path
            d="M22.5386 18.5624C22.5386 17.9164 22.0149 17.3927 21.3688 17.3927H5.16971C4.5237 17.3927 4 17.9164 4 18.5624C4 19.2084 4.5237 19.7321 5.16971 19.7321H21.3688C22.0149 19.7321 22.5386 19.2084 22.5386 18.5624Z"
            fill="currentColor"
          />
          <path
            d="M14.0494 24.7827C14.0494 24.1367 13.5257 23.613 12.8797 23.613H5.16971C4.5237 23.613 4 24.1367 4 24.7827C4 25.4287 4.5237 25.9524 5.16971 25.9524H12.8797C13.5257 25.9524 14.0494 25.4287 14.0494 24.7827Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <AsideMenuListItem
        :selected="isUnreadSelected"
        title="Непрочитанные"
        @click="gotoUnread"
      >
        <svg
          width="30"
          height="30"
          viewBox="0 0 30 30"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M22.6793 6.92814C22.505 6.75384 22.2726 6.66669 22.0111 6.66669C21.7497 6.66669 21.5173 6.75384 21.343 6.92814L18.7575 9.5136C17.4503 8.9326 16.0849 8.64209 14.7196 8.64209C12.628 8.64209 10.5363 9.31025 8.50284 10.6466C5.91738 12.3315 4.37772 14.4521 4.34867 14.4812C4.08722 14.8298 4.11627 15.2946 4.37772 15.6141C5.74308 17.299 7.19558 18.6353 8.67714 19.594L6.49838 21.8018C6.14978 22.1504 6.14978 22.7605 6.49838 23.1381C6.67268 23.3124 6.90508 23.3996 7.16653 23.3996C7.42798 23.3996 7.66039 23.3124 7.83469 23.1381L22.6793 8.26444C23.0279 7.91584 23.0279 7.30579 22.6793 6.92814ZM6.32408 15.0331C6.90508 14.365 8.03804 13.203 9.51959 12.2443C11.2626 11.1114 13.0056 10.5304 14.7196 10.5304C15.5911 10.5304 16.4916 10.6756 17.3341 10.9952L16.4626 11.8667C15.8816 11.5471 15.2425 11.3438 14.5743 11.3438C13.5866 11.3438 12.657 11.7214 11.9598 12.4186C11.2626 13.1158 10.8849 14.0454 10.8849 15.0331C10.8849 15.7013 11.0592 16.3404 11.4079 16.9214L10.0715 18.2577C8.79334 17.5024 7.54418 16.4275 6.32408 15.0331ZM13.2961 13.7259C13.6447 13.3773 14.1095 13.1739 14.6034 13.1739C14.7486 13.1739 14.9229 13.203 15.0682 13.232L12.8313 15.4689C12.8023 15.3236 12.7732 15.1784 12.7732 15.0041C12.7442 14.5393 12.9475 14.0745 13.2961 13.7259Z"
            fill="currentColor"
          />
          <path
            d="M24.8 14.4521C23.7542 13.1739 22.6793 12.0991 21.5754 11.2276C21.4011 11.1114 21.1977 11.0242 20.9944 11.0242C20.7039 11.0242 20.4424 11.1404 20.2391 11.3728C19.9195 11.7795 20.0067 12.3605 20.4134 12.6801C21.2268 13.3192 22.0402 14.1035 22.8536 15.0331C22.2726 15.7013 21.372 16.6018 20.2391 17.4443C18.3218 18.8387 16.3754 19.5359 14.4872 19.5359C13.9352 19.5359 13.4123 19.4778 12.8604 19.3616C12.8023 19.3616 12.7442 19.3325 12.657 19.3325C12.2213 19.3325 11.8436 19.6521 11.7565 20.0878C11.6984 20.3202 11.7565 20.5817 11.8727 20.785C12.0179 20.9884 12.2213 21.1336 12.4537 21.1917C13.0928 21.337 13.7609 21.3951 14.4581 21.3951C15.4749 21.3951 16.4916 21.2498 17.4793 20.9303C18.7866 20.5236 20.0938 19.8554 21.372 18.9258C23.3475 17.4733 24.5966 15.9046 24.829 15.5851C25.0614 15.2365 25.0614 14.7717 24.8 14.4521Z"
            fill="currentColor"
          />
        </svg>
      </AsideMenuListItem>
      <div class="my-[10px]">
        <template v-for="(menuGroup, index) in menu">
          <div
            v-if="typeof menuGroup === 'string'"
            :key="`a-${index}`"
            class="my-2"
          >
            <hr
              :key="`a-${index}`"
              class="text-xs mx-3 custom-border-divider"
              :class="[ asideMenuLabelStyle ]"
            >
          </div>
          <aside-menu-list
            v-else
            :key="`b-${index}`"
            :menu="menuGroup"
            :assigments="assigments"
            @menu-click="menuClick"
            @assigments-click="assigmentsClick"
          />
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import AsideMenuList from '@/components/AsideMenu/AsideMenuList.vue'
import AsideMenuSkeleton from '@/components/AsideMenu/AsideMenuSkeleton.vue'
import AsideMenuListItem from '@/components/AsideMenu/AsideMenuListItem.vue'
import TasksSubmenuCalendar from '@/components/AsideMenu/TasksSubmenuCalendar.vue'
import { UID_TO_ACTION } from '@/store/helpers/functions'

import * as TASK from '@/store/actions/tasks'

export default {
  components: {
    AsideMenuSkeleton,
    AsideMenuList,
    TasksSubmenuCalendar,
    AsideMenuListItem
  },
  props: {
    menu: {
      type: Array,
      default: () => []
    }
  },
  emits: ['closeSubMenu'],
  data () {
    return {
      userParentId: null
    }
  },
  computed: {
    status () {
      return this.$store.state.navigator.status
    },
    isAsideMobileExpanded () {
      return this.$store.state.isAsideMobileExpanded
    },
    isPropertiesMobileExpanded () {
      return this.$store.state.isPropertiesMobileExpanded
    },
    navStack () {
      return this.$store.state.navbar.navStack
    },
    user () {
      return this.$store.state.user.user
    },
    storeNavigator () {
      return this.$store.getters.sortedNavigator
    },
    assigments () {
      return { delegate_iam: this.storeNavigator.delegate_iam, delegate_to_me: this.storeNavigator.delegate_to_me }
    },
    lastNavStack () {
      return this.$store.getters.lastNavStackElement
    },
    isTodaySelected () {
      return this.isDateSelected(new Date())
    },
    isOverdueSelected () {
      // console.log('isOverdueSelected', this.lastNavStack)
      return this.isActionSelected('46418722-a720-4c9e-b255-16db4e590c34')
    },
    isUnsortedSelected () {
      return this.isActionSelected('5183b619-3968-4c3a-8d87-3190cfaab014')
    },
    isUnreadSelected () {
      return this.isActionSelected('fa042915-a3d2-469c-bd5a-708cf0339b89')
    }
  },
  methods: {
    pushToRouter () {
      localStorage.setItem('lastTab', 'tasks')
      this.$router.push('/tasks')
    },
    isActionSelected (uid) {
      return this.lastNavStack?.value?.uid === uid
    },
    isDateSelected (date) {
      if (
        this.lastNavStack?.value?.uid === '901841d9-0016-491d-ad66-8ee42d2b496b' &&
        this.lastNavStack?.value?.param
      ) {
        const compareDate = new Date(date)
        const selectedDate = new Date(this.lastNavStack?.value?.param)
        return compareDate.getDate() === selectedDate.getDate() &&
               compareDate.getMonth() === selectedDate.getMonth() &&
               compareDate.getFullYear() === selectedDate.getFullYear()
      }
      return false
    },
    dateToLabelFormat (calendarDate) {
      const day = calendarDate.getDate()
      const month = calendarDate.toLocaleString('default', { month: 'short' })
      const weekday = calendarDate.toLocaleString('default', { weekday: 'short' })
      return day + ' ' + month + ', ' + weekday
    },
    // TODO: clean up messy logic
    menuClick (event, item) {
      // Если уже находимся на этой вкладке игнорировать дальнейший код
      this.pushToRouter()
      if (this.checkOnWhichTab(item)) {
        return
      }
      this.$emit('closeSubMenu')
      this.userParentId = null
      if (item.uid === '901841d9-0016-491d-ad66-8ee42d2b496b') {
        this.$store.commit('setCalendarLastPicked', new Date().date)
      }

      if (item.path === 'new_private_boards' || item.path === 'new_private_projects') {
        this.$store.state.navigator.submenu.status = true
        this.$store.state.navigator.submenu.path = item.path
        return
      } else {
        this.$store.state.navigator.submenu.status = false
      }

      if (this.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', false)
      }
      if (this.isAsideMobileExpanded) {
        this.$store.dispatch('asideMobileToggle', false)
      }

      // Tasks list source
      if (UID_TO_ACTION[item.uid] && item.type === 'uid') {
        this.$store.dispatch(UID_TO_ACTION[item.uid])
        const navElem = {
          name: item.label,
          key: 'taskListSource',
          value: { uid: item.uid, param: new Date() },
          typeVal: new Date(),
          type: 'date'
        }
        this.$store.commit('updateStackWithInitValue', navElem)
        this.$store.commit('basic', { key: 'taskListSource', value: { uid: item.uid, param: null } })
        this.$store.commit('basic', { key: 'mainSectionState', value: 'tasks' })
      // Grid source (projects, employees, colors, tags)
      } else {
        this.$store.commit('basic', { key: 'mainSectionState', value: 'greed' })
        this.$store.commit('basic', { key: 'greedPath', value: item.path })
        if (item.path === 'new_private_projects' || item.path === 'new_emps' || item.path === 'new_delegate' || item.path === 'new_private_boards') {
          const navElem = {
            name: item.label,
            key: 'greedSource',
            greedPath: item.path,
            value: this.storeNavigator[item.path]
          }
          this.$store.commit('updateStackWithInitValue', navElem)
          this.$store.commit('basic', { key: 'greedSource', value: this.storeNavigator[item.path] })
        } else {
          console.log(item.uid)
          const navElem = {
            name: item.label,
            key: 'greedSource',
            greedPath: item.path,
            value: this.storeNavigator[item.path]?.items
          }
          this.$store.commit('updateStackWithInitValue', navElem)
          this.$store.commit('basic', { key: 'greedSource', value: this.storeNavigator[item.path]?.items })
        }
      }
    },
    gotoToDate (date) {
      this.pushToRouter()
      this.$emit('closeSubMenu')
      // если день уже выбран - ничего не делаем
      if (this.isDateSelected(date)) return
      //
      if (this.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', false)
      }
      if (this.isAsideMobileExpanded) {
        this.$store.dispatch('asideMobileToggle', false)
      }
      //
      this.userParentId = null
      this.$store.dispatch(TASK.TASKS_REQUEST, new Date(date))
      const navElem = {
        name: this.dateToLabelFormat(date),
        key: 'taskListSource',
        value: { uid: '901841d9-0016-491d-ad66-8ee42d2b496b', param: new Date(date) },
        typeVal: new Date(date),
        type: 'date'
      }
      this.$store.commit('setCalendarLastPicked', date)
      this.$store.commit('updateStackWithInitValue', navElem)
      this.$store.commit('basic', { key: 'taskListSource', value: { uid: '901841d9-0016-491d-ad66-8ee42d2b496b', param: new Date(date) } })
      this.$store.commit('basic', { key: 'mainSectionState', value: 'tasks' })
    },
    gotoAction (uid, label) {
      this.pushToRouter()
      this.$emit('closeSubMenu')
      // если уже выбран - ничего не делаем
      if (this.isActionSelected(uid)) return
      //
      if (this.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', false)
      }
      if (this.isAsideMobileExpanded) {
        this.$store.dispatch('asideMobileToggle', false)
      }
      //
      this.userParentId = null
      //
      if (UID_TO_ACTION[uid]) {
        this.$store.dispatch(UID_TO_ACTION[uid])
        const navElem = {
          name: label,
          key: 'taskListSource',
          value: { uid: uid, param: new Date() },
          typeVal: new Date(),
          type: 'date'
        }
        this.$store.commit('setCalendarLastPicked', null)
        this.$store.commit('updateStackWithInitValue', navElem)
        this.$store.commit('basic', { key: 'taskListSource', value: { uid: uid, param: null } })
        this.$store.commit('basic', { key: 'mainSectionState', value: 'tasks' })
      } else {
        console.error(`gotoAction - ${label} > UID_TO_ACTION[${uid}] is null!!!`)
      }
    },
    gotoToday () {
      this.gotoToDate(new Date())
    },
    onDayClick (day) {
      this.gotoToDate(day.date)
    },
    gotoOverdue () {
      this.gotoAction('46418722-a720-4c9e-b255-16db4e590c34', 'Просроченные')
    },
    gotoUnsorted () {
      this.gotoAction('5183b619-3968-4c3a-8d87-3190cfaab014', 'Неразобранные')
    },
    gotoUnread () {
      this.gotoAction('fa042915-a3d2-469c-bd5a-708cf0339b89', 'Непрочитанные')
    },
    checkOnWhichTab (item) {
      const lastNavStack = this.navStack[this.navStack.length - 1]
      if (lastNavStack?.value?.uid === item.uid ||
        lastNavStack?.uid === item.uid ||
        lastNavStack?.name === item.label ||
        (lastNavStack?.name && item.name && lastNavStack?.name === item.name)) {
        return true
      }
    },
    assigmentsClick (user) {
      if (this.$store.state.navbar.lastSelectedAsideTab === user.uid && this.userParentId === user.parentID) {
        return
      }
      console.log(user.parentID)
      if (this.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', false)
      }

      const action = UID_TO_ACTION[user.parentID]
      if (!action) {
        console.error('UID_TO_ACTION in undefined', user.parentID)
        return
      }
      this.$store.dispatch(action, user.email)
      const navElem = {
        name: user.name,
        key: 'taskListSource',
        value: { uid: user.parentID, param: user.email }
      }
      this.$store.commit('updateStackWithInitValue', navElem)
      this.$store.commit('basic', { key: 'taskListSource', value: { uid: user.parentID, param: user.email } })
      this.$store.commit('basic', { key: 'mainSectionState', value: 'tasks' })
      this.$store.commit(TASK.CLEAN_UP_LOADED_TASKS)
      this.$store.state.navbar.lastSelectedAsideTab = user.uid
      this.userParentId = user.parentID

      this.$emit('closeSubMenu')
    }
  }
}
</script>

<style scoped>
</style>
