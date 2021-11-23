<template>
  <div class="user-profile">
    <!-- 导航栏 -->
    <van-nav-bar
      left-arrow
      left-text="返回"
      right-text="哒咩"
      title="个人信息"
      @click-left="$router.back()"
      @click-right="onClickRight"
    />
    <!-- /导航栏 -->

    <input
      ref="file"
      hidden
      type="file"
      @change="onFileChange"
    >

    <!-- 个人信息 -->
    <van-cell
      center
      class="photo-cell"
      is-link
      title="头像"
      @click="$refs.file.click()"
    >
      <van-image
        :src="user.photo"
        class="avatar"
        fit="cover"
        round
      />
    </van-cell>
    <van-cell
      :value="user.name"
      is-link
      title="昵称"
      @click="isUpdateNameShow = true"
    />
    <van-cell
      :value="user.gender === 0 ? '男' : '女'"
      is-link
      title="性别"
      @click="isUpdateGenderShow = true"
    />
    <van-cell
      :value="user.birthday"
      is-link
      title="生日"
      @click="isUpdateBirthdayShow = true"
    />
    <!-- 个人信息 -->

    <!-- 编辑昵称 -->
    <van-popup
      v-model="isUpdateNameShow"
      position="bottom"
      style="height: 100%;"
    >
      <update-name
        v-if="isUpdateNameShow"
        v-model="user.name"
        @close="isUpdateNameShow = false"
      />
    </van-popup>
    <!-- /编辑昵称 -->

    <!-- 编辑性别 -->
    <van-popup
      v-model="isUpdateGenderShow"
      position="bottom"
    >
      <update-gender
        v-if="isUpdateGenderShow"
        v-model="user.gender"
        @close="isUpdateGenderShow = false"
      />
    </van-popup>
    <!-- /编辑性别 -->

    <!-- 编辑生日 -->
    <van-popup
      v-model="isUpdateBirthdayShow"
      position="bottom"
    >
      <update-birthday
        v-if="isUpdateBirthdayShow"
        v-model="user.birthday"
        @close="isUpdateBirthdayShow = false"
      />
    </van-popup>
    <!-- /编辑生日 -->

    <!-- 编辑头像 -->
    <van-popup
      v-model="isUpdatePhotoShow"
      position="bottom"
      style="height: 100%;"
    >
      <UpdatePhoto
        v-if="isUpdatePhotoShow"
        :img="img"
        @close="isUpdatePhotoShow = false"
        @updata-photo="user.photo=$event"
      />
    </van-popup>
    <!-- /编辑头像 -->
  </div>
</template>

<script>
import { Toast } from 'vant'
import { getUserProfile } from '@/api/user'
import UpdateName from './components/update-name'
import UpdateGender from './components/update-gender'
import UpdateBirthday from './components/update-birthday'
import UpdatePhoto from './components/update-photo'

export default {
  data () {
    return {
      user: {},
      isUpdateNameShow: false,
      isUpdateGenderShow: false,
      isUpdateBirthdayShow: false,
      isUpdatePhotoShow: false,
      img: null // 预览的图片
    }
  },
  components: {
    UpdateName,
    UpdateGender,
    UpdateBirthday,
    UpdatePhoto
  },
  created () {
    this.loadUserProfile()
  },
  methods: {
    onClickRight () {
      Toast('按钮')
    },
    async loadUserProfile () {
      try {
        const { data } = await getUserProfile()
        this.user = data.data
      } catch (e) {

      }
    },
    onFileChange () {
      // 获取文件对象
      const file = this.$refs.file.files[0]
      this.img = window.URL.createObjectURL(file)
      this.isUpdatePhotoShow = true

      // file-input选择了同一张图片，change事件不会触发
      // this.$refs.file.value = ''
    }
  }
}
</script>

<style lang="less" scoped>
.user-profile {
  .avatar {
    width: 60px;
    height: 60px;
  }

  .van-popup {
    background-color: #f5f7f9;
  }
}
</style>
