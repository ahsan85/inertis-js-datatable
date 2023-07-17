<script >
import { reactive, onMounted, computed, onUpdated, ref } from "vue";
import { Head, Link, usePage } from "@inertiajs/inertia-vue3";
import { Inertia } from "@inertiajs/inertia";

import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";
import Pagination from "@/Components/Pagination.vue";

export default {
  components: {
    Head,
    Link,
    Pagination,
  },

  props: ["records", "columns", "_route", "actions"],

  setup(props, { slots }) {
    const columns = props.columns;
    const _route = props._route;
    const actions = props.actions;

    const vueModalRef = ref();

    const data_table = reactive({
      sortField: "name",
      sortDir: "asc",
      searchString: "",
      page_number: "1",
      per_page: "",
      timeout: null,
    });

    onUpdated(() => {
      if (usePage().props.value.flash) {
        createToast(usePage().props.value.flash.message, {
          type: usePage().props.value.flash.type,
        });
        usePage().props.value.flash = null;
      }
    });
    // onMounted

    onMounted(() => {
      if (usePage().props.value.flash) {
        createToast(usePage().props.value.flash.message, {
          type: usePage().props.value.flash.type,
        });
        usePage().props.value.flash = null;
      }

      const params = new URLSearchParams(window.location.search);
      params.sort();

      if (params.has("sort_field")) {
        data_table.sortField = params.get("sort_field");
        data_table.sortDir = params.get("sort_direction").toLowerCase();
      }
      if (params.has("per_page")) {
        data_table.per_page = params.get("per_page");
      }
      if (params.has("page")) {
        data_table.page_number = params.get("page");
        //   params.get("page")>1?1:params.get("page");
      }
      if (params.has("search")) {
        data_table.searchString = params.get("search");
        // this.focusInput();
      }
    });
    // methods

    function deleteRecord(_record) {
      Inertia.delete(route(`${_route}.destroy`, _record));
    }

    function search() {
      clearTimeout(data_table.timeout);

      data_table.timeout = setTimeout(function () {
        if (data_table.searchString.trim()) {
          searchData(data_table.searchString.trim());
        } else {
          searchData(null);
        }
      }, 1000);
    }
    function searchData(search_string) {
      Inertia.visit(_route, {
        method: "get",
        data: {
          search: search_string,
        },
      });
    }

    function onColChange(event) {
      if (event.target.value && data_table.sortDir) {
        sortData(
          data_table.page_number,
          data_table.per_page,
          event.target.value,
          data_table.sortDir
        );
      }
    }
    function sortData(page, p_page, sortField, sort_direction) {
      Inertia.visit(_route, {
        method: "get",
        data: {
          page: page,
          per_page: p_page,
          sort_field: sortField,
          sort_direction: sort_direction,
        },
      });
    }

    function onSortDirChange(event) {
      if (event.target.value && data_table.sortField) {
        sortData(
          data_table.page_number,
          data_table.per_page,
          data_table.sortField,
          event.target.value
        );
      }
    }

    function onPerPageChange(value) {
      data_table.per_page = value;
      sortData(
        data_table.page_number,
        data_table.per_page,
        data_table.sortField,
        data_table.sortDir
      );
    }

    function formatValue(value, format) {
      if (typeof format == "string" && format.split("|").length > 1) {
        let [type, pattern] = format.split("|");
        if (type == "date") {
          return window.formatDate(value, pattern);
        }
      }

      if (typeof format == "function") {
        return format(value);
      }
    }

    function openModel(r, f) {
      vueModalRef.value.showDialog(r, f);
    }

    function confirmDelete(e) {
      deleteRecord(_route, e);
    }

    function is_html(string) {
      console.log(string);
      // return string.match("/<[^<]+>/");
    }
    // computed properties

    const sortableCols = computed(() => {
      return columns.filter(function (col) {
        if (col.sortable === 1) {
          return col;
        }
      });
    });

    const colKey = computed(() => {
      return function (column) {
        var key = "";
        if (typeof column === "string" || column instanceof String) {
          key = column;
        } else if (typeof column === "object" || column instanceof Object) {
          key = column.key;
        }
        return key;
      };
    });

    const colHeading = computed(() => {
      return function (column) {
        var title = "";
        if (typeof column === "string" || column instanceof String) {
          title = column;
        } else if (typeof column === "object" || column instanceof Object) {
          title = column.heading;
        }
        return title.replace(/\./g, " ").toUpperCase();
      };
    });

    const checkActions = computed(() => {
      return function (key) {
        var _action = actions.find(function (action) {
          return action == key;
        });
        return _action == key;
      };
    });

    const getVal = computed(() => {
      return function (record, column) {
        let value = "";
        let key = column;
        let format = null;
        let default_value = null;
        let _record = null;

        if (typeof column == "object") {
          key = column.key;
          default_value = column.default_value;
          format = column.format;
          _record = column._record;
        }

        var keyparts = key.split(".");
        if (keyparts.length > 2) {
          value =
            record[keyparts[0]] != null && record[keyparts[1]] != null
              ? record[keyparts[0]][keyparts[1]][keyparts[2]]
              : null;
        } else if (keyparts.length > 1) {
          if (
            record[keyparts[0]] != null &&
            Array.isArray(record[keyparts[0]])
          ) {
            value = record[keyparts[0]].forEach((element) => {
              return element;
              // console.log(element);
              return record[keyparts[0]];
              // console.log(record[keyparts[0]][index][keyparts[1]]);
            });
          } else {
            value =
              record[keyparts[0]] != null
                ? record[keyparts[0]][keyparts[1]]
                : null;
          }
        } else {
          value = record[key];
        }

        if (format && typeof format !== "undefined") {
          value = formatValue(value, format);
        }

        if (
          _record &&
          typeof _record !== "undefined" &&
          typeof _record == "function"
        ) {
          return _record(record);
        }

        return value;
      };
    });

    const hasSlot = (name) => !!slots[name];

    return {
      openModel,
      is_html,
      confirmDelete,
      hasSlot,
      vueModalRef,
      checkActions,
      actions,
      data_table,
      sortableCols,
      colKey,
      colHeading,
      getVal,
      deleteRecord,
      search,
      onColChange,
      sortData,
      onSortDirChange,
      searchData,
      onPerPageChange,
    };
  },
};
</script>


<template>
  <div class="mx-auto bg-white p-5">
    <div v-if="records.data.length > 0">
      <div class="flex flex-col justify-end my-4 sm:flex-row">
        <div class="flex flex-row mb-1 sm:mb-0">
          <div class="relative mx-2">
            <select
              v-model="data_table.sortField"
              @change="onColChange($event)"
              class="block w-full h-full px-4 py-2 pr-8 leading-tight text-gray-700 bg-white border-solid border-gray-400 rounded appearance-none focus:outline-none focus:bg-white focus:border-gray-500"
            >
              <option value="" disabled selected>Sort Field</option>
              <option
                v-for="column in sortableCols"
                :key="column"
                :value="
                  typeof column.sort_key !== 'undefined'
                    ? column.sort_key
                    : colKey(column)
                "
              >
                {{ colHeading(column) }}
              </option>
            </select>
          </div>

          <div class="relative mx-2">
            <select
              v-model="data_table.sortDir"
              @change="onSortDirChange($event)"
              class="block w-full h-full px-4 py-2 pr-8 leading-tight text-gray-700 bg-white border-gray-400 rounded appearance-none focus:outline-none focus:border-l focus:border-r focus:bg-white focus:border-gray-500"
            >
              <option value="" disabled selected>Sort Direction</option>
              <option value="asc">ASC</option>
              <option value="desc">DESC</option>
            </select>
          </div>
        </div>
        <div class="relative block ml-2">
          <input
            @keyup="search()"
            v-model="data_table.searchString"
            required
            placeholder="Search"
            class="block w-full py-2 pl-2 pr-6 text-sm text-gray-700 placeholder-gray-400 bg-white border border-b border-gray-400 rounded rounded-l appearance-none focus:bg-white focus:placeholder-gray-600 focus:text-gray-700 focus:outline-none"
          />
          <span
            class="absolute inset-y-0 flex items-center h-full pl-2 right-2"
          >
            <svg viewBox="0 0 24 24" class="w-4 h-4 text-gray-500 fill-current">
              <path
                d="M10 4a6 6 0 100 12 6 6 0 000-12zm-8 6a8 8 0 1114.32 4.906l5.387 5.387a1 1 0 01-1.414 1.414l-5.387-5.387A8 8 0 012 10z"
              ></path>
            </svg>
          </span>
        </div>
      </div>

      <table class="w-full border-collapse table-auto">
        <thead>
          <tr>
            <th
              v-for="column in columns"
              :key="column"
              :value="column"
              class="px-1 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50"
              style="background-color: #f8f8f8"
            >
              <div class="inline-flex">
                <slot :name="`heading-${column.key}`" :column="column">{{
                  colHeading(column)
                }}</slot>

                <span
                  v-if="data_table.sortField == colKey(column)"
                  class="mx-1"
                >
                  <span v-if="data_table.sortDir == 'asc'">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke-width="1.5"
                      stroke="currentColor"
                      class="w-3 h-3"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="M19.5 13.5L12 21m0 0l-7.5-7.5M12 21V3"
                      />
                    </svg>
                  </span>
                  <span v-if="data_table.sortDir == 'desc'">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke-width="1.5"
                      stroke="currentColor"
                      class="w-3 h-3"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="M4.5 10.5L12 3m0 0l7.5 7.5M12 3v18"
                      />
                    </svg>
                  </span>
                </span>
              </div>
            </th>
            <th
              class="px-1 w-24 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50"
            >
              Actions
            </th>
          </tr>
        </thead>
        <tbody class="text-sm font-normal text-gray-700">
          <tr v-for="record in records.data" :key="record.id" class="">
            <slot name="row" :record="record">
              <td
                v-for="column in columns"
                :key="column"
                class="border-b border-gray-200 whitespace-nowrap"
              >
                <slot :name="`column-${column.key}`" :record="record">
                  <span class="px-1 py-4">{{ getVal(record, column) }}</span>
                </slot>
              </td>
              <td class="px-2 py-4 border-b border-gray-200 whitespace-nowrap">
                <div class="flex justify-center item-center">
                  <slot
                    v-for="action in actions.filter(
                      (s) => ['view', 'edit', 'delete'].includes(s) == false
                    )"
                    :name="'action-' + action"
                    :record="record"
                  >
                  </slot>
                  <Link
                    v-if="checkActions('view') && !hasSlot('action-view')"
                    :href="route(`${_route}.show`, record.id)"
                    class="mr-2 w-4 transform hover:text-purple-500 hover:scale-110"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                      />
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                      />
                    </svg>
                  </Link>
                  <slot
                    v-else-if="hasSlot('action-view')"
                    name="action-view"
                    :record="record"
                  >
                  </slot>
                  <Link
                    v-if="checkActions('edit') && !hasSlot('action-edit')"
                    :href="route(`${_route}.edit`, record.id)"
                    method="get"
                    class="w-4 mr-2 text-purple-600 transform hover:text-purple-600 hover:scale-110"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"
                      />
                    </svg>
                  </Link>
                  <slot
                    v-else-if="hasSlot('action-edit')"
                    name="action-edit"
                    :record="record"
                  >
                  </slot>
                  <button
                    v-if="checkActions('delete') && !hasSlot('action-delete')"
                    @click="deleteRecord(record.id)"
                    class="w-4 mr-2 text-red-500 transform cursor-pointer hover:text-red-600 hover:scale-110"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                      />
                    </svg>
                  </button>
                  <slot
                    v-else-if="hasSlot('action-delete')"
                    name="action-delete"
                    :record="record"
                  >
                  </slot>
                </div>
              </td>
            </slot>
          </tr>
        </tbody>
      </table>
      <div>
        <slot name="pagination" :links="records.links">
          <pagination
            class="mt-6"
            :links="records.links"
            @recordsPerPage="onPerPageChange"
          />
        </slot>
      </div>
    </div>
    <div v-else>
      <slot name="empty">
        <h2 class="text-xl font-bold text-center py-4 text-gray-700">
          No Data Available
        </h2>
      </slot>
    </div>
  </div>
</template>

<style scoped>
/* we will explain what these classes do next! */
.v-enter-active,
.v-leave-active {
  transition: opacity 1s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
