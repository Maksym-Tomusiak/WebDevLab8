<script setup lang="ts">

interface User {
  name: string;
  email: string;
}

interface Category {
  title: string;
  description: string;
}

interface Post {
  id: number;
  user: User;
  category: Category;
  title: string;
  published_at: string;
  created_at: string;
  updated_at: string;
  content_raw: string;
  is_published: boolean;
}

const route = useRoute();
const postId = ref<number | null>(null);
const post = ref<Post | null>(null);

const getPost = async (id: number) => {
  try {
    const response = await $fetch<Post>(`http://127.0.0.1:8000/api/blog/posts/${id}`);
    post.value = response;
  } catch (error) {
    console.error('Error fetching post:', error);
  }
};

onMounted(() => {
  const id = parseInt(route.params.id as string, 10);
  if (!isNaN(id)) {
    postId.value = id;
    getPost(id);
  } else {
    console.error('Invalid post ID');
  }
});
</script>


<!-- pages/[id].vue -->
<template>
  <div class="flex flex-row justify-evenly my-2">
    <div class="w-8/12 text-center">
      <UCard>
        <template #header>
          <p>Post ID: {{ postId }}</p>
        </template>
        <ul>
          <li class="border-b-2 my-3 pb-3">Title: {{post?.title}}</li>
          <li class="border-b-2 my-3 pb-3">Is published: {{post?.is_published ? 'True' : 'False'}}</li>
          <li class="border-b-2 my-3 pb-3">Published at: {{post?.published_at}}</li>
          <li class="border-b-2 my-3 pb-3">Created at: {{post?.created_at}}</li>
          <li>Updated at: {{post?.updated_at}}</li>
        </ul>

        <template #footer>
          <p class="mt-3">Content: {{post?.content_raw}}</p>
        </template>
      </UCard>
    </div>
    <div class="w-3/12 text-center flex flex-col gap-3">
      <UCard>
        <template #header>
          <p>Category</p>
        </template>
        <ul>
          <li class="border-b-2 mb-3 pb-3">Title: {{post?.category?.title}}</li>
          <li class=" my-3">Description: {{post?.category?.description ? post?.category?.description : 'Empty'}}</li>
        </ul>
      </UCard>
      <UCard>
        <template #header>
          <p>User</p>
        </template>
        <ul>
          <li class="border-b-2 mb-3 pb-3">Name: {{post?.user?.name}}</li>
          <li class=" my-3">Email: {{post?.user?.email}}</li>
        </ul>
      </UCard>
    </div>
  </div>
</template>