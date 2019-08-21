<template>
  <div class="home">
    <div class="index">
      <div v-for="student in students">
        <div class="button" style="text-align: center; margin-bottom: 50px">
          <a class="waves-effect waves-light btn" v-on:click="getInfo(student)">
            <i class="material-icons right"></i>
            {{ student.first_name }}
          </a>
          <!-- <button v-on:click="getInfo(student)">{{ student.first_name }}</button> -->
        </div>
      </div>
    </div>
      <div class="resume" style="text-align: center">
        <h1>{{ student.first_name }} {{ student.last_name }}</h1>


        <div class="contact" v-if="student">
          <div>
            {{ student.email }}
          </div>
          <div>
            {{ student.phone_number }}
          </div>
          <div>
            {{ student.linkedin_url }}
          </div>
          <div>
            {{ student.twitter_handle }}
          </div>
          <div>
            {{ student.blog }}
          </div>
          <div>
            {{ student.resume_url }}
          </div>
          <div>
            {{ student.github_url }}
          </div>
          <div class="content">

          <div>
            {{ student.photo_url }}
          </div>
          <div>
            {{ student.short_bio }}
          </div>
          <div v-if="student">
          <h1>Experience:</h1>
        </div>
          <div v-for="experience in student.experiences">
            <h2>
              {{ experience.job_title }}
            </h2>

            <div>
              {{ student.photo_url }}
            </div>
            <div>
              {{ student.short_bio }}
            </div>

          </div>
          <div v-if="student">
          <h1>Education:</h1>
        </div>
          <div v-for="education in student.educations">
            <strong>{{ education.university_name }}</strong> {{ education.start_date }} - {{ education.end_date }}
            <div>
              {{ education.degree }}

            <h1>Experience:</h1>
            <div v-for="experience in student.experiences">
              <h2>
                {{ experience.job_title }}
              </h2>
              <div>
                <strong>{{ experience.company_name }}</strong>
                {{ experience.start_date }} - {{ experience.end_date }}
              </div>
              <div>
                {{ experience.details }}
              </div>

            </div>
            <h1>Education:</h1>
            <div v-for="education in student.educations">
              <strong>{{ education.university_name }}</strong>
              {{ education.start_date }} - {{ education.end_date }}
              <div>
                {{ education.degree }}
              </div>
              <div>
                {{ education.details }}
              </div>
            </div>
            <h1>Skills:</h1>
            <div v-for="skill in student.skills">
              {{ skill.skill_name }}
            </div>
            <h1>Capstone:</h1>
            <div v-for="capstone in student.capstones">
              {{ capstone.name }}
              {{ capstone.description }}
            </div>
          </div>

          <div v-if="student">
          <h1>Skills:</h1>
        </div>
          <div v-for="skill in student.skills">
            {{ skill.skill_name }} 
          </div>
          <div v-if="student">
          <h1>Capstone:</h1>
        </div>
          <div v-for="capstone in student.capstones">
            {{ capstone.name }}
            {{ capstone.description }}
          </div>
          </div>
        </div>

        </div>
      </div>
      <div class="button" style="text-align: center">
        <a class="waves-effect waves-light btn" v-on:click="renderPDF()">
          <i class="material-icons right"></i>
          Create PDF
        </a>
      </div>
    </div>
  </div>
</template>

<style>
#a {
  display: flex;
  justify-content: center;
}
</style>
<script>
import * as jsPDF from "jspdf";
import axios from "axios";

export default {
  data: function() {
    return {
      message: "welcome to the pdf renderer!",
      student: "",
      students: {}
    };
  },
  created: function() {
    axios.get("https://sleepy-citadel-35395.herokuapp.com/api/students").then(response => {
      this.students = response.data;
      console.log(this.students);
    });
  },
  methods: {
    getInfo: function(student) {
      axios.get("https://sleepy-citadel-35395.herokuapp.com/api/students/" + student.id).then(response => {
        this.student = response.data;
        console.log(this.student);
      });
    },

    renderPDF: function() {
      var doc = new jsPDF();
      doc.setFontSize(14);

      doc.text(`Full name: ${this.student.first_name} ${this.student.last_name}`, 10, 10);
      doc.text(`Contact info: ${this.student.email} ${this.student.phone_number}`, 10, 20);
      doc.text(
        `Social: ${this.student.twitter_handle} ${this.student.linkedin_url} ${this.student.personal_url}`,
        10,
        30
      );
      doc.text(`Bio: ${this.student.short_bio}`, 10, 40);
      doc.line(10, 50, 200, 50, "s");
      doc.text(`Experience: `, 10, 60);
      var experiences = this.student.experiences;
      var x = 10;
      var y = 70;
      experiences.forEach(function(experience) {
        doc.text(
          `${experience.job_title}, ${experience.company_name}, ${experience.start_date}- ${experience.end_date}`,
          x,
          y
        );
        y += 10;
      });

      y += 10;
      doc.line(10, y, 200, y, "s");
      y += 10;

      doc.text(`Education:`, x, y);
      y += 10;
      var education = this.student.educations;
      education.forEach(function(school) {
        doc.text(
          `${school.university_name}, ${school.start_date} - ${school.end_date}, degree in ${school.degree}`,
          x,
          y
        );
        y += 10;
      });
      y += 10;

      doc.line(10, y, 200, y, "s");
      y += 10;

      doc.text(`Capstone projects:`, 10, y);
      y += 10;
      var capstone = this.student.capstones;
      capstone.forEach(function(capstone) {
        doc.text(`${capstone.name}: ${capstone.description}`, x, y);
        y += 10;
      });
      y += 10;
      doc.line(10, y, 200, y, "s");
      y += 10;
      doc.text(`Skills:`, 10, y);
      y += 10;
      var skills = this.student.skills;
      skills.forEach(function(skill) {
        doc.text(`${skill.skill_name}`, 10, y);
        y += 10;
      });

      doc.save("resume.pdf");
    }
  }
};
</script>
