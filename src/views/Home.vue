<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div id="testcase">
      <section id="content">
        <h2>{{ student.firstName }} {{ student.lastName }}</h2>
        <button v-on:click="renderPDF()">Make PDF</button>
      </section>
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
      message: "Welcome to the PDF Renderer!!",
      student: {
        firstName: "Kathleen",
        lastName: "Tresslar"
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

      doc.text(`First name: ${this.student.firstName}`, 20, 20);
      doc.save("resume.pdf");
    }
  }
};
</script>
