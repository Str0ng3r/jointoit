<script lang="ts" setup>
import { ref } from "vue";
import FullCalendar from "@fullcalendar/vue3";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";
import timeGridPlugin from "@fullcalendar/timegrid";
import { createEventId } from "./event-utils";

const currentEvents = ref<any[]>([]);
const showModal = ref(false);
const showDeleteModal = ref(false);
const newEvent = ref({
	title: "",
	description: "",
	startStr: "",
	endStr: "",
	allDay: false,
});
const eventToDelete = ref<any>(null);
const calendarApi = ref(null);
const calendarOptions = ref({
	plugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
	headerToolbar: {
		left: "prev,next today",
		center: "title",
		right: "dayGridMonth,timeGridWeek,timeGridDay",
	},
	initialView: "dayGridMonth",
	editable: true,
	selectable: true,
	selectMirror: true,
	dayMaxEvents: true,
	weekends: true,
	select: handleDateSelect,
	eventClick: handleEventClick,
	eventsSet: handleEvents,
});

// Функции
function handleDateSelect(selectInfo: any) {
	newEvent.value.startStr = selectInfo.startStr;
	newEvent.value.endStr = selectInfo.endStr;
	newEvent.value.allDay = selectInfo.allDay;
	calendarApi.value = selectInfo.view.calendar;
	showModal.value = true;
}

function addEvent() {
	if (newEvent.value.title && calendarApi.value) {
		calendarApi.value.addEvent({
			id: createEventId(),
			title: newEvent.value.title,
			start: newEvent.value.startStr,
			end: newEvent.value.endStr,
			allDay: newEvent.value.allDay,
			extendedProps: {
				description: newEvent.value.description,
			},
		});
		calendarApi.value.unselect();
	}
	newEvent.value.title = "";
	newEvent.value.description = "";
	showModal.value = false;
}

function handleEventClick(clickInfo: any) {
	eventToDelete.value = clickInfo.event;
	showDeleteModal.value = true;
}

function deleteEvent() {
	if (eventToDelete.value) {
		eventToDelete.value.remove();
		eventToDelete.value = null;
	}
	showDeleteModal.value = false;
}

function handleEvents(events: any[]) {
	currentEvents.value = events;
}

function cancelEvent() {
	showModal.value = false;
	newEvent.value.title = "";
	newEvent.value.description = "";
}

function cancelDelete() {
	showDeleteModal.value = false;
	eventToDelete.value = null;
}
</script>

<template>
	<FullCalendar ref="calendarRef" class="calendar" :options="calendarOptions">
		<template v-slot:eventContent="arg">
			<b>{{ arg.timeText }}</b>
			<i>{{ arg.event.title }}</i
			><br />
			<small>{{ arg.event.extendedProps.description }}</small>
		</template>
	</FullCalendar>

	<!-- MODAL CREATE -->
	<div v-if="showModal" class="modal_background">
		<div class="modal">
			<h3 class="modal__title">Create New Event</h3>
			<input
				v-model="newEvent.title"
				placeholder="Event title"
				required
				class="input_title_event"
			/>

			<p class="selected_date">
				{{ newEvent.startStr }}
				<nuxt-icon name="calendarevent" filled class="calendaricon"></nuxt-icon>
			</p>
			<textarea
				v-model="newEvent.description"
				placeholder="Event description"
				class="input_description_event"
			></textarea>
			<div class="wrapper_buttons">
				<button @click="addEvent" class="wrapper_buttons__save">Save</button>
				<button @click="cancelEvent" class="wrapper_buttons__cancel">
					Cancel
				</button>
			</div>
		</div>
	</div>

	<!-- MODAL DELETE -->
	<div v-if="showDeleteModal" class="modal_background">
		<div class="modal">
			<h3 class="modal__title">Delete Event</h3>
			<p class="modal_delete_ask">Are you sure you want to delete the event?</p>
			<div class="wrapper_buttons">
				<button @click="deleteEvent" class="wrapper_buttons__delete">
					Delete
				</button>
				<button @click="cancelDelete" class="wrapper_buttons__cancel">
					Cancel
				</button>
			</div>
		</div>
	</div>
</template>

<style scoped lang="scss">
.modal_background {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.5);
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 2;
}
.calendaricon {
	width: 2rem;
}
.wrapper_buttons__delete {
	font-family: "Source Code Pro";
	font-size: 1.4rem;
	cursor: pointer;
	font-weight: 600;
}
.modal_delete_ask {
	font-family: "Source Code Pro";
	font-size: 1.6rem;
	font-weight: 500;
}
.calendar:deep(.fc-h-event) {
	font-family: "Source Code Pro";
	padding: 0.7rem 1.4rem;
	font-size: 1.3rem;
}
.calendar:deep(.fc-daygrid-more-link) {
	font-family: "Source Code Pro";
	font-size: 1.6rem;
}
.calendar:deep(.fc-popover-title) {
	font-family: "Source Code Pro";
	font-size: 1.6rem;
}
.input_description_event {
	border: none;
	outline: none;
	resize: none;
	width: 90%;
	border-bottom: 1px solid #c5c5c5;
	margin-bottom: 4rem;
	color: #43425d;
	font-family: "Source Code Pro";
}
.modal {
	background: #ffffff;
	padding: 20px;
	border-radius: 8px;
	border-radius: 10px;
	border: 1px solid #43425d;
	text-align: center;
	z-index: 10000;
	display: flex;
	flex-direction: column;
	align-items: center;
	min-width: 40rem;
	justify-content: center;
}
.selected_date {
	border-bottom: 1px solid #c5c5c5;
	width: 90%;
	color: #43425d;
	font-family: "Source Code Pro";
	margin-bottom: 4rem;
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.input_title_event {
	border: none;
	outline: none;
	width: 90%;
	border-bottom: 1px solid #c5c5c5;
	margin-bottom: 4rem;
	color: #43425d;
	font-family: "Source Code Pro";
}
.input_title_event::placeholder {
	font-family: "Source Code Pro";
	margin-bottom: 0.4rem;
	font-weight: 500;
	color: #c5c5c5;
}
.wrapper_buttons__save {
	font-size: 1.4rem;
	font-weight: 600;
	color: #6a6996;
	font-family: "Source Code Pro";
}
.wrapper_buttons__cancel {
	font-size: 1.4rem;
	color: #ff5f5f;
	font-weight: 600;
	font-family: "Source Code Pro";
}
.wrapper_buttons {
	display: flex;
	width: 100%;
	justify-content: space-around;
	margin-top: 1rem;
}
.modal__title {
	font-size: 1.8rem;
	font-family: "Source Code Pro";
	color: #212121;
	margin-bottom: 1rem;
}
.calendar:deep(.fc-toolbar-title) {
	font-family: "Source Code Pro" !important;
	color: #4d4f5c;
	font-size: 1.8rem;
}
.calendar:deep(.fc-today-button) {
	font-family: "Source Code Pro" !important;
	color: #4d4f5c;
	font-size: 1.3rem;
	background-color: transparent;
	border: #d7dae2 solid 2px;
}
.calendar:deep(.fc-dayGridMonth-button) {
	font-family: "Source Code Pro" !important;
	color: #4d4f5c !important;
	font-size: 1.3rem !important;
	border: 1px solid #d7dae2 !important;
	background-color: transparent !important;
}
.calendar:deep(.fc-timegrid-axis-cushion) {
	font-family: "Source Code Pro";
	font-size: 1rem;
}
.calendar:deep(.fc-timegrid-slot-label-cushion) {
	font-family: "Source Code Pro";
	font-size: 1.4rem;
}

.calendar:deep(.fc-timeGridDay-button) {
	font-family: "Source Code Pro" !important;
	color: #4d4f5c !important;
	font-size: 1.3rem !important;
	border: 1px solid #d7dae2 !important;
	background-color: transparent !important;
}
.calendar:deep(.fc-timeGridWeek-button) {
	font-family: "Source Code Pro" !important;
	color: #4d4f5c !important;
	font-size: 1.3rem !important;
	border: 1px solid #d7dae2 !important;
	background-color: transparent !important;
}
.calendar::-webkit-scrollbar {
	width: 0.7rem;
}
.calendar::-webkit-scrollbar-track {
	width: 0.7rem;
}
.calendar::-webkit-scrollbar-thumb {
	width: 0.7rem;
	border-radius: 0.35rem;
	border: 1px solid rgba(255, 255, 255, 0.15);
	background: var(--Stroke-medium-grey-stroke, #d5d6de);
}
.calendar:deep(.fc-button-active) {
	color: #3b86ff !important;
}
.calendar:deep(.fc-next-button) {
	background-color: #a77da8 !important;
	border: none;
}
.calendar:deep(.fc-prev-button) {
	background-color: #a77da8 !important;
	border: none;
}
.calendar:deep(.fc-col-header-cell-cushion) {
	font-family: "Source Code Pro";
	font-size: 1.4rem;
	color: #a3a6b4;
	padding: 1.6rem 0;
}
.calendar:deep(.fc-col-header) {
	background-color: #f5f6fa;
}
.calendar:deep(.fc-daygrid-day-number) {
	font-size: 1.5rem;
	color: #212121;
	font-family: "Source Code Pro";
	padding: 1.5rem 1.5rem;
}
.calendar:deep(.fc-day-today) {
	background-color: #eadff3;
}
.calendar {
	margin: 0 auto;
	padding: 4rem;
	width: 100%;
	max-height: 85rem;
	box-sizing: border-box;
	overflow-x: auto;
}
.calendar::-webkit-scrollbar {
	width: 0.7rem;
}
.calendar::-webkit-scrollbar-track {
	width: 0.7rem;
}
.calendar::-webkit-scrollbar-thumb {
	width: 0.7rem;
	border-radius: 0.35rem;
	border: 1px solid rgba(255, 255, 255, 0.15);
	background: var(--Stroke-medium-grey-stroke, #d5d6de);
}
</style>
