<template>
  <div id="app">
    <ejs-grid ref="grid" :dataSource="data" :allowGrouping="true" :groupSettings="groupOptions"> 
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" textAlign="Right"></e-column>
        <e-column field="CustomerID" headerText="Customer ID"></e-column>
        <e-column field="Freight" headerText="Freight" textAlign="Right"></e-column>
        <e-column field="ShipCountry" headerText="ShipCountry"></e-column>
      </e-columns>
      <e-aggregates>
        <e-aggregate>
          <e-columns>
            <e-column type="Sum" field="Freight" :groupFooterTemplate="footerSum"></e-column>
            <!-- Uncomment below line to check custom aggregate -->
            <!-- <e-column type="Custom" field="ShipCountry" :customAggregate="generateCount" :footerTemplate="footerSum"></e-column> -->
          </e-columns>
        </e-aggregate>
        <e-aggregate>
          <e-columns>
            <e-column type="Max" field="Freight" :groupCaptionTemplate="footerMax"></e-column>
          </e-columns>
        </e-aggregate>
      </e-aggregates>
    </ejs-grid>
  </div>
</template>

<script>

import Vue from "vue";
import { GridPlugin, Aggregate, Group } from "@syncfusion/ej2-vue-grids"; 
import { data } from './dataSource';
Vue.use(GridPlugin);

export default {
  data() {
    return {
      data: data,
      footerSum: function(){
        return{
          template: Vue.component('Sum',{
            template: `<span>Sum: {{data.Sum}}</span>`,
            data(){
              return {
                data: {}
              }
            }
          })
        }
      },
      footerMax: function(){
        return{
          template: Vue.component('Max',{
            template: `<span>Max: {{data.Max}}</span>`,
            data(){
              return {
                data: {}
              }
            }
          })
        }
      },
      // Uncomment below line to check custom aggregate
      // footerCount: function(){
      //   return{
      //     template: Vue.component('Count',{
      //       template: `<span>Count: {{data.Custom}}</span>`,
      //       data(){
      //         return {
      //           data: {}
      //         }
      //       }
      //     })
      //   }
      // }
      groupOptions: {
        columns: ['CustomerID'],
        showDropArea: false
      }
    };
  },
  // Uncomment below line to check custom aggregate
  // methods: {
  //   generateCount: function(data){
  //     return data.result.filter((item)=> item.ShipCountry === 'Brazil').length;
  //   }
  // },
  provide: {
    grid: [ Aggregate, Group ] 
  },
};
</script>

<style>
@import url("https://cdn.syncfusion.com/ej2/material.css");
</style>
