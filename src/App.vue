<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { Application, Container, Graphics, Text, TextStyle } from 'pixi.js'

const mountEl = ref<HTMLDivElement | null>(null)
let app: Application | null = null

async function initPixi(container: HTMLDivElement) {
  const pixiApp = new Application()
  await pixiApp.init({
    resizeTo: container,
    antialias: true,
    autoDensity: true,
    background: '#12151d',
    resolution: window.devicePixelRatio || 1,
  })

  container.appendChild(pixiApp.canvas)

  const stage = new Container()
  pixiApp.stage.addChild(stage)

  const grid = new Graphics()
  grid.rect(0, 0, 4000, 4000)
  grid.fill(0x12151d)
  stage.addChild(grid)

  const accent = new Graphics()
  accent.roundRect(0, 0, 320, 180, 24)
  accent.fill({ color: 0x1d2533, alpha: 0.95 })
  accent.stroke({ color: 0x3f4d66, width: 2 })
  accent.position.set(40, 40)
  stage.addChild(accent)

  const title = new Text({
    text: 'Vue + TS + PixiJS',
    style: new TextStyle({
      fill: 0xffffff,
      fontFamily: 'system-ui, sans-serif',
      fontSize: 28,
      fontWeight: '700',
    }),
  })
  title.position.set(64, 64)
  stage.addChild(title)

  const subtitle = new Text({
    text: 'Blank starter • feature-based structure',
    style: new TextStyle({
      fill: 0xb3bccf,
      fontFamily: 'system-ui, sans-serif',
      fontSize: 16,
    }),
  })
  subtitle.position.set(64, 108)
  stage.addChild(subtitle)

  app = pixiApp
}

onMounted(() => {
  if (mountEl.value) {
    void initPixi(mountEl.value)
  }
})

onBeforeUnmount(() => {
  app?.destroy(true)
  app = null
})
</script>

<template>
  <main class="app-shell">
    <header class="topbar">
      <p class="eyebrow">Game Base</p>
      <h1>Vue + TypeScript + PixiJS</h1>
      <p class="lead">A clean blank starter for feature-first mini games.</p>
    </header>

    <section class="stage-shell">
      <div ref="mountEl" class="pixi-mount" aria-label="Pixi canvas mount" />
    </section>

    <footer class="footnote">Structure: <code>src/features/</code> + <code>src/game/</code> + <code>src/shared/</code></footer>
  </main>
</template>
