<script setup>
    import { ref, onMounted } from "vue";
    import TodoForm from "./components/TodoForm.vue";
    import TaskCard from "./components/TaskCard.vue";
    import { toast } from "vue3-toastify";
    import "vue3-toastify/dist/index.css";

    // Lista de tareas
    const taskList = ref([]);
    const filterBy = ref("");

    // Funcion que agrega una tarea a la lista de tareas
    const updateTaskList = (newTask) => {
        taskList.value.push(newTask);
        saveDataInLocalStorage();
    };

    // Manejador para completar una tarea
    const completedTask = (event, taskNew) => {
        if (event.target.checked) {
            taskNew.completed = true;
        } else {
            taskNew.completed = false;
        }
        /*console.log(event.target.checked)*/
        const newListTasksModified = taskList.value.map((task) => {
            if (task.name == taskNew.name) return taskNew;
            return task;
        });

        taskList.value = newListTasksModified;

        // Guardado de datos luego de su modificacion
        saveDataInLocalStorage();
    };

    // Mostrando el contenido valido
    const handlerDeleted = (task) => {
        // Filtramos las tareas diferentes a las que llega por porametro
        const newListTasksModified = taskList.value.filter(
            (oldTask) => oldTask.name != task.name,
        );

        // Remplazamos con la lista filtrada
        taskList.value = newListTasksModified;

        // Guardar data en el localStorage
        saveDataInLocalStorage();

        // Retornamos una notificacion con el mensaje nuevo
        return toast.success(`Se elimino la tarea "${task.name}"`, {
            autoClose: 4000,
            position: "top-center",
        });
    };

    const handleOptions = (event) => {
        filterBy.value = event.target.value;

        // Filtrar por el tipo de opcion
    };

    const handlerEdit = (task) => {
        console.log("Se preciono el boton de editar");
        console.log(task.id);
        console.log(task.name);
    }

    const saveDataInLocalStorage = () => {
        localStorage.setItem("tasks", JSON.stringify(taskList.value));
    };

    const readDataFromLocalStorage = () => {
        const data = localStorage.getItem("tasks");
        if (data) {
            return (taskList.value = JSON.parse(data));
        }

        // Si no, se agrega un valor especifico
        return (taskList.value = []);
    };

    // La funcion "onMounted" ejecuta logica cuando el componente es montado
    onMounted(() => {
        // Leer datos del local storage
        // Plasmar esos datos dentro del local storage
        readDataFromLocalStorage();
    });
    
</script>

<template>
    <div class="w-screen h-screen flex flex-col items-center mt-8">
        <TodoForm :fn="updateTaskList" />

        <div class="w-11/12 sm:w-8/12 lg:w-5/12 mt-8 transition-all">
            <p class="text-center font-semibold text-4xl text-white mb-2">
                Listado de <span class="text-sky-600">tareas</span>
            </p>

            <div>
                <select
                    class="w-full px-6 py-4 rounded-xl shadow"
                    :value="filterBy"
                    @input="handleOptions"
                >
                    <option value="">Filtrar por...</option>
                    <option value="completed">Completadas</option>
                    <option value="noncompleted">No Completadas</option>
                </select>
            </div>

            <!--             <div class="mt-4 grid grid-cols-2 text-xl text-white">
                <p>Tareas completadas:</p>
                <p>Por finalizar: </p>
            </div>
 -->
            <div class="w-full flex flex-col gap-4 mt-5 h-full">
                <TransitionGroup>
                    <TaskCard
                        v-for="task in taskList"
                        :key="task"
                        :task="task"
                        :handlerCompleted="completedTask"
                        :handlerDeleted="handlerDeleted"
                        :handlerEdit="handlerEdit"
                    />
                </TransitionGroup>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .v-move,
    .v-enter-active,
    .v-leave-active {
        opacity: 1;
        transition: all 0.3s ease;
    }

    .v-enter-from,
    .v-leave-to {
        opacity: 0;
        transform: translateX(30px);
        transition: all 0.3s ease-out;
    }
</style>
