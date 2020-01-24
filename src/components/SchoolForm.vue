<template>
    <div class="SchoolForm">
        <h1>School Form</h1>
        <form>
            <label>
                <input v-model="form.title" placeholder="Video Title">
            </label>
            <input type="checkbox" id="checkbox" v-model="form.allowDownloadForStudents">
            <label for="checkbox">{{ form.allowDownloadForStudents }}</label>
            <label>
                <textarea v-model="form.description" placeholder="description for video"></textarea>
            </label>
            <input type="radio" id="one" value="Youtube" v-model="form.videoLink">
            <label for="one">Youtube</label>
            <br>
            <input type="radio" id="two" value="Vimeo" v-model="form.videoLink">
            <label for="two">Vimeo</label>
            <br>
            <input type="radio" id="three" value="Others Link" v-model="form.videoLink">
            <label for="three">Others Link</label>
            <br>
            <span>Picked: {{ form.videoLink }}</span>
            <datepicker placeholder="Select Publish Date" v-model="form.publishDate"></datepicker>
            <label>
                <select v-model="form.selectedPreparation">
                    <option disabled value="">Please a preparation</option>
                    <option>A</option>
                    <option>B</option>
                    <option>C</option>
                </select>
            </label>
            <span>Selected: {{ form.selectedPreparation}}</span>
            <vue-tags-input
                    v-model="tag"
                    :tags="form.tags"
                    @tags-changed="newTags => form.tags = newTags"
            />
            <label>
                <select v-model="form.publishItIn">
                    <option disabled value="">Please where to publish</option>
                    <option>Public Library</option>
                    <option>Schools</option>
                    <option>Nothing</option>
                </select>
            </label>
            <div id='example-3'>
                <input type="checkbox" id="jack" value="Jack" v-model="form.publishInSpecialLibraries">
                <label for="jack">Jack</label>
                <input type="checkbox" id="john" value="John" v-model="form.publishInSpecialLibraries">
                <label for="john">John</label>
                <input type="checkbox" id="mike" value="Mike" v-model="form.publishInSpecialLibraries">
                <label for="mike">Mike</label>
                <br>
                <span>Checked names: {{ form.publishInSpecialLibraries }}</span>
            </div>
            <div>
                <table>
                    <tr>
                        <th>Name</th>
                        <th>Select All<label>
                            <input type="checkbox" @click="selectAll" v-model="allSelected">
                        </label></th>
                    </tr>
                    <tr v-for="user in users">
                        <td>{{ user.name }}</td>
                        <td><label>
                            <input type="checkbox" v-model="userIds" @click="select" :value="user.id">
                        </label></td>
                    </tr>
                </table>
            </div>
            <span>Selected Ids: {{ userIds }}</span>
        </form>
    </div>
</template>

<script>
  import './SchoolForm.scss';
  import Datepicker from 'vuejs-datepicker';
  import VueTagsInput from '@johmun/vue-tags-input';

  export default {
    name: 'SchoolForm',
    components: {
      Datepicker,
      VueTagsInput,
    },
    props: {},
    data: () => ({
      form: {
        title: '',
        allowDownloadForStudents: false,
        description: '',
        videoLink: '',
        publishDate: null,
        selectedPreparation: '',
        tags: [],
        publishItIn: '',
        publishInSpecialLibraries: [],
      },
      tag: '',
      users: [
        {
          id: 'Shad',
          name: 'Shad',
        },
        {
          id: 'Duane',
          name: 'Duane',
        },
        {
          id: 'Myah',
          name: 'Myah',
        },
        {
          id: 'Kamron',
          name: 'Kamron',
        },
        {
          id: 'Brendon',
          name: 'Brendon',
        },
      ],
      selected: [],
      allSelected: false,
      userIds: []
    }),
    methods: {
      selectAll: function () {
        this.userIds = [];
        this.allSelected = true;
        if (this.allSelected) {
          console.log(this.allSelected);
          for (const user in this.users) {
            this.userIds.push(this.users[user].id.toString());
          }
        }
      },
      select: function () {
        this.allSelected = false;
      }
    }
  };
</script>
