<script lang="ts" setup>
const props = defineProps({
    nodeValue: {
        type: Object,
        required: true,
    },
    language: {
        type: String,
        required: true,
    },
    breadcrumbs: {
        type: String,
        default: "",
    },
});
const isChildren = computed(() =>
    props.nodeValue.childs === undefined ? false : true
);
const isNodeOpen = ref<boolean>(false);
const computedLocale = computed(() => {
    if (!props.nodeValue.locale[props.language].cg_name) {
        for (let fallbackLanguage in props.nodeValue.locale) {
            if (props.nodeValue.locale[fallbackLanguage].cg_name) {
                return props.nodeValue.locale[fallbackLanguage];
            }
        }
    }
    return props.nodeValue.locale[props.language];
});
const computedBreadcrumbs = computed(
    () =>
        (props.breadcrumbs ? props.breadcrumbs + "->" : "") +
        computedLocale.value.cg_name
);
</script>

<template>
    <div>
        <div class="cursor-pointer relative m-1.5 border px-5 py-1 hover:bg-[#e0c8c8]"
            @click="isNodeOpen = !isNodeOpen">
            <span v-if="isChildren"
                class="inline-block w-2 h-2 border-r border-t border-solid border-black rotate-45 mr-3"
                :class="{ 'rotate-[135deg]': isNodeOpen }"></span>
            <NuxtLink class="text-lg hover:text-[#153aab]" :to="'/' + computedLocale.link + computedLocale.id"
                @click="(ev) => ev.stopPropagation()">{{ computedLocale.cg_name }}</NuxtLink>
            <div class="text-xs">{{ computedBreadcrumbs }}</div>
        </div>
        <div v-if="isChildren && isNodeOpen" class="relative pl-5">
            <node v-for="child in nodeValue.childs" :nodeValue="child" :language="language"
                :breadcrumbs="computedBreadcrumbs" />
        </div>
    </div>
</template>
