<template>
  <SfImage :src="imgUrl" :srcsets="srcsets" v-bind="{ ...$props, ...$attrs }" :placeholder="placeholder" />
</template>

<script>
import { SfImage } from "@storefront-ui/vue"
import getPlaceholderImage from "@/helpers/images/getPlaceholderImage.js"
import { computed } from "@vue/composition-api"

export default {
  name: "SwImage",
  components: {
    SfImage,
  },
  props: {
    srcset: Array,
    src: {
      type: String,
      default: ''
    }
  },
  setup(props, { root }) {
    const width = (props.imageWidth && props.imageWidth + "px") || "100%"
    const height = (props.imageHeight && props.imageHeight + "px") || "100%"
    const srcsets = computed(() => props.srcset?.map(item => ({
      src: item.url,
      width: item.width,
      resolution: item.width
      })) ?? []
    )
    const imgUrl = computed(() => {
      if (props.srcset && props.srcset.length) {
        return props.srcset.reduce(
          (res, thumb) => (thumb.width < res.width ? thumb : res),
          props.srcset[0]
        )?.url ?? props.src
      } else {
        return props.src
      }
    });

    return {
      placeholder: getPlaceholderImage(width, height),
      srcsets,
      imgUrl
    }
  },
}
</script>

<style lang="scss">
.sf-image--placeholder {
  min-width: 100%;
}
</style>
