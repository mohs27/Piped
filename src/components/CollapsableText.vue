<template>
    <div v-if="text" class="mx-1 whitespace-pre-wrap py-2">
        <!-- eslint-disable-next-line vue/no-v-html -->
        <span v-if="showFullText" class="contentText" v-html="fullText()" />
        <!-- eslint-disable-next-line vue/no-v-html -->
        <span v-else v-html="collapsedText()" />
        <span v-if="text.length > visibleLimit && !showFullText">...</span>
        <button
            v-if="text.length > visibleLimit"
            class="block whitespace-normal text-neutral-500 font-semibold hover:underline"
            @click="showFullText = !showFullText"
        >
            [{{ showFullText ? $t("actions.show_less") : $t("actions.show_more") }}]
        </button>
    </div>
</template>

<script>
import { purifyHTML, rewriteDescription } from "@/utils/HtmlUtils";

export default {
    props: {
        text: {
            type: String,
            default: null,
        },
        visibleLimit: {
            type: Number,
            default: 100,
        },
    },
    data() {
        return {
            showFullText: false,
        };
    },
    methods: {
        fullText() {
            return purifyHTML(rewriteDescription(this.text));
        },
        collapsedText() {
            return purifyHTML(rewriteDescription(this.text.slice(0, this.visibleLimit)));
        },
    },
};
</script>

<style>
.contentText {
    word-wrap: anywhere;
}
</style>
