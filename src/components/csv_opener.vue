<template>
  <h1 id="asset">{{ asset }}</h1>
</template>

<script>
export default {
  name: "csv_opener",
  props: {
    asset: String
  }
}

// eslint-disable-next-line no-unused-vars
let txt = document.getElementById("asset")
//txt.addEventListener("click", function () {
//
//  fetch("/embeddings_1.csv")
//      .then((response) => console.log(response.text()))
//      .then((data) => {
//        console.log(data)
//      })
//});


</script>

<style scoped>

</style>
