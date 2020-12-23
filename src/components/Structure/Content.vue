<template>
    <div class="max-w-6xl mx-auto">
        <InputSlider
            @slide="generateArray($event)"
            @sort="sort($event)"
            @generate="generateArray($event)"
        ></InputSlider>
        
        <div>
            
        </div>
        <div class="flex text-center">
            <template  v-for="(item, index) in data.array">
                <Bar
                    width="5"
                    :height="`${item.value}`"
                    :compare="compare"
                    :swap="swap"
                    :key="index"
                    :sorted="sorted"
                    :index="index"
                >
                </Bar>
            </template>
        </div>
    </div>
</template>
<script>
import Bar from '../Bar'
import InputSlider from '../Actions/InputSlider'
import { onMounted, reactive, ref } from '@vue/composition-api'
export default {
    components: {
        Bar,
        InputSlider
    },
    setup() {

        var interval = null

        const data = reactive({
            array: []
        })

        const array_copied = ref([])

        const compare = ref([])
            , swap = ref([])
            , sorted = ref([])

        const sorted_array = ref([])

        /**------------------------------
         * Functions || Sorting Algorithmns
         *------------------------------*/
        const generateArray = value => {
            clearInterval(interval)
            clear_cached_array()

            data.array = []

            var new_array
            var _LENGTH = value
            var _NUM = parseInt(value / 4)

            for( var i = 0; i < parseInt(_NUM) ; i ++ ) {
                new_array = Math.floor((Math.random() * _LENGTH) + 20);

                if (data.array.includes(new_array)) {
                    new_array = Math.floor((Math.random() * _LENGTH) + 20);
                }

                data.array[i] = {value: new_array, status: 1};
            }

            array_copied.value = [...data.array]
        }

        const algorithmns = {
            bubble_sort: () => {
                clearInterval(interval);
                var i = 0
                var step = 1
                interval = setInterval(() => {

                    compare.value = [i,(i) + 1]

                    if((i)+1 >= data.array.length) {

                        clear_cached_array()

                        compare.value = []

                        if(check_if_sorted()) {
                            sorted_animation()
                            clearInterval(interval)
                        }

                        clearInterval(interval)
                    }

                    if(check_if_sorted()) {
                        sorted_animation()
                    }
                        

                    if (data.array[i].value > data.array[(i)+1].value) {
                        var _temp = data.array[i];

                        data.array[i] = data.array[(i) +1];
                        data.array[(i) +1] = _temp;
                        compare.value = []
                        swap.value = [i,(i) + 1]
                    }

                    i++

                    if(i == data.array.length - step) {
                        sorted.value.push(i)
                        i = 0
                        step++
                    }
                    
                }, 50 - (50 % step))

                interval
            },
            selection_sort: () => {
                clearInterval(interval);

                var i = 0
                var step = 0
                var startIndex = 0
                var currentMinimum = data.array[startIndex].value
                var currentMinimumIdx = parseInt(`${startIndex}`)

                interval = setInterval(() => {

                    compare.value = [(step)+i]

                    if(check_if_sorted()) {
                        sorted_animation()
                    }

                    if(data.array[(startIndex)+i]?.value < currentMinimum) {
                        swap.value = []
                        currentMinimum = data.array[(startIndex)+i].value
                        currentMinimumIdx = ((startIndex)+i)
                        swap.value.push(currentMinimumIdx)
                    } 

                    i++

                    if(((i)+step) == data.array.length) {
                        var temp = JSON.parse(JSON.stringify(data.array[(startIndex)]))
                        data.array[(startIndex)] = JSON.parse(JSON.stringify(data.array[currentMinimumIdx]))
                        data.array[currentMinimum] = temp

                        console.log(data.array)

                        // console.log(temp)
                        // console.log(data.array[currentMinimumIdx])

                        clearInterval(interval)

                        i = 0
                        step++
                        swap.value = []
                        startIndex++
                    }
                    
                }, 50 - (50 % step))

                interval
            }
        }

        const sorted_animation = () => {
            clearInterval(interval);
            var i = 0
            interval = setInterval(() => {
                sorted.value.push(i)
                compare.value = []
                swap.value = []
                i++
                if(i == data.array.length - 1) {
                    i = 0
                }
                
            }, 100)

            interval
        }

        const clear_cached_array = () => {
            compare.value = []
            swap.value = []
            sorted.value = []
            sorted_array.value = []
        }

        const check_if_sorted = () => {
            if(JSON.stringify(data.array) == JSON.stringify(array_copied.value.sort(function(a, b){return a.value - b.value}))) {
                return true
            }

            return false
        }

        const sort = key => {
            (algorithmns[key]) ? algorithmns[key]() : false
        }

        onMounted(() => generateArray(parseInt(70)))

        return {generateArray,data, compare,sorted_array,swap, sorted, clear_cached_array,algorithmns, sort}
    }
}
</script>
<style scoped>
    
</style>