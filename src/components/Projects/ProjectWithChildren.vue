<template>
  <NavBar />
  <div
    class="w-full"
    :class="{ 'pt-[30px]': !canAddChild, 'pt-[45px]' : canAddChild}"
  >
    <ProjectModalBoxProjectsLimit
      v-if="showProjectsLimit"
      @cancel="showProjectsLimit = false"
      @ok="showProjectsLimit = false"
    />
    <div class="grid gap-2 mt-3 grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
      <template
        v-for="project in projects"
        :key="project.uid"
      >
        <ProjectBlocItem
          :project="project"
          @click.stop="gotoChildren(project)"
        />
      </template>
    </div>
    <div class="mt-5">
      <TasksListNew class="pt-[0px]" />
    </div>
  </div>
</template>

<script>
import ProjectBlocItem from '@/components/Projects/ProjectBlocItem.vue'
import ProjectModalBoxProjectsLimit from '@/components/ProjectModalBoxProjectsLimit.vue'
import TasksListNew from '@/components/TasksListNew.vue'
import * as TASK from '@/store/actions/tasks'

import NavBar from '@/components/NavBar.vue'

export default {
  components: {
    ProjectBlocItem,
    ProjectModalBoxProjectsLimit,
    TasksListNew,
    NavBar
  },
  props: {
    projects: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      showAdd: false,
      showProjectsLimit: false
    }
  },
  computed: {
    currentProject () {
      const projects = this.$store.state.projects.projects
      const navStack = this.$store.state.navbar.navStack
      const currProjectUid = navStack[navStack.length - 1].uid
      const project = projects[currProjectUid]
      return project
    }
  },
  methods: {
    print (msg, val) {
      console.log(msg, val)
    },
    canAddChild () {
      const user = this.$store.state.user.user
      return this.project?.email_creator === user.current_user_email
    },
    gotoChildren (project) {
      this.$store.dispatch('asidePropertiesToggle', false)

      this.$store.dispatch(TASK.PROJECT_TASKS_REQUEST, project.uid)
      this.$store.commit('basic', {
        key: 'taskListSource',
        value: { uid: project.global_property_uid, param: project.uid }
      })

      this.$store.commit(TASK.CLEAN_UP_LOADED_TASKS)

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
      this.$store.commit('basic', { key: 'greedPath', value: 'projects_children' })
    }
  }
}
</script>

<style scoped></style>
