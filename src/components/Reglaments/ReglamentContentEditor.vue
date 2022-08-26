<template>
  <div>
    <div class="w-full top-[45px] left-0 right-0 z-[6]">
      <div class="top-0 bg-[#f4f5f7] pt-[5px]">
        <div
          class="flex flex-row justify-between items-center"
        >
          <div class="pt-3 pb-3 pr-3 bg-[#f4f5f7] w-[700px] hover:transition hover:opacity-[0.8]">
            <input
              v-model="currName"
              type="text"
              placeholder="Наименование"
              class="bg-[#f4f5f7] p-0 font-roboto font-bold font-[18px] leading-[21px] text-[#424242] w-full border-none focus:ring-0 focus:outline-none"
            >
          </div>
          <div class="flex justify-end gap-[8px] mr-3">
            <ReglamentSmallButton
              v-if="!isTesting"
              class="flex items-center px-[10px] py-[5px] mr-1"
              @click="getBack()"
            >
              <svg
                class="mr-1.5"
                width="14"
                height="8"
                viewBox="0 0 14 8"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M5.23531 6.86625L2.99406 4.625H13.8516V3.375H2.99406L5.23531 1.13375L4.35156 0.25L0.601562 4L4.35156 7.75L5.23531 6.86625Z"
                  fill="#4C4C4D"
                />
              </svg>
              Назад
            </ReglamentSmallButton>
            <ReglamentSmallButton
              :class="{'cursor-default opacity-[0.5]': buttonDisabled === true || saveContentStatus === 0, 'animate-pulse bg-[#E3F4E8]': buttonSaveReglament === 0, 'bg-[FFEDED]': buttonSaveReglament === 2}"
              @click="onSaveReglamentButtonClick"
            >
              <svg
                width="20"
                height="20"
                viewBox="0 0 20 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M16.6727 6.17275L13.8273 3.32725C13.696 3.196 13.535 3.09975 13.36 3.04725V3H3.56C3.25025 3 3 3.25025 3 3.56V16.44C3 16.7498 3.25025 17 3.56 17H16.44C16.7498 17 17 16.7498 17 16.44V6.96375C17 6.66625 16.8827 6.38275 16.6727 6.17275ZM7.76 4.26H12.24V6.08H7.76V4.26ZM15.74 15.74H4.26V4.26H6.64V6.64C6.64 6.94975 6.89025 7.2 7.2 7.2H12.8C13.1097 7.2 13.36 6.94975 13.36 6.64V4.6415L15.74 7.0215V15.74ZM10 8.775C8.60875 8.775 7.48 9.90375 7.48 11.295C7.48 12.6862 8.60875 13.815 10 13.815C11.3912 13.815 12.52 12.6862 12.52 11.295C12.52 9.90375 11.3912 8.775 10 8.775ZM10 12.695C9.2265 12.695 8.6 12.0685 8.6 11.295C8.6 10.5215 9.2265 9.895 10 9.895C10.7735 9.895 11.4 10.5215 11.4 11.295C11.4 12.0685 10.7735 12.695 10 12.695Z"
                  fill="#4C4C4D"
                />
              </svg>
            </ReglamentSmallButton>
            <ReglamentSmallButton
              class="w-auto flex flex-row justify-center items-center"
              :class="{'cursor-default opacity-[0.5]': buttonDisabled === true || buttonSaveReglament === 0}"
              @click="setEdit"
            >
              <svg
                width="20"
                height="20"
                viewBox="0 0 20 20"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M16.6727 6.17275L13.8273 3.32725C13.696 3.196 13.535 3.09975 13.36 3.04725V3H3.56C3.25025 3 3 3.25025 3 3.56V16.44C3 16.7498 3.25025 17 3.56 17H16.44C16.7498 17 17 16.7498 17 16.44V6.96375C17 6.66625 16.8827 6.38275 16.6727 6.17275ZM7.76 4.26H12.24V6.08H7.76V4.26ZM15.74 15.74H4.26V4.26H6.64V6.64C6.64 6.94975 6.89025 7.2 7.2 7.2H12.8C13.1097 7.2 13.36 6.94975 13.36 6.64V4.6415L15.74 7.0215V15.74ZM10 8.775C8.60875 8.775 7.48 9.90375 7.48 11.295C7.48 12.6862 8.60875 13.815 10 13.815C11.3912 13.815 12.52 12.6862 12.52 11.295C12.52 9.90375 11.3912 8.775 10 8.775ZM10 12.695C9.2265 12.695 8.6 12.0685 8.6 11.295C8.6 10.5215 9.2265 9.895 10 9.895C10.7735 9.895 11.4 10.5215 11.4 11.295C11.4 12.0685 10.7735 12.695 10 12.695Z"
                  fill="#4C4C4D"
                />
              </svg>

              {{ editButtonText }}
            </ReglamentSmallButton>
          </div>
        </div>

        <div
          class="flex justify-start gap-[8px] mr-3 leading-[30px] text-[13px] text-[#424242]"
        >
          <span class="font-medium">Отдел:</span>
          <PopMenu>
            <div class="flex flex-row items-center cursor-pointer hover:transition hover:opacity-[0.8]">
              <div class="mr-1">
                {{ currDepTitle }}
              </div>
              <svg
                width="10"
                height="6"
                viewBox="0 0 10 6"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M0.235034 0.204742C0.536173 -0.0796667 1.01085 -0.0661044 1.29526 0.235034L5 4.1577L8.70474 0.235034C8.98915 -0.0661044 9.46383 -0.0796667 9.76497 0.204742C10.0661 0.489151 10.0797 0.963831 9.79526 1.26497L5.54526 5.76497C5.40358 5.91499 5.20635 6 5 6C4.79366 6 4.59642 5.91499 4.45474 5.76497L0.204742 1.26497C-0.0796667 0.963831 -0.0661044 0.489151 0.235034 0.204742Z"
                  fill="#7E7E80"
                />
              </svg>
            </div>
            <template #menu>
              <div class="max-h-[220px] overflow-y-auto scroll-style max-w-[260px]">
                <PopMenuItem
                  @click="currDep = ''"
                >
                  Общий для всех отделов
                </PopMenuItem>
                <PopMenuItem
                  v-for="dep in allDepartments"
                  :key="dep.uid"
                  @click="currDep = dep.uid"
                >
                  {{ dep.name }}
                </PopMenuItem>
              </div>
            </template>
          </PopMenu>
          <div
            class="flex justify-start leading-[30px] text-[13px] text-[#424242]"
          >
            <span class="font-medium pr-3">Редакторы:</span>
            <PopMenu v-if="!editorsCanEdit">
              <div class="flex flex-row items-center cursor-pointer hover:transition hover:opacity-[0.8]">
                <span class="mr-1">Добавить редактора</span>
                <svg
                  class="mr-3"
                  width="12"
                  height="12"
                  viewBox="0 0 12 12"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M6.70474 5.29517L6.70943 1.00347C6.70941 0.815309 6.63466 0.634852 6.5016 0.501794C6.36854 0.368736 6.18808 0.293977 5.99992 0.293964C5.81176 0.29395 5.63132 0.368683 5.49828 0.501722C5.36524 0.634761 5.29051 0.815208 5.29052 1.00337L5.29582 5.29507L1.00412 5.28976C0.815963 5.28975 0.635516 5.36448 0.502477 5.49752C0.369438 5.63056 0.294705 5.81101 0.294718 5.99917C0.294732 6.18733 0.36949 6.36779 0.502548 6.50084C0.635606 6.6339 0.816064 6.70866 1.00422 6.70867L5.29592 6.70398L5.29123 10.9957C5.29085 11.089 5.30895 11.1814 5.34447 11.2676C5.38 11.3539 5.43225 11.4322 5.49821 11.4982C5.56417 11.5642 5.64253 11.6164 5.72878 11.6519C5.81503 11.6875 5.90746 11.7056 6.00073 11.7052C6.09401 11.7056 6.18643 11.6875 6.27268 11.652C6.35892 11.6165 6.43728 11.5642 6.50323 11.4983C6.56918 11.4323 6.62142 11.354 6.65693 11.2677C6.69244 11.1815 6.71053 11.0891 6.71014 10.9958L6.70484 6.70408L10.9965 6.70938C11.0898 6.70977 11.1822 6.69169 11.2685 6.65618C11.3547 6.62067 11.4331 6.56843 11.499 6.50248C11.565 6.43653 11.6172 6.35817 11.6527 6.27192C11.6882 6.18568 11.7063 6.09325 11.7059 5.99998C11.7063 5.90671 11.6882 5.81428 11.6527 5.72803C11.6172 5.64178 11.5649 5.56341 11.499 5.49745C11.433 5.43149 11.3546 5.37924 11.2684 5.34372C11.1821 5.30819 11.0897 5.2901 10.9964 5.29048L6.70474 5.29517Z"
                    fill="#7E7E80"
                  />
                </svg>
              </div>
              <template #menu>
                <div class="max-h-[220px] overflow-y-auto scroll-style max-w-[260px]">
                  <BoardPropsMenuItemUser
                    v-for="editor in usersCanAddToAccess"
                    :key="editor.email"
                    :show-check-mark="checkEditor(editor.email)"
                    :user-email="editor.email"
                    @click="addReglamentEditor(editor.email)"
                  />
                </div>
              </template>
            </PopMenu>
            <EmployeeProfile
              v-for="editor in currEditors"
              :key="editor"
              class="mr-3"
              :photo="editorFoto(editor)"
              :name="editorName(editor)"
            />
          </div>
        </div>
      </div>
    </div>
    <div class="bg-white rounded-[28px]">
      <!-- Пустой див для корректного поведения quill-tollbar'a-->
      <div>
        <QuillEditor
          v-model:content="currText"
          content-type="html"
          :toolbar="'full'"
          class="h-auto mb-5 bg-white"
          @paste="pasteEvent"
        />
      </div>
      <hr class="border-[1px] mr-10 ml-10">
      <div
        v-if="questions.length > 0"
        class="text-center font-roboto font-bold text-[19px] mb-[20px] mr-[90px] text-[#424242] mt-[30px]"
      >
        В тесте будут представлены следующие вопросы
      </div>
      <div>
        <template
          v-for="(question , index) in questions"
          :key="index"
        >
          <ReglamentQuestion
            :ref="question.uid"
            :is-editing="true"
            :question="question"
            :reglament="reglament"
            @deleteQuestion="onDeleteQuestion"
            @deleteAnswer="deleteAnswer"
            @addQuestion="onAddQuestion"
            @updateQuestionName="updateQuestionName"
            @updateAnswerName="updateAnswerName"
            @pushAnswer="pushAnswer"
            @selectAnswer="selectAnswer"
            @setRightAnswer="setRightAnswer"
          />
        </template>
      </div>
      <div class="flex justify-center">
        <button
          v-if="canEdit"
          class="flex items-center justify-center rounded-[8px] mt-5 w-[220px] mb-5 bg-[#F2B679] px-[40px] py-[12px] hover:transition hover:opacity-[0.8]"
          @click.stop="onAddQuestion"
        >
          Создать вопрос
        </button>
      </div>
    </div>
    <div
      v-if="!isTesting && contributors.length"
      class="mt-5"
    >
      <button
        class="flex items-center font-['Roboto'] text-[#424242] dark:bg-gray-700 dark:text-gray-100 rounded-lg text-[13px] font-medium hover:text-black transition"
        @click="showEmployees = !showEmployees"
      >
        <span class="mr-1.5">Сотрудники, прошедшие регламент</span>
        <svg
          width="12"
          height="7"
          viewBox="0 0 12 7"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M0.750134 0.493017C0.397701 0.850149 0.40151 1.42536 0.758642 1.7778L5.17757 6.13858C5.51147 6.46809 6.04134 6.48919 6.40039 6.18728L11.1876 2.16194C11.5716 1.83903 11.6211 1.26594 11.2982 0.881909C10.9753 0.497881 10.4022 0.448338 10.0182 0.771252L5.86461 4.26382L2.03491 0.484509C1.67778 0.132076 1.10257 0.135885 0.750134 0.493017Z"
            fill="#4C4C4D"
          />
        </svg>
      </button>
      <div
        v-if="showEmployees"
        class="flex items-center mt-2.5"
      >
        <button
          v-if="canEdit"
          class="flex items-center h-[30px] mr-3 px-[10px] py-[5px] bc-none border-none text-xs text-[#4C4C4D]"
          @click="clearContributors"
        >
          <svg
            class="mr-1.5"
            width="20"
            height="20"
            viewBox="0 0 18 18"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M3.37364 15.8768H17.7486V17.1268H3.37364V15.8768ZM16.1111 5.57051L11.1549 0.620514C11.0388 0.504294 10.9009 0.412095 10.7492 0.34919C10.5974 0.286284 10.4348 0.253906 10.2705 0.253906C10.1062 0.253906 9.94359 0.286284 9.79184 0.34919C9.64009 0.412095 9.50223 0.504294 9.38614 0.620514L0.636139 9.37051C0.519919 9.48661 0.42772 9.62447 0.364815 9.77621C0.30191 9.92796 0.269531 10.0906 0.269531 10.2549C0.269531 10.4192 0.30191 10.5818 0.364815 10.7336C0.42772 10.8853 0.519919 11.0232 0.636139 11.1393L3.45489 14.0018H9.44864L16.1111 7.33926C16.2274 7.22317 16.3196 7.08531 16.3825 6.93357C16.4454 6.78182 16.4777 6.61916 16.4777 6.45489C16.4777 6.29062 16.4454 6.12796 16.3825 5.97621C16.3196 5.82447 16.2274 5.68661 16.1111 5.57051ZM8.92989 12.7518H3.99864L1.49864 10.2518L5.44239 6.30801L10.3986 11.258L8.92989 12.7518ZM11.2799 10.4018L6.32989 5.44551L10.2486 1.50176L15.2486 6.45801L11.2799 10.4018Z"
              fill="#4C4C4D"
            />
          </svg>

          Очистить
        </button>
        <EmployeeProfile
          v-for="contributor in contributorsInfo"
          :key="contributor.uid"
          class="mr-3"
          :photo="contributor.photo"
          :name="contributor.name"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { QuillEditor } from '@vueup/vue-quill'

import ReglamentSmallButton from '@/components/Reglaments/ReglamentSmallButton.vue'
import EmployeeProfile from '../Employees/EmployeeProfile.vue'
import PopMenu from '@/components/Common/PopMenu.vue'
import PopMenuItem from '@/components/Common/PopMenuItem.vue'
import BoardPropsMenuItemUser from '@/components/Board/BoardPropsMenuItemUser.vue'
import ReglamentQuestion from '@/components/Reglaments/ReglamentQuestion'
import '@vueup/vue-quill/dist/vue-quill.snow.css'
import * as REGLAMENTS from '@/store/actions/reglaments'
import * as QUESTIONS from '@/store/actions/reglament_questions'
import * as ANSWER from '@/store/actions/reglament_answers'

export default {
  components: {
    QuillEditor,
    ReglamentSmallButton,
    PopMenu,
    PopMenuItem,
    BoardPropsMenuItemUser,
    ReglamentQuestion,
    EmployeeProfile
  },
  props: {
    reglament: {
      type: Object,
      default: () => {}
    }
  },
  emits: ['exitEditMode'],
  data () {
    return {
      currEditors: [],
      currName: '',
      currDep: '',
      currText: this.$store.state.reglaments.reglaments[this.reglament?.uid].content ?? '',
      saveContentStatus: 1, // 1 - is saved, 2 error, 0 request processing
      buttonSaveReglament: 1, // то же самое что и saveContentStatus, сделано для того, чтобы 2 кнопки не принимали 1 статус
      isFormInvalid: false,
      showEmployees: false,
      buttonDisabled: false
    }
  },
  computed: {
    allDepartments () {
      const deps = Object.values(this.$store.state.departments.deps)
      deps.sort((item1, item2) => {
        // сначала по порядку
        if (item1.order > item2.order) return 1
        if (item1.order < item2.order) return -1
        // если одинаковый, то по имени
        if (item1.name > item2.name) return 1
        if (item1.name < item2.name) return -1
        return 0
      })
      if (this.showAllReglaments) return deps
      const currentUserEmail = this.$store.state.user.user.current_user_email.toLowerCase()
      return deps.filter(dep => dep.emails.find(email => email.toLowerCase() === currentUserEmail))
    },
    showAllReglaments () {
      const employees = this.$store.state.employees.employees
      const user = this.$store.state.user.user
      const userType = employees[user.current_user_uid].type
      const userAdmin = userType === 1 || userType === 2
      return userAdmin
    },
    user () {
      return this.$store.state.user.user
    },
    questions () {
      return this.$store?.state?.reglaments?.reglamentQuestions
    },
    currReglament () {
      return this.$store.state.reglaments.reglaments[this.reglament?.uid]
    },
    editorsCanEdit () {
      return this.currReglament?.editors?.includes(this.$store.state.user.user.current_user_email)
    },
    canEdit () {
      return (this.currReglament?.email_creator === this.user.current_user_email) || (this.editorsCanEdit) || (this.userType === 2 || this.userType === 1)
    },
    reglamentTitle () {
      return this.currReglament?.name ?? ''
    },
    reglamentContent () {
      return this.currReglament?.content ?? ''
    },
    reglamentCreatorEmail () {
      return this.currReglament?.email_creator ?? ''
    },
    reglamentDep () {
      return this.currReglament?.department_uid ?? ''
    },
    reglamentEditors () {
      return this.currReglament?.editors ?? []
    },
    contributors () {
      return this.$store?.state?.reglaments?.contributors
    },
    contributorsInfo () {
      return this.contributors.map((contributor) => {
        const user = this.$store.state.employees.employees[contributor.uid_user]
        return {
          uid: contributor.uid_user,
          photo: user?.fotolink ?? '',
          name: user?.name || user?.email || '???'
        }
      })
    },
    currDepTitle () {
      const dep = this.$store.state.departments.deps[this.currDep]
      return dep?.name || 'Общий для всех отделов'
    },
    usersCanAddToAccess () {
      const users = []
      const employees = Object.values(this.$store.state.employees.employees)
      const creator = this.reglamentCreatorEmail.toLowerCase()
      for (const emp of employees) {
        if (emp.email.toLowerCase() !== creator) {
          users.push({
            uid: emp.uid,
            email: emp.email
          })
        }
      }
      return users
    },
    editButtonText () {
      if (this.saveContentStatus === 1) {
        return 'Закрыть и сохранить'
      } else if (this.saveContentStatus === 2) {
        return 'Ошибка сохранения регламента'
      } else if (this.saveContentStatus === 0) {
        return 'Сохраняется'
      }
      return 'Сохраняется'
    }
  },
  mounted () {
    this.currName = this.reglamentTitle
    this.currEditors = [...this.reglamentEditors]
    this.currDep = this.reglamentDep
  },
  methods: {
    onDeleteQuestion (uid) {
      this.$store.commit(REGLAMENTS.REGLAMENT_DELETE_QUESTION, uid)
    },
    deleteAnswer (uid) {
      this.$store.commit(REGLAMENTS.REGLAMENT_DELETE_ANSWER, uid)
    },
    pushAnswer (data) {
      this.$store.commit(REGLAMENTS.REGLAMENT_PUSH_ANSWER, data)
    },
    updateAnswerName (data) {
      this.$store.commit(REGLAMENTS.REGLAMENT_UPDATE_ANSWER_NAME, data)
    },
    selectAnswer (data) {
      this.$store.commit(REGLAMENTS.REGLAMENT_SELECT_ANSWER, data)
    },
    setRightAnswer (data) {
      this.$store.commit(REGLAMENTS.REGLAMENT_SET_RIGHT_ANSWER, data)
    },
    clearContributors () {
      this.$store.dispatch(REGLAMENTS.DELETE_USERS_REGLAMENT_ANSWERS, this.reglament.uid)
    },
    editorName (email) {
      return this.$store.state.employees.employeesByEmail[email]?.name
    },
    editorFoto (email) {
      return this.$store.state.employees.employeesByEmail[email]?.fotolink
    },
    updateQuestionName (data) {
      this.$store.commit(REGLAMENTS.REGLAMENT_UPDATE_QUESTION_NAME, data)
    },
    uuidv4 () {
      return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
        (
          c ^
          (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
        ).toString(16)
      )
    },
    gotoNode (uid) {
      this.$refs[uid][0].onFocus()
    },
    onAddQuestion () {
      const question = {
        uid: this.uuidv4(),
        name: '',
        uid_reglament: this.currReglament.uid
      }
      const answer = {
        uid: this.uuidv4(),
        uid_question: question.uid,
        name: '',
        is_right: 0,
        needToCreate: true,
        needToUpdate: false
      }

      const questionToPush = {
        uid: question.uid,
        name: question.name,
        uid_reglament: question.uid_reglament,
        needToCreate: true,
        needToUpdate: false,
        answers: [answer]
      }
      this.$store.commit(REGLAMENTS.REGLAMENT_PUSH_QUESTION, questionToPush)
      this.$nextTick(() => {
        this.gotoNode(questionToPush.uid)
      })
    },
    addReglamentEditor (email) {
      const index = this.currEditors.findIndex(editor => editor.toLowerCase() === email.toLowerCase())
      if (index !== -1) {
        this.currEditors.splice(index, 1)
      } else {
        this.currEditors.push(email)
      }
    },
    checkEditor (email) {
      return this.currEditors.includes(email)
    },
    saveReglament (reglament) {
      if (!reglament.name.length) {
        reglament.name = 'Регламент без названия'
      }

      for (const question of this.questions) {
        if (question.needToCreate || question.needToUpdate) {
          const questionData = {
            name: question.name,
            uid: question.uid,
            uid_reglament: question.uid_reglament
          }
          Promise.resolve().then(() => {
            if (question.needToCreate) {
              return this.$store.dispatch(QUESTIONS.CREATE_REGLAMENT_QUESTION_REQUEST, questionData)
            } else if (question.needToUpdate) {
              return this.$store.dispatch(QUESTIONS.UPDATE_REGLAMENT_QUESTION_REQUEST, questionData)
            }
          })
            .then(() => {
              question.needToCreate = false
              question.needToUpdate = false
            })
        }

        for (const answer of question.answers) {
          if (answer.needToCreate || answer.needToUpdate) {
            const answerData = {
              uid: answer.uid,
              uid_question: answer.uid_question,
              name: answer.name,
              is_right: answer.is_right
            }

            Promise.resolve().then(() => {
              if (answer.needToCreate) {
                return this.$store.dispatch(ANSWER.CREATE_REGLAMENT_ANSWER_REQUEST, answerData)
              } else if (answer.needToUpdate) {
                return this.$store.dispatch(ANSWER.UPDATE_REGLAMENT_ANSWER_REQUEST, answerData)
              }
            })
              .then(() => {
                answer.needToCreate = false
                answer.needToUpdate = false
              })
          }
        }
      }

      this.$store.state.reglaments.answersToDelete.forEach((uid, index) => {
        this.$store.dispatch(ANSWER.DELETE_REGLAMENT_ANSWER_REQUEST, uid).then(() => {
          this.$store.state.reglaments.answersToDelete.splice(index, 1)
        })
      })

      this.$store.state.reglaments.questionsToDelete.forEach((uid, index) => {
        this.$store.dispatch(QUESTIONS.DELETE_REGLAMENT_QUESTION_REQUEST, uid).then(() => {
          this.$store.state.reglaments.questionsToDelete.splice(index, 1)
        })
      })

      return this.$store.dispatch(REGLAMENTS.UPDATE_REGLAMENT_REQUEST, reglament)
    },
    setEdit () {
      if (this.buttonDisabled === true || this.buttonSaveReglament === 0) {
        return
      }
      const reglament = { ...this.currReglament }
      reglament.content = this.currText
      reglament.name = this.currName.trim()
      reglament.department_uid = this.currDep
      reglament.editors = [...this.currEditors]

      this.saveContentStatus = 0
      this.isFormInvalid = false
      this.firstInvalidQuestionUid = null
      this.validateReglamentQuestions()
      if (this.isFormInvalid && this.firstInvalidQuestionUid) {
        this.gotoNode(this.firstInvalidQuestionUid)
        return
      }

      this.saveReglament(reglament).then(() => {
        this.saveContentStatus = 1
        // обновляем регламент в сторе
        // надо бы сделать по нормальному через мутацию
        const reglaments = this.$store.state.navigator.navigator.reglaments
        const index = reglaments.items.findIndex(item => item.uid === reglament.uid)
        if (index !== -1) reglaments.items[index] = reglament
        this.$emit('exitEditMode')
      }).catch(() => {
        this.saveContentStatus = 2
      })
    },
    getBack () {
      this.$emit('exitEditMode')
    },
    validateReglamentQuestions () {
      for (const question of this.questions) {
        question.invalid = question.name === ''

        const checkOnEmptyRightAnswers = question.answers.find((answer) => {
          if (answer.is_right === true || answer.is_right === 1) {
            return true
          }
          return false
        })

        if (!checkOnEmptyRightAnswers) {
          question.invalid = true
          question.errorText = 'Отметьте как минимум один правильный ответ'
          if (!this.isFormInvalid) {
            this.isFormInvalid = true
          }
          if (!this.firstInvalidQuestionUid) {
            this.firstInvalidQuestionUid = question.uid
          }
        }

        if (!this.isFormInvalid && question.name === '') {
          this.isFormInvalid = true
          this.firstInvalidQuestionUid = question.uid
        }

        for (const answer of question.answers) {
          answer.invalid = answer.name === ''

          if (!this.isFormInvalid && answer.name === '') {
            this.isFormInvalid = true
            this.firstInvalidQuestionUid = question.uid
          }
        }
      }
    },
    onSaveReglamentButtonClick () {
      if (this.buttonDisabled === true || this.saveContentStatus === 0) {
        return
      }
      const reglament = { ...this.currReglament }
      reglament.content = this.currText
      reglament.name = this.currName.trim()
      reglament.department_uid = this.currDep
      reglament.editors = [...this.currEditors]

      this.isFormInvalid = false
      this.firstInvalidQuestionUid = null
      this.validateReglamentQuestions()
      if (this.isFormInvalid && this.firstInvalidQuestionUid) {
        this.gotoNode(this.firstInvalidQuestionUid)
        return
      }

      this.buttonSaveReglament = 0
      this.saveReglament(reglament).then(() => {
        this.buttonSaveReglament = 1
        // обновляем регламент в сторе
        // надо бы сделать по нормальному через мутацию
        const reglaments = this.$store.state.navigator.navigator.reglaments
        const index = reglaments.items.findIndex(item => item.uid === reglament.uid)
        if (index !== -1) reglaments.items[index] = reglament
        this.buttonDisabled = true
        setTimeout(() => {
          this.buttonDisabled = false
        }, 5000)
      }).catch(() => {
        this.buttonSaveReglament = 2
      })
    },
    pasteEvent (e) {
      const node = e.target.parentNode.classList.contains('ql-editor') ? e.target : e.target.parentNode
      setTimeout(() => {
        node.scrollIntoView({ block: 'center' })
      }, 1)
    }
  }
}
</script>
