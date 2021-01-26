<template>
  <div class="editable-text"><div class="icon editable-text-icon"></div><p>{{ thisText }}</p></div>
</template>

<script>
export default {
    name: 'EditableText',
    data() {
        return {
            thisText: null
        }
    },
    props: {
        rel: String
    },
    mounted() {
        //this.fetchText();
    },
    methods: {
        fetchText() {
            this.fireFetch("text", "rel == $rel", (docs) => {
                docs.forEach((doc) => {
                    console.log(doc.data());
                    this.thisText = doc.data().value;
                    console.log(this.thisText);
                });
            });
        }
    }
}
</script>

<style scoped lang="scss">
@import '../assets/styles/global'



</style>