<template lang="pug">
  .card(@mouseover="filtered" @mouseleave="unfiltered")
</template>

<script lang="js">
/** tslint:disbale */
/** バージョン関連の厄介なエラーにハマったのでjsに戻しています */
import * as PIXI from "pixi.js";
import { TweenMax } from 'gsap'

export default {
  name: "Card",

  data() {
    return {
      app: null,
      renderer: null,
      container: null,
      thumbnail: null,
      filter: null
    }
  },

  props: {
    'name': String,
    'path': String,
    'image': String
  },

  mounted() {
    this.app = new PIXI.Application({
      antialias: true,
      width: window.innerWidth,
      height: window.innerWidth * 0.6,
      transparent: true,
      resolution: window.devicePixelRatio || 1,
      autoResize: true
    });
    this.$el.appendChild(this.app.view)

    this.renderer = new PIXI.Renderer()
    this.container = new PIXI.Container()
    this.app.stage.addChild(this.container)

    const thumbnailTexture = PIXI.Texture.from("https://picsum.photos/id/1019/1920/1080")
    this.thumbnail = new PIXI.Sprite(thumbnailTexture)
    this.thumbnail.anchor.set(0.5)

    const filterTexture = PIXI.Texture.from("https://picsum.photos/id/301/1920/1080")
    this.filter = new PIXI.filters.DisplacementFilter(new PIXI.Sprite(filterTexture))
    this.filter.scale.x = 0
    this.filter.scale.y = 0
    this.thumbnail.filters = [this.filter]

    this.container.addChild(this.thumbnail)
  },
  methods: {
    filtered() {
      TweenMax.to(this.filter.scale, 0.6, {
        x: 131,
        y: 81
      })
    },
    unfiltered() {
      TweenMax.to(this.filter.scale, 0.6, {
        x: 0,
        y: 0
      })
    }
  }
}
</script>
