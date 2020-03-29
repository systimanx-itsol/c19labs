<template>
  <div>
    <section class="hero">
      <div class="hero-body">
        <div class="container has-text-centered">
          <form @submit.prevent="onSubmit" class="form-horizontal">
            <fieldset>
              <h1 class="title is-spaced">Screener</h1>
              <h2
                class="subtitle"
              >1) In the past 14 days, have you had contact (of more than 15 minutes, at less than 6 feet distance) with someone who has a confirmed case of Corona Virus (Covid-19)?</h2>
              <div class="buttons is-centered">
                <b-field>
                  <b-radio-button v-model="two" native-value="YES" type="is-primary" required>
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button v-model="two" native-value="NO" type="is-success" required>
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
              </div>
              <h2
                class="subtitle"
              >2) Within the past 14 days, have you travelled or transited through any of the country?</h2>
              <div v-if="locationYes" class="buttons is-centered">
                <div class="control">
                  <b-autocomplete
                    v-model="three"
                    placeholder="Enter your location"
                    :loading="isFetching"
                    @typing="getData"
                    :data="filteredDataObj"
                    field="title"
                    @select="option => selected = option"
                    :required="locationYes"
                  ></b-autocomplete>
                  <p class="help">Eg: Madurai</p>
                </div>
              </div>
              <div v-else class="buttons is-centered">
                <b-field>
                  <b-radio-button
                    v-model="locationYes"
                    :native-value="true"
                    type="is-primary"
                    required
                  >
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button
                    v-model="locationYes"
                    :native-value="false"
                    type="is-success"
                    required
                  >
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
              </div>
              <h2 class="subtitle">3) Do you currently have any of the following symptoms?</h2>
              <div class="buttons is-centered">
                <div class="block">
                  <b-checkbox
                    v-model="four"
                    native-value="Fever"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Fever</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Fatigue"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Fatigue</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Coughing"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Coughing</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Sneezing"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Sneezing</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Shortness of breath"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Shortness of breath</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Aches & Pains"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Aches & Pains</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Runny or Stuffy Nose"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Runny or Stuffy Nose</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    native-value="Sore throat"
                    :disabled="isDisable"
                    :required="isRequired"
                  >Sore throat</b-checkbox>
                  <b-checkbox v-model="four" native-value="Diarrhea" :disabled="isDisable">Diarrhea</b-checkbox>
                  <b-checkbox
                    v-model="four"
                    @input="change"
                    native-value="None of these"
                    :required="isRequired"
                  >None of these</b-checkbox>
                </div>
              </div>
              <h2
                class="subtitle"
              >4) In General, do you currently have any health conditions that you manage (such as Diabetes, asthma, or high blood pressure)?</h2>
              <div class="buttons is-centered">
                <b-field>
                  <b-radio-button v-model="five" native-value="YES" type="is-primary" required>
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button v-model="five" native-value="NO" type="is-success" required>
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
              </div>
              <h2
                class="subtitle"
              >5) Do you work in any of the following: Clinic, hospital, Nursing home, or senior care facility?</h2>
              <div class="buttons is-centered">
                <b-field>
                  <b-radio-button v-model="six" native-value="YES" type="is-primary" required>
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button v-model="six" native-value="NO" type="is-success" required>
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
              </div>
              <h2
                class="subtitle"
              >6) Even if you are not a doctor or nurse, if you work in one of these nearby locations! please select “YES”</h2>
              <div class="buttons is-centered">
                <b-field>
                  <b-radio-button v-model="seven" native-value="YES" type="is-primary" required>
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button v-model="seven" native-value="NO" type="is-success" required>
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
              </div>
              <h2 v-if="checkGender" class="subtitle">7) Are you currently Pregnant?</h2>
              <div v-if="checkGender" class="buttons is-centered">
                <b-field>
                  <b-radio-button
                    v-model="eight"
                    native-value="YES"
                    type="is-primary"
                    :required="checkGender"
                  >
                    <b-icon icon="close"></b-icon>
                    <span>YES</span>
                  </b-radio-button>

                  <b-radio-button
                    v-model="eight"
                    native-value="NO"
                    type="is-success"
                    :required="checkGender"
                  >
                    <b-icon icon="check"></b-icon>
                    <span>NO</span>
                  </b-radio-button>
                </b-field>
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
                  >Submit</button>
                </div>
              </div>
            </fieldset>
          </form>
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container has-text-centered">
        <div class="block">
          <img src="/placeholder/icons/unicorn.svg" alt />
        </div>
        <p>Our mission is not to outsell Hooli with a product like their latest Box 3. We are not in it for the money - we are in it to make the whole world decentralized. To give you control over your data. To change by the internet as we know it by integrating a very important feature into it - freedom.</p>
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
      two: "NO",
      three: "",
      four: [],
      five: "NO",
      six: "NO",
      seven: "NO",
      eight: "NO",
      isDisable: false,
      locationYes: false,
      loc: [],
      isFetching: false,
      isLoading: false,
      checkGender: false
    };
  },
  async created() {
    // console.log("D:", localStorage.getItem("gender"));
    // alert("dfsdf");
    await this.checkGen();
  },
  computed: {
    // isDisable() {
    //   return JSON.stringify(this.four).includes("None of these");
    // },
    isRequired() {
      return !this.four.length > 0;
    },
    filteredDataObj() {
      let self = this;
      return self.loc.filter(option => {
        return (
          option.title
            .toString()
            .toLowerCase()
            .indexOf(self.three.toLowerCase()) >= 0
        );
      });
    }
  },
  methods: {
    infectionCheck() {
      let context = this;
      let test =
        context.two +
        context.five +
        context.six +
        context.seven +
        context.eight;
      if (test.includes("YES")) {
        localStorage.setItem("infected", 1);
      }
      if (context.three != "") {
        localStorage.setItem("infected", 1);
      }
      if (!JSON.stringify(context.four).includes("None of these")) {
        localStorage.setItem("infected", 1);
      }
      return localStorage.getItem("infected");
    },
    async onSubmit() {
      let context = this;
      let infected = await context.infectionCheck();
      const positive=`<b><h2>If you have a fever or respiratory symptoms:</h2></b><br/>
Self-quarantine yourself and ring your nearest emergency department to arrange for testing and appropriate care.<br/>
<u>If you test positive</u> - You will receive care at home or in hospital depending on the severity of your illness<br/>
<u>If you test negative</u> - If you were in self-quarantine continue to self-quarantine for the remainder of the 14 days. If you are a casual contact, continue to monitor yourself for the remainder of the 14 days.
`;
const negative=`You do not need to self-quarantine or be tested for COVID-19.<br/>If you are unwell with any other illness your doctor will assess and manage you in the normal way.`;
      try {
        context.isLoading = true;
        let requestData = {
          Age: localStorage.getItem("age") || 0,
          Answers: {
            "1": localStorage.getItem("1") || "",
            "2": context.two || "",
            "3": context.three || "",
            "4": context.four.join(",") || "",
            "5": context.five || "",
            "6": context.six || "",
            "7": context.seven || "",
            "8": context.eight || ""
          },
          Gender: localStorage.getItem("gender") || "",
          Location: JSON.parse(localStorage.getItem("location")) || {
            Country: "",
            IP: "127.0.0.1",
            Latitude: 0,
            Longitude: 0
          },
          Name: localStorage.getItem("name") || ""
        };
        let url =
          "https://cors-anywhere.herokuapp.com/https://c5ythps5hf.execute-api.ap-south-1.amazonaws.com/api/corona";
        // context.$axios.setHeader("Access-Control-Allow-Origin", "*");
        // context.$axios.setHeader(
        //   "Access-Control-Allow-Methods",
        //   "GET, POST, PUT, DELETE, OPTIONS"
        // );
        // context.$axios.setHeader(
        //   "Access-Control-Allow-Headers",
        //   "Content-Type"
        // );
        const response = await context.$axios.$post(url, requestData);
        console.log(response);
        if (response.status == 201) {
          const notif = this.$buefy.notification.open({
            duration: 999999,
            message: infected?positive:negative,
            position: "is-bottom",
            type: infected?"is-danger":"is-success",
            hasIcon: true
          });
          notif.$on("close", () => {
            localStorage.clear();
            this.$router.push("/");
          });
        }
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
    checkGen() {
      // console.log(localStorage.getItem("gender").includes("Male"));
      if (localStorage.getItem("gender").includes("Male")) {
        this.checkGender = false;
      } else {
        this.checkGender = true;
      }
    },
    change() {
      this.isDisable = !this.isDisable;
      if (this.isDisable) this.four = ["None of these"];
    },
    setAnswer(label, value) {
      //   console.log(data);
      localStorage.setItem(label, value);
      this.$router.push("/questions/2");
    },
    async getData() {
      let context = this;
      if (context.three.length >= 3) {
        try {
          context.isFetching = true;
          let url =
            "https://places.sit.ls.hereapi.com/places/v1/autosuggest?at=0,0&apiKey=LlQuEoVbt8RG3rvrIQogDyLkTKXjVpXStLtc9VRzJJg&q=" +
            context.three;
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
