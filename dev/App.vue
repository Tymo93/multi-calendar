<template>
  <div>
    <div class="current-date">{{ getTextDate }}</div>
      <vue-good-table
      :columns="getTableCols"
      :rows="getTableRows">
      <div slot="table-actions">
        <button @click="prevMonth" class="back">Back</button>
        <button @click="nextMonth" class="forward">Forward</button>
      </div>
  </vue-good-table>
  </div>
</template>

<script>

export default {
  name: 'multi-calendar',
  data(){
    const date = new Date();
    return {
      tableCols: [],
      tableRows: [],
      months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
      currentMonth: date.getMonth(),
      currentYear: date.getFullYear(),
      listings: [{property:{id:"12345",property_name:"Villa"},photos:["/","/","/"],calendar_status:[{date:"03/04/2019",price:2400,minimum_nights:2,status:"half_booked",check_in_booking:12345},{date:"03/05/2019",price:2400,minimum_nights:2,status:"half_booked",booking:12345},{date:"03/06/2019",price:2400,minimum_nights:2,status:"half_booked",check_out_booking:12345},{date:"03/20/2019",price:2400,minimum_nights:2,status:"fully_booked",check_in_booking:12346},{date:"03/21/2019",price:2400,minimum_nights:2,status:"fully_booked",booking:12346},{date:"03/22/2019",price:2400,minimum_nights:2,status:"fully_booked",check_out_booking:12346},{date:"04/13/2019",price:2400,minimum_nights:2,status:"cancelled",check_in_booking:12347},{date:"04/14/2019",price:2400,minimum_nights:2,status:"cancelled",booking:12347},{date:"04/15/2019",price:2400,minimum_nights:2,status:"cancelled",booking:12347},{date:"04/16/2019",price:2400,minimum_nights:2,status:"cancelled",booking:12347},{date:"04/17/2019",price:2400,minimum_nights:2,status:"cancelled",check_out_booking:12347},{date:"03/06/2019",price:2400,minimum_nights:2,status:"payment_pending",check_in_booking:12345},{date:"03/07/2019",price:2400,minimum_nights:2,status:"payment_pending",booking:12345},{date:"03/08/2019",price:2400,minimum_nights:2,status:"payment_pending",check_out_booking:12345}]}],
      // rows: [
      //   { id:1, name:"John", age: 20, createdAt: '201-10-31:9: 35 am',score: 0.03343 },
      //   { id:2, name:"Jane", age: 24, createdAt: '2011-10-31', score: 0.03343 },
      //   { id:3, name:"Susan", age: 16, createdAt: '2011-10-30', score: 0.03343 },
      //   { id:4, name:"Chris", age: 55, createdAt: '2011-10-11', score: 0.03343 },
      //   { id:5, name:"Dan", age: 40, createdAt: '2011-10-21', score: 0.03343 },
      //   { id:6, name:"John", age: 20, createdAt: '2011-10-31', score: 0.03343 },
      // ],
    };
  },
  methods: {
    getCurrMonth() {
      this.tableCols = [];      
      const days = ['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'];
      const monthDays = new Date(this.currentYear, this.currentMonth +1, 0).getDate();
      for (let x = 0; x < monthDays; x++) {
        const createDate = new Date(this.currentYear, this.currentMonth, x+1);
        this.tableCols[x] = {
          label: `${days[createDate.getDay()]} ${x+1}`,
          field: `${createDate.getTime()}`,
          html: true
        }
      }
    },
    prevMonth() {
      if (this.currentMonth == 0) {
        this.currentMonth = 11;
        this.currentYear = this.currentYear - 1;
      } else {
        this.currentMonth = this.currentMonth - 1;
      }
      this.getCurrMonth();
      this.getListings();
    },
    nextMonth() {
      if (this.currentMonth == 11) {
        this.currentMonth = 0;
        this.currentYear = this.currentYear + 1;
      } else {
        this.currentMonth = this.currentMonth + 1;
      }
      this.getCurrMonth();
      this.getListings();
    },
    getListings() {
      this.tableRows = [];
      for (let y = 0; y < this.listings.length; y++) {
        const bookingDates = {
            id: y
        }
        for (let z = 0; z < this.listings[y].calendar_status.length; z++) {
          this.listings[y].calendar_status[z].date = new Date(this.listings[y].calendar_status[z].date).getTime();
          const status = `${Object.keys(this.listings[y].calendar_status[z])[Object.keys(this.listings[y].calendar_status[z]).length -1]} ${this.listings[y].calendar_status[z].status}`;

          bookingDates[this.listings[y].calendar_status[z].date] = bookingDates[this.listings[y].calendar_status[z].date] !== undefined ? `${bookingDates[this.listings[y].calendar_status[z].date]}<div class="${status}"></div>` : `<div class="${status}"></div>`;
          
          this.tableRows[y] = bookingDates;
        }
      }
    }
  },
  computed: {
    getTableCols() {
      return this.tableCols;
    },
    getTextDate() {
      return `${this.months[this.currentMonth]} ${this.currentYear}`;
    },
    getTableRows() {
      return this.tableRows;
    }
  },
  created() {
    this.getCurrMonth();
    this.getListings();
  }
};
</script>

<style lang="css">
  table.vgt-table tr td {
    padding: 0;
  }

  table.vgt-table tr th {
    width: 60px;
    height: 50px;
  }

  table.vgt-table tr td > span > div {
    display: inline-block;
    width: 100%;
    height: 50px;
  }

  table.vgt-table tr td > span > div.check_out_booking, table.vgt-table tr td > span > div.check_in_booking {
    width: 50%;
  }

  .half_booked {
    background-color: #56ffd9;
  }

  .fully_booked {
    background-color: #3fcf8e;
  }

  .cancelled {
    background-color: #ff7b7b;
  }

  .payment_pending {
    background-color: #ffa27b;
  }
</style>

