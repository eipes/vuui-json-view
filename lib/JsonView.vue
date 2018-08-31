<template>
    <div class="json-view">
        <!-- object -->
        <div v-if="isObject(filterData)" class="object-container">
            <i class="toggle-open" @click="toggleOpen">
                <i class="opened" v-if="opend">-</i>
                <i class="closed" v-if="!opend">+</i>
            </i>
            <span class="symbol">{</span>
            <div class="view-object" :key="index" v-for="(value, key, index) in filterData" v-if="opend">
                <!-- If value is array or object, begin with a new line -->
                <template v-if="isArray(value) || isObject(value)">
                    <div>
                        <span class="key">{{ key }}</span>
                        <span class="symbol colon">: </span>
                    </div>
                    <json-view :json="value" :parentLength="objectKeysLength(filterData)" :currentIndex="index"></json-view>
                </template>

                <!-- else -->
                <template v-else>
                    <span class="key">{{ key }}</span>
                    <span class="symbol colon">: </span>
                    <json-view :json="value" :parentLength="objectKeysLength(filterData)" :currentIndex="index"></json-view>
                </template>
            </div>
            <span class="symbol ellipsis" v-if="!opend" @click="toggleOpen">···</span>
            <span class="symbol">}</span>
            <span class="symbol" v-if="isLastItem">,</span>
            <span class="note" v-if="!opend">
                // {{ objectKeysLength(filterData) }}
                <span v-if="objectKeysLength(filterData) > 1">items</span>
                <span v-else>item</span>
            </span>
        </div>

        <!-- array -->
        <div v-else-if="isArray(filterData)"  class="array-container">
            <i class="toggle-open" @click="toggleOpen">
                <i class="opened" v-if="opend">-</i>
                <i class="closed" v-if="!opend">+</i>
            </i>
            <span class="symbol">[</span>
            <div class="view-array" :key="index" v-for="(value, index) in filterData" v-if="opend">
                <json-view :json="value" :parentLength="filterData.length" :currentIndex="index"></json-view>
            </div>
            <span v-if="!opend" @click="toggleOpen">···</span>
            <span class="symbol">]</span>
            <span class="symbol" v-if="isLastItem">,</span>
            <span class="note" v-if="!opend">
                // {{ filterData.length }}
                <span v-if="filterData.length > 1">items</span>
                <span v-else>item</span>
            </span>
        </div>

        <!-- string -->
        <span class="value" v-else-if="isString(filterData)">
            <span class="symbol">"</span>
            <span class="string">{{ filterData }}</span>
            <span class="symbol">"</span>
            <span class="symbol" v-if="isLastItem">,</span>
        </span>

        <!-- number -->
        <span class="value" v-else-if="isNumber(filterData)">
            <span class="number">{{ filterData }}</span>
            <span class="symbol" v-if="isLastItem">,</span>
        </span>

        <!-- null -->
        <span class="value" v-else-if="filterData === null">
            <span class="null">null</span>
            <span class="symbol" v-if="isLastItem">,</span>
        </span>
    </div>
</template>

<style scoped>
    .json-view {
        display: inline-block;
        position: relative;
        text-align: left;
    }

    .key {
        color: #787878;
        font-weight: bold;
    }

    .string {
        color: #e96900;
    }

    .number, .null {
        color: #42b983;
    }

    .symbol {
        color: #898989;
    }

    .symbol.colon {
        padding: 0 0.4rem;
    }

    .view-array, .view-object {
        padding-left: 1.6rem;
        border-left: 1px dotted #ccc;
    }

    .display-inline-block {
        display: inline-block;
    }

    .toggle-open {
        color: #969696;
        cursor: pointer;
        font-size: 2rem;
        font-weight: bold;
        display: inline-block;
        user-select: none;
    }

    .opened:hover, .closed:hover {
        color: #222222;
    }

    .note {
        margin-left: 1rem;
        color: #acacac;
    }

    .symbol.ellipsis {
        cursor: pointer;
    }
</style>

<script>
export default {
    name: 'json-view',
    props: ['json', 'parentLength', 'currentIndex'],
    data () {
        return {
            opend: true
        }
    },
    computed: {
        filterData () {
            return this.json
        },
        isLastItem () {
            let parentLength = this.parentLength
            let currentIndex = this.currentIndex
            let isFirst = false
            if (!parentLength) {
                parentLength = this.json.length || this.objectKeysLength(this.json)
                isFirst = true
            }
            if (!currentIndex) currentIndex = 0
            return !isFirst && currentIndex < parentLength - 1
        }
    },
    methods: {
        toggle (index) {
            this.showObj[index] = !this.showObj[index];
            this.showObj.splice(index, 1, this.showObj[index]);
        },
        getDataType (value) {
            return Object.prototype.toString.call(value).slice(8, -1).toLowerCase()
        },
        isObject (value) {
            return this.getDataType(value) === 'object'
        },
        isArray (value) {
            return this.getDataType(value) === 'array'
        },
        isString (value) {
            return this.getDataType(value) === 'string'
        },
        isNumber (value) {
            return this.getDataType(value) === 'number'
        },
        objectKeysLength (object) {
            return Object.keys(object).length
        },
        toggleOpen () {
            this.opend = !this.opend
        }
    }
}
</script>
