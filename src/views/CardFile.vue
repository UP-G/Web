<script>
import { FILE_REQUEST } from '@/store/actions/cardfilesandmessages'

export default {
  data () {
    return {
      intervalId: 0,
      text: ('Идет загрузка файла. Пожалуйста, подождите'),
      dots: ('.')
    }
  },

  mounted () {
  // Start dots blinking
    this.intervalId = setInterval(() => {
      this.dots.length < 3 ? this.dots += '.' : this.dots = '.'
    }, 500)
    const type = this.$router.currentRoute.value.query.type
    const format = this.$router.currentRoute.value.query.format

    this.$store.dispatch(FILE_REQUEST, this.$router.currentRoute.value.params.id).then((resp) => {
      const fileBlob = new Blob([resp.data], { type: type + '/' + format })
      const urlCreator = window.URL || window.webkitURL
      const fileURL = urlCreator.createObjectURL(fileBlob)
      this.text = 'Файл был успешно загружен'
      this.dots = '.'
      clearInterval(this.intervalId)
      window.location.href = fileURL
    }).catch((err) => {
      this.text = err
      this.dots = '.'
      clearInterval(this.intervalId)
    })
  }
}
</script>
<template>
  <p class="text-[40px] font-[700]">
    {{ text }} {{ dots }}
  </p>
</template>
