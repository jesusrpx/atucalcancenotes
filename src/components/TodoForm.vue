<script setup>
	import { ref, reactive } from "vue";
	import { v4 } from 'uuid';
	import { toast } from "vue3-toastify";

	const { fn, taskForEdit } = defineProps(["fn", ""]);

	// Ref donde se guardara el nombre de la tarea
	const taskName = ref("");
	const taskData = reactive({
		name: "",
		finish_data: "",
		priority: "",
		description: "",
	});

	// Manejador del estado de la tarea
	const handleSaveTask = (event) => {
		event.preventDefault();

		// Validamos los datos
		if (taskData.name.trim() == "") {
			return;
		}

		// No se puede pasar el ref completo, solo el value ya que solo se necesita el valor
		// y al pasar toda la referencia completa actualizara cada vez que cambie el ref aqui tambien lo hara en el padre
		const taskObj = {
			id: v4(),
			name: taskData.name,
			completed: false,
			finish_data: taskData.finish_data,
			priority: taskData.priority,
			description: taskData.description,
		};

		// Enviamos el objeto a la funcion pasada por props
		fn(taskObj);

		// Limpiamos el taskname
		/*taskData = {
			finish_data: "",
			priority: "",
			description: "",
		};*/

		// Notificacion de exito
		return toast.success("Tarea agregada con exito", {
			autoClose: 4000,
			position: "top-center",
		});
	}

	// Funcion que asigna el valor de la tarea
	/*const handleInputTaskName = (event) => {
		taskName.value = event.target.value; // Asignamos el valor ingresado al target y
	}*/

	const handleInputs = (event) => {
		// Asinamos de forma dinamica los valores al target
		taskData[event.target.name] = event.target.value;
	};
</script>

<template>
	<form
		class="transition-all border bg-white shadow-lg py-6 px-6 rounded-xl grid grid-cols-12 justify-items-center content-center gap-4 w-11/12 sm:w-8/12 lg:w-5/12"
		@submit="handleSaveTask"
	>
		<p class="text-center font-semibold text-3xl col-span-12">
			Crear <span class="text-sky-500">Tarea</span>
		</p>

		<input
			class="border bg-gray-50 rounded-xl col-span-12 w-full py-2 px-4 transition-all outline-none focus:ring-4 focus:ring-lime-500 shadow-inner"
			type="text"
			name="name"
			placeholder="Ingrese el nombre de la tarea"
			@input="handleInputs"
		/>

		<input
			class="border bg-gray-50 rounded-xl w-full col-span-6 py-2 px-4 transition-all outline-none focus:ring-4 focus:ring-lime-500"
			name="finish_data"
			type="date"
			@input="handleInputs"
		/>

		<select
			class="w-full col-span-6 px-4 py-2 rounded-xl shadow border bg-white transition-all outline-none focus:ring-4 focus:ring-lime-500"
			name="priority"
			@input="handleInputs"
		>
			<option value="">Prioridad</option>
			<option value="baja">Baja</option>
			<option value="media">Media</option>
			<option value="alta">Alta</option>
		</select>

		<textarea
			class="border rounded-xl shadow-inner transition-all outline-none focus:ring-4 focus:ring-lime-500 py-2 px-4 bg-gray-50 col-span-12 w-full"
			placeholder="Ingrese una descripcion de la tarea (Opcional)"
			name="description"
			@input="handleInputs"
		></textarea>

		<button
			class="rounded-xl bg-emerald-400 border border-emerald-500 px-4 py-2 w-full col-span-12 text-white shadow"
		>
			Guardar Tarea
		</button>
	</form>
</template>
