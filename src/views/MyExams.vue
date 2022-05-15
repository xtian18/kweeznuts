<template>
  <div>
    <h1>My Quizzes</h1>

    <a class="help" @click="showHelp=true">Help <font-awesome-icon icon="circle-question" /></a>

    <button class="btn btn-main" @click="showModal = true">Create new</button>
    <div class="exam-list">
      <div class="exam" v-for="quiz in quizzes" :key="quiz.id">
        <div class="me-auto">
          <h2>{{ quiz.name }}</h2>
          <p>{{ quiz.number_of_questions }} Questions</p>
        </div>
        <div class="exam-icon mb-auto">
          <router-link :to="`/my-quizzes/${quiz.id}`"
            ><font-awesome-icon icon="pen-to-square" class="action"
          /></router-link>
          <button class="btn-icon action" @click="handleDelete(quiz.id)">
            <font-awesome-icon icon="trash-can" />
          </button>
        </div>
      </div>
    </div>

    <DialogModal :showDialog="showDialog">
      <template v-slot:head>
        <h1>Delete Quiz</h1>
        <button type="button" class="btn-close action" @click="showDialog=false">
        </button>
      </template>
      <template v-slot:body>
        <p>Are you sure you want to delete this quiz?</p>
      </template>
      <template v-slot:foot>
        <button class="btn btn-secondary me-3" @click="showDialog=false">Cancel</button>
        <button class="btn btn-danger" @click="deleteQuiz()">Delete</button>
      </template>
    </DialogModal>

    <HelpModal :showHelp="showHelp">
      <template v-slot:head>
        <h1>Need Help?</h1>
        <button type="button" class="btn-close action" @click="showHelp=false, help_page=1">
        </button>
      </template>
      <template v-slot:body>
        <!--Title - By page format
        <h3 v-if="help_page >= 1 && help_page < 5">How to create a quiz:</h3>
        <h3 v-if="help_page >=5 && help_page < 10">How to update a quiz:</h3>
        <h3 v-if="help_page == 10">How to delete a quiz:</h3> -->

        <!--Pages
        <p v-if="help_page == 1">Step 1: Click the Create New button.</p>
        <img src="../assets/helpcreate_1.jpg" v-if="help_page == 1" style="width: 100%;">

        <p v-if="help_page == 2">Step 2: Fill out the required details and select Create New.</p>
        <img src="../assets/helpcreate_2.jpg" v-if="help_page == 2" style="width: 100%;">

        <p v-if="help_page == 3">Step 3: Enter the point equivalent, question, and choices.</p>
        <img src="../assets/helpcreate_3.jpg" v-if="help_page == 3" style="width: 100%;">

        <p v-if="help_page == 4">Step 4: Select the correct answer and save your selection. Repeat for other items.</p>
        <img src="../assets/helpcreate_4.jpg" v-if="help_page == 4" style="width:80%;">

        <p v-if="help_page == 5">Step 1: Select the update button of your quiz of choice.</p>
        <img src="../assets/helpupdate_1.jpg" v-if="help_page == 5" style="width: 100%;">

        <p v-if="help_page == 6">Step 2.1.(Quiz Profile): Select the update button of the profile you wish to change.</p>
        <img src="../assets/helpupdate_2.jpg" v-if="help_page == 6" style="width: 100%;">

        <p v-if="help_page == 7">Step 2.2.1.(Quiz Questions): Select the update button of the question you wish to change.</p>
        <img src="../assets/helpupdate_3.jpg" v-if="help_page == 7" style="width: 100%;">

        <p v-if="help_page == 8">Step 2.2.2.(Quiz Questions): Update the fields and Save.</p>
        <img src="../assets/helpupdate_4.jpg" v-if="help_page == 8" style="width: 80%;">

        <p v-if="help_page == 9">Step 2.3.(Delete Questions): Select and click delete icon if you wish to remove a question from the quiz.</p>
        <img src="../assets/helpupdate_5.jpg" v-if="help_page == 9" style="width: 100%;">

        <p v-if="help_page == 10">Select and click delete icon of the quiz you wish to remove.</p>
        <img src="../assets/helpdelete_1.jpg" v-if="help_page == 10" style="width: 100%;">
        -->

        <!--Scroll format-->
        <!--Create Quiz-->
          <h1>How to Create a Quiz</h1><br>

          <p>Step 1: Click the Create New button.</p>
          <img src="../assets/helpcreate_1.jpg" style="width: 100%;">

          <p>Step 2: Fill out the required details and select Create New.</p>
          <img src="../assets/helpcreate_2.jpg" style="width: 100%;">

          <p>Step 3: Enter the point equivalent, question, and choices.</p>
          <img src="../assets/helpcreate_3.jpg" style="width: 100%;">

          <p>Step 4: Select the correct answer and save your selection. Repeat for other items.</p>
          <img src="../assets/helpcreate_4.jpg" style="width:80%;">

        <!--Update Quiz-->
          <h1>How to Update Quiz</h1><br>

          <p>Step 1: Select the update button of your quiz of choice.</p>
          <img src="../assets/helpupdate_1.jpg" style="width: 100%;">

          <p>Step 2.1.(Quiz Profile): Select the update button of the profile you wish to change.</p>
          <img src="../assets/helpupdate_2.jpg" style="width: 100%;">

          <p>Step 2.2.1.(Quiz Questions): Select the update button of the question you wish to change.</p>
          <img src="../assets/helpupdate_3.jpg" style="width: 100%;">

          <p>Step 2.2.2.(Quiz Questions): Update the fields and Save.</p>
          <img src="../assets/helpupdate_4.jpg" style="width: 80%;">

          <p>Step 2.3.(Delete Questions): Select and click delete icon if you wish to remove a question from the quiz.</p>
          <img src="../assets/helpupdate_5.jpg" style="width: 100%;">

        <!--Delete Quiz-->
          <h1>How to Delete Quiz</h1><br>

          <p>Select and click delete icon of the quiz you wish to remove. WARNING: This action is final and will not be undoable!</p>
          <img src="../assets/helpdelete_1.jpg" style="width: 100%;">

      </template>
      <template v-slot:foot>
        <button class="btn btn-main" @click="showHelp=false, help_page=1">OK</button>
        <!--NEXT AND PREV BUTTON FOR PAGE BY PAGE FORMAT
        <button class="btn btn-left btn-main" v-if="help_page < 10" @click="help_page++">next</button>
        <button class="btn btn-left btn-main" v-if="help_page > 1" @click="help_page--">prev</button> 
        -->


      </template>
    </HelpModal>

    <!-- create quiz modal -->
    <teleport to="#app">
      <div class="modal-overlay" v-if="showModal">
        <div class="modal-container d-flex flex-column">
          <div class="modal-head">
            <h1 class="me-auto">Create New Quiz</h1>
            <button type="button" class="btn-close action" @click="showModal = !showModal"></button>
          </div>
          <div class="modal-body text-center">
            <form>
              <div class="form-group pb-3">
                <label for="name">Quiz Name:</label>
                <input type="text" id="name" placeholder="Enter quiz name" v-model="name" @keyup="enableButton"/>
              </div>
              <div class="form-group pb-3">
                <label for="desc">Subject:</label>
                <input type="text" id="desc" placeholder="Enter subject" v-model="desc" @keyup="enableButton"/>
              </div>
              <div>
                <label for="due_date">Due Date:</label><br>
                <input type="datetime-local" id="due_date" name="due_date" v-model="due_date" @change="enableButton">
              </div>
            </form>
          </div>
          <div class="modal-foot">
            <button class="btn btn-main" @click="createQuiz" :disabled="!isButtonEnabled">Create new</button>
            <button class="btn btn-secondary me-3" style="float: right;" @click="closeModal">Cancel</button>
          </div>
        </div>
      </div>
    </teleport>

  </div>
</template>

<script>
import DialogModal from "@/components/DialogModal.vue"
import HelpModal from "@/components/HelpModal.vue"

export default {
  components: { DialogModal, HelpModal },
  data() {
    return {
      selected_quiz_id: '',
      showModal: false,
      showDialog: false,
      showHelp: false,
      isButtonEnabled: false,
      name: "",
      desc: "",
      created_at: "2022-04-26T19:13:47.782661",
      due_date: "",
      quiz_code: "",
      number_of_questions: "",
      new_quiz_id: "",
      quizzes: [],
      help_page: 1,
    };
  },
  methods: {
    test() {
      console.log('hello')
    },
    async loadQuizzes() {
      try {
        const loadQuiz = await fetch("http://localhost:8000/quizzes/", {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Credetials": "true",
          },
          credentials: "include",
        });
        this.quizzes = await loadQuiz.json();
      } catch (e) {
        console.log(e);
      }
    },
    async createQuiz() {
      let formData = new FormData();
      formData.append("name", this.name);
      formData.append("desc", this.desc);
      formData.append("due_date", this.due_date);
      formData.append("teacher_id", this.user_id);

      const data = {};
      formData.forEach((value, key) => (data[key] = value));

      try {
        const postQuiz = await fetch("http://localhost:8000/quizzes/", {
          method: "POST",
          credentials: "include",
          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Credetials": "true",
          },
          body: JSON.stringify(data)
        });
        const newQuiz = await postQuiz.json();
        this.$router.push(`/my-quizzes/${newQuiz.id}`);
      } catch (e) {
        console.log(e);
      }
    },
    handleDelete(id) {
      this.selected_quiz_id = id;
      this.showDialog = true;
    },
    async deleteQuiz() {
      try {
        const response = await fetch("http://localhost:8000/quizzes/" + this.selected_quiz_id, {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Credetials": "true",
          },
          credentials: "include",
        });
        
        this.showDialog = false;
        await this.loadQuizzes();
      } catch(e) {
        console.log(e)
      }
    },
    closeModal() {
      this.showModal = false;
      this.name =  '';
      this.desc = '';
      this.due_date = '';
      this.isButtonEnabled = false;
    },
    enableButton() {
      if (this.name && this.desc && this.due_date) {
        this.isButtonEnabled = true;
      } else {
        this.isButtonEnabled = false;
      }
    }
  },
  computed: {
    user_id() {
      return this.$store.state.user.id;
    },
  },
  mounted() {
    this.loadQuizzes();
  },
};
</script>

<style scoped>
.btn-main {
  float: right;
}

.exam-icon {
  font-size: 2em;
}
.exam-icon svg {
  color: #93989c;
  margin-left: 15px;
}
.modal-container {
  width: 50%;
  height: auto;
}
.modal-head {
  width: 100%;
}
.modal-body {
  text-align: left !important;
  width: 100%;
}
.modal-foot {
  width: 100%;
  padding: 0 1rem;
}

p{
  text-align: left;
}
</style>