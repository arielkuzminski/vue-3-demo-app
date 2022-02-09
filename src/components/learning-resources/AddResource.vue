<template>
  <base-card>
    <form @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text" id="title" name="title" v-model="userTitle" />
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea
          name="description"
          id="description"
          rows="3"
          v-model="userDescription"
        ></textarea>
      </div>
      <div class="form-control">
        <label for="link">link</label>
        <input type="url" id="link" name="link" v-model="userLink" />
      </div>
      <div>
        <base-button type="submit">Add resource</base-button>
      </div>
    </form>
  </base-card>
  <base-dialog v-if="isErrorDialogOpened">
    <template v-slot:header>
      <h2>Błąd!</h2>
    </template>
    <h2>{{ error }}!</h2>
    <template #actions>
      <base-button @click="isErrorDialogOpened = false">Okay</base-button>
    </template>
  </base-dialog>
</template>

<script>
export default {
  data() {
    return {
      userTitle: '',
      userDescription: '',
      userLink: '',
      isErrorDialogOpened: false,
      error: '',
    };
  },
  inject: ['addResource'],
  methods: {
    onSubmit() {
      const isInvalid = this.validateInput();
      if (isInvalid) {
        this.showErrorModal(isInvalid);
      } else {
        this.addResource(this.userTitle, this.userDescription, this.userLink);
        this.userTitle = '';
        this.userDescription = '';
        this.userLink = '';
        this.isErrorDialogOpened = false;
      }
    },
    validateInput() {
      if (this.userTitle.trim() === '') {
        return 'Uzupełnij tytuł';
      } else if (this.userDescription.trim() === '') {
        return 'Uzupełnij opis';
      } else if (this.userLink.trim() === '') {
        return 'Uzupełnij link';
      }
      return false;
    },
    showErrorModal(error) {
      this.error = error;
      this.isErrorDialogOpened = true;
    },
  },
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
