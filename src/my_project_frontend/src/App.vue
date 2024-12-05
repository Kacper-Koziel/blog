<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';
let blogs = ref([]);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const title = target.querySelector('#title').value;
  const content = target.querySelector('#content').value;
  const tags = target.querySelector('#tags').value;

  const splittedTags = tags.split(",");

  await my_project_backend.add_blog(title, content, splittedTags)
  await getBlogs()
}

async function getBlogs()
{
  const tempBlogs = await my_project_backend.get_blogs();
  blogs.value = tempBlogs.map((blog) => {
    return {
      ...blog,
      date: blog.date.toString()
    }
  })
}

getBlogs()
</script>

<template>
  <main class="container mx-auto mt-20">
    <form class="grid gap-10" action="#" @submit="handleSubmit">
      <div><p class="text-white">Title: </p>
      <input id="title" alt="title" type="text" class="w-full rounded-full py-1 px-4" /></div>

      <div><p class="text-white">Content: </p>
      <textarea id="content" alt="content" type="text" class="w-full rounded-3xl py-1 px-4 min-h-[70px]" /></div>

      <div><p class="text-white">Tags: </p>
      <input id="tags" alt="tags" type="text" class="w-full rounded-full py-1 px-4" /></div>

      <div class="flex justify-end"><button type="submit" class="text-white bg-red-600 rounded-full w-50 py-2 px-12">Add blog</button></div>
    </form>
    {{ blogs }}
  </main>
</template>
