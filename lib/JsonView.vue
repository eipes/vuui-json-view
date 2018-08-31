<template>
    <div class="page-content">
        <section class="content-section">
            <div class="section-heading"><h1><em>DEMO</em> / json-view</h1></div>
            <div class="main text-left">
                <div class="left">
                    <div class="title margin-bottom-20">JSON String</div>
                    <textarea class="form-control" cols="30" rows="10" v-model="json"></textarea>
                </div>
                <div class="right">
                    <div class="title margin-bottom-20">JSON View</div>
                    <json-view :json="jsonObject"></json-view>
                </div>
            </div>
        </section>
    </div>
</template>

<style scoped>
    .main {
        display: flex;
    }

    .left {
        flex: 1;
        padding: 20px;
    }

    .left textarea {
        border: 1px solid #bfbfbf;
        background-color: transparent;
        border-radius: 0;
    }

    .right {
        flex: 1;
        padding: 20px;
    }
</style>

<script>
import JsonView from '@/components/JsonView.vue'

export default {
    computed: {
        jsonObject () {
            let ret = ''
            try {
                ret = JSON.parse(this.json)
            } catch (e) {
                ret = ''
            }
            return ret
        }
    },
    data () {
        return {
            json: ''
        }
    },
    components: {
        JsonView
    }
}
</script>
