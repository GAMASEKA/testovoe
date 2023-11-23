<template>
<tr><td>{{ user.o_id }}</td> <td>{{ user.client_name }}</td><td> <div class="diet-block" v-for="diet in user.diets">{{ diet }}</div> </td><td> <div class="tarif-block" v-for="tarif in user.tariff">{{ tarif }}</div></td><td>{{ user.address }}</td><td>{{ user.phone }}</td><td>
 <div class="date-block" v-for="date in user.dates" > 
    {{ date.start_date }}
    {{ date.end_date }}
</div> 

      </td><td><div class="payment-block" >{{ user.discount }}, {{ user.order_sum }}, {{ user.order_payed }}</div></td><td>{{ user.pay_status }}</td><td style="width: 150px;">{{ user.courier_comment }}</td><td style="width: 300px;">{{ user.inner_comment }}</td></tr>
</template>

<script>
export default {
    data() {
        return {
        dayStart: "",
        dayEnd: "",
        dayOver: ""
    }
},
props: {
    user: Object
},

}



</script>

<style scoped>
tr {
    outline: 2px solid red;
}
th {
    border: 1px solid green;
}
td {
    border: 1px solid green;
}

.diet-block {
    border-bottom: 1px solid black;
}

.date-block {
    border-bottom: 1px solid black;

}
</style>