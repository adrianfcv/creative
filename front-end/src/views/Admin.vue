<template>
  <div class="admin">
    <h1>The Admin Page!</h1>
      <div class="heading">
        <div class="circle">1</div>
        <h2>Add an Item</h2>
      </div>
      <div class="add">
        <div class="form">
          <input v-model="title" placeholder="Name">
          <p></p>
          <input type="file" name="photo" @change="fileChanged">
          <br><br>
          <input v-model="country" placeholder="Country"><p></p>
          <input v-model="club" placeholder="Soccer Club"><p></p>
          <input v-model="birth" placeholder="Birth"><p></p>
          <input v-model="position" placeholder="Position"><p></p>
          <input v-model="category" placeholder="Category"><p></p>
          <textarea v-model="description" placeholder="Facts">
          </textarea>
          <button @click="upload">Upload</button>
        </div>
        <div class="upload" v-if="addItem">
          <h2>{{addItem.title}}</h2>
          <img :src="addItem.path" />
        </div>
      </div>
      <div class="heading">
         <div class="circle">2</div>
         <h2>Edit/Delete Soccer Players</h2>
      </div>
      <div class="edit">
         <div class="form">
           <input v-model="findTitleSoccer" placeholder="Search">
           <div class="suggestions" v-if="suggestions('Soccer').length > 0">
             <div class="suggestion" v-for="s in suggestions('Soccer')" :key="s.id" @click="selectItemSoccer(s)">{{s.title}}
             </div>
           </div>
         </div>
        <div class="upload" v-if="findItemSoccer">
           <input v-model="findItemSoccer.title"><p></p>
           <img :src="findItemSoccer.path" />
           <br>
           <input v-model="findItemSoccer.country"><p></p>
           <input v-model="findItemSoccer.club"><p></p>
           <input v-model="findItemSoccer.birth"><p></p>
           <input v-model="findItemSoccer.position"><p></p>
           <input v-model="findItemSoccer.category"><p></p>
           <textarea v-model="findItemSoccer.description"></textarea>
        </div>
           <div class="actions" v-if="findItemSoccer">
             <button @click="deleteItem(findItemSoccer)">Delete</button>
             <button @click="editItem(findItemSoccer)">Edit</button>
           </div>
        </div>
        <div class="heading">
           <div class="circle">3</div>
           <h2>Edit/Delete Football Players</h2>
        </div>
        <div class="edit">
           <div class="form">
             <input v-model="findTitleFootball" placeholder="Search">
             <div class="suggestions" v-if="suggestions('Football').length > 0">
               <div class="suggestion" v-for="r in suggestions('Football')" :key="r.id" @click="selectItemFootball(r)">{{r.title}}
               </div>
             </div>
           </div>
          <div class="upload" v-if="findItemFootball">
             <input v-model="findItemFootball.title"><p></p>
             <img :src="findItemFootball.path" />
             <br>
             <input v-model="findItemFootball.country"><p></p>
             <input v-model="findItemFootball.club"><p></p>
             <input v-model="findItemFootball.birth"><p></p>
             <input v-model="findItemFootball.position"><p></p>
             <input v-model="findItemFootball.category"><p></p>
             <textarea v-model="findItemFootball.description"></textarea>
          </div>
             <div class="actions" v-if="findItemFootball">
               <button @click="deleteItem(findItemFootball)">Delete</button>
               <button @click="editItem(findItemFootball)">Edit</button>
             </div>
          </div>
  </div>
</template>


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

.heading h2 {
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
  margin-right: 50px;
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


<script>
import axios from 'axios';
export default {
  name: 'Admin',

    data() {
      return {
        title: "",
        country: "",
        club: "",
        birth: "",
        position: "",
        category: "",
        description: "",
        file: null,
        addItem: null,
        items: [],
        findTitleSoccer: "",
        findItemSoccer: null,
        findTitleFootball: "",
        findItemFootball: null,
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
             title: this.title,
             country: this.country,
             club: this.club,
             birth: this.birth,
             position: this.position,
             category: this.category,
             description: this.description,
             path: r1.data.path
           });
           this.addItem = r2.data;
           this.getItems();
         } catch (error) {
           console.log(error);
         }
       },
       async getItems() {
          try {
            let response = await axios.get("/api/items/:category");
            this.items = response.data;
            return true;
          } catch (error) {
            console.log(error);
          }
        },
        selectItemSoccer(item) {
          this.findTitleSoccer = "";
          this.findItemSoccer = item;
        },
        selectItemFootball(item) {
          this.findTitleFootball = "";
          this.findItemFootball = item;
        },
        async deleteItem(item) {
          try {
            await axios.delete("/api/items/" + item._id);
            this.findItemSoccer = null;
            this.findItemFootball = null;
            this.getItems();
            return true;
          } catch (error) {
            console.log(error);
          }
        },
        async editItem(item) {
          if (item.category=="Soccer") {
            try {
              await axios.put("/api/items/" + item._id, {
                title: this.findItemSoccer.title,
                country: this.findItemSoccer.country,
                club: this.findItemSoccer.club,
                birth: this.findItemSoccer.birth,
                position: this.findItemSoccer.position,
                category: this.findItemSoccer.category,
                description: this.findItemSoccer.description,
              });
                this.findItemSoccer = null;
                this.getItems();
                return true;
              } catch (error) {
                console.log(error);
              }
            }
            else if (item.category=="Football") {
              try {
                await axios.put("/api/items/" + item._id, {
                  title: this.findItemFootball.title,
                  country: this.findItemFootball.country,
                  club: this.findItemFootball.club,
                  birth: this.findItemFootball.birth,
                  position: this.findItemFootball.position,
                  category: this.findItemFootball.category,
                  description: this.findItemFootball.description,
                });
                  this.findItemFootball = null;
                  this.getItems();
                  return true;
                } catch (error) {
                  console.log(error);
                }
            }
        },
        suggestions(parameter) {
          if (parameter=="Soccer") {
            let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitleSoccer.toLowerCase()) && item.category=="Soccer");
            return items.sort((a, b) => a.title > b.title);
          }
          else if (parameter=="Football"){
            let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitleFootball.toLowerCase()) && item.category=="Football");
            return items.sort((a, b) => a.title > b.title);
          }
          else {
            return [];
          }
       }
     }
   }

</script>
