<template>
  <div class="section-container section-left col-6 pt-3 pb-3">
    <div class="text-left subtitle-section">
      <p>Enter Student Details</p>
    </div>
    <form onsubmit="return false" class="form-box text-left">
    <div class="form-element-container" 
         v-for="item in formInputElements"
         :key="item.formValueKey" 
    >
        <div class="form-element font-icon">
        <label class="form-label"> {{item.labelName}} </label>
        <input class="form-input" 
               type="text" 
               id="name" 
               :value="formData[item.formValueKey]"
               @input="changeFormData({ [item.formValueKey]: $event.target.value })"
        />
        <i :class="item.iconClass"></i>
        </div>
    </div>
      <div class="d-flex">
        <button class="btn-generate" @click="generateCard()">
          Generate Card
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
export default {
  name: 'UserInput',
  props: {
      formData:  Object
  },
  data() {
     return{
         formInputElements: [
             {
                labelName: 'Enter Name',
                formValueKey: 'fullName',
                placeholder: 'Enter full name',
                iconClass: 'fa fa-user fa-lg'    
             },
             {
                labelName: 'Enter College Name',
                formValueKey: 'collegeName',
                placeholder: 'Enter college name',
                iconClass: 'fa fa-university'    
             },
             {
                labelName: 'Enter Location',
                formValueKey: 'locationName',
                placeholder: 'Enter location',
                iconClass: 'fa fa-address-book'    
             }
         ]
     }; 
  },
  methods: {
      changeFormData(value) {
          this.$emit("changeFormData", value);
      },
      async generateCard() {
        console.log("Button clicked");
        const canvas = await html2canvas(document.getElementById("generatedIdCard"));
        canvas.style.display = "none";
        document.body.appendChild(canvas);
        const image = canvas.toDataURL("image/png").replace("image/png", "image/octet-stream");
        const a = document.createElement("a");
        a.setAttribute("download", `collegeidcard.png`);
        a.setAttribute("href", image);
        a.click();
      }
  }
}
</script>