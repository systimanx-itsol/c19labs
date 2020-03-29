<template>
  <div>
    <section class="hero">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h1 class="title is-spaced">What I will need to do?</h1>
          <h2
            class="subtitle"
          >Complete a short survey. This will include questions about yourself, and any known contact with others who may have exposed to CORONA virus.</h2>
          <div class="container is-centered">
            <h3 class="subtitle">
              <b>Basic Information</b>
            </h3>
          </div>
          <div class="container is-centered">
            <form @submit.prevent="onSubmit" class="form-horizontal">
              <fieldset>
                <!-- Text input-->
                <div class="field">
                  <label class="label" for="textinput-0">Name</label>
                  <div class="control">
                    <input
                      id="textinput-0"
                      name="textinput-0"
                      type="text"
                      v-model="name"
                      placeholder="Enter your name"
                      class="input"
                      required
                    />
                    <!-- <p class="help">Eg: Raja</p> -->
                  </div>
                </div>

                <!-- Text input-->
                <div class="field">
                  <label class="label" for="textinput-1">Location</label>
                  <div class="control">
                    <b-autocomplete
                      v-model="location"
                      placeholder="Enter your location"
                      :loading="isFetching"
                      @typing="getData"
                      :data="filteredDataObj"
                      field="title"
                      @select="option => selected = option"
                      required
                    ></b-autocomplete>
                    <p class="help">Eg: Madurai</p>
                  </div>
                </div>

                <!-- Text input-->
                <div class="field">
                  <label class="label" for="textinput-2">Age</label>
                  <div class="control">
                    <input
                      id="textinput-2"
                      name="textinput-2"
                      v-model="age"
                      type="number"
                      min="1"
                      max="100"
                      placeholder="Enter Age"
                      class="input"
                    />
                    <p class="help">Eg: 21</p>
                  </div>
                </div>

                <!-- Multiple Radios (inline) -->
                <div class="field">
                  <!-- <label class="label" for>Select Gender</label> -->
                  <div class="control">
                    <label class="radio inline" for="multipleradiosinline-0-0">
                      <input
                        type="radio"
                        name="multipleradiosinline-0"
                        id="multipleradiosinline-0-0"
                        v-model="gender"
                        value="Male"
                        checked="checked"
                        required="required"
                      />
                      Male
                    </label>
                    <label class="radio inline" for="multipleradiosinline-0-1">
                      <input
                        type="radio"
                        name="multipleradiosinline-0"
                        id="multipleradiosinline-0-1"
                        v-model="gender"
                        value="Female"
                        required="required"
                      />
                      Female
                    </label>
                    <label class="radio inline" for="multipleradiosinline-0-2">
                      <input
                        type="radio"
                        name="multipleradiosinline-0"
                        id="multipleradiosinline-0-2"
                        v-model="gender"
                        value="Others"
                        required="required"
                      />
                      Others
                    </label>
                  </div>
                </div>

                <!-- Button (Double) -->
                <div class="field">
                  <div class="level-right control">
                    <button
                      id="doublebutton-0"
                      name="doublebutton-0"
                      class="button is-light"
                      @click="onClear()"
                    >Clear</button>&nbsp;
                    <button
                      id="doublebutton2-0"
                      name="doublebutton2-0"
                      class="button is-primary"
                    >Next</button>
                  </div>
                </div>
              </fieldset>
            </form>
          </div>
        </div>
      </div>
    </section>
    <Loading :isLoading="isLoading" />
  </div>
</template>

<script>
import Loading from "@/components/Loading";
export default {
  components: {
    Loading
  },
  data() {
    return {
      loc: [],
      isFetching: false,
      isLoading: false,
      ip: "",
      name: "",
      age: "",
      gender: "Male",
      location: "",
      position: []
    };
  },
  async created() {
    this.isLoading = true;
    try {
      var publicIp = require("public-ip");
      this.ip = await publicIp.v4();
    } catch (err) {
      if (err) {
        this.$buefy.toast.open({
          duration: 2000,
          message: err,
          position: "is-bottom",
          type: "is-danger"
        });
      } else {
        this.$buefy.toast.open({
          duration: 2000,
          message: `<b>Error occurred :</b> Please check your Network Connection or Contact Admin.`,
          position: "is-bottom",
          type: "is-danger"
        });
      }
    } finally {
      this.isLoading = false;
    }
    // this.getData();
  },
  mounted() {},
  computed: {
    filteredDataObj() {
      let self = this;
      return self.loc.filter(option => {
        return (
          option.title
            .toString()
            .toLowerCase()
            .indexOf(self.location.toLowerCase()) >= 0 &&
          option.position != undefined
        );
      });
    }
  },
  methods: {
    async onSubmit() {
      let context = this;
      try {
        context.isLoading = true;
        await context.getGeo(context.location.toLowerCase());
        let Location = {
          Country: context.location,
          IP: context.ip || "",
          Latitude: context.position[0] || 0,
          Longitude: context.position[1] || 0
        };
        localStorage.setItem("age", context.age);
        localStorage.setItem("name", context.name);
        localStorage.setItem("gender", context.gender);
        localStorage.setItem("location", JSON.stringify(Location));
        this.$router.push("/questions");
      } catch (err) {
        if (err) {
          context.$buefy.toast.open({
            duration: 2000,
            message: err,
            position: "is-bottom",
            type: "is-danger"
          });
        } else {
          context.$buefy.toast.open({
            duration: 2000,
            message: `<b>Error occurred :</b> Please check your Network Connection or Contact Admin.`,
            position: "is-bottom",
            type: "is-danger"
          });
        }
        console.log("Error", err);
      } finally {
        setTimeout(function() {
          context.isLoading = false;
        }, 2000);
      }
    },
    getGeo(location) {
      let self = this;
      self.loc.filter(option => {
        if (
          option.title.toLowerCase() == location &&
          option != undefined &&
          option.position != undefined
        ) {
          // alert(JSON.stringify(option));
          self.position = option.position;
        }
      });
    },
    onClear() {
      this.name = "";
      this.age = "";
      this.gender = "Male";
      this.location = "";
      this.position = [];
    },
    async getData() {
      let context = this;
      if (context.location.length >= 3) {
        try {
          context.isFetching = true;
          let url =
            "https://places.sit.ls.hereapi.com/places/v1/autosuggest?at=0,0&apiKey=LlQuEoVbt8RG3rvrIQogDyLkTKXjVpXStLtc9VRzJJg&q=" +
            context.location;
          const response = await context.$axios.$get(url);
          context.loc = response.results;
          //   console.log(response);
        } catch (err) {
          if (err) {
            context.$buefy.toast.open({
              duration: 2000,
              message: err,
              position: "is-bottom",
              type: "is-danger"
            });
          } else {
            context.$buefy.toast.open({
              duration: 2000,
              message: `<b>Error occurred :</b> Please check your Network Connection or Contact Admin.`,
              position: "is-bottom",
              type: "is-danger"
            });
          }
          console.log("Error", err);
        } finally {
          setTimeout(function() {
            context.isFetching = false;
          }, 2000);
        }
      }
    }
  }
};
</script>

<style>
</style>