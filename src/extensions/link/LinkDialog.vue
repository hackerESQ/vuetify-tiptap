<template>
  <v-dialog v-model="dialog" :dark="dark" max-width="400" @click:outside="close">
    <v-card>
      <v-card-title>
        <span class="headline">{{ t('editor.link.dialog.title') }}</span>

        <v-spacer />

        <v-btn icon @click="close">
          <v-icon>{{ mdiClose }}</v-icon>
        </v-btn>
      </v-card-title>

      <v-card-text>
        <v-text-field :value="value" @input="url = $event" :label="t('editor.link.dialog.link')" hide-details autofocus />
      </v-card-text>

      <v-card-actions>
        <v-btn :disabled="isDisabled" text @click="apply">
          {{ t('editor.link.dialog.button.apply') }}
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts">
import { defineComponent, ref, unref, computed, watchEffect } from 'vue-demi'
import { string, bool, object } from 'vue-types'
import type { Editor } from '@tiptap/vue-2'
import { useLocale } from '@/locales'
import { mdiClose } from '@/constants/icons'
import useContext from '@/hooks/use-context'

export default defineComponent({
  props: {
    value: string(),
    editor: object<Editor>().isRequired,
    show: bool().def(false),
    dark: bool().def(false)
  },
  setup(props) {
    const root = useContext()

    const { t } = useLocale()
    const url = ref<string>('')

    const dialog = ref<boolean>(false)

    watchEffect(() => (dialog.value = props.show))

    const isDisabled = computed(() => {
      if (props.value === url.value) return true
      if (!unref(url)) return true
      return false
    })

    function apply() {
      if (unref(url)) {
        props.editor
          .chain()
          .focus()
          .extendMarkRange('link')
          .setLink({ href: unref(url) })
          .run()
      }
      close()
    }

    function close() {
      dialog.value = false

      if (!root) return

      setTimeout(() => {
        root.$destroy()
        root.$el.parentNode?.removeChild(root.$el)
      }, 300)
    }

    return {
      mdiClose,
      url,
      isDisabled,
      t,
      dialog,
      apply,
      close
    }
  }
})
</script>
