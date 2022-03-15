<template>
  <div>
    <main id="main">
      <section>
        <h2>My lattest proojects</h2>
        <div class="list_projets" id="listeProjet">
          <div v-for="realisation in listeProjets" :key="realisation.id"
               data-aos="fade-up"
               data-aos-easing="ease-out-cubic"
               data-aos-delay="150"
               data-aos-duration="1000"
               class="projet" id="projet" :id="realisation.acf.id">
            <a class="haut-projet" :href="realisation.acf.url" target="_blank" rel="noopener">
              <div :style='{ backgroundImage: `url(${realisation.acf.imagetext})`}' class="wrappers"></div>
            </a>
            <div class="bas_projet" :style='{ background: `linear-gradient(280deg, ${realisation.acf.color}55, #5a3b4022 )` }'>
              <i class="projects_type">{{ realisation.acf.type }}</i>
              <br>
              <p class="projects_title">{{ realisation.acf.name }}</p>
              <br>
              <p class="projects_date">{{ realisation.acf.date }}</p>
              <br>
              <p class="projects_tools">{{ realisation.acf.languages }}</p>
              <br>
              <p class="projects_text">{{ realisation.acf.description}}</p>
            </div>
          </div>
        </div>
      </section>
      <section class="sections_bas">
        <div class="left">
          <h2>My skkills</h2>
          <br>
          <b>Front-End development</b>
          <p>HTML - CSS - VueJS - Java Script - BDD - WordPress</p>
          <br>
          <b>Web design</b>
          <p>UI - UX - Photoshop - Adobe XD - Illustrator </p>
          <br>
          <b>Filmmaking</b>
          <p>Première pro - After effect - Compositing - Filming - Cinema4D</p>
        </div>
        <div class="text-right">
          <h2>My cuursus</h2>
          <br>
          <b>DUT Métiers du Multimédia et de l'Internet</b>
          <p>2020 - 2022</p>
          <br>
          <b>Learn by myself</b>
          <p>2018 - 2022</p>
        </div>
      </section>
      <section class="sections_bas" style="border-top: none;">
        <div class="left">
          <h2>Get inn touch</h2>
          <br>
          <form class="contact-form" @submit="checkForm">
            <div class="haut">
              <input v-model="form.name" type="text" id="name" name="name" placeholder="Your name">
              <input v-model="form.email"  type="email" id="mail" name="mail" placeholder="Your email" required>
            </div>
            <textarea v-model="form.message" name="message" id="msg" placeholder="Your message" required></textarea>
            <button class="button" type="submit" value="Submit" name="submit">Sennd it</button>
          </form>
        </div>
        <div class="text-right">
          <h2>Finnd me</h2>
          <br>
          <b><a href="https://www.instagram.com/spielprod/?hl=en" target="_blank" rel="noopener">🌈 Instagram</a></b>
          <br>
          <br>
          <b><a href="https://www.linkedin.com/in/thomasjeu/" target="_blank" rel="noopener">👨‍🎓 LinkedIn</a></b>
          <br>
          <br>
          <b><a href="mailto: contact@thomasjeu.fr">📧 contact@thomasjeu.fr</a></b>
          <br>
          <br>
          <b><a href="static/Curriculum_Vitae_Thomas_JEU.pdf" download="">📜 download my resume</a></b>
        </div>
      </section>
      <footer>
        <p style="color: #fd5530; font-weight: bold;">Thomas Jeu <span>2022</span></p>
        <a href="mailto: contact@thomasjeu.fr" style="color: #fd5530; font-weight: bold;">contact@thomasjeu.fr</a>
      </footer>
    </main>
  </div>
</template>

<script>
import axios from 'axios';

import param from '@/param/param'

export default {
  name: 'Accueil',
  data () {
    return {
      liste:[],
      form :{
        name: null,
        email: null,
        message: null
      }
    }
  },

  methods: {
    validEmail: function (email) {
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    checkForm: function (e) {
      e.preventDefault();

        var bodyFormData = new FormData();
        bodyFormData.set("your-name", this.form.name)
        bodyFormData.set("your-mail", this.form.email);
        bodyFormData.set("your-msg", this.form.message);

        axios({
          method: "post",
          url:
            "https://thomasjeu.fr/wp-json/contact-form-7/v1/contact-forms/28833/feedback",
          data: bodyFormData,
          config: { headers: { "Content-Type": "multipart/form-data" } },
        })
          .then(function (response) {
            console.log(response);
            window.location.href = "https://thomasjeu.fr";
          })
          .catch(function (response) {
            console.log(response);
          });
    },
  },

  computed:{
    listeProjets: function(){
      function compare(a, b) {
        if (a.acf.id < b.acf.id) return -1;
        if (a.acf.id > b.acf.id) return 1;
        return 0;
      }
      return this.liste.sort(compare);
    }
  },

  created(){
    axios.get(param.host+"realisation?per_page=50")
      .then(response=>{
        this.liste = response.data;
      })
      .catch(error => console.log(error))
  }
};
</script>

<style scoped>
.bas_projet{
  border-radius: 0 0 20px 0;
}
.projet:hover .haut-projet{
  border: 1px solid #fd5530;
}
</style>
