<template>
    <div class="py-4">
        <input 
            class="mr-4"
            type="range" 
            @input="slide()" 
            v-model="value"
            step="1" id="sort_size" min="70" max="500"
        />
        <button class="uppercase px-8 mr-2 py-2 border border-blue-600 text-blue-600 max-w-max shadow-sm hover:shadow-md" @click="generate()">Generate New Array</button>
        <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md" @click="sort('bubble_sort')">Bubble Sort</button>
        <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md" @click="sort('selection_sort')">Selection Sort</button>
        <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md">Merge Sort</button>
    </div>
</template>
<script>
import {ref} from '@vue/composition-api'
export default {
    
    setup() {

        const value = ref(0)

        function slide(){
            this.$emit('slide', value.value)
        }

        function sort(id) {
            this.$emit('sort', id)
        }

        function generate() {
            this.$emit('generate', (value.value) ? value.value : 70)
        }

        return {value,slide, sort,generate}
    }
}
</script>
<style scoped>

</style>