<template>
  <div class="container">
    <h1>Daftar Tugas Inola 𖹭</h1>
    <div class="input-container">
      <input type="text" v-model="tugasBaru" placeholder="Tambah tugas baru..." @keyup.enter="tambahTugas">
      <button @click="tambahTugas">Tambah</button>
    </div>
    <div class="task-list">
      <transition-group name="list" tag="div">
        <div v-for="(tugas, index) in tugasBelumSelesai" :key="tugas.id" class="task-card" :class="{ 'selesai': tugas.selesai }">
          <div class="task-content">
            <input type="checkbox" v-model="tugas.selesai" class="checkbox">
            <div v-if="!tugas.editing" class="task-text">{{ tugas.judul }}</div>
            <div v-else class="edit-mode">
              <input v-model="tugas.judul" class="edit-input">
              <div class="button-group">
                <button class="update-btn" @click="updateTugas(index)">Update</button>
                <button class="cancel-btn" @click="cancelTugas(index)">Cancel</button>
              </div>
            </div>
          </div>
          <div v-if="!tugas.editing" class="button-group">
            <button class="edit-btn" @click="editTugas(index)">Edit</button>
            <button class="delete-btn" @click="hapusTugas(index)">Delete</button>
          </div>
        </div>
      </transition-group>
    </div>
    <button class="filter-btn" @click="tampilkanBelumSelesai = !tampilkanBelumSelesai">
      {{ tampilkanBelumSelesai ? 'Tampilkan Semua Tugas' : 'Tampilkan Tugas Belum Selesai Saja' }}
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      tugasBaru: '',
      tampilkanBelumSelesai: true
    };
  },
  methods: {
    tambahTugas() {
      if (this.tugasBaru.trim() !== '') {
        this.tasks.push({ id: Date.now(), judul: this.tugasBaru, selesai: false, editing: false });
        this.tugasBaru = '';
      }
    },
    cancelTugas(index) {
      this.tasks[index].editing = false;
    },
    editTugas(index) {
      this.tasks[index].editing = true;
    },
    updateTugas(index) {
      this.tasks[index].editing = false;
    },
    hapusTugas(index) {
      this.tasks.splice(index, 1);
    }
  },
  computed: {
    tugasBelumSelesai() {
      if (this.tampilkanBelumSelesai) {
        return this.tasks.filter(tugas => !tugas.selesai);
      } else {
        return this.tasks;
      }
    }
  }
};
</script>

<style scoped>

body {
  font-family:fantasy;
  background-color: #6ac01e;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url('/assets/background.png');
}

.container {
  width: 500px;
  max-width: 1000px;
  background-color: #8EB69B;
  padding: 30px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  animation: fadeIn 0.5s ease-in-out;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 27%;
  left: 30%;
  transform: translate(-0%, -50%);
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

h1 {
  font-family: 'Nunito', sans-serif;
  text-align: center;
  color: #051F20;
  margin-bottom: 30px;
  font-size: 28px;
}

.input-container {
  display: flex;
  margin-bottom: 30px;
  color: #051F20;
}

input[type="text"] {
  flex: 1;
  padding: 15px;
  border: 1px solid #051F20;
  border-radius: 8px 0 0 8px;
  font-size: 16px;
  transition: box-shadow 0.3s ease;
  color: #051F20;
}

input[type="text"]:focus {
  box-shadow: 0 0 5px #235347;
  border: 1px solid #8EB69B;
  color:#051F20;
}

button {
  padding: 15px 25px;
  border: none;
  cursor: pointer;
  border-radius: 0 8px 8px 0;
  background-color: #163832;
  color: #DAF1DE;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #235347;
}

button:focus {
  outline: none;
}

.task-list {
  margin-bottom: 30px;
}

.task-card {
  background-color: #235347;
  padding: 20px;
  margin-bottom: 15px;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 20px rgba(203, 10, 10, 0.1);
}

.task-card.selesai .task-text {
  text-decoration: line-through;
  color: rgb(236, 14, 14);
}

.task-content {
  display: flex;
  align-items: center;
}

.task-text {
  flex-grow: 1;
  padding: 0 10px;
  font-size: 18px;
}

.edit-mode {
  display: flex;
  align-items: center;
  width: 100%;
}

.edit-input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #0B2B26;
  border-radius: 4px;
  font-size: 16px;
}

.button-group {
  display: flex;
  gap: 10px;
}

.checkbox {
  margin-right: 15px;
  transform: scale(1.3);
}

.update-btn, .cancel-btn, .edit-btn, .delete-btn {
  padding: 8px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s ease;
}

.update-btn {
  background-color: #163832;
  color: #DAF1DE;
}

.update-btn:hover {
  background-color: #8EB69B ;
}

.cancel-btn {
  background-color: #dc3545;
  color: white;
}

.cancel-btn:hover {
  background-color: #d46c77 ;
}

.edit-btn {
  background-color: #163832;
  color: #DAF1DE;
}

.edit-btn:hover {
  background-color: #8EB69B;
}

.delete-btn {
  background-color: #dc3545;
  color: #DAF1DE;
}

.delete-btn:hover {
  background-color:#d46c77 ;
}

.filter-btn {
  padding: 15px;  
  border: none;
  background-color: #051F20;
  color: #DAF1DE;
  cursor: pointer;
  border-radius: 8px;
  transition: background-color 0.3s ease;
}

.filter-btn:hover {
  background-color: #235347;
}

.list-enter-active, .list-leave-active {
  transition: all 0.5s ease;
}

.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

@media (max-width: 768px) {
  .container {
    width: 95%;
    padding: 20px;
  }

  input[type="text"] {
    width: calc(100% - 80px);
    color: #051F20
  }
}
</style>
