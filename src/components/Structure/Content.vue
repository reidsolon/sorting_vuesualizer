<template>
    <div class="max-w-6xl mx-auto">
        <div class="mt-5">
            Number of Arrays: <code>{{data.array.length}}</code>; Speed: <code>{{data.sorting_speed}}ms</code>; Status: 
            {{(data.status == 1) ? 'Idle' : ''}}
            {{(data.status == 2) ? 'Sorting' : ''}}
            {{(data.status == 3) ? 'Sorted' : ''}}
        </div>
        <InputSlider
            :sorting="sorting"
            @slide="generateArray($event)"
            @sort="sort($event)"
            @generate="generateArray($event)"
            @speed="speed($event)"
            @stopsort="stopsort()"
        ></InputSlider>
        
        <div>
            
        </div>
        <div class="flex text-center">
            <div class="" style="display: inline-flex; margin: auto;">

            <template  v-for="(item, index) in data.array">
                <Bar
                    width="3"
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

        const SORTING_SPEED = 150

        var interval = null

        const data = reactive({
            array: [],
            sorting_speed: 150,
            status: 1
        })

        const sorting = ref(false)

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

                if(check_if_sorted()) {
                    sorted_animation()
                }

                data.status = 2
                
                sorting.value = true 

                clearInterval(interval)
                var i = 0, step = 1
                interval = setInterval(() => {

                    compare.value = [i,(i) + 1]

                    if((i)+1 >= data.array.length) {

                        clear_cached_array()

                        if(check_if_sorted()) {
                            sorted_animation()
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

                    } else {
                        swap.value = []
                    }

                    i++

                    if(i == data.array.length - step) {
                        sorted.value.push(i)
                        i = 0
                        step++
                    }
                    
                }, data.sorting_speed)
                // 40 - (50 % step)
                interval
            },
            selection_sort: () => {

                data.status = 2
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

                    currentMinimum
                    currentMinimumIdx

                    // console.log(typeof data.array[(startIndex) + i])

                    // if(data.array[(startIndex)+i].value < currentMinimum) {
                    //     swap.value = []
                    //     // currentMinimum = data.array[(startIndex)+i].value
                    //     // currentMinimumIdx = ((startIndex)+i)
                    //     swap.value.push(currentMinimumIdx)
                    // } 
                    
                    i++

                    if((i) == data.array.length - step) {
                        // var temp = JSON.parse(JSON.stringify(data.array[(startIndex)]))
                        // data.array[(startIndex)] = JSON.parse(JSON.stringify(data.array[currentMinimumIdx]))
                        // data.array[currentMinimum] = temp

                        i = 0
                        step++
                        swap.value = []
                        startIndex++
                    } 

                    if(step == data.array.length - 1) {
                        //last step here.
                        sorted_animation()
                        clearInterval(interval)
                    }
                    
                }, data.sorting_speed)

                interval
            },
            merge_sort: async () => {

                if(check_if_sorted()) {
                    sorted_animation()
                } 

                data.status = 2
                sorting.value = true

                var array2 = data.array.map( (e,i) => {
                    e.index = i
                    data.array[i].index = i
                    return e
                })

                async function merge(left, right, ll,rl) {
                    return await new Promise((resolve) => {
                        var arr = []

                        interval = setInterval(async () => {

                            if(left.length && right.length) {
                                
                                var _l = left[0].index
                                var _r = right[0].index
                                
                                if (left[0].value < right[0].value) {
                                    let popped = left.shift()
                                    arr.push(popped)  
                                } else {
                                    let popped = right.shift()
                                    arr.push(popped)
                                }
                                // compare.value = [data.array[_l].index, _r];
                                swap.value = [data.array[_r].index, _l];
                                // swap.value = [data.array[_r].index, data.array[_l].index]; _l

                                var _t = [ ...arr, ...left, ...right ]
                        
                                for(var i = ll[0], t = 0 ; i < rl[1] + 1 ; i++, t++) {
                                    data.array[i] = _t[t]
                                }    

                                // compare.value = [data.array[_l].index, data.array[_r].index];

                            } else {

                                data.array.map( (e,i) => {
                                    e.index = i
                                    data.array[i].index = i
                                    return e
                                })

                                clearInterval(interval)
                                resolve([ ...arr, ...left, ...right ])

                            }
                        }, data.sorting_speed)
                    })
                }

                async function mergeSort(array) {

                    if(check_if_sorted()) {
                        sorted_animation()
                    }

                    const half = array.length / 2
                    
                    // Base case or terminating case
                    if(array.length < 2){
                        return array 
                    }
                    
                    const left = array.splice(0, half)

                    const al = [...[data.array[left[0].index].index, data.array[left[left.length-1].index].index]]
                        , bl = [...[data.array[array[0].index].index, data.array[array[array.length-1].index].index]]

                    const a = await mergeSort(left)
                        , b = await mergeSort(array)
                        , res = await merge(a,b, al, bl)

                    return res
                }

                data.array = await mergeSort (array2)

                if(check_if_sorted()) {
                    data.array.map((e,i) => data.array[i] = e)
                    sorted_animation()
                }
                
            }
        }

        const sorted_animation = () => {
            compare.value = []
            swap.value = []
            sorted.value = []
            clearInterval(interval);

            var i = 0, _t = []

            interval = setInterval(() => {
                _t.push(i)
                compare.value.push(i)
                swap.value = []
                i++
                if(i == data.array.length) {
                    i = 0
                    compare.value = []
                    data.status = 3
                    sorting.value = false
                    clearInterval(interval)
                } 
                
            }, 10)

            interval
        }

        const clear_cached_array = () => {
            compare.value = []
            swap.value = []
            sorted.value = []
            sorted_array.value = []
        }

        const stopsort = () => {
            clear_cached_array()
            clearInterval(interval)
            sorting.value = false
            data.status = 1
        }

        const check_if_sorted = () => {
            if(JSON.stringify(data.array) == JSON.stringify([...data.array].sort(function(a, b){return a.value - b.value}))) {
                return true
            }

            return false
        }

        const sort = key => {
            (algorithmns[key]) ? algorithmns[key]() : false
        }

        const speed = value => {
            data.sorting_speed = SORTING_SPEED - parseInt(value)
        }

        onMounted(() => generateArray(parseInt(70)))

        return {generateArray,data, compare,sorted_array,swap, sorted, clear_cached_array,algorithmns, sort, speed, sorting, stopsort}
    }
}
</script>
<style scoped>
    
</style>