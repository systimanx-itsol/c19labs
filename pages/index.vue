<template>
  <div>
    <section class="hero">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h1 class="title is-spaced">BASIC SURVEY</h1>
          <h2
            class="subtitle"
          >Are You Currently Experiencing Severe Symptoms, Such As Severe Cough, Severe Shortness of Breath, Severe Fever or Other Concerning Symptoms?</h2>
          <div class="buttons is-centered">
            <a class="control button is-primary" @click="send('YES')">YES</a>
            <a class="control button is-light" @click="send('NO')">NO</a>
          </div>
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
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    send(data) {
      // console.log(data);
      if (data == "YES") {
        localStorage.setItem("infected", 1);
      }
      localStorage.setItem("1", data);
      this.$router.push("/profile");
    }
  }
};
</script>
