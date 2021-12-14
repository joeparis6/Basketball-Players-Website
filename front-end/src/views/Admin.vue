<template>
<div class="admin">
  <h1>Add/Delete Players</h1>
  <div class="heading">
    <div class="circle">1</div>
    <h2>Add a Player</h2>
  </div>
  <div class="add">
    <div class="form">
      <input v-model="player" placeholder="Name">
      <p></p>
      <textarea v-model="college" placeholder="College"></textarea>
      <p></p>
      <input type="file" name="photo" @change="fileChanged">
      <button @click="upload">Upload</button>
    </div>
    <div class="upload" v-if="addItem">
      <h2>{{addItem.player}}</h2>
      <h2>{{addItem.college}}</h2>
      <img :src="addItem.path" />

    </div>
  </div>


<div class="heading">
<div class="circle">2</div>
<h2>Edit/Delete a Player</h2>
</div>
<div class="edit">
<div class="form">
  <input v-model="findTitle" placeholder="Search">
  <div class="suggestions" v-if="suggestions.length > 0">
    <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.player}}
    </div>
  </div>
</div>
<div class="upload" v-if="findItem">
  <input v-model="findItem.player">
  <textarea v-model="findItem.college"></textarea>
  <p></p>
  <img :src="findItem.path" />
</div>
<div class="actions" v-if="findItem">
  <button @click="deleteItem(findItem)">Delete</button>
  <button @click="editItem(findItem)">Edit</button>

</div>
</div>

<footer>
  
</footer>

</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Admin',
  data() {
  return {
    player: "",
    college: "",
    file: null,
    addItem: null,
    items: [],
    findTitle: "",
    findItem: null,
  }
},
computed: {
  suggestions() {
    let items = this.items.filter(item => item.player.toLowerCase().startsWith(this.findTitle.toLowerCase()));
    return items.sort((a, b) => a.player > b.player);
  }
},

created() {
  this.getItems();
},

  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          player: this.player,
          college: this.college,
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        // error
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;

        return true;
      } catch (error) {
        // error
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        // error
      }
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          player: this.findItem.player,
          college: this.findItem.college,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        // error
      }
    },


  }

}


</script>


<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 h3 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-left: 250px;
  margin-right: 100px;
  padding: 12px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}


</style>
