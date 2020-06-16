<template>
  <div>
    <h2>Orders :</h2>
    <div class="container-fluid" style="float:right">
      <button
        class="btn btn-success"
        type="button"
        data-toggle="modal"
        data-target="#exampleModal"
        @click="openModal()"
      >
        <i class="fa fa-plus"></i> Add
      </button>
    </div>
    <div class="table-responsive">
      <table class="table">
        <thead>
          <tr>
            <th>S.NO</th>
            <th>Name</th>
            <th>Price</th>
            <th>Notes</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(order, index) in order_lists" v-bind:key="order.name">
            <td>{{ index + 1 }}</td>
            <td>{{ order.name }}</td>
            <td>{{ order.price }}</td>
            <td>{{ order.notes }}</td>
            <td class="action-btns">
              <button
                class="btn btn-primary"
                @click="openModal(order,index)"
                data-toggle="modal"
                data-target="#exampleModal"
              >
                <i class="fa fa-edit"></i> Edit
              </button>
              <button class="btn btn-danger" data-toggle="modal"
                data-target="#deleteModal" @click="openDeleteModal(index)">
                <i class="fa fa-trash"></i> Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- Modal for Add/Edit Record -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header text-center">
            <h3 class="modal-title" id="exampleModalLabel">
              <strong>{{is_edit_order ? "Update" : "Create" }} Order</strong>
            </h3>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form class="form-horizontal">
              <div class="form-group">
                <label class="control-label col-sm-2" for="name">Name:</label>
                <div class="col-sm-10">
                  <input
                    type="text"
                    class="form-control"
                    id="name"
                    placeholder="Enter Name"
                    v-model="items_list.name"
                  />
                </div>
              </div>
              <div class="form-group">
                <label class="control-label col-sm-2" for="Price">Price:</label>
                <div class="col-sm-10">
                  <input
                    type="number"
                    class="form-control"
                    id="Price"
                    placeholder="Enter Price"
                    v-model="items_list.price"
                  />
                </div>
              </div>
              <div class="form-group">
                <label class="control-label col-sm-2" for="notes">Notes:</label>
                <div class="col-sm-10">
                  <textarea class="form-control" rows="5" id="notes" v-model="items_list.notes"></textarea>
                </div>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
            <button
              type="button"
              class="btn btn-primary"
              @click="addRecord()"
            >{{is_edit_order ? "Update" : "Create" }}</button>
          </div>
          <div class="alert alert-danger" v-if="has_error" role="alert">
            <b>Please Enter Name and Price of Order</b>
          </div>
        </div>
      </div>
    </div>
    <!-- End of Add/Edit Modal -->
    <!-- Start Delete Modal -->
    <div
      class="modal fade"
      id="deleteModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="deleteModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h3 class="modal-title" id="deleteModalLabel">
              <strong>Delete Record</strong>
            </h3>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            Are you want to delete the record
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
            <button
              type="button"
              class="btn btn-danger"
              @click="deleteRecord()"
            >Delete</button>
          </div>
        </div>
      </div>
    </div>
    <!-- End Delete Modal -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
// import $ from "jquery";

export default {
  name: "Home",
  data() {
    return {
      is_modal_open: false,
      items_list: {
        name: "",
        price: null,
        notes: ""
      },
      has_error: false,
      order_lists: {},
      is_edit_order: false,
      currentIndex: Number
    };
  },
  methods: {
    openModal(order, index) {
      this.currentIndex = index ? index : null;
      this.is_edit_order = order ? true : false;
      this.items_list.name = order ? order.name : "";
      this.items_list.price = order ? order.price : 0;
      this.items_list.notes = order ? order.notes : "";
    },
    addRecord() {
      if (this.items_list.name && this.items_list.price) {
        this.has_error = false;
        let order_list = [];
        if (this.is_edit_order) {
          this.order_lists[this.currentIndex] = this.items_list;
          localStorage.setItem("order_lists", JSON.stringify(this.order_lists));
        } else {
          order_list = JSON.parse(localStorage.getItem("order_lists")) || [];
          order_list.push(this.items_list);
          localStorage.setItem("order_lists", JSON.stringify(order_list));
        }
        this.order_lists = JSON.parse(localStorage.getItem("order_lists"));
        location.reload();
      } else {
        this.has_error = true;
      }
    },
    openDeleteModal(index){
      this.currentIndex = index; 
    },
    deleteRecord(){
      this.order_lists.splice(this.currentIndex, 1);
      localStorage.setItem("order_lists", JSON.stringify(this.order_lists));
      location.reload();
    }
  },
  mounted() {
    this.order_lists = JSON.parse(localStorage.getItem("order_lists"));
  },
  components: {}
};
</script>
<style scoped>
.action-btns .btn {
  margin-right: 10px;
}
</style>