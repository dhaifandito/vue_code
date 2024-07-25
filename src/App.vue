<script setup>
import { ref } from 'vue';

const step = ref(localStorage.getItem('recentStep') ? localStorage.getItem('recentStep') : '1' );
const userString = localStorage.getItem('user');
const user = userString ? JSON.parse(userString) : null;
const name = ref(user && user.name ? user.name : '');
const gender = ref(user && user.gender ? user.gender : 'male');
const description = ref(user && user.description ? user.description : '');
const title = ref(user && user.title ? user.title : '');

const nextStep = () => {
  localStorage.setItem('recentStep', '2');
  localStorage.setItem('user', JSON.stringify({
  name: name.value,
  gender: gender.value,
  description: description.value,
  title: title.value
}));
  step.value = '2';
};

const prevStep = () => {
  localStorage.setItem('recentStep', '1');
  localStorage.setItem('user', JSON.stringify({
  name: name.value,
  gender: gender.value,
  description: description.value,
  title: title.value
}));
  step.value = '1';
};

const submit = () => {
  localStorage.setItem('recentStep', 'done');
  localStorage.setItem('user', JSON.stringify({
  name: name.value,
  gender: gender.value,
  description: description.value,
  title: title.value
}));
  step.value = 'done';
};

const refill = () => {
  localStorage.clear();
  step.value = '1';
  name.value='';
  gender.value='male';
  description.value='';
  title.value='';
}
</script>

<template>
  <div class='flex justify-center items-center h-screen bg-prime bg-opacity-80'>
    <div
      v-if="step === '1'"
      class='bg-white w-[70%] h-[70%] p-10 rounded-lg shadow-lg'
    >
      <div class='h-full'>
        <p class='text-right py-3 px-2 text-gray-500'>{{ step }}/2</p>
        <div class='w-full h-2 bg-gray-300 rounded-lg mb-8'>
          <p class='w-1/2 bg-prime h-2 rounded-lg'></p>
        </div>
        <div>
          <h1 class='text-4xl font-bold my-1'>Personal Information</h1>
          <p class='text-lg'>Please fill out your personal information</p>
        </div>
        <form
          @submit.prevent='nextStep'
          class='lg:m-8 flex flex-col justify-between h-[60%]'
        >
          <div class='flex flex-col justify-around h-full'>
            <div class='lg:grid grid-cols-2 px-40'>
              <label for='name'>Name<span class='text-prime'>*</span></label>
              <div>
                <input
                  type='text'
                  name='name'
                  id='name'
                  placeholder='Enter your name'
                  v-model='name'
                  required
                  class='bg-gray-50 w-full border border-gray-200 text-gray-900 p-3 text-sm rounded-lg focus:outline-none focus:border-prime focus:border-2'
                />
                <p v-if="name === ''" class='text-prime text-xs'>
                  *please input your name
                </p>
                <p v-else class='text-white text-xs'>*p</p>
              </div>
            </div>
            <div class='lg:grid grid-cols-2 px-40'>
              <label for='gender'
                >Gender<span class='text-prime'>*</span></label
              >
              <div>
                <label>
                  <input
                    type='radio'
                    v-model='gender'
                    value='male'
                    class='accent-prime my-1'
                  />
                  Male
                </label>
                <br />
                <label>
                  <input
                    type='radio'
                    v-model='gender'
                    value='female'
                    class='accent-prime my-1'
                  />
                  Female
                </label>
                <br />
                <label>
                  <input
                    type='radio'
                    v-model='gender'
                    value='other'
                    class='accent-prime my-1'
                  />
                  Other
                </label>
              </div>
            </div>
          </div>
          <div class='text-right pt-5'>
            <button
              v-if="name === '' || gender === ''"
              type='submit'
              class='bg-gray-500 bg-opacity-50 text-white font-bold py-2 px-4 rounded'
              disabled
            >
              Next
            </button>
            <button
              v-else
              type='submit'
              @click='nextStep'
              class='bg-prime hover:bg-opacity-80 text-white font-bold py-2 px-4 rounded'
            >
              Next
            </button>
          </div>
        </form>
      </div>
    </div>

    <div
      v-else-if="step === '2'"
      class='bg-white w-[70%] h-[70%] p-10 rounded-lg shadow-lg'
    >
      <div class='h-full'>
        <p class='text-right py-3 px-2 text-gray-500'>{{ step }}/2</p>
        <div class='w-full h-2 bg-gray-300 rounded-lg mb-8'>
          <p class='w-full bg-prime h-2 rounded-lg'></p>
        </div>
        <div>
          <h1 class='text-4xl font-bold my-1'>Additional Information</h1>
          <p class='text-lg'>Please provide additional detail</p>
        </div>
        <form
          @submit.prevent='keClick'
          class='lg:m-8 flex flex-col justify-between h-[60%]'
        >
          <div class='flex flex-col justify-around h-full'>
            <div class='lg:grid grid-cols-2 px-40'>
              <label for='description'>Description</label>
              <div>
                <textarea
                  name='description'
                  id='description'
                  placeholder='Enter a description'
                  v-model='description'
                  class='bg-gray-50 w-full h-24 border border-gray-200 text-gray-900 p-3 text-sm rounded-lg focus:outline-none focus:border-prime focus:border-2'
                />
              </div>
            </div>
            <div class='lg:grid grid-cols-2 px-40'>
              <label for='title'>Title<span class='text-prime'>*</span></label>
              <div>
                <input
                  name='title'
                  list='titles'
                  placeholder='Enter a title'
                  v-model='title'
                  class='bg-gray-50 w-full border border-gray-200 text-gray-900 p-3 text-sm rounded-lg focus:outline-none focus:border-prime focus:border-2'
                />
                <datalist id='titles'>
                  <option value='Mr.' />
                  <option value='Mrs.' />
                  <option value='Ms.' />
                  <option value='Dr.' />
                  <option value='Prof.' />
                </datalist>
                <p v-if="title === ''" class='text-prime text-xs'>
                  *please input your title
                </p>
                <p v-else class='text-white text-xs'>*p</p>
              </div>
            </div>
          </div>
          <div class='flex justify-between pt-5'>
            <button
              @click='prevStep'
              class='bg-prime hover:bg-opacity-80 text-white font-bold py-2 px-4 rounded'
            >
              Prev
            </button>
            <button
              v-if="title === ''"
              type='submit'
              class='bg-gray-500 bg-opacity-50 text-white font-bold py-2 px-4 rounded'
              disabled
            >
              Submit
            </button>
            <button
              v-else
              type='submit'
              @click='submit'
              class='bg-prime hover:bg-opacity-80 text-white font-bold py-2 px-4 rounded'
            >
              Submit
            </button>
          </div>
        </form>
      </div>
    </div>

    <div v-else class='bg-white w-[70%] h-[70%] p-10 rounded-lg shadow-lg'>
      <div class='h-full flex flex-col'>
        <div class='w-full'>
          <p class='text-right py-3 px-2 text-gray-500'>Done</p>
          <div class='w-full h-2 bg-gray-300 rounded-lg mb-8'>
            <p class='w-full bg-prime h-2 rounded-lg'></p>
          </div>
        </div>
        <div class='flex justify-around flex-col items-center h-full'>
          <div class="flex flex-col justify-center items-center">
          <h1 class='text-2xl font-bold my-1'>Thank you {{title}} {{name}}</h1>
          <h1 class='text-3xl font-bold my-1'>Your Form is Submitted</h1>
          </div>
          <button
            class='bg-prime hover:bg-opacity-80 text-white font-bold py-2 px-4 rounded'
            @click='refill'
          >
            Fill new form
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
