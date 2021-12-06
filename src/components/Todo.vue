<template>
     <div class="row">
            <div
              class="col-md-6 offset-md-3 border d-flex justify-content-between align-items-center pt-2 pb-2"
            >
              <span>{{todo.text}}</span>
              <button @click="$emit('deleteToDo',todo.id)" class="btn btn-danger">Sil</button>
            </div>
          </div>
</template>
<script>
export default {
    props:["todo"]
}
</script>