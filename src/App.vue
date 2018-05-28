<script>
import config from './config'
import {get} from './utils'

export default {
  async created() {

    // const res = await get('/weapp/demo')
    // console.log(123, res);
    // wx.request({
    //   url: `${config.host}/weapp/demo`,
    //   success: function(res) {
    //     console.log(res)
    //   }
    // });
    console.log('小程序启动了123')
  }
}
</script>

<style>

</style>
