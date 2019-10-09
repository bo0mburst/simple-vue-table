<template>
    <div class="simple-vue-table">
        <table>
            <thead>
                <tr>
                    <th 
                        class="checkbox-column"
                        v-if="hasCheckbox"
                    >
                        <input 
                            type="checkbox"
                            @input="selectAll($event)"
                        >
                    </th>
                    <th 
                        v-for="col in columns" 
                        :key="col.name"
                    >
                        <slot 
                            name="column" 
                            :col="col"
                        >
                            {{ col.text }}
                        </slot>
                    </th>                    
                </tr>
            </thead>
            <tbody v-if="!loading">
                <tr 
                    v-for="(item,index) in items" 
                    :key="index"
                    @click="$emit('click', item)"
                >
                    <td 
                        class="checkbox-column"
                        v-if="hasCheckbox"
                    >
                        <input 
                            type="checkbox"
                            @input="select($event, item)"
                            :checked="selected.map(i=>i[columns[0].name]).indexOf(item[columns[0].name]) > -1"
                        >
                    </td>

                    <td 
                        v-for="col in columns" 
                        :key="col.name"                 
                    >
                        <slot 
                            name="row-data" 
                            :data="{value: item[col.name], property: col.name}"
                        >
                            {{ item[col.name] }}
                        </slot>
                    </td>
                </tr>
            </tbody>

            <tbody class="table-loading" v-else>
                <tr>
                    <td :colspan="columns.length">
                        <slot name="loading">
                            Loading...
                        </slot>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'SimpleVueTable', // vue component name

    props: {
        items: {
            type: Array,
            default: () => [],
            required: true
        },

        columns: {
            type: Array,
            default: () => [],
            required: true
        },

        loading: {
            type: Boolean,
            default: false
        },

        hasCheckbox: {
            type: Boolean,
            default: false
        },

        selectedItems: {
            type: Array,
            default: () => []
        }
    },

    watch: {
        selectedItems (newVal) {
            this.selected = newVal
        }
    },

    data () {
        return {
            selected: []
        }
    },

    methods: {
        selectAll (e) {
            if (e.target.checked) {
                this.items.forEach(item => {
                    this.selected.push(item)
                });
            } else {
                this.selected = []
            }

            this.$emit('select', this.selected)
        },

        select (e, item) {
            if (e.target.checked) {
                this.selected.push(item)
            } else {
                this.selected.splice(this.selected.map(i => i[this.columns[0].name]).indexOf(item[this.columns[0].name]), 1)
            }

            this.$emit('select', this.selected)
        }
    }
}
</script>

<style scoped>
    .simple-vue-table {
        width: 100%;
        height: 100%;
    }

    table {
        width: 100%;
        height: 100%;
        border-collapse: collapse;
        font-family: sans-serif;
        box-shadow: 0 2px 4px #00000033;
        overflow: hidden;
        border-radius: 8px;
    }

    td, th {
        border-bottom: 1px solid #ddd;
        color: #555;
        padding: 10px;
        text-align: left;
        font-size: 1em;
    }

    td:not(:last-child), th:not(:last-child) {
        border-right: 1px solid #ddd;
    }

    th {
        font-weight: 600;
    }

    td {
        font-weight: 400;
    }

    thead > tr{
        background-color: #fff;
        border-bottom: 2px solid #ccc;
    }

    tbody > tr:nth-child(odd) {
        background-color: #f5f5f5;
    }

    tbody > tr:nth-child(even) {
        background-color: #fff;
    }

    tbody > tr:hover {
        background-color: #eee;
    }

    tbody.table-loading {
        height: 100%;
    }

    tbody.table-loading > tr > td {
        text-align: center;
    }

    .checkbox-column {
        text-align: center;
        width: 1em;
    }

    input[type=checkbox] {
        width: 1em;
        height: 1em;
        cursor: pointer;
    }
</style>