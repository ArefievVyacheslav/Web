<template>
  <TagModalBoxTagsLimit
    v-if="showTagsLimit"
    @cancel="showTagsLimit = false"
    @ok="showTagsLimit = false"
  />
  <div
    class="w-full flex items-center justify-between order-1 pt-[30px]"
  >
    <p
      class="font-['Roboto'] text-[#424242] text-[19px] leading-[22px] font-bold"
    >
      Метки
    </p>
    <div
      class="flex"
    >
      <Icon
        :path="listView.path"
        :width="listView.width"
        :height="listView.height"
        :box="listView.viewBox"
        class="cursor-pointer hover:text-gray-800 mr-2 mt-0.5"
        :class="isGridView ? 'text-gray-400' : 'text-gray-800'"
        @click="updateGridView(false)"
      />
      <Icon
        :path="gridView.path"
        :width="gridView.width"
        :height="gridView.height"
        :box="gridView.viewBox"
        class="cursor-pointer hover:text-gray-800 mr-2 mt-0.5"
        :class="!isGridView ? 'text-gray-400' : 'text-gray-800'"
        @click="updateGridView(true)"
      />
    </div>
  </div>
  <div
    class="grid gap-2 mt-3 grid-cols-1 order-2"
    :class="{ 'md:grid-cols-2 lg:grid-cols-4': isGridView, 'lg:grid-cols-2': isPropertiesMobileExpanded && isGridView }"
  >
    <InputValue
      v-if="showAddTag"
      @save="createTag"
      @cancel="showAddTag = false"
    />
    <AddTag
      v-else
      @click="clickAddTag"
    />
    <template
      v-for="tag in tags"
      :key="tag.uid"
    >
      <ListBlocItem
        :count="tag.children?.length ?? 0"
        :color="tag.back_color"
        :title="tag.name"
        @click="openProperties(tag)"
      >
        <TagIcon />
      </ListBlocItem>
    </template>
  </div>
  <EmptyTasksListPics v-if="isEmpty" />
</template>

<script>
import ListBlocItem from '@/components/Common/ListBlocItem.vue'
import TagModalBoxTagsLimit from '@/components/Tags/TagModalBoxTagsLimit.vue'
import TagIcon from '@/components/Tags/Icons/TagIcon.vue'
import Icon from '@/components/Icon.vue'
import AddTag from '@/components/Tags/AddTag.vue'
import EmptyTasksListPics from '@/components/TasksList/EmptyTasksListPics'
import { setLocalStorageItem } from '@/store/helpers/functions'
import gridView from '@/icons/grid-view.js'
import listView from '@/icons/list-view.js'
import * as TASK from '@/store/actions/tasks'
import { SELECT_TAG } from '@/store/actions/tasks'
import * as NAVIGATOR from '@/store/actions/navigator'
import InputValue from '@/components/InputValue'

export default {
  components: {
    ListBlocItem,
    TagIcon,
    AddTag,
    Icon,
    TagModalBoxTagsLimit,
    EmptyTasksListPics,
    InputValue
  },
  data () {
    return {
      gridView,
      listView,
      focusedTag: '',
      showTagsLimit: false,
      showModal: false,
      randomColors: [
        '#F5F5DC',
        '#FFE5B4',
        '#FFC0CB',
        '#D0F0C0',
        '#C9A0DC',
        '#D8BFD8',
        '#FFCC00',
        '#F4A460',
        '#FFDB58',
        '#E6E6FA'
      ],
      showAddTag: false
    }
  },
  computed: {
    tags () {
      return this.$store.getters.sortedNavigator.tags?.items
    },
    isGridView () {
      return this.$store.state.isGridView
    },
    isPropertiesMobileExpanded () {
      return this.$store.state.isPropertiesMobileExpanded
    },
    user () {
      return this.$store.state.user.user
    },
    storeTasks () {
      return this.$store.state.tasks.newtasks
    },
    navStack () {
      return this.$store.state.navbar.navStack
    },
    isEmpty () {
      return this.tags.length < 1
    }
  },
  mounted () {
    localStorage.setItem('lastTab', 'directory')
    this.$store.commit('basic', { key: 'mainSectionState', value: 'greed' })
    this.$store.commit('basic', { key: 'greedPath', value: 'tags' })
    const navElem = {
      name: 'Метки',
      key: 'greedSource',
      greedPath: 'tags',
      value: this.$store.state.navigator.navigator.tags?.items
    }
    this.$store.commit('updateStackWithInitValue', navElem)
    this.$store.commit('basic', { key: 'greedSource', value: navElem.value })
  },
  methods: {
    updateGridView (value) {
      this.$store.commit('basic', { key: 'isGridView', value: value })
      setLocalStorageItem('isGridView', value)
    },
    openProperties (tag) {
      if (!this.$store.state.isPropertiesMobileExpanded) {
        this.$store.dispatch('asidePropertiesToggle', true)
      }
      this.$store.commit('basic', { key: 'propertiesState', value: 'tag' })
      this.$store.commit(SELECT_TAG, tag)
    },
    gotoChildren (value) {
      this.$store.dispatch(TASK.TAG_TASKS_REQUEST, value.uid)
      this.$store.commit('basic', {
        key: 'taskListSource',
        value: { uid: value.global_property_uid, param: value.uid }
      })

      this.$store.commit(TASK.CLEAN_UP_LOADED_TASKS)

      const navElem = {
        name: value.name,
        key: 'greedSource',
        uid: value.uid,
        global_property_uid: value.global_property_uid,
        greedPath: 'tags_children',
        value: value.children
      }

      this.$store.commit('pushIntoNavStack', navElem)
      this.$store.commit('basic', { key: 'greedSource', value: value.children })
      this.$store.commit('basic', { key: 'greedPath', value: 'tags_children' })
    },
    uuidv4 () {
      return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
        (
          c ^
          (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
        ).toString(16)
      )
    },
    clickAddTag () {
      // если лицензия истекла
      if (Object.keys(this.$store.state.tasks.tags).length >= 3 && this.user.days_left <= 0) {
        this.showTagsLimit = true
        return
      }
      this.showAddTag = true
    },
    createTag (name) {
      this.showAddTag = false
      const title = name.trim()
      const tag = {
        uid_parent: '00000000-0000-0000-0000-000000000000',
        back_color: this.randomColors[Math.floor(Math.random() * this.randomColors.length - 1)],
        comment: '',
        collapsed: 0,
        children: [],
        order: 1,
        group: 0,
        show: 0,
        favorite: 0,
        uid: this.uuidv4(),
        name: title,
        email_creator: this.user.current_user_email,
        bold: 0
      }
      this.$store.dispatch(TASK.CREATE_TAG_REQUEST, tag)
        .then(() => {
          tag.global_property_uid = '00a5b3de-9474-404d-b3ba-83f488ac6d30'
          this.$store.commit(NAVIGATOR.NAVIGATOR_PUSH_TAG, [tag])
          this.openProperties(tag)
        })
    }
  }
}
</script>
