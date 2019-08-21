<template>
  <div class="home">
        <div class="resume">
          <h1>{{ student.firstName }} {{ student.lastName }}</h1>
          <div class="contact">
          <div>
            {{ student.email }}
          </div>
          <div>
            {{ student.phoneNumber }}
          </div>
          <div>
            {{ student.linkedinURL }}
          </div>
          <div>
            {{ student.twitterHandle }}
          </div>
          <div>
            {{ student.blog }}
          </div>
          <div>
            {{ student.resumeURL }}
          </div>
          <div>
            {{ student.gitHub }}
          </div>
          <div class="content">
          <div>
            {{ student.photo }}
          </div>
          <div>
            {{ student.bio }}
          </div>

          <h1>Experience:</h1>
          <div v-for="experience in student.experience">
            <h2>
              {{ experience.jobTitle }}
            </h2>
            <div>
              <strong>{{ experience.company }}</strong> {{ experience.startDate }} - {{ experience.endDate }}
            </div>
            <div>
              {{ experience.details }}
            </div>
          </div>
          <h1>Education:</h1>
          <div v-for="education in student.education">
            <strong>{{ education.university }}</strong> {{ education.startDate }} - {{ education.endDate }}
            <div>
              {{ education.degree }}
            </div>
            <div>
              {{ education.details }}
            </div>
          </div>
          <h1>Skills:</h1>
          <div v-for="skill in student.skills">
            {{ skill }} 
          </div>
          <h1>Capstone:</h1>
          <div v-for="capstone in student.capstone">
            {{ capstone }}
          </div>
          </div>
        </div>
        </div>
      <button v-on:click="renderPDF()">Make PDF</button>
    </div>
  </div>
</template>

<style></style>
<script>
import * as jsPDF from "jspdf";
import axios from "axios";


export default {
  data: function() {
    return {
      message: "welcome to the pdf renderer!",
      student: {
        firstName: "Peter",
        lastName: "Jang",
        email: "peter@email.com",
        phoneNumber: "123-456-7890",
        bio: "...",
        linkedinURL: "...@linkedin.com",
        twitterHandle: "@PJCoolIce",
        blog: "...@blog.com",
        resumeURL: "...@resume.com",
        gitHub: "...@gitHub.com",
        photo: "...",
        experience: [
          {
            startDate: "1/2/1802",
            endDate: "8/19/1901",
            jobTitle: "Junior Headmaster",
            company: "Actualize",
            details: "... .... ..."
          },
          {
            startDate: "1/1/1901",
            endDate: "1/2/1902",
            jobTitle: "Headmaster",
            company: "Hogwarts",
            details: "... .... ..."
          },
          {
            startDate: "1/2/1902",
            endDate: "8/19/2019",
            jobTitle: "Headmaster",
            company: "Actualize",
            details: "... .... ..."
          }
        ],
        education: [
          {
            startDate: "9/1/1894",
            endDate: "1/1/1901",
            degree: "Muggle Studies",
            university: "Hogwarts",
            details: "... .... ..."
          }
        ],
        skills: ["Ruby", "HTML", "Potions", "Strong Earth Magnets", "Arithmancy"],
        capstone: {
          name: "Magical Applications of Computer Programming",
          description: "...",
          url: "capstone@hogwarts.edu",
          screenshot: "..."
        }
      }
    };
  },
  created: function() {
    // axios.get("/api/students/" + this.$route.params.id).then(response => {
    //   this.student = response.data;
    //   console.log(this.student);
    // });
  },
  methods: {
    renderPDF: function() {
      var doc = new jsPDF();
      doc.addFont("ArialMS", "Arial", "normal");
      doc.setFont("Arial");
      doc.setFontSize(14);

      doc.fromHTML(`<b>Full name:</b> ${this.student.firstName} ${this.student.lastName}`, 10, 10);
      doc.text(`Contact info: ${this.student.email} ${this.student.phoneNumber}`, 10, 20);
      doc.text(`Social: ${this.student.twitterHandle} ${this.student.linkedinURL} ${this.student.blog}`, 10, 30);
      doc.text(`Bio: ${this.student.bio}`, 10, 40);
      doc.line(10, 50, 200, 50, "s");
      doc.text(`Experience: `, 10, 60);
      var experiences = this.student.experience;
      var x = 10;
      var y = 70;
      experiences.forEach(function(experience) {
        doc.text(`${experience.jobTitle}, ${experience.company}, ${experience.startDate}- ${experience.endDate}`, x, y);
        y += 10;
      });

      y += 10;
      doc.line(10, y, 200, y, "s");
      y += 10;

      doc.text(`Education:`, x, y);
      y += 10;
      var education = this.student.education;
      education.forEach(function(school) {
        doc.text(`${school.university}, ${school.startDate} - ${school.endDate}, degree in ${school.degree}`, x, y);
        y += 10;
      });
      y += 10;

      doc.line(10, y, 200, y, "s");
      y += 10;

      doc.text(`Capstone project: ${this.student.capstone.name}`, 10, y);
      y += 10;
      doc.text(`${this.student.capstone.description}`, 10, y);
      y += 10;
      doc.text(`${this.student.capstone.url}`, 10, y);
      y += 10;
      doc.line(10, y, 200, y, "s");
      y += 10;
      doc.text(`Skills:`, 10, y);
      y += 10;
      var skills = this.student.skills;
      skills.forEach(function(skill) {
        doc.text(`${skill}`, 10, y);
        y += 10;
      });

      doc.save("resume.pdf");
    }
  },
};
</script>
