<template>
    <b-container class="SchoolForm p-3" fluid="md">
        <b-row>
            <b-col>
                <b-jumbotron header="School Form" lead="Easy school form..">
                    <h3>Create Video:</h3>
                </b-jumbotron>
            </b-col>
        </b-row>
        <b-form-row>
            <b-col>
                <b-form @submit="onSubmit">
                    <b-form-group
                            label="Video Title : "
                            label-for="input-video-title"
                            class="text-left"
                    >
                        <b-form-input
                                id="input-video-title"
                                v-model="form.title"
                                type="text"
                                required
                                placeholder="Please enter video title"
                        ></b-form-input>
                    </b-form-group>

                    <b-form-checkbox
                            id="checkbox-allow-download-for-students" class="text-left"
                            v-model="form.can_students_download"
                            name="checkbox-allow-download-for-students"
                            value="true"
                            unchecked-value="not_accepted"
                    >
                        Allow Download For Students
                    </b-form-checkbox>

                    <b-form-checkbox
                            id="publish-timeline" class="text-left"
                            v-model="form.publish_timeline"
                            name="publish-timeline"
                            value="true"
                            unchecked-value="not_accepted"
                    >
                        Publish on timeline
                    </b-form-checkbox>
                    <b-form-group label="Video URL : " class="text-left">
                        <b-form-radio v-model="form.video_type" name="video-url-radios" value="youtube">Youtube
                        </b-form-radio>
                        <b-form-radio v-model="form.video_type" name="video-url-radios" value="vimeo">Vimeo
                        </b-form-radio>
                        <b-form-radio v-model="form.video_type" name="video-url-radios" value="other Link">Others
                        </b-form-radio>
                    </b-form-group>
                    <div class="mt-3 text-left" v-if="form.video_type !== null">Video URL Source: <strong>{{form.video_type}}</strong>
                    </div>
                    <b-form-group v-if="form.video_type !== null"
                                  id="video url"
                                  label-for="video-url"
                    >
                        <b-form-input
                                id="video-url"
                                v-model="form.video_url"
                                type="text"
                                required
                                placeholder="Enter the video URL link"
                        ></b-form-input>
                    </b-form-group>
                    <b-row>
                        <b-col>
                            <h6 class="text-left">Publish Date : </h6>
                            <datepicker class="text-left form-control" placeholder="Select Publish Date"
                                        v-model="form.publish_date"></datepicker>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col>
                            <h6 class="text-left">Preparations : </h6>
                            <b-form-select v-model="form.preparation_id" class="text-left"
                                           :options="preparationsOptions">
                                <template v-slot:first>
                                    <b-form-select-option :value="null" disabled>-- Please select a preparation --
                                    </b-form-select-option>
                                </template>
                            </b-form-select>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col>
                            <h6 class="text-left">Tags : </h6>
                            <vue-tags-input
                                    class="text-left form-control"
                                    v-model="tag"
                                    :tags="form.tags"
                                    @tags-changed="newTags => form.tags = newTags"
                            />
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col>
                            <h6 class="text-left">Publish it in : </h6>
                            <b-form-select v-model="form.sharing_with" :options="publishItInOptions"></b-form-select>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col>
                            <b-form-group label="Publish In Special Libraries:" class="text-left">
                                <b-form-checkbox-group
                                        id="checkbox-group-1"
                                        v-model="form.publish_custom_library"
                                        :options="specialLibrariesOptions"
                                        name="flavour-1"
                                ></b-form-checkbox-group>
                            </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col>
                            <b-form-group class="text-left">
                                <template v-slot:label>
                                    <b class="text-left">Share contents with students : </b><br>
                                    <b-form-checkbox
                                            v-model="allSelected"
                                            :indeterminate="indeterminate"
                                            aria-describedby="classes"
                                            aria-controls="classes"
                                            @change="toggleAll"
                                    >
                                        {{ allSelected ? 'Un-select All' : 'Select All' }}
                                    </b-form-checkbox>
                                </template>

                                <b-form-checkbox-group
                                        id="classes"
                                        v-model="form.students"
                                        :options="classes"
                                        name="classes"
                                        class="ml-4"
                                        aria-label="Individual classes"
                                        stacked
                                ></b-form-checkbox-group>
                            </b-form-group>
                        </b-col>
                    </b-row>
                    <b-button type="submit" variant="primary">Submit</b-button>
                </b-form>
            </b-col>
        </b-form-row>
    </b-container>
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
        can_students_download: false,
        video_type: null,
        video_file: null,
        srt_file: null,
        video_url: '',
        publish_date: null,
        publish_timeline: false,
        preparation_id: null,
        tags: [],
        sharing_with: null,
        publishItIn: null,
        publish_custom_library: [],
        students: [],
        subject_id: 1
      },
      preparationsOptions: [],
      publishItInOptions: [
        {
          value: null,
          text: 'Please select an option'
        },
        {
          value: 'Public Library',
          text: 'Public Library'
        },
        {
          value: 'Schools',
          text: 'Schools'
        },
        {
          value: 'Nothing',
          text: 'Nothing'
        }
      ],
      specialLibrariesOptions: [],
      tag: '',
      classes: [],
      allSelected: false,
      indeterminate: false,
      userIds: [],
      loading: true
    }),
    methods: {
      onSubmit(evt) {
        evt.preventDefault();
        console.log(this.form);
      },
      toggleAll(checked) {
        this.form.students = checked ? this.classes.slice() : [];
      },
      fetchPreparations: function () {
        const params = {
          subject_id: 1,
        };
        const preparationsBaseURI = 'https://roles.viewclass.com/api/preparing.list';
        this.$http.get(preparationsBaseURI, { params })
          .then((result) => {
            console.log(result.data);
            this.preparationsOptions = Object.values(result.data[1]);
            this.preparationsOptions = this.preparationsOptions.map((x, i) => {
              x = {
                value: i,
                text: x
              };
              return x;
            });
            console.log('preparationsOptions : ', this.preparationsOptions);
          })
          .catch(error => {
            console.log(error);
            // this.errored = true;
          })
          .finally(() => this.loading = false);
      },
      fetchSpecialLibraries: function () {
        const specialLibrariesBaseURI = 'https://roles.viewclass.com/api/custom-libraries.list';
        this.$http.get(specialLibrariesBaseURI)
          .then((result) => {
            this.specialLibrariesOptions = [...new Set(Object.values(result.data[1]))]; // take only values from the object adn get me an array
            //Set is to remove the array duplicates
            this.specialLibrariesOptions = this.specialLibrariesOptions.map((x, i) => {
              x = {
                value: i,
                text: x
              };
              return x;
            });
            console.log('specialLibrariesOptions : ', this.specialLibrariesOptions);
          })
          .catch(error => {
            console.log(error);
            // this.errored = true;
          })
          .finally(() => this.loading = false);
      },
      fetchClasses: function () {
        const params = {
          class_id: 1
        };
        const classesBaseURI = 'https://roles.viewclass.com/api/class.students.list';
        this.$http.get(classesBaseURI, { params })
          .then((result) => {
            this.classes = [...new Set(Object.values(result.data[1]))]; // take only values from the object adn get me an array
            //Set is to remove the array duplicates
            this.classes = this.classes.map((x, i) => {
              x = {
                value: i,
                text: x
              };
              return x;
            });
            console.log('classes : ', this.classes);
          })
          .catch(error => {
            console.log(error);
            // this.errored = true;
          })
          .finally(() => this.loading = false);
      }
    },
    watch: {
      selected(newVal, oldVal) {
        // Handle changes in individual flavour checkboxes
        if (newVal.length === 0) {
          this.indeterminate = false;
          this.allSelected = false;
        } else if (newVal.length === this.users.length) {
          this.indeterminate = false;
          this.allSelected = true;
        } else {
          this.indeterminate = true;
          this.allSelected = false;
        }
      }
    },
    mounted() {
      this.fetchPreparations();
      this.fetchSpecialLibraries();
      this.fetchClasses();
    }
  };
</script>
