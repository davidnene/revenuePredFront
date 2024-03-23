<script setup>
import { ref } from 'vue';

const month1 = ref('');
const month2 = ref('');
const month3 = ref('');
const results  = ref(null);


function sendRevenueData(){
    const revenueData = {
    'amount_1': month1.value,
    'amount_2': month2.value,
    'amount_3': month3.value
}
    fetch("http://127.0.0.1:5000/predict", {
    method: "POST",
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(revenueData)
  })
  .then(response => {
    if (!response.ok) {
      throw new Error("Network response was not ok");
      console.error(response)
    }
    return response.json();
  })
  .then(data => {
    console.log(data)
    results.value = data
    console.log(results.value)
  })
  .catch(error => {
    console.error("Error:", error);
  
  });

}

</script>

<template>
    <div class="">
        <div class="container ">
            <div class="row">
                <div class="col ">               
                    <form @submit.prevent="sendRevenueData" class="row g-3 position-absolute top-50 start-50 translate-middle ">
                        <h1>Revenue Prediction Model</h1>
                        <div class="col-md-6">
                            <label for="inputName4" class="form-label">Username</label>
                            <input type="text" class="form-control" id="inputName4">
                        </div>
                        <div class="col-md-6">
                            <label for="inputRole4" class="form-label">Role</label>
                            <input type="text" class="form-control" id="inputRole4">
                        </div>
                        <div class="col-4">
                            <label for="inputMonth1" class="form-label">Month 1 Amount</label>
                            <input type="text" class="form-control" id="inputMonth1" placeholder=0.00 v-model="month1" pattern="\d*\.?\d*" required >
                        </div>
                        <div class="col-4">
                            <label for="inputMonth2" class="form-label">Month 2 Amount</label>
                            <input type="text" class="form-control" id="inputMonth2" placeholder=0.00 v-model="month2" pattern="\d*\.?\d*" required>
                        </div>
                        <div class="col-4">
                            <label for="inputMonth3" class="form-label">Month 3 Amount</label>
                            <input type="text" class="form-control" id="inputMonth3" placeholder=0.00 v-model="month3" pattern="\d*\.?\d*" required>
                        </div>
                    
                        <div class="col-12">
                            <button type="submit" class="btn btn-primary" style="color: white;" >Predict</button>
                        </div>
                        <div>
                        <p v-if="results!==null"><b>Prediction: </b>{{ results['prediction'] }}</p>
                    </div>
                    </form>
                </div>
                <div class="col">
                    
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>