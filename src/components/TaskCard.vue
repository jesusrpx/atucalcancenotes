<script setup>
	import { ref } from "vue";

	// Props necesarios para este componenten
	const { task, handlerCompleted, handlerDeleted, handlerEdit } = defineProps([
		"task",
		"handlerCompleted",
		"handlerDeleted",
		"handlerEdit"
	]);

	// Los ref mantienen un estado reactivo local de este componente
	const completedTask = ref(task.completed);
	const showDes = ref(false);

</script>

<template>
	<div
		class="transition-all bg-white rounded-lg shadow w-full py-4 px-6 grid grid-cols-1 lg:grid-cols-3 justify-center items-center gap-2"
	>
		<p class="font-semibold text-lg w-50 sm:w-25">{{ task.name }}</p>

		<div class="flex flex-row gap-2 w-50 sm:w-25">
			<p>Completada:</p>
			<input
				class="accent-lime-500 text-white"
				type="checkbox"
				name="completed"
				:value="completedTask"
				:checked="completedTask"
				@click="(event) => handlerCompleted(event, task)"
			/>
		</div>

		<div
			class="flex flex-row items-center justify-between sm:justify-end gap-2 w-full"
		>
			<button
				class="border rounded px-4 py-2"
				@click="showDes = !showDes"
			>
				Detalles
			</button>
			<!-- <button
				class="border rounded bg-sky-500 border-sky-600 px-4 py-2 text-white"
				@click="() => handlerEdit(task)"
			>
				Editar
			</button> -->
			<button
				@click="() => handlerDeleted(task)"
				class="border rounded bg-amber-500 border-amber-600 px-4 py-2 text-white shadow"
			>
				Eliminar
			</button>
		</div>

		<transition name="show-desc">
			<div v-if="showDes" class="col-span-3">
				<p class="mb-4 font-semibold">Fecha de Finalizacion: <span class="text-indigo-600">{{task.finish_data}}</span></p>
				<div
					class="border rounded px-4 py-2 shadow-inner w-full"
				>
					<p class="font-semibold text-lg text-indigo-600">
						Descripcion de la tarea:
					</p>
					<p class="px-4">{{ task.description }}</p>
				</div>
			</div>
		</transition>
	</div>
</template>

<style scoped>
	.show-desc-enter-active,
	.show-desc-leave-active {
		opacity: 1;
		transition: all .3s ease-in;
	}

	.show-desc-enter-from,
	.show-desc-leave-to {
		opacity: 0;
		transition: all .3s ease-out;
		transform: translateY(5px);
	}
</style>
