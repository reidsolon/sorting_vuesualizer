<template>
    <div class="py-4">
        <input 
            class="mr-4"
            type="range" 
            @input="slide()" 
            v-model="value" 
            step="2" id="sort_size" min="70" max="700"
        />
        <input  
            class="mr-4"
            type="range" 
            @input="fspeed()" 
            v-model="speed"
            step="2" id="speed" min="50" max="140"
        />
            <button class="uppercase px-8 mr-2 py-2 border border-blue-600 text-blue-600 max-w-max shadow-sm hover:shadow-md" @click="generate()">Generate New Array</button>
        <template v-if="!sorting">
            <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md" @click="sort('bubble_sort')">Bubble Sort</button>
            <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md" @click="sort('selection_sort')">Selection Sort</button>
            <button class="uppercase px-8 py-2 rounded-full mr-2 bg-blue-600 text-blue-50 max-w-max shadow-sm hover:shadow-md" @click="sort('merge_sort')">Merge Sort</button>
        </template>
        <template v-else>
            <button class="uppercase px-8 mr-2 py-2 border border-red-600 text-red-600 max-w-max shadow-sm hover:shadow-md" @click="stopsort()">Stop Sort</button>
        </template>
    </div>
</template>
<script>
import {ref} from '@vue/composition-api'
export default {
    props: {
        sorting: Boolean,
    },
    setup() {

        const value = ref(0)
        const speed = ref(0)

        function slide(){
            this.$emit('slide', value.value)
        }

        function sort(id) {
            this.$emit('sort', id)
        }

        function fspeed() {
            this.$emit('speed', speed.value)
        }

        function generate() {
            this.$emit('generate', (value.value) ? value.value : 70)
        }

        function stopsort() {
            this.$emit('stopsort')
        }

        return {value,slide, sort,generate,speed, fspeed, stopsort}
    }
}
</script>
<style scoped>

</style>