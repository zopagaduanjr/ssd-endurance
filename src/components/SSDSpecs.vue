<template>
  <form>
      <label>Total Host Writes (GB):</label>
      <input type="number" min="0" required v-model="total_host_writes_gb">
      <label>Date Used:</label>
      <input type="date" min="1991-01-02" required v-model="date_used">
      <label>Endurance Rating (TB):</label>
      <input type="number" min="0" required v-model="endurance_tb">
  </form>
  <p style="text-indent: 0px;">average writes per day: <b>{{avg_writes_day.toFixed(2)}}</b> GBW / day</p>
  <p style="text-indent: 0px;">average writes per month: <b> {{avg_writes_month.toFixed(2)}}</b> GBW / month</p>
  <p style="text-indent: 0px;">average writes per year: <b>{{avg_writes_year.toFixed(2)}}</b> GBW / year</p>
  <p style="text-indent: 0px;">life expectancy based on current usage: <b>{{expected_life.toFixed(2)}}</b> years</p>
  <br/>
  <h2>How it works</h2>
  <div class="how-it-works">
    <h3>1. Download an HDD/SSD utility software </h3>
    <p>In this example I've used <a href="https://crystalmark.info/en/software/crystaldiskinfo/" target="_"> CrystalDiskInfo</a> which monitors my disk drive status.</p>
    <img :src=step_1> 
    <h3>2. Identify current Total Hosts Writes </h3>
    <p> The drive Intel SSD has a Total Host Writes of <i>17521 GB.</i></p>
    <h3>3. Identify Usage date of SSD </h3>
    <p> Identify when was the SSD first used. In my case it was installed and functioning as OS drive on <i>August 18, 2019.</i></p>
    <h3>4. Identify SSD Endurance Rating </h3>
    <p> Examine the drive's spreadsheet and search for <b> Endurance Rating. </b> 
    In this example I google searched for <i>Intel 660p 1 TB Spreadsheet.</i></p>
    <img :src=step_4a> 
    <p>Accessed the first result and located <i>Endurance Rating.</i>  </p>
    <img :src=step_4b> 
    
    <h3>5. Discover your SSD life expectancy based on current usage 🎉 </h3>
    <p> As of april 2021, my SSD's expected life is around <i>18 years.</i></p>


  </div>
  <small>made with vue.<br/><a href="https://github.com/zopagaduanjr" target="_">2021</a></small>
</template>

<script>
import moment from 'moment';

export default {
  
  data(){
    return{
      step_1: require('@/assets/step_1.jpg'),
      step_4a: require('@/assets/step_4a.png'),
      step_4b: require('@/assets/step_4b.png'),
      total_host_writes_gb: 17521,
      date_used: '2019-08-18',
      endurance_tb: 200,
      date_now: moment(String(new Date())).format('YYYY-MM-DD'),
    }
  },
  computed: {
    months: function(){
      var float_months =  moment(this.date_now).diff(moment(this.date_used), 'months', true);
      return Math.ceil(float_months)
    },
    
    avg_writes_month: function(){
      return this.total_host_writes_gb/this.months
    },
    avg_writes_day: function(){
      return this.avg_writes_month/30
    },
    avg_writes_year: function(){
      return this.avg_writes_day*365
    },
    expected_life: function(){
      return (this.endurance_tb*1000)/this.avg_writes_year
    }
  }
}
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label{
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input{
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
.how-it-works{
    max-width: 720px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
    line-height: 1.6;
}
img {
  width: 100%;
  height: auto;
}
small{
  padding-bottom: 15px;
}
p{
  text-indent: 50px;
}
</style>