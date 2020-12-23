<template>
    <div
        :class="`
            ${([...compare].includes(index)) ? `bg-green-300 ` : ` `}
            ${([...swap].includes(index)) ? `bg-red-500 ` : ` `}
            ${([...sorted].includes(index)) ? `bg-purple-500 ` : ` `}
            ${(![...swap].includes(index) && ![...compare].includes(index) && ![...sorted].includes(index)) ? `bg-blue-300 ` : ` `}
        `"
        :style="`height: ${parseInt(height)}px; width: ${parseInt(width)}px; margin-right: 2px;transition: 0.1s;`"
    >
    .
    </div>
</template>
<script>
export default {
    props: {
        index: Number,
        width: String,
        height: String,
        compare: Array,
        swap: Array,
        sorted: Array
    }, 
    setup() {

    }
}
</script>
<style scoped>

</style>