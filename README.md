# Vuetify Pro Tiptap

A WYSIWYG rich-text editor using [tiptap](https://github.com/scrumpy/tiptap) and [vuetify](https://github.com/vuetifyjs/vuetify) for Vue.js

[![download](http://img.shields.io/npm/dm/vuetify-pro-tiptap.svg)](https://npmcharts.com/compare/vuetify-pro-tiptap?minimal=true)
[![version](https://img.shields.io/npm/v/vuetify-pro-tiptap.svg)](https://www.npmjs.org/package/vuetify-pro-tiptap)
[![gzip](http://img.badgesize.io/https://unpkg.com/vuetify-pro-tiptap/lib/vuetify-pro-tiptap.mjs?compression=gzip&label=gzip)](http://img.badgesize.io/https://unpkg.com/vuetify-pro-tiptap/lib/vuetify-pro-tiptap.mjs?compression=gzip&label=gzip)
[![Test](https://github.com/yikoyu/vuetify-pro-tiptap/actions/workflows/test.yml/badge.svg?branch=master)](https://github.com/yikoyu/vuetify-pro-tiptap/actions/workflows/test.yml)
![LICENSE](https://img.shields.io/badge/License-MIT-yellow.svg)
[![semantic-release: vue](https://img.shields.io/badge/semantic--release-vue-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)

English | [中文](./README.zh-CN.md)

## Demo
👉[https://yikoyu.github.io/vuetify-pro-tiptap/](https://yikoyu.github.io/vuetify-pro-tiptap/)

## Features
- Use [vuetify](https://github.com/vuetifyjs/vuetify) components
- Markdown support
- TypeScript support
- I18n support(`en`, `zhHans`)
- Vuetify 2.x and Vue 2.x support

## Installation
### NPM
```shell
pnpm add vuetify-pro-tiptap
# or
yarn add vuetify-pro-tiptap
# or
npm i vuetify-pro-tiptap -S
```
To make `vuetify-pro-tiptap` work for Vue 2 (<2.7.0), you need to have `@vue/composition-api` installed:
```shell
pnpm add @vue/composition-api
```
### Install plugin

<details>
<summary>Vue 2.6.x</summary>

```TypeScript
import Vue from 'vue'
import VCA from '@vue/composition-api'

import Vuetify from 'vuetify/lib'

import { createVuetifyProTipTap } from 'vuetify-pro-tiptap'
import 'vuetify-pro-tiptap/style.css'

import App from './App.vue'

Vue.use(VCA)
Vue.use(Vuetify)

const vuetify = new Vuetify()

const VuetifyProTipTap = createVuetifyProTipTap({
  vuetify,
  lang: 'zhHans'
})
Vue.use(VuetifyProTipTap)

new Vue({
  vuetify,
  render: h => h(App)
}).$mount('#app')
```

</details>

<details>
<summary>Vue 2.7.x</summary>

```TypeScript
import Vue from 'vue'

import Vuetify from 'vuetify/lib'

import { createVuetifyProTipTap } from 'vuetify-pro-tiptap'
import 'vuetify-pro-tiptap/style.css'

import App from './App.vue'

Vue.use(Vuetify)

const vuetify = new Vuetify()

const VuetifyProTipTap = createVuetifyProTipTap({
  vuetify,
  lang: 'zhHans'
})
Vue.use(VuetifyProTipTap)

new Vue({
  vuetify,
  render: h => h(App)
}).$mount('#app')
```

</details>

## Global Settings
```TypeScript
import { VuetifyTiptap, VuetifyViewer, createVuetifyProTipTap } from 'vuetify-pro-tiptap'
import 'vuetify-pro-tiptap/style.css'

const VuetifyProTipTap = createVuetifyProTipTap({
  vuetify,
  lang: 'zhHans',
  components: {
    VuetifyTiptap,
    VuetifyViewer
  },
  // edsitor global Settings
  config: {
    image: {
      imageTabs: [{ name: 'SELECT', component: SelectImage }],
      hiddenTabs: ['upload'],
      upload(file: File) {
        return Promise.resolve(/** image url */)
      }
    }
  }
})

Vue.use(VuetifyProTipTap)
```

## I18n

### Setting language
You can declare when you install the plugin.
```TypeScript
import { createVuetifyProTipTap } from 'vuetify-pro-tiptap'

const VuetifyProTipTap = createVuetifyProTipTap({
  lang: 'zhHans'
})
```
Or use `setLang` dynamic change
```TypeScript
import { locale } from 'vuetify-pro-tiptap'

locale.setLang('en')
```
Available languages:
- en (default)
- zhHans

### Use unavailable language
Loading unavailable language, use `setMessage` for Settings
```TypeScript
import { locale } from 'vuetify-pro-tiptap'

locale.setMessage('zhHant', {
  // i18n text
})
locale.setLang('zhHant')
```

## Usage
```vue
<script lang="ts">
import { defineComponent, ref, type Ref } from '@vue/composition-api'
import { mdiDeleteCircleOutline, mdiSend, mdiFileCodeOutline } from '@mdi/js'
import { VuetifyTiptap, VuetifyViewer, type ToolbarType, type StarterKitOptions, locale } from 'vuetify-pro-tiptap'
import SelectImage from './components/SelectImage.vue'

export default defineComponent({
  components: {
    VuetifyTiptap,
    VuetifyViewer
  },
  setup() {
    const content = ref(`<h2 style="text-align: center">Welcome To Vuetify Tiptap Editor Demo</h2>`)

    const config: Ref<Partial<StarterKitOptions>> = ref<Partial<StarterKitOptions>>({
      image: {
        upload: uploadImage, // image upload function
        imageTabs: [{ name: 'SELECT', component: SelectImage }] // custom tab
        hiddenTabs: ['upload'] // use hidden tab
      }
    })

    // custom toolbar
    const toolbar = ref<ToolbarType[]>([
      'bold',
      'italic',
      'underline',
      'strike',
      'color',
      'highlight',
      '|',
      'h1',
      'h2',
      'h3',
      'h4',
      'h5',
      'h6',
      'p',
      '|',
      'left',
      'center',
      'right',
      'justify',
      '|',
      'bulletList',
      'orderedList',
      'taskList',
      'indent',
      'outdent',
      '|',
      'link',
      'image',
      'video',
      '|',
      'blockquote',
      'rule',
      'code',
      '|',
      'clear',
      'fullscreen',
      'undo',
      'redo',
      '#preview'
    ])

    function uploadImage(file: File): Promise<string> {
      return Promise.resolve('https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/RE4wE9P?ver=e767')
    }

    return {
      mdiDeleteCircleOutline,
      mdiFileCodeOutline,
      mdiSend,
      content,
      config,
      toolbar,
      dialog: ref(false)
    }
  }
})
</script>

<template>
  <div>
    <vuetify-tiptap
      v-model="content"
      label="Title"
      :config="config"
      :toolbar="toolbar"
      :dark="$vuetify.theme.dark"
      placeholder="Enter some text..."
      rounded
      :maxHeight="465"
    >
      <template #preview="{ attrs }">
        <v-dialog v-model="dialog" fullscreen hide-overlay>
          <template #activator="{ on, attrs: dialog }">
            <v-btn v-bind="{ ...attrs, ...dialog }" v-on="on">
              <v-icon>{{ mdiFileCodeOutline }}</v-icon>
            </v-btn>
          </template>

          <v-card>
            <v-toolbar dark color="primary">
              <v-btn icon dark @click="dialog = false">
                <v-icon>$close</v-icon>
              </v-btn>
            </v-toolbar>

            <v-container>
              <vuetify-viewer :value="content" />
            </v-container>
          </v-card>
        </v-dialog>
      </template>
    </vuetify-tiptap>
  </div>
</template>
```

## Props

### VuetifyTiptap

#### Props
| Name | Type | Default | Description |
| ---- | ---- | ---- | ---- |
| value | string | '' | The input’s value |
| dark | boolean | false | Applies the dark theme variant to the component. |
| dense | boolean | false | Reduces the input height |
| outlined | boolean | true | Applies the outlined style to the input |
| disabled | boolean | false | Disable the input |
| label | string | undefined | Sets input label |
| placeholder | string | undefined | Sets the input’s placeholder text |
| toolbar | ToolbarType[] | All component  | Toolbar button config |
| hideToolbar | boolean | false | Hidden the toolbar |
| disableToolbar | boolean | false | Disable the toolbar |
| maxWidth | string \| boolean | undefined | Sets the maximum width for the component. |
| minHeight | string \| boolean | undefined | Sets the minimum height for the component. |
| maxHeight | string \| boolean | undefined | Sets the maximum height for the component. |
| extensions | AnyExtension[] | [] | Tiptap the extensions |
| config | Partial\<StarterKitOptions> | {} | Editor config |
| editorClass | string \| string[] \| Record\<string, any> | undefined | Editor class |

#### Slots
| Name | Description |
| ---- | ---- |
| editor | Slot to customize editor |
| bottom | Slot to customize editor bottom |

### VuetifyViewer

#### Props
| Name | Type | Default | Description |
| ---- | ---- | ---- | ---- |
| value | value | '' | The preview’s value |
| dark | boolean | false | Applies the dark theme variant to the component. |
| dense | boolean | false | Reduces the input height |
| hideMarkdownStyle | boolean | false | Hide .markdown-body class |
| xss | boolean | true | Enable xss filter |
| xssOptions | xss.IWhiteList | Default rule | Xss filter rule config |

#### Slots
| Name | Description |
| ---- | ---- |
| before | Add content at the before |
| after | Add content at the after |