<template>
  <h1 id="asset">{{ asset }}</h1>
  <button id="test_button">PUSH ME!</button>
  <h1 id="status">0</h1>
</template>

<script>
/* eslint-disable no-unused-vars */
export default {
  name: "csv_opener",
  props: {
    asset: String
  }
}
import * as localforage from 'localforage';
import Vue, {markRaw} from 'vue';

const dl_list = localforage.createInstance({
  driver: localforage.INDEXEDDB,
  name: "hello_picture_selector",
  storeName: "dl_list",
  version: 1
})
const dl_data = localforage.createInstance({
  driver: localforage.INDEXEDDB,
  name: "hello_picture_selector",
  storeName: "dl_data",
  version: 1
})

async function sha256(text) {
  const uint8 = new TextEncoder().encode(text)
  const digest = await crypto.subtle.digest('SHA-256', uint8)
  return Array.from(new Uint8Array(digest)).map(v => v.toString(16).padStart(2, '0')).join('')
}

window.onload = function () {
// eslint-disable-next-line no-unused-vars
  let txt = document.getElementById("test_button")
  txt.addEventListener("click", async function () {
    const dl_list_obj = await get_dl_list();
    console.log(dl_list_obj);
    let dl_data_keys = await dl_data.keys();
    Object.keys(dl_list_obj).forEach(
        name => {
          if (dl_data_keys.includes(name)) {
            console.log(name);
          } else {
            console.warn(name);
            (async () => {
              let response = await fetch("/data/" + name);
              let data_blob = await response.text();
              dl_data.setItem(name, [await sha256(data_blob), data_blob], function (err, value) {
                if (err != null) {
                  console.log(err, value);
                }
              }).then(() => display_num());
            })()
          }
        }
    )
  });
}

function display_num() {
  let stat = parseInt(document.getElementById("status").innerText);
  stat += 1;
  document.getElementById("status").innerText = stat.toString();
}

async function get_dl_list() {

  const resp = await fetch("/data/data.list");
  const list_rows = (await resp.text()).split('\n');
  let list = {};
  let name, sha256sum;
  list_rows.forEach(value => {
    [sha256sum, name] = value.split(' ');
    if (name === undefined) return;
    list[name] = sha256sum;
  })
  return list;

}

//let fetches = [];
//for (let i = 0; i <= range; i++) {
//  fetches.push("/data/x" + ('000' + i).slice(-3))
//
//}
//const data = await Promise.all(fetches.map(async url => {
//  const resp = await fetch(url);
//  let stat = parseInt(document.getElementById("status").innerText);
//  stat += 1;
//  document.getElementById("status").innerText = stat.toString();
//  return resp.text();
//}));
//
//let obj = {};
//data.forEach(chunk =>
//    chunk.split('\n').forEach(row => {
//          let pic_name = row.split(',')[0];
//          let str_arr = row.split(',')
//          str_arr.shift()
//          let arr;
//          str_arr.forEach(val => arr.push(parseFloat(val)));
//          Object.assign(obj, {[pic_name]: arr})
//        }
//    ));

// Object.keys(obj).slice(0, 10).forEach(
//     key => console.log({[key]: obj[key]})
// )
//}

// function make_obj(line) {
//   let return_obj = {};
//   let rows = line.split('\n');
//   for (const row in rows) {
//     let div = row.split(',')
//     Object.assign(return_obj, {[div[0]]: div.shift()})
//   }
//   return return_obj;
// }


</script>

<style scoped>

</style>
