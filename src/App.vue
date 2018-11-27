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

const init = "%3Ctemplate%3E%0A%20%20%3Cdiv%3E%0A%20%20%20%20%3Cdiv%20class%3D%22name%22%3E%7B%7Bname%7D%7D's%20todo%20list%3C%2Fdiv%3E%0A%20%20%20%20%3Cdiv%3E%0A%20%20%20%20%20%20%3Cinput%20type%3D%22text%22%20v-model%3D%22todo%22%20%2F%3E%0A%20%20%20%20%20%20%3Cbutton%20%40click%3D%22add%22%3Eadd%20task%3C%2Fbutton%3E%0A%20%20%09%3C%2Fdiv%3E%0A%20%20%20%20%3Cul%3E%0A%09%09%09%3Cli%20v-for%3D%22(item%2Cindex)%20in%20list%22%20%3Akey%3D%22index%22%3E%0A%20%20%20%20%20%20%20%20%3Cspan%3E%7B%7Bitem%7D%7D%3C%2Fspan%3E%0A%20%20%20%20%20%20%20%20%3Cbutton%20%40click%3D%22remove(index)%22%20class%3D%22remove%22%3Ex%3C%2Fbutton%3E%0A%09%09%09%3C%2Fli%3E%0A%09%09%3C%2Ful%3E%0A%20%20%3C%2Fdiv%3E%0A%3C%2Ftemplate%3E%0A%3Cscript%3E%0Aexport%20default%20%7B%0A%20%20name%3A%20'demo'%2C%0A%20%20data()%20%7B%0A%20%20%20%20return%20%7B%0A%20%20%20%20%20%20name%3A%20'sonacy'%2C%0A%20%20%20%20%20%20todo%3A%20''%2C%0A%20%20%20%20%20%20list%3A%20%5B%5D%0A%20%20%20%20%7D%0A%20%20%7D%2C%0A%20%20methods%3A%20%7B%0A%20%20%09add()%20%7B%0A%20%20%20%20%09if%20(this.todo)%20%7B%0A%20%20%20%20%20%20%09this.list.push(this.todo)%0A%20%20%20%20%20%20%20%20this.todo%20%3D%20''%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%2C%0A%20%20%20%20remove(index)%20%7B%0A%20%20%20%20%09this.list.splice(index%2C%201)%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A%3C%2Fscript%3E%0A%3Cstyle%3E%0A.name%20%7B%0A%20%20text-align%3A%20center%3B%0A%20%20font-size%3A%2032px%3B%0A%20%20color%3A%20%23f00%3B%0A%20%20margin-bottom%3A%2020px%3B%0A%7D%0A.remove%20%7B%0A%09padding%3A%200%2010px%3B%0A%20%20text-align%3A%20center%3B%0A%20%20line-height%3A%2040px%3B%0A%20%20margin-left%3A%2020px%3B%0A%20%20cursor%3A%20pointer%3B%0A%7D%0A%3C%2Fstyle%3E%0A"

export default {
  name: 'app',
  components: {
    editor
  },
  data() {
    return {
      code: decodeURIComponent(init),
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
  },
  mounted() {
    if (this.code) {
      this.handleChangeCode(this.code)
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
