# simple-vue-table
A simple vue component that display tabular data.

![image](https://user-images.githubusercontent.com/56244118/66449146-981c0080-ea86-11e9-9bb0-0497e2e06bcc.png)

## Installation
```
npm install simple-vue-table
```

### Basic Usage
```html
<template>
    <div id="app">
        <simple-vue-table :items="items" :columns="columns"></simple-vue-table>
    </div>
</template>
```

```javascript
import SimpleVueTable from './simple-vue-table' 

export default {
  components: {
    SimpleVueTable
  },
  data () {
    return {
      items: [
        { id: 1, username: "Curran Wong", contact: "16690110 3116", email: "Mauris.eu@volutpat.net" },
        { id: 2, username: "Donovan Lambert", contact: "16670921 6862", email: "ante.dictum.cursus@natoque.ca" },
        { id: 3, username: "Austin Lindsay", contact: "16591004 4485", email: "non.bibendum.sed@dictummi.com" },
        { id: 4, username: "Jerome Velazquez", contact: "16060105 2525", email: "magna@Etiamimperdiet.com" },
        { id: 5, username: "Alden Hudson", contact: "16880710 2754", email: "torquent@enim.org" },
        { id: 6, username: "Gregory Britt", contact: "16260904 9933", email: "nostra.per@velconvallisin.net" },
        { id: 7, username: "Jarrod Mcconnell", contact: "16930717 4434", email: "metus@acarcu.org" },
        { id: 8, username: "Leonard Pitts", contact: "16690125 6773", email: "a.dui.Cras@utaliquam.com" },
        { id: 9, username: "Jamal Sanders", contact: "16070716 6989", email: "Aliquam.erat@odio.edu" },
        { id: 10, username: "Armand Barry", contact: "16170519 4759", email: "non.hendrerit@ipsum.edu" }
      ],

      columns: [
        { name: 'id', text: 'User ID' },
        { name: 'username', text: 'Name' },
        { name: 'contact', text: 'Contact No.' },
        { name: 'email', text: 'Email Address' }
      ]
    }
  }
}
```

### Slots and Props
Use this slots if you want to display links, images, or any custom display for your data.
- column
- row-data
- loading

Props:
- items (array of objects)
- columns (array of objects) with name and text property
- loading - boolean

```html
<template>
    <div id="app">
        <simple-vue-table :items="items" :columns="columns" :loading="false">
            <!-- column header  -->
             <template #column="{ col }">
                <span>{{ col.text }}</span>
            </template>
            <!-- rows -->
            <template #row-data="{ data }">
                <a
                  :href="data.value" 
                  v-if="data.property === 'email'"
                >
                  {{ data.value }}
                </a>

                <span v-else>
                  {{ data.value }}
                </span>
            </template>
            <template #loading>
                <span>Now loading...</span>
            </template>
        </simple-vue-table>
    </div>
</template>
```

### LICENSE
MIT
