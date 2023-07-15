# Inertia js datatable for Laravel

<<<<<<< HEAD
[![npm version](https://img.shields.io/npm/v/your-package-name.svg)](https://www.npmjs.com/package/@velitsol/inertia-vue-datatable)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

This package is written in vue 3 with the support of Inertia js for Laravel projects.

## Features

=======
# Inertia js datatable for Laravel

[![npm version](https://img.shields.io/npm/v/your-package-name.svg)](https://www.npmjs.com/package/@velitsol/inertia-vue-datatable)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

This package  is written in vue 3 with the support of Inertia js for Laravel projects. 

## Features
>>>>>>> f9e0de2cdd34b7708731037216a443a83d1b440f
- Global search
- Sorting column
- Pagination (support for Eloquent)

## Compatibility
<<<<<<< HEAD

- Vue 3
- Inertia.js
- Tailwind CSS
- Laravel 9

## Limitations

- Not responsive for mobile display
=======
   - Vue 3
   - Inertia.js
   - Tailwind CSS
   - Laravel 9

## Limitations
  - Not responsive for mobile display
>>>>>>> f9e0de2cdd34b7708731037216a443a83d1b440f

## Installation

You can install the package via npm:

```bash
npm install @velitsol/inertia-vue-datatable
```

## Usage

<<<<<<< HEAD
#### Register in app.js

In your app.js file, you can register the component globally to make it available across your entire Vue.js application.

```javascript
import { InertiaDataTable } from "@velitsol/inertia-vue-datatable";

createApp(App).component("InertiaDataTable", InertiaDataTable).mount("#app");
```

By registering the component globally, you can use it in any Vue component template without the need for importing it specifically.

#### Register locally in a Vue component

If you prefer to register the component locally in a specific Vue component, you can do so as follows:

```javascript
import { InertiaDataTable } from "@velitsol/inertia-vue-datatable";
=======
#### Register in app.js 
In your app.js file, you can register the component globally to make it available across your entire Vue.js application.
```javascript
import { InertiaDataTable } from '@velitsol/inertia-vue-datatable';

createApp(App)
  .component('InertiaDataTable', InertiaDataTable)
  .mount('#app');
```
By registering the component globally, you can use it in any Vue component template without the need for importing it specifically.

#### Register locally in a Vue component 
If you prefer to register the component locally in a specific Vue component, you can do so as follows:
```javascript
import { InertiaDataTable } from '@velitsol/inertia-vue-datatable';
>>>>>>> f9e0de2cdd34b7708731037216a443a83d1b440f

export default {
  components: {
    InertiaDataTable,
  },
  // ...
};
```

## Props

<<<<<<< HEAD
| Prop      | Type     | Default                   | Description                        |
| --------- | -------- | ------------------------- | ---------------------------------- |
| `records` | `object` | `null`                    | Paginated response from Laravel    |
| `columns` | `object` | `null`                    | Heading cloumns of table           |
| `actions` | `array`  | `['view','edit','delete]` | Actions of table                   |
| `_route`  | `string` | `null`                    | Route prop is required for actions |

## Slots
=======


| Prop        | Type   | Default | Description                |
| ----------- | ------ | ------- | ---------------------------|
| `records` | `object` | `null` | Paginated response from Laravel    |
| `columns` | `object` | `null` | Description of the prop 2    |
| `actions` | `array` | `['view','edit','delete]` | Description of the prop 2    |
| `_route` | `string` | `null` | Description of the prop 2    |


>>>>>>> f9e0de2cdd34b7708731037216a443a83d1b440f

| Slot       | Return            | Description         |
| ---------- | ----------------- | ------------------- |
| heading    | `column object`   | Column heading      |
| row        | `record object`   | Table row           |
| column     | `record object`   | Table column        |
| action     | `record object`   | Table action column |
| pagination | `links object`    | Table pagination    |
| empty      | No Data Available | Table empty         |

## Examples

#### General

```javascript
<script>
import { reactive } from "vue";

export default {

  props: ["events"],
  setup(props) {

    /**
     *  key should database field name
     *  heading is column heading in datatable
     *  sortable represents a column should sortable or not
     *  format callback : it returns single
     *  _record callback : it returns row
     */
    const dataTableColumns = reactive([
     { key: "name", heading: "event", sortable: 1 },
      {
        key: "start_date",
        heading: "start_date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      },
      {
        key: "end_date",
        heading: "End Date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      {
        key: "published_app",
        heading: "Published App",
        sortable: 1,
      },
      {
        key: "published_web",
        heading: "Published Web",
        sortable: 0,
      },
    ]);

    return { dataTableColumns };
  },
};
</script>

<template>
     <inertia-data-table
          :records="events"
          :columns="dataTableColumns"
          :_route="'events'"
          :actions="['view', 'edit', 'delete']">
      </inertia-data-table>
</template>

```

#### Heading slot

```javascript
<script>
import { reactive } from "vue";

export default {

  props: ["events"],
  setup(props) {

    /**
     *  key should database field name
     *  heading is column heading in datatable
     *  sortable represents a column should sortable or not
     *  format callback : it returns single
     *  _record callback : it returns row
     */
    const dataTableColumns = reactive([
     { key: "name", heading: "event", sortable: 1 },
      {
        key: "start_date",
        heading: "start_date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      },
      {
        key: "end_date",
        heading: "End Date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      {
        key: "published_app",
        heading: "Published App",
        sortable: 1,
      },
      {
        key: "published_web",
        heading: "Published Web",
        sortable: 0,
      },
    ]);

    return { dataTableColumns };
  },
};
</script>

<template>
     <inertia-data-table
          :records="events"
          :columns="dataTableColumns"
          :_route="'events'"
          :actions="['view', 'edit', 'delete']">

             /**
             *  Syntax : #heading-column_key
             *  Return : It returns column object
             *  Description : You can add any icon with column heading.
             */

            <template #heading-name="{ column }"> Event Name </template>
            <template #heading-start_date="{ column }">  Event Start Date </template>

      </inertia-data-table>
</template>

```

#### Column slot

```javascript
<script>
import { reactive } from "vue";

export default {

  props: ["events"],
  setup(props) {

    /**
     *  key should database field name
     *  heading is column heading in datatable
     *  sortable represents a column should sortable or not
     *  format callback : it returns single
     *  _record callback : it returns row
     */
    const dataTableColumns = reactive([
     { key: "name", heading: "event", sortable: 1 },
      {
        key: "start_date",
        heading: "start_date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      },
      {
        key: "end_date",
        heading: "End Date",
        sortable: 1,
        format: function (v) {
          return moment(v).format("MM/DD/YYYY h:mm:ss A");
        },
      {
        key: "published_app",
        heading: "Published App",
        sortable: 1,
      },
      {
        key: "published_web",
        heading: "Published Web",
        sortable: 0,
      },
    ]);

    return { dataTableColumns };
  },
};
</script>

<template>
     <inertia-data-table
          :records="events"
          :columns="dataTableColumns"
          :_route="'events'"
          :actions="['view', 'edit', 'delete']">

             /**
             *  Syntax : #heading-column_key
             *  Return : It returns column object
             *  Description : You can add any icon with column heading.
             */

            <template #heading-name="{ column }"> Event Name </template>
            <template #heading-start_date="{ column }">  Event Start Date </template>

      </inertia-data-table>
</template>

```

## License

This project is licensed under the [MIT License](LICENSE.md). Make sure to mention the license type and provide a link to the full license file.

## Credits

If your package includes code or assets from other projects, provide proper attribution or credits to the original authors or sources.

## Support

If users encounter any issues or have questions, provide instructions on how they can reach out for support. This can be through a GitHub issue tracker, email, or any other communication channel.
<<<<<<< HEAD
=======

## Contributing

Explain how other developers can contribute to your project. Include guidelines for submitting bug reports, feature requests, and pull requests. You can also provide information on your code of conduct and any development practices you follow.

---

Feel free to modify this template based on your specific package's needs. Make sure to provide clear and concise instructions, examples, and documentation to help users effectively utilize your Vue.js package.
>>>>>>> f9e0de2cdd34b7708731037216a443a83d1b440f
