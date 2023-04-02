<template>
<div class="bg-white">
    <div class="mx-auto max-w-7xl px-4 py-8 sm:px-6 sm:py-8 lg:px-8 bg-gray-100">
        <div class="mx-auto max-w-2xl bg-gray-200 p-14">
            <form @submit.prevent="submitForm">

                <div class="space-y-12">
                    <div class="border-b border-gray-900/10 pb-12">
                        <h2 class="text-base font-semibold leading-7 text-gray-900">Profile</h2>
                    </div>

                    <div class="border-b border-gray-900/10 pb-12">
                        <h2 class="text-base font-semibold leading-7 text-gray-900">Personal Information</h2>
                        <p class="mt-1 text-sm leading-6 text-gray-600">Use a permanent address where you can receive mail.</p>

                        <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-6">
                            <div class="sm:col-span-3">
                                <label for="first_name" class="block text-sm font-medium leading-6 text-gray-900">First name</label>
                                <div class="mt-2">
                                    <input
                                        v-model="data.first_name"
                                        type="text"
                                        name="first_name"
                                        id="first_name"
                                        autocomplete="given-name"
                                        class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    />
                                    <ErrorMessage name="first_name" />
                                    <!-- <p>{{ error.first_name }}</p> -->
                                </div>
                            </div>

                            <div class="sm:col-span-3">
                                <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">Last name</label>
                                <div class="mt-2">
                                    <input
                                        v-model="data.last_name"
                                        type="text"
                                        name="last_name"
                                        id="last_name"
                                        autocomplete="family-name"
                                        class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    />
                                </div>
                            </div>

                            <div class="sm:col-span-4">
                                <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email address</label>
                                <div class="mt-2">
                                    <input
                                        v-model="data.email"
                                        id="email"
                                        name="email"
                                        type="email"
                                        autocomplete="email"
                                        class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    />
                                </div>
                            </div>

                            <div class="sm:col-span-3">
                                <label for="selectVendorName" class="block text-sm font-medium leading-6 text-gray-900">VendorName : </label>
                                <div class="mt-2">
                                    <select
                                        v-model="data.selectVendorName"
                                        id="selectVendorName"
                                        name="selectVendorName"
                                        autocomplete="optionVendorName"
                                        class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6"
                                    >
                                        <option value="">Select a City</option>
                                        <option
                                        v-for="(name, index) in vendorName"
                                        :value="name.vendorName"
                                        :key="index"
                                      >{{  name.vendorName }}</option>
                                    </select>
                                </div>
                            </div>

                            <div class="sm:col-span-2">
                                <label for="date" class="block text-sm font-medium leading-6 text-gray-900">Date</label>
                                <div class="mt-2">
                                    <input
                                        v-model="data.date"
                                        type="date"
                                        name="date"
                                        id="date"
                                        autocomplete="date"
                                        class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    />
                                </div>
                            </div>
                        </div>
                    </div>

                    <Contact componentTitle="Contact person"/>
                    <Address componentTitle="Address person"/>
                </div>

                <div class="mt-6 flex items-center justify-end gap-x-6">
                    <button type="button" class="text-sm font-semibold leading-6 text-gray-900">Cancel</button>
                    <button type="submit" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Save</button>
                </div>
            </form>
        </div>
    </div>
</div>
</template>

<script>
import Contact from './components/Contact.vue';
import Address from './components/Address.vue';
import { Form, Field, ErrorMessage } from 'vee-validate';
import { useForm } from 'vue-hooks-form'

import axios from 'axios'
export default {
  name: 'App',
  components: {
    Contact,
    Address,
    Form,
    Field,
    ErrorMessage,
    useForm
  },
  data() {
    return {
      data : {
        first_name : "",
        last_name : "",
        email : "",
        selectVendorName: "",
        date:"",
      },
      vendorName : [],
      error : {},
    }
  },
  created() {
    this.loadVendorName();
  },
  methods : {
    loadVendorName() {
      axios.get("https://mocki.io/v1/ab6f47fa-033e-4b99-b8de-5425530fa779")
        .then((res) => {
          if(res.data.response_code == 200) {
            this.vendorName = res.data.data;
          }
          else {
            this.vendorName = [];
          }
        });
    },
    submitForm(submitEvent) {
      console.log('form submitted');
      

      if(this.data.first_name == "") {
        this.error.first_name = "Please enter a first name";
        console.log(this.error.first_name);
        return false;
      }
    },
    validateFirstName(value) {

      if (value && value.trim()) {
        console.log('yes');
        return true;
      }
      console.log('no');
      return 'This field is required';
    },
  }
}
</script>

<style>#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}</style>
