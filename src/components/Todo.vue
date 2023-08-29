<script setup lang="ts">
import { reactive, ref } from 'vue';
import { Icon } from "@iconify/vue";
import { computed } from 'vue';

const todos = reactive({
  list: [
    { id: 1, text: 'Learn JavaScript', done: false },
    { id: 2, text: 'Learn Vue', done: false },
    { id: 3, text: 'Learn E2E Testing', done: true },
    { id: 4, text: 'Build something awesome', done: true }
  ]
});

let editingTodo = ref({
  id: null,
  text: '',
  done: false
});

const editTodo = (todo: any) => {
  editingTodo.value = todo;
}

const removeTodo = (id: number) => {
  todos.list = todos.list.filter(todo => todo.id !== id);
}

const isAnyToDoBeingEdited = computed(() => {
  return editingTodo.value.id !== null;
});

const handleTodoEdit = (data: any) => {
  const { task } = data;
  const todoIndex = todos.list.findIndex(todo => todo.id === editingTodo.value.id);
  todos.list[todoIndex].text = task;
  editingTodo.value = {
    id: null,
    text: '',
    done: false
  };
}

const createNewTodo = () => {
  const newTodo = {
    id: todos.list.length + 1,
    text: '',
    done: false
  };
  todos.list.push(newTodo);
  editTodo({
    id: newTodo.id,
    text: newTodo.text,
    done: newTodo.done
  });
}

</script>

<template>
  <div class="grid grid-cols-1 gap-2 w-full py-10 h-[33rem] overflow-y-auto">    
    <div
      v-for="todo in todos.list"
      :key="todo.text"
      class="stack"
    >
      <div
        v-if="isAnyToDoBeingEdited && editingTodo.id === todo.id"
        class="stack"
      >
        <FormKit
          type="form"
          :actions="false"
          :classes="{
            form: 'w-full max-w-full min-h-[14rem] rounded bg-primary text-primary-content py-3 pt-5 pl-5',
            message: 'max-h-6 p-0 m-0',
          }"
          @submit="handleTodoEdit"
        >
          <h2
            class="text-xl mb-4 text-center"
          >
            Editar task
          </h2>
          <div
            class="h-[4rem] pr-5 mb-2"
          >
            <FormKit
              type="text"
              name="task"
              validation="required"
              :value="editingTodo.text"
              :classes="{
                input: 'input input-bordered w-full text-black',
                label: 'label',
                error: 'label label-error',
                wrapper: 'form-control',
              }"
            />
          </div>
          <FormKit
            type="submit"
            :classes="{
              input: 'btn max-w-[10rem] min-h-[3rem]',
              wrapper: 'form-control',
              label: 'btn btn-primary',
              error: 'btn btn-error',
            }"
          >
            Confirmar
          </FormKit>
        </FormKit>
      </div>
      <div
        v-else
        class="grid w-full z-[3] max-w-full min-w-32 h-20 rounded bg-primary text-primary-content place-content-center"
      >
        {{todo.text}}
        <details className="dropdown mb-32 absolute right-1 top-1">
          <summary className="btn bg-transparent p-0 m-0 w-[24px] border-none hover:bg-transparent">
            <Icon icon="bx:dots-vertical-rounded" class="w-6 h-6 text-white" />
          </summary>
          <ul className="p-2 shadow menu dropdown-content text-black bg-base-100 rounded-box w-50 border border-neutral">
            <li>
              <button
                @click="removeTodo(todo.id)"
              >
                Excluir
              </button>
            </li>
            <li>
              <button
                @click="editTodo(todo)"
              >
                Editar
              </button>
            </li>
          </ul>
        </details>
      </div> 
      <div class="grid w-full max-w-full min-w-32 h-20 rounded bg-accent text-accent-content place-content-center"/>
      <div class="grid w-full max-w-full min-w-32 h-20 rounded bg-secondary text-secondary-content place-content-center"/>
    </div>
    <div
      class="stack mt-3"
    >
      <button
        class="flex items-center w-full max-w-[100px] h-10 rounded bg-primary text-white hover:text-primary hover:bg-white place-content-center"
        :class="{
          'opacity-50 cursor-not-allowed': isAnyToDoBeingEdited,
          'opacity-100 cursor-pointer': !isAnyToDoBeingEdited
        }"
        :disabled="isAnyToDoBeingEdited"
        @click="createNewTodo"
      >
        <Icon icon="bx:plus" class="w-6 h-6" />
      </button> 
      <div class="grid w-full max-w-[100px] h-10 rounded bg-accent text-accent-content place-content-center"/>
      <div class="grid w-full max-w-[100px] h-10 rounded bg-secondary text-secondary-content place-content-center"/>
    </div>
  </div>
</template>
