<template>
    <div class="SchoolForm">
        <h1>School Form</h1>
        <b-form @submit="onSubmit">
            <b-form-group
                    label="Video Title"
                    label-for="input-video-title"
                    description="title for the video"
            >
                <b-form-input
                        id="input-video-title"
                        v-model="form.title"
                        type="text"
                        required
                        placeholder="Video Title"
                ></b-form-input>
            </b-form-group>
            <b-form-group id="input-allow-download-for-students">
                <b-form-checkbox-group v-model="form.allowDownloadForStudents"
                                       id="checkbox-allow-download-for-students">
                    <b-form-checkbox value="true">Allow Download For Students</b-form-checkbox>
                </b-form-checkbox-group>
            </b-form-group>
            <b-form-textarea
                    id="video-description"
                    v-model="form.description"
                    placeholder="Enter description for video..."
                    rows="3"
                    max-rows="6"
            ></b-form-textarea>
            <div>
                <b-form-group label="video Url">
                    <b-form-radio v-model="form.videoUrl" name="video-url-radios" value="Youtube">Option A</b-form-radio>
                    <b-form-radio v-model="form.videoUrl" name="video-url-radios" value="Vimeo">Option B</b-form-radio>
                    <b-form-radio v-model="form.videoUrl" name="video-url-radios" value="Other Link">Option B</b-form-radio>
                </b-form-group>
                <div class="mt-3">Selected Video URL: <strong>{{ form.videoUrl }}</strong></div>
            </div>
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
            <b-button type="submit" variant="primary">Submit</b-button>
        </b-form>
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
        videoUrl: '',
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
      onSubmit(evt) {
        evt.preventDefault();
        console.log(this.form);
      },
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
