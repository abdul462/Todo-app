<template>
  <div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="genericPopup"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Task Detail</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="content-wrapper">
              <div class="task-wrapper">
                <div class="task-name" v-if="showTitle">{{taskDetail.title}}</div>
                <input
                  type="text"
                  class="form-control"
                  :value="taskDetail.title"
                  v-if="showEditTodoTitleField"
                  @blur="saveTodoTitle"
                />
                <div class="task-actions">
                  <i
                    class="fa fa-edit"
                    aria-hidden="true"
                    @click="editTodoTitle"
                    v-if="showTitleBtn"
                  ></i>
                </div>
              </div>
            </div>
          </div>

          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="saveTaskData">Save changes</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Bus } from "./utils/bus";
import ClickOutside from "vue-click-outside";
import vueStore from "./store/index";
import { mapActions, mapGetters } from "vuex";
export default {
  name: "todoDetailModal",
  data() {
    return {
      taskDetail: {},
      showTitle: true,
      showTitleBtn: true,
      searchText: "",
    };
  },
  components: {
    Bus
  },
  directives: {
    ClickOutside
  },
  computed: {
    ...mapGetters(["colorPalete", "getAllTags"]),

    searchTag() {
      return vueStore.getters.filterTags(this.searchText);
    }
  },

  methods: {
    ...mapActions(["addNewTag", "changeTagColor", "updateTodoTags"]),
    saveTaskData() {
      $("#genericPopup").modal("hide");
    },
    editTodoTitle() {
      this.showEditTodoTitleField = !this.showEditTodoTitleField;
      this.showTitle = !this.showTitle;
      this.showTitleBtn = !this.showTitleBtn;
    },
    saveTodoTitle(e) {
      if (e.target.value.trim().length > 0) {
        this.taskDetail.title = e.target.value.trim();
        this.showEditTodoTitleField = !this.showEditTodoTitleField;
        this.showTitle = !this.showTitle;
        this.showTitleBtn = !this.showTitleBtn;
      }
    },
    resetModal() {
      this.showEditTodoTitleField = false;
      this.showTitle = true;
      this.showTitleBtn = true;
      this.searchText = "";
      $(".tab-pane, .nav-item a").removeClass("active show");
    },
    showModal(data) {
      this.taskDetail = data;
      this.resetModal();
      $("#genericPopup").modal("show");
    }
  },
  mounted() {
    Bus.$on("showDetailedTaskModal", this.showModal);
  }
};
</script>

<style scoped lang="scss" >
.task-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  .task-name {
    flex: auto;
  }
  .task-actions {
    i {
      font-size: 20px;
      cursor: pointer;
    }
  }
}
.custom-control {
  .icon-wrapper {
    width: 40px;
    display: inline-block;
    text-align: center;
  }
  .high i {
    color: #f5365c;
  }
  .medium i {
    color: #ffbb33;
  }
  .low i {
    color: #5e72e4;
  }
  .none i {
    color: #37474f;
  }
}

.group-color-dialog {
  position: relative;
  padding: 5px;
  line-height: 0px;
  .group-color-item {
    cursor: pointer;
    width: 20px;
    height: 20px;
    display: inline-block;
    border-radius: 50%;
    transition: opacity 0.1 ease;
    margin: 3px;
  }
}
</style>
