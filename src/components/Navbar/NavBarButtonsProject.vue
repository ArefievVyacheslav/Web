<template>
  <div class="flex items-center gap-[10px]">
    <BoardModalBoxDelete
      v-if="showDeleteProject"
      title="Удалить проект"
      :text="deleteMessage"
      @cancel="showDeleteProject = false"
      @yes="onDeleteProject"
    />
    <BoardModalBoxRename
      v-if="showAddProject"
      :show="showAddProject"
      title="Добавить подпроект"
      @cancel="showAddProject = false"
      @save="onAddNewProject"
    />
    <PopMenu>
      <NavBarButtonIcon icon="menu" />
      <template #menu>
        <PopMenuItem
          @click="clickCompletedTasks"
        >
          {{ showCompletedTasks ? 'Скрыть завершенные задачи' : 'Показать завершенные задачи' }}
        </PopMenuItem>
        <PopMenuDivider />
        <PopMenuItem
          icon="edit"
          @click="clickEditProject"
        >
          Свойства проекта
        </PopMenuItem>
        <PopMenuItem
          v-if="canEditProject"
          icon="add"
          @click="clickAddProject"
        >
          Создать подпроект
        </PopMenuItem>
        <PopMenuItem
          v-if="canEditProject"
          icon="delete"
          @click="clickDeleteProject"
        >
          Удалить проект
        </PopMenuItem>
        <PopMenuItem
          @click="favoriteToggle"
        >
          {{ !isFavorite ? 'Добавить в избранное' : 'Удалить из избранного' }}
        </PopMenuItem>
      </template>
    </PopMenu>
  </div>
</template>

<script>
import NavBarButtonIcon from '@/components/Navbar/NavBarButtonIcon.vue'
import BoardModalBoxRename from '@/components/Board/BoardModalBoxRename.vue'
import PopMenu from '@/components/Common/PopMenu.vue'
import PopMenuItem from '@/components/Common/PopMenuItem.vue'
import PopMenuDivider from '@/components/Common/PopMenuDivider.vue'
import BoardModalBoxDelete from '@/components/Board/BoardModalBoxDelete.vue'

import {
  ADD_PROJECT_TO_FAVORITE,
  REMOVE_PROJECT_FROM_FAVORITE,
  REMOVE_PROJECT_REQUEST,
  SELECT_PROJECT,
  CREATE_PROJECT_REQUEST,
  PUSH_PROJECT
} from '@/store/actions/projects'
import { NAVIGATOR_REMOVE_PROJECT } from '@/store/actions/navigator'
import * as NAVIGATOR from '@/store/actions/navigator'

export default {
  components: {
    NavBarButtonIcon,
    BoardModalBoxRename,
    PopMenu,
    PopMenuItem,
    PopMenuDivider,
    BoardModalBoxDelete
  },
  props: {
    projectUid: {
      type: String,
      default: ''
    },
    showCompletedTasks: {
      type: Boolean,
      default: false
    }
  },
  emits: ['popNavBar', 'toggleCompletedTasks'],
  data () {
    return {
      showDeleteProject: false,
      showProjectLimit: false,
      showAddProject: false
    }
  },
  computed: {
    deleteMessage () {
      if (this.project.children.length !== 0) {
        return 'Вы действительно хотите удалить проект? \n\n Внимание! Все дочерние проекты будут удалены.'
      }
      return 'Вы действительно хотите удалить проект?'
    },
    project () {
      return this.$store.state.projects.projects[this.projectUid]
    },
    canEditProject () {
      return this.project?.email_creator === this.$store.state.user?.user?.current_user_email
    },
    canAddChild () {
      const user = this.$store.state.user.user
      return this.project?.email_creator === user.current_user_email
    },
    isFavorite () {
      return this.project?.favorite
    }
  },
  methods: {
    clickEditProject () {
      if (!this.$store.state.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', true)
      }
      this.$store.commit('basic', { key: 'propertiesState', value: 'project' })
      this.$store.commit(SELECT_PROJECT, this.project)
    },
    clickDeleteProject () {
      this.showDeleteProject = true
    },
    clickAddProject () {
      const user = this.$store.state.user.user
      // если лицензия истекла
      if (Object.keys(this.$store.state.projects.projects).length >= 3 && user.days_left <= 0) {
        this.showProjectLimit = true
        return
      }
      this.showAddProject = true
    },
    onAddNewProject (name) {
      this.showAddProject = false
      const title = name.trim()
      if (title) {
        // добавляем новую доску и переходим в неё
        const maxOrder =
          this.project?.children?.reduce(
            (maxOrder, child) =>
              child.order > maxOrder ? child.order : maxOrder,
            0
          ) ?? 0
        const user = this.$store.state.user.user
        const members = {}
        members[user.current_user_uid] = 1
        const project = {
          uid: this.uuidv4(),
          name: title,
          uid_parent: this.project.uid,
          email_creator: user.current_user_email,
          order: maxOrder + 1,
          collapsed: 0,
          color: '',
          public_link_status: 0,
          show_date: 0,
          favorite: 0,
          stages: [],
          children: [],
          members
        }
        console.log(`create project uid: ${project.uid}`, project)
        this.$store.dispatch(CREATE_PROJECT_REQUEST, project).then((res) => {
          // заполняем недостающие параметры
          project.global_property_uid = '1b30b42c-b77e-40a4-9b43-a19991809add'
          project.type_access = res.data.type_access
          project.color = '#A998B6'
          //
          this.$store.commit(PUSH_PROJECT, [project])
          this.$store.commit(NAVIGATOR.NAVIGATOR_PUSH_PROJECT, [project])
          this.gotoChildren(project)
        })
      }
    },
    gotoChildren (project) {
      this.$store.commit('basic', {
        key: 'taskListSource',
        value: { uid: project.global_property_uid, param: project.uid }
      })
      const navElem = {
        name: project.name,
        key: 'greedSource',
        uid: project.uid,
        global_property_uid: project.global_property_uid,
        greedPath: 'projects_children',
        value: project.children
      }
      this.$store.commit('pushIntoNavStack', navElem)
      this.$store.commit('basic', { key: 'greedSource', value: project.children })
      this.$store.commit('basic', {
        key: 'greedPath',
        value: 'projects_children'
      })
    },
    uuidv4 () {
      return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
        (
          c ^
          (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
        ).toString(16)
      )
    },
    onDeleteProject () {
      this.showDeleteProject = false
      //
      this.$store.dispatch(REMOVE_PROJECT_REQUEST, this.projectUid)
        .then(() => {
          this.$store.dispatch('asidePropertiesToggle', false)
          this.$store.commit(SELECT_PROJECT, undefined)
          //
          this.$store.commit(NAVIGATOR_REMOVE_PROJECT, this.project)
          // для актуального значения количества проектов
          this.$store.commit(REMOVE_PROJECT_REQUEST, this.projectUid)
          //
          this.$emit('popNavBar')
        })
    },
    clickCompletedTasks () {
      this.$emit('toggleCompletedTasks')
    },
    favoriteToggle () {
      if (!this.isFavorite) {
        this.$store.dispatch(ADD_PROJECT_TO_FAVORITE, this.project)
          .then(res => {
            this.project.favorite = res.data.favorite
          })
      } else {
        this.$store.dispatch(REMOVE_PROJECT_FROM_FAVORITE, this.project)
          .then(res => {
            this.project.favorite = res.data.favorite
          })
      }
    }
  }
}
</script>

<style scoped>

</style>
