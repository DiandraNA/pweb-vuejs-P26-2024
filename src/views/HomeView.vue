<script lang="ts">
import BookCard from "../components/BookCard.vue";

interface BookObject {
  _id: string;
  title: string;
  author: string;
  tags: string[];
  publishedDate: string;
  initialQty: number;
  qty: number;
  publisher: string;
}

export default {
  name: "HomeView",
  data: () => ({
    booksData: [] as BookObject[],
    fetchError: false,
  }),
  async mounted() {
  try {
    const response = await fetch("http://localhost:8080/book", {
      headers: {
        'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3MzIyNTFmOGVmNTE5YzZhODhlNDQ0MCIsImVtYWlsIjoiYWRtaW5AZ21haWwuY29tIiwiaWF0IjoxNzMzMjE4NzAxfQ.kT43Ie-eAlfvtehxdoDHj1MBmL0E4UjBOcsWso82L6c'
      }
    });
    const data = await response.json();
    console.log(data); // Log the entire response data
    if (Array.isArray(data)) {
      this.booksData = [...data];
    } else {
      console.error("Unexpected data format:", data);
      this.fetchError = true;
    }
  } catch (error) {
    console.error(error);
    this.fetchError = true;
  }
}
  ,
  components: {
    BookCard,
  },
};
</script>

<template>
  <main class="h-full bg-white">
    <!-- Hero Section -->
    <section class="hero min-h-screen bg-beige text-white text-center px-5 py-5 md:px-[100px] md:py-[36px] flex flex-col justify-center items-center">
      <div class="w-full md:w-2/3 h-fit flex flex-col">
        <h1 class="text-4xl md:text-6xl font-bold text-teal">Find Your Next Books</h1>
        <p class="text-sm md:text-md mt-4 text-teal">Discover your next favorite read! Whether you're into the latest bestsellers, timeless classics, or niche genres, we have a wide selection of books to suit every taste. Start your journey today and explore our collection of handpicked titles.</p>
      </div>
      
      <a
        href="#books"
        class="mt-6 px-4 py-2 bg-teal text-beige font-semibold rounded shadow"
      >
        Discover Now
      </a>
    </section>

    <!-- Books Section -->
    <section id="books" class="min-h-screen py-10 px-4 bg-sky">
      <h2 class="text-4xl font-bold text-center text-navy">Best Seller</h2>
      <div class="flex flex-wrap justify-center gap-4 mt-10">
        <BookCard
          v-if="booksData.length"
          v-for="book in booksData"
          :key="book._id"
          :book="book"
          class="w-full sm:w-full md:w-2/6 lg:w-1/4"
        />
        <h1
          class="font-bold text-3xl text-center w-full"
          v-else-if="fetchError"
        >
          Failed to load books
        </h1>
        <h1 class="font-bold text-3xl text-center w-full" v-else>Loading...</h1>
      </div>
    </section>

    <!-- Footer Section -->
    <footer class="footer bg-navy text-beige py-4 text-center">
      <p>&copy; 2024 Book Gallery App P26. All rights reserved.</p>
    </footer>
  </main>
</template>
