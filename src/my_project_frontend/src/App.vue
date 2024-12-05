<script setup>
import { my_project_backend } from 'declarations/my_project_backend/index';
import { ref } from 'vue';
let blogs = ref([]);

async function handleSubmit(e) {
  e.preventDefault();
  const target = e.target;
  const title = target.querySelector('#title').value;
  const content = target.querySelector('#content').value;
  const tags = target.querySelector('#tags').value;
  const splitedTags = tags.split(',');

  console.log(title, content, tags);

  await my_project_backend.add_blog(title, content, splitedTags);
  await getBlogs();
}

async function getBlogs() {
  const tempBlogs = await my_project_backend.get_blogs();
  blogs.value = tempBlogs.map(blog => {
    return {
      ...blog,
      date: blog.date.toString(),
    };
  });
}
getBlogs();
</script>

<template>
  <main
    class="bg- 100 h-screen flex flex-col justify-center items-center font-sans"
  >
    <img src="/logo2.svg" alt="DFINITY logo" />
    <br />
    <br />
    <form
      action="#"
      @submit="handleSubmit"
      class="w-1/3 flex flex-col justify-center items-center m-10 gap-8"
    >
      <div class="w-full">
        <p class="text-xl">Title:</p>
        <input
          id="title"
          alt="title"
          type="text"
          class="h-12 rounded-md p-2 border-gray-800 border-2 w-full"
        />
      </div>
      <div class="w-full">
        <p class="text-xl">Content:</p>
        <textarea
          id="content"
          alt="content"
          class="h-12 rounded-md p-2 border-gray-800 border-2 w-full px-4"
        >
        </textarea>
      </div>
      <div class="w-full">
        <p class="text-xl">Tags:</p>
        <input
          id="tags"
          alt="tags"
          type="text"
          class="h-12 w-full rounded-md p-2 border-gray-800 border-2"
        />
      </div>
      <button
        type="submit"
        class="hover:bg-gray-800 p-4 w-1/4 rounded-xl hover:text-white border-gray-900 border-solid border-2 transition-colors"
      >
        Click to add!
      </button>
    </form>
    <div
      class="w-2/3 border-solid border-2 border-gray-800 m-2 px-10 py-5"
      v-for="blog in blogs"
    >
      <h2 class="font-bold text-3xl">{{ blog.title }}</h2>
      <span class="tags">
        <span v-for="tag in blog.tags">
          {{ tag }}
        </span>
      </span>
      <p>{{ blog.content }}</p>
    </div>
  </main>
</template>
