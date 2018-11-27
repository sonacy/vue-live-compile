<template>
  <codemirror class="editor" :value="code" @input="onChange" :options="cmOptions"></codemirror>
</template>

<script>
// language
  import 'codemirror/mode/vue/vue.js'
  // theme css
  import 'codemirror/theme/monokai.css'
  // active-line.js
  import 'codemirror/addon/selection/active-line.js'
  // styleSelectedText
  import 'codemirror/addon/selection/mark-selection.js'
  import 'codemirror/addon/search/searchcursor.js'
  // highlightSelectionMatches
  import 'codemirror/addon/scroll/annotatescrollbar.js'
  import 'codemirror/addon/search/matchesonscrollbar.js'
  import 'codemirror/addon/search/searchcursor.js'
  import 'codemirror/addon/search/match-highlighter.js'
  // keyMap
  import 'codemirror/mode/clike/clike.js'
  import 'codemirror/addon/edit/matchbrackets.js'
  import 'codemirror/addon/comment/comment.js'
  import 'codemirror/addon/dialog/dialog.js'
  import 'codemirror/addon/dialog/dialog.css'
  import 'codemirror/addon/search/searchcursor.js'
  import 'codemirror/addon/search/search.js'
  import 'codemirror/keymap/sublime.js'
  // foldGutter
  import 'codemirror/addon/fold/foldgutter.css'
  import 'codemirror/addon/fold/brace-fold.js'
  import 'codemirror/addon/fold/comment-fold.js'
  import 'codemirror/addon/fold/foldcode.js'
  import 'codemirror/addon/fold/foldgutter.js'
  import 'codemirror/addon/fold/indent-fold.js'
  import 'codemirror/addon/fold/markdown-fold.js'
  import 'codemirror/addon/fold/xml-fold.js'

export default {
  props: {
    code: {
      type: String,
      required: true
    },
    onChange: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      cmOptions: {
        tabSize: 2,
        styleActiveLine: true,
        lineNumbers: true,
        line: true,
        lineWrapping: true,
        foldGutter: true,
        styleSelectedText: true,
        mode: 'text/x-vue',
        keyMap: "sublime",
        matchBrackets: true,
        showCursorWhenSelecting: true,
        theme: "monokai"
      }
    }
  }
}
</script>

<style>
.editor {
  text-align: left;
  font-size: 16px;
  width: 100%;
  height: 100%;
}
.editor .CodeMirror {
  height: 100%;
}
</style>
