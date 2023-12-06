---
title: 卡片式选择器组件-antdv
date: 2023-12-06 18:59:44
categories: 笔记
tags: ["笔记"]
cover: date
banner:
    type: video
    bgurl: /img/video/cloud.mp4
    bannerText: 
reward: true
---
> antdv4.0 官方组件库没有一个卡片式选择器的组件，于是我尝试自己封装了一个基于a-card组件的卡片式选择器组件。使用的是阿里的[ant-design-vue4.0](https://www.antdv.com/components/overview-cn)版本组件。
<!-- more -->
## 1. 需求

一个基于antdv4.0组件库a-card组件设计的卡片式的选择器组件，可以用于选择一个卡片选项，并且可以自定义选项内容包括图标和文字。antdv 组件详细使用查看[antdv官方文档](https://www.antdv.com/components/overview-cn)。

## 2. 设计

### 2.1 组件说明
这是一个卡片式选择器组件，可以通过单击选中一个卡片，被选中卡片会添加阴影及高亮效果。

该组件的原生是a-card组件，通过antdv的a-row及a-col组件进行布局。

### 2.2 使用

### 详细代码实现
```HTML
<template>
    <div>
        <a-row :gutter="[16, 8]">
            <a-col :span="6" v-for="(card, index) in props.cards" :key="index" >
                <a-card 
                    class="defualt-center-layout"
                    :class="{ 
                        'shadow-[0_4px_20px_-5px_rgba(0,0,0,0.35)]': card === props.selectedCard,
                        'highlighted': card === props.selectedCard,
                    }"
                    @click="selectCard(card)"
                    hoverable
                >
                    <div class="card-icon defualt-center-layout">
                        <slot name="icon" :card="card"></slot>
                    </div>
                    <div class="defualt-center-layout">
                        <slot name="content" :card="card"></slot>
                    </div>
                </a-card>
                <div class="defualt-center-layout" :class="{'highlighted-font': card === props.selectedCard}">
                    <slot name="title" :card="card" ></slot>
                </div>
            </a-col>
        </a-row>
    </div>
</template>

<script setup lang="ts">
interface Props {
    cards: string[];
    selectedCard: string;
}

const props = defineProps<Props>();

const emit = defineEmits(['update:selectedCard']);

watch(
    () => props.selectedCard,
    (newVal) => {
        console.log(newVal);
    }
);

function selectCard(card: string) {
    emit('update:selectedCard', card);
}

</script>

<style lang="less" scoped>

.defualt-center-layout {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.card-icon {
    font-size: 30px;
}

:deep(.ant-card-body) {
    padding: 12px;
}

.highlighted {
    border: 1px solid #0a6efa;
}

.highlighted-font {
    color: #0a6efa;
}
</style>
```
