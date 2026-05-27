<template>
    <h3>条件渲染</h3>
    <div v-if="flag">flag is true</div>
    <div v-else>flag is false</div>
    <div v-if="type === 'A'">type is A</div>
    <div v-else-if="type === 'B'">type is B</div>
    <div v-else-if="type === 'C'">type is C</div>
    <div v-else>type is other</div>
    <div v-show="flag">flag is {{ flag }}</div>
</template>

<script>
export default {
    data() {
        return {
            // flag: true,
            flag: true,
            type: 'D'
        }
    }
}
</script>