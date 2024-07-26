<template>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12">
          <h2 class="text-center my-4 fw-bold">BUKU</h2>
          <div class="input-group flex-nowrap rounded">
            <form @submit.prevent="getbooks" class="col mb-3">
              <input v-model="keyword" type="search" class="form-control from-control-lg rounded-5" placeholder="Mau Baca Buku Apa Hari Ini?..." aria-label="Search" @input="getbooks" />
            </form>
          </div>
            <!-- <div class="my-3">Rekomendasi Buku {{ books.length }}  {{ jmlbuku }}</div> -->  
          <div class="my-3 text-muted">Menampilkan {{ books.length }} dari {{ banyak }} </div>
          <div class="row">
            <div v-for="(book,i) in books" :key="i" class="col-lg-2">
              <nuxt-link :to="`/buku/${book.id}`">
                <div class="card mb-4">  
                  <div class="card-body">
                    <img :src="book.cover" class="cover" alt="cover buku" />
                  </div>
                  <div>
                  </div>
                </div>
              </nuxt-link>
            </div>
          </div>
        </div>
      </div>
      <nuxt-link to="/" class="btn btn-danger btn-lg rounded-4 px-4 ms.auto">KEMBALI</nuxt-link>
    </div>
</template>

<script setup>
useHead({
  title: "PERPUS DIGITAL",
  meta: [
  {
    name: "description",
    content: "Halaman detail buku",
  },
],
})
const supabase= useSupabaseClient()

const keyword = ref('')
const books = ref([])
const banyak = ref([]);
// const jmlbuku = ref(0)

const getbooks = async () => {
  const { data ,error } = await supabase
  .from("buku")
  .select(`*, kategori(*)`)
  .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
  if (error) throw error
}

const banyakBuku = async () => {
  const { data, count } = await supabase.from("buku").select(`*`, { count: "exact"})
  if (data) banyak.value= count
}
// const getJmlBuku = async () => {
//   const{ data, count } = await supabase
//     .from("buku") 
//     .select('*', { count: "exact" })
//     if(data) jmlbuku.value = count
// }

onMounted(() =>{
  getbooks()
  banyakBuku()
  // getJmlBuku()
});
</script>

<style scoped>
.card-body {
  width: 100%;
  height: 25rem;
  padding: 0;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

.btn-light {
  background-color: 5fd8fe !important;
  box-shadow: 1px 1px 10px #5fd8fe !important;
}
</style>