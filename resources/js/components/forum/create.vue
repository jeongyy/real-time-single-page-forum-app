<template>
    <v-container>
        <v-form @submit.prevent="create">

            <v-text-field
                label="Title"
                v-model="form.title"
                type="text"
                required
            ></v-text-field>

            <!-- deprecated
            <v-select
            :items="categories"
            v-model="form.category_id"
            item-text="name"
            label="Category"
            autocomplete
            >
            </v-select>
            -->

            <v-autocomplete
            :items="categories"
            v-model="form.category_id"
            item-text="name"
            item-value="id"
            label="Category"
            ></v-autocomplete>

            <markdown-editor v-model="form.body"></markdown-editor>

            <!--
            :search-input.sync="search"
            :loading="loading"
            cache-items
            class="mx-3"
            flat
            hide-no-data
            hide-details
            solo-inverted
             -->

            <v-btn color="green" type="submit">
                Create
            </v-btn>

        </v-form>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            form: {
                title:null,
                category_id:null,
                body:null

            },
            categories:{},
            errors:{}
        }
    },
    created() {
        axios.get('/api/category')
        .then(res => this.categories = res.data.data)
    },
    methods: {
        create() {
            axios.post('/api/question', this.form)
            .then(res => this.$router.push(res.data.path))
            .catch(error => this.errors = error.response.data.error)
        }
    }
}
</script>

<style>
    @import '~simplemde/dist/simplemde.min.css';
</style>
