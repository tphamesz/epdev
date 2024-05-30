<template>
  <div class="form-container">
    <h1>EVERYTHING PROPERTY LTD</h1>
    <h2>TENANT'S ACQUAINTANCE FORM</h2>
    <form @submit.prevent="generatePdf">
      <div class="form-group">
        <label for="applicantName">1. APPLICANT'S NAME:</label>
        <input type="text" id="applicantName" v-model="form.applicantName" required>
      </div>
      <div class="form-group">
        <label for="address">PRESENT RESIDENTIAL ADDRESS:</label>
        <input type="text" id="address" v-model="form.address" required>
      </div>
      <div class="form-group">
        <label for="stateOfOrigin">STATE OF ORIGIN:</label>
        <input type="text" id="stateOfOrigin" v-model="form.stateOfOrigin" required>
      </div>
      <div class="form-group">
        <label for="homeTown">HOME TOWN:</label>
        <input type="text" id="homeTown" v-model="form.homeTown" required>
      </div>
      <div class="form-group">
        <label for="religion">RELIGION:</label>
        <input type="text" id="religion" v-model="form.religion" required>
      </div>
      <div class="form-group">
        <label for="placeOfWorship">PLACE OF WORSHIP:</label>
        <input type="text" id="placeOfWorship" v-model="form.placeOfWorship" required>
      </div>
      <div class="form-group">
        <label for="phoneNumber">TELEPHONE NUMBER:</label>
        <input type="text" id="phoneNumber" v-model="form.phoneNumber" required>
      </div>
      <div class="form-group">
        <label for="email">E-MAIL ADDRESS:</label>
        <input type="email" id="email" v-model="form.email" required>
      </div>
      <div class="form-group">
        <label for="maritalStatus">MARITAL STATUS:</label>
        <input type="text" id="maritalStatus" v-model="form.maritalStatus" required>
      </div>
      <div class="form-group">
        <label for="occupation">OCCUPATION/PROFESSION:</label>
        <input type="text" id="occupation" v-model="form.occupation" required>
      </div>
      <div class="form-group">
        <label for="employer">NAME & ADDRESS OF PRESENT EMPLOYER:</label>
        <input type="text" id="employer" v-model="form.employer" required>
      </div>
      <div class="form-group">
        <label for="jobTitle">JOB TITLE/DESCRIPTION:</label>
        <input type="text" id="jobTitle" v-model="form.jobTitle" required>
      </div>
      <div class="form-group">
        <label for="yearsOfEmployment">NO. OF YEARS IN EMPLOYMENT:</label>
        <input type="text" id="yearsOfEmployment" v-model="form.yearsOfEmployment" required>
      </div>
      <div class="form-group">
        <label for="signature">SIGNATURE:</label>
        <input type="file" id="signature" @change="handleFileUpload" required>
      </div>
      <button type="submit">Submit</button>
    </form>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';
import jsPDF from 'jspdf';

export default {
  data() {
    return {
      form: {
        applicantName: '',
        address: '',
        stateOfOrigin: '',
        homeTown: '',
        religion: '',
        placeOfWorship: '',
        phoneNumber: '',
        email: '',
        maritalStatus: '',
        occupation: '',
        employer: '',
        jobTitle: '',
        yearsOfEmployment: '',
      },
      signature: null,
    };
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        this.signature = e.target.result;
      };

      reader.readAsDataURL(file);
    },
    generatePdf() {
      const pdfContent = document.createElement('div');
      pdfContent.style.width = '500px';
      pdfContent.innerHTML = `
        <h1>EVERYTHING PROPERTY LTD</h1>
        <h2>TENANT'S ACQUAINTANCE FORM</h2>
        <p><strong>APPLICANT'S NAME:</strong> ${this.form.applicantName}</p>
        <p><strong>PRESENT RESIDENTIAL ADDRESS:</strong> ${this.form.address}</p>
        <p><strong>STATE OF ORIGIN:</strong> ${this.form.stateOfOrigin}</p>
        <p><strong>HOME TOWN:</strong> ${this.form.homeTown}</p>
        <p><strong>RELIGION:</strong> ${this.form.religion}</p>
        <p><strong>PLACE OF WORSHIP:</strong> ${this.form.placeOfWorship}</p>
        <p><strong>TELEPHONE NUMBER:</strong> ${this.form.phoneNumber}</p>
        <p><strong>E-MAIL ADDRESS:</strong> ${this.form.email}</p>
        <p><strong>MARITAL STATUS:</strong> ${this.form.maritalStatus}</p>
        <p><strong>OCCUPATION/PROFESSION:</strong> ${this.form.occupation}</p>
        <p><strong>NAME & ADDRESS OF PRESENT EMPLOYER:</strong> ${this.form.employer}</p>
        <p><strong>JOB TITLE/DESCRIPTION:</strong> ${this.form.jobTitle}</p>
        <p><strong>NO. OF YEARS IN EMPLOYMENT:</strong> ${this.form.yearsOfEmployment}</p>
        <p><strong>SIGNATURE:</strong></p>
        <img src="${this.signature}" alt="Signature" style="width: 200px; height: auto;">
      `;

      document.body.appendChild(pdfContent);

      html2canvas(pdfContent).then((canvas) => {
        const imgData = canvas.toDataURL('image/png');
        const pdf = new jsPDF();
        pdf.addImage(imgData, 'PNG', 0, 0);
        pdf.save('tenant-form.pdf');
        document.body.removeChild(pdfContent);
      });
    },
  },
};
</script>

<style>
.form-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.form-group {
  margin-bottom: 10px;
}
label {
  display: block;
  margin-bottom: 5px;
}
input[type="text"], input[type="email"], input[type="file"] {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}
button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
</style>
