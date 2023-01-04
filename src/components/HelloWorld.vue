<template>
  <v-container>
    <v-row>
      <v-col class="text-h4 font-weight-bold">Project</v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-stepper v-model="stepVal">
            <v-stepper-header>
              <v-stepper-step :complete="e1 > 1" step="1">
                Configuration
              </v-stepper-step>

              <v-divider></v-divider>

              <v-stepper-step :complete="e1 > 2" step="2">
                Summary
              </v-stepper-step>
            </v-stepper-header>
          </v-stepper>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="stepVal==1">
      <v-col cols="12">
        <v-data-table :headers="headers" :items="items" hide-default-footer>
          <template v-slot:body.append>
            <div class="d-flex justify-space-between mt-2">
                <v-btn color="blue" text @click="addRow">+ ADD ROW</v-btn>
                <v-spacer></v-spacer>
                <v-btn class="align-right" color="blue" rounded @click="stepVal=2" dark>NEXT</v-btn>
            </div>
          </template>
          <template v-slot:body.prepend>
            <tr>
              <td>
                <v-text-field
                  class="mt-3 mb-n3"
                  outlined
                  dense
                  x-small
                  v-model="newRecord.plan_name"
                  label="Product Name"
                ></v-text-field>
              </td>
              <td>
                <v-text-field
                  class="mt-3 mb-n3"
                  outlined
                  dense
                  x-small
                  v-model="newRecord.amount"
                  label="Amount"
                ></v-text-field>
              </td>
              <td>
                <v-select
                  item-text="name"
                  item-value="name"
                  class="mt-3 mb-n3"
                  dense
                  :items="ownerList"
                  outlined
                  label="Select Job Owner"
                  v-model="newRecord.job_owner"
                ></v-select>
              </td>
              <td>
                <v-icon>mdi-trash-can</v-icon>
              </td>
            </tr>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
    <v-row v-if="stepVal==2">
      <v-col>
        <v-data-table :headers="headers2" :items="items" hide-default-footer>
          <template v-slot:body.append>
            <div class="d-flex justify-space-between mt-2">
                <v-btn color="blue" text @click="stepVal=1">BACK</v-btn>
            </div>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "exam",
  data: () => ({
    stepVal: 1,
    headers: [
      { text: "Add plan name", value: "plan_name" },
      { text: "$ Amount", value: "amount" },
      { text: "Job Owner", value: "job_owner" },
      { text: "", value: "action" },
    ],
    headers2: [
      { text: "$ Amount", value: "amount" },
      { text: "Job Owner", value: "job_owner" },
    ],
    items: [],
    ownerList: [],
    isLoading: false,
    newRecord: {
      plan_name: "",
      amount: "",
      job_owner: "",
    },
  }),
  mounted() {
    this.getOwnerList();
  },
  methods: {
    async getOwnerList() {
      this.isLoading = true;
      await axios
        .get("https://demo-api.bettercommissions.com/interview-data/users")
        .then((resp) => {
          this.ownerList = resp.data.users;
        });
      this.isLoading = false;
    },
    addRow() {
      this.items.push(this.newRecord);
      this.newRecord = {
        plan_name: "",
        amount: "",
        job_owner: "",
      };
    },
  },
};
</script>
