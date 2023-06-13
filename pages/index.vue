<template>
  <div>
    <div
      class="col-lg-12 col-md-12 bg-white overflow-hidden shadow sm:rounded-lg"
    >
      <h2 class="col-lg-6 col-md-6 font-semibold float-left">Cobra</h2>
    </div>
    <div class="clearfix"></div>

    <div class="col-lg-12 col-md-12" style="margin-top: 10px">
      <div class="card">
        <h3
          class="card-header text-center font-weight-bold text-uppercase py-4"
        >
          User Validate
        </h3>
        <div class="card-body">
          <div class="row mb-3">
            <input
              type="search"
              class="form-control col-md-6"
              placeholder="Search By SS Number"
              aria-controls="dtBasicExample"
              v-model="search"
            />
          </div>
          {{ this.getSearchlist }}
          <div
            class="item error"
            v-if="search && this.getallproducts.length == 0"
          >
            <p>Please try with a valid social security number!</p>
          </div>
          <div v-if="search && this.getallproducts.length > 0">
            <h3 class="card-header text-center font-weight-bold py-4">
              Enrollment
            </h3>
            <form enctype="multipart/form-data">
              <div class="row g-3">
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="First name"
                    aria-label="First name"
                    v-model="ViewFormData.FirstName"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Last name"
                    aria-label="Last name"
                    v-model="ViewFormData.LastName"
                  />
                </div>
              </div>
              <div style="margin-top: 10px"></div>
              <div class="row g-3">
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="First name"
                    aria-label="First name"
                    v-model="ViewFormData.Email"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Last name"
                    aria-label="Last name"
                    v-model="ViewFormData.ZIP"
                  />
                </div>
              </div>
              <div style="margin-top: 10px"></div>
              <div class="row g-3">
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="PF no"
                    aria-label="First name"
                    v-model="ViewFormData.pfno"
                  />
                </div>
                <div class="col">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Mobile no"
                    aria-label="Last name"
                    v-model="ViewFormData.mobileno"
                  />
                </div>
              </div>
              <div style="margin-top: 10px"></div>
              <button
                type="button"
                class="btn btn-primary"
                @click="saveupdateForm"
              >
                Submit
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import fetch from "node-fetch";
export default {
  name: "IndexPage",
  data() {
    return {
      allproducts: [],
      search: "",
      ViewFormData: {
        id: "",
        FirstName: "",
        LastName: "",
        Email: "",
        ZIP: "",
        pfno: "",
        mobileno: "",
      },
    };
  },
  async mounted() {
    this.getallproductslist();
  },

  computed: {
    getallproducts() {
      console.log("this.search", this.search);
      if (this.search) {
        return this.allproducts.filter((item) => {
          console.log("item", item);
          return this.search
              .toLowerCase()
              .split(" ")
              .every((v) => item.SocialSecurity.toLowerCase().includes(v));
        });
      }
      return this.allproducts;
      // else {
      //   return this.allproducts;
      // }
    },
    getSearchlist() {
      console.log("getallproducts", this.getallproducts);
      let searchdata = {};
      if (this.getallproducts) {
        this.getallproducts.forEach((item, index) => {
          console.log("parentMessage", item);
          searchdata = item;
          this.ViewFormData = searchdata;
        });
        // return this.ViewFormData;
      }
      return this.ViewFormData;
    },
  },

  methods: {
    getallproductslist() {
      const url = "http://localhost:3000/posts";
      const options = {
        method: "GET",
        headers: {
          "Accept": "application/json",
          "Content-Type": "application/json;charset=UTF-8",
        },
      };
      fetch(url, options)
          .then((response) => response.json())
          .then((data) => {
            this.allproducts = data;
          });
    },
    async saveupdateForm() {
      const id = this.ViewFormData.id;
      console.log("idval", id);

      const url = `http://localhost:3000/posts/${id}`;
      const options = {
        method: "PUT",
        headers: {
          "Accept": "application/json",
          "Content-Type": "application/json",
        },

        body: JSON.stringify(this.ViewFormData),
      };
      try {
        await fetch(url, options)
            .then((response) => response.json())
            .then((data) => {
            // this.$swal('Product Updated!!!');
            });
      } catch (error) {
        throw error;
      }
    },
  },
};
</script>
