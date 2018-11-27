<template>
  <div class="main">
    <div class="top">
      <editor :code='code' :onChange="this.handleChangeCode"></editor>
    </div>
    <div class="bottom">
      <component :is='test'></component>
    </div>
  </div>
</template>

<script>
import editor from './components/editor'
import {parseComponent, compileToFunctions} from 'vue-template-compiler'
import {transformSync} from '@babel/core'
import env from '@babel/preset-env'

export default {
  name: 'app',
  components: {
    editor
  },
  data() {
    return {
      code: '',
      test: ''
    }
  },
  methods: {
    handleChangeCode(code) {
      this.code = code
      try {
        const sfc = parseComponent(this.code)        

        const renderFns = compileToFunctions(sfc.template.content)
        
        const script = transformSync(sfc.script.content, {
          presets: [[env]]
        })
        window.exports = {}
        
        new Function(script.code)()
        const vms = window.exports.default
        
        const newCom = {...renderFns, ...vms}

        const styles = sfc.styles[0].content
        const styleEle = document.createElement('style')
        styleEle.innerText = styles
        document.head.appendChild(styleEle)
        this.test = newCom
      } catch (e) {
        // console.log(e)
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-size: 24px;
}
.main {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.top {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 50%;
}
.bottom {
  border-top: 1px solid #ccc;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 50%;
  background-color: #272822;
  color: #fff;
}
</style>
