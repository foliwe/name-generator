<template>

  <section class="movie">
    <div v-auto-animate class="p-8 rounded-lg bg-neutral-900">
      <h2 @click="isOpen = !isOpen" class="text-xl font-bold">
        More information
      </h2>
      <p v-if="isOpen" class="py-8">
        Super hidden Treasure
      </p>
    </div>
  </section>

<section>
  <div ref="dropdown" class="dropdown">
    <strong class="dropdown-label" @click="show = !show">
      Click me to open!
    </strong>
    <p class="dropdown-content" v-if="show">Lorum ipsum...</p>
  </div>
</section>

  <section class="mx-auto h-screen grid md:grid-cols-3 gap-6 place-content-center p-3 md:w-[1200px]">

   <div class="">{{ $router.currentRoute.value!.matched[0]['components']!.default['name'] }}</div>
    <div class=" bg-white p-5 md:col-span-3">
      <div class="header text-center  text-stone-700">
        <h1 class="text-3xl pt-7">African Names Generator</h1>
        <p class="py-3">Choose your options and click the "Find Names" button Below</p>
        </div>
    </div>
    <div class="bg-stone-50 md:col-span-2 rounded-xl ">

        
        <div class="bg-white pb-6"> 

          <div class="gender-container flex flex-col justify-between items-center py-5 gap-3">
            <h1 class="text-3xl">1 Choose a gender</h1>
            <div class="genders flex">
                <button 
                  class="border border-indigo-500 p-1 w-20 rounded-l-lg font-bold"
                  :class="{'bg-sky-400 text-white': options.gender === Gender.BOY}"
                  @click="options.gender = Gender.BOY">Boy
                </button>

                <button 
                  class="border border-indigo-500 p-1 w-20 font-bold"
                  :class="{'bg-sky-400 text-white': options.gender === Gender.UNISEX}" 
                  @click="options.gender = Gender.UNISEX">
                  Unisex
                </button>

                <button 
                  class="border border-indigo-500 p-1 rounded-r-lg w-20  font-bold"
                  :class="{'bg-sky-400 text-white': options.gender === Gender.GIRL}" 
                  @click="options.gender = Gender.GIRL">
                  Girl
                </button>
              </div>
          </div>

          <div class="gender-container flex flex-col justify-between items-center py-5 gap-3">
            <h2 class=""> 2 Choose Name's Popularity</h2>
            <div class="genders flex">

                <button 
                  class="border border-indigo-500 p-1 rounded-l-lg w-20  font-bold"
                  :class="{'bg-sky-400 text-white': options.popularity === Popularity.TRENDY}"
                  @click="options.popularity = Popularity.TRENDY">
                  Trendy
              </button>
              <button 
              class="border border-indigo-500 p-1 rounded-r-lg w-20  font-bold"
              :class="{'bg-sky-400 text-white': options.popularity === Popularity.UNIQUE}"
              @click="options.popularity = Popularity.UNIQUE">
              unique</button>
            </div>
          </div>

          <div class="gender-container flex flex-col justify-between items-center py-5 gap-3">
            <h2 class=""> 3 Choose Name's Length</h2>
            <div class="genders flex">

              <button 
              class="border border-indigo-500 p-1 rounded-l-lg w-20 font-bold"
              :class="{'bg-sky-500 text-white': options.length === Length.ALL}"
              @click="options.length = Length.ALL">
              All
            </button>

              <button 
                class="border border-indigo-500 p-1 border-x-blue -6000 w-20 font-bold"
                :class="{'bg-sky-500 text-white': options.length === Length.LONG}"
                @click="options.length = Length.LONG">
                Long
              </button>

              <button 
                class="border border-indigo-500 p-1 w-20 font-bold"
                :class="{'bg-sky-500 text-white': options.length === Length.MEDIUM}"
                @click="options.length = Length.MEDIUM">
                Medium
              </button>

              <button 
                class="border border-indigo-500 p-1 rounded-r-lg w-20 font-bold"
                :class="{'bg-sky-500 text-white': options.length === Length.SHORT}"
                @click="options.length = Length.SHORT">
                Short
              </button>

            </div>
          </div>

          <div class="flex justify-center items-center gap-2"> 
              <button class="bg-sky-700 text-white p-2 rounded-xl w-1/3"
              @click="computeSelectedNames"
              > Find Names</button>

              <span class="text-red-700 hover:cursor-pointer" @click="clearnames"> Clear form</span>
          </div>

          <!-- <div class="flex justify-center my-4">
            {{ Selectednames }}
          </div> -->

        </div> 

       
    </div>

    <div class="md:pb-10">
      <div class=" flex justify-center items-center gap-3 bg-stone-100 text-stone-700 py-7">
        <h1>Results</h1>
        <span class="bg-teal-300 text-center rounded-full p-1 w-8 h-8">{{ Selectednames.length }}</span>
      </div>

      <div class="bg-white">
          <div class="" v-for="name in Selectednames" :key="name">
            <div class="flex justify-between items-center p-3 bg-stone-300 my-1 text-blue-600 font-semibold">
            <p>{{ name }}</p>
            <span> <IconsLove /> </span>
            </div>
            
          </div>
      </div>
    </div>

  </section>
</template>

<script setup lang="ts">
import {Gender, Popularity, Length, names } from '@/data'

import autoAnimate from "@formkit/auto-animate"

const dropdown = ref() // we need a DOM node
const show = ref(false)

const isOpen = ref(false)

const Selectednames = ref<string[]>([

]);

const rt = useRoute()

const routey = rt.params.pageName

console.log(routey);

const options = reactive({
gender: "",
popularity: "",
length: ""
})

const computeSelectedNames = () => {
  const filterNames = names.filter( name => { return name.gender === options.gender})
                           .filter(name => { return name.popularity === options.popularity})
                           .filter( name => {
                            if (options.length === Length.ALL) return true
                            else return name.length ==options.length;
                           })
                           Selectednames.value = filterNames.map( name => {
                            return name.name
                           })
}

const clearnames = () => {
  options.gender = ""
  options.popularity = ""
  options.length = ""
  Selectednames.value = []
  
  
}
</script>