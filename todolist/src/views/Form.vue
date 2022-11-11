<template>
  <div class="container mt-2">
    <b-form>
      <b-form-group
        label="Titulo"
        label-for="subject"
      >
        <b-form-input
          id="subject"
          v-model="form.subject"
          type="text"
          placeholder="Ex: lavar carro"
          required
          autocomplete="off"
        >
        </b-form-input>
      </b-form-group>
      <b-form-group
        label="Descrição"
        label-for="description"
      >
        <b-form-textarea
          id="description"
          v-model="form.description"
          type="text"
          placeholder="Ex: preciso levar o carro para lavar"
          required
          autocomplete="off"
        >
        </b-form-textarea>
      </b-form-group>

      <b-button type="submit" variant="outline-primary" @click="saveTask"> Salvar </b-button>
    </b-form>
  </div>
</template>

<script>
  import toatMixin from '@/mixins/toatMixin.js'

  export default {
    name: 'Form',
    mixins: [toatMixin],
    data() {
      return {
        form: {
          subject: "",
          description: ""
        },
        methodSave: "new"
      }
    },
    created() {
      if(this.$route.params.index === 0 || this.$route.params.index !== undefined) {
        this.methodSave = "update"
        let tasks = JSON.parse(localStorage.getItem("tasks"))
        this.form = tasks[this.$route.params.index]
      }
    },
    methods: {
      saveTask() {
        if(this.methodSave === "update") {
          var tasks = JSON.parse(localStorage.getItem("tasks"))
          tasks[this.$route.params.index] = this.form
          this.showToast("success", "Sucesso!", "Tarefa atualizada com sucesso!")
        }
        else {
          var tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
          tasks.push(this.form)
          this.showToast("success", "Sucesso!", "Tarefa criada com sucesso!")

        }

        localStorage.setItem("tasks", JSON.stringify(tasks))
        this.$router.push({ name: "list" })
      }
    }
  }
</script>
