<template>
<div>
    <div>
        <div v-for="type in params" :key="`key-${type}`" style="margin: 10px">
            <div>
                <label for="">{{type}}</label>
                <br />
                <input type="text" @change="setValue($event, type)">
            </div>
        </div>
    </div>
    <template v-if="hits.length">
        <div v-for="hit in hits" :key="hit.id">
            <img :src="hit.webformatURL" alt="">
        </div>
    </template>
    <div v-else>Please, select param and wait...</div>

</div>
</template>

<script>
import axios from 'axios'
export default {

    data() {
        return {
            params: [
                'id',
                'category',
                'colors',
                'image_type'
            ],

            key: '17773979-eea9afe5558f8f143664122b6',

            selected: {},

            hits: []
        }
    },
    methods: {
        show() {
            let url = 'https://pixabay.com/api/?key=17773979-eea9afe5558f8f143664122b6'
            let keys = Object.keys(this.selected)
            keys.forEach(key => {
                if (this.selected[key] != null) {
                    url = url + `&${key}=${this.selected[key]}`
                }
            })
            axios.get(url).then(res => this.hits = res.data.hits).catch(e => console.log(e))
        },
        setValue(e, type) {
            this.selected[type] = e.target.value
            this.show()
        }
    },

    created() {
        this.params.forEach(element => {
            this.$set(this.selected, element, null)
        });

    }

}
</script>

<style scoped>
select {
    width: 200px;
    height: 20px;
}
</style>
