
# Inertia js datatable for Laravel

[![npm version](https://img.shields.io/npm/v/your-package-name.svg)](https://www.npmjs.com/package/@velitsol/inertia-vue-datatable)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

This package  is written in vue 3 with the support of Inertia js for Laravel projects. 

## Features
- Global search
- Sorting column
- Pagination (support for Eloquent)

## Compatibility
   - Vue 3
   - Inertia.js
   - Tailwind CSS
   - Laravel 9

## Limitations
  - Not responsive for mobile display

## Installation

You can install the package via npm:

```bash
npm install @velitsol/inertia-vue-datatable
```

## Usage

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

export default {
  components: {
    InertiaDataTable,
  },
  // ...
}
```

## Props



| Prop        | Type   | Default | Description                |
| ----------- | ------ | ------- | ---------------------------|
| `records` | `object` | `null` | Paginated response from Laravel    |
| `columns` | `object` | `null` | Description of the prop 2    |
| `actions` | `array` | `['view','edit','delete]` | Description of the prop 2    |
| `_route` | `string` | `null` | Description of the prop 2    |



## Events

If your package emits events, document them here. Include the event names, the data they provide, and a description of when and why they are emitted.

| Event           | Data      | Description                     |
| --------------- | --------- | ------------------------------- |
| `eventName1`    | `data`    | Description of event 1          |
| `eventName2`    | `data`    | Description of event 2          |
| ...             | ...       | ...                             |

## Development

If you want to contribute to this project or modify the package, provide instructions on how to set up a development environment and how to run tests, if applicable.

```bash
# Clone the repository
git clone https://github.com/your-username/your-package-name.git

# Navigate to the project folder
cd your-package-name

# Install dependencies
npm install

# Run tests (if applicable)
npm run test

# Start the development server
npm run serve
```

## License

This project is licensed under the [MIT License](LICENSE.md). Make sure to mention the license type and provide a link to the full license file.

## Credits

If your package includes code or assets from other projects, provide proper attribution or credits to the original authors or sources.

## Related Projects

If you have any related projects, libraries, or resources that users might find useful, list them here with a brief description.

- [Related Project 1](https://github.com/user/repo) - Description of the project.
- [Related Project 2](https://github.com/user/repo) - Description of the project.
- ...

## Support

If users encounter any issues or have questions, provide instructions on how they can reach out for support. This can be through a GitHub issue tracker, email, or any other communication channel.

## Contributing

Explain how other developers can contribute to your project. Include guidelines for submitting bug reports, feature requests, and pull requests. You can also provide information on your code of conduct and any development practices you follow.

---

Feel free to modify this template based on your specific package's needs. Make sure to provide clear and concise instructions, examples, and documentation to help users effectively utilize your Vue.js package.
