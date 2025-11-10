<template>
  <div class="submenu rounded-4 p-3 h-100 d-flex flex-column">
    <div v-if="category === 'notes'" class="d-flex flex-column flex-grow-1">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h6 class="mb-0 fw-bold">My notes</h6>
        <button
          class="btn btn-sm btn-outline-primary rounded-pill"
          @click="addNote"
        >
          + Add
        </button>
      </div>

      <ul class="list-unstyled flex-grow-1 overflow-auto mb-0">
        <li
          v-for="note in notes"
          :key="note.id"
          class="note-item d-flex align-items-center gap-2 mb-2"
          :class="{ active: 'note-' + note.id === activeSub }"
          @click="$emit('select-sub', 'note-' + note.id)"
        >
          <input
            type="checkbox"
            class="form-check-input"
            v-model="note.done"
            @click.stop="note.done = !note.done"
          />
          <span
            class="priority-dot"
            :class="'priority-' + note.priority"
          ></span>
          <div class="flex-grow-1">
            <div class="fw-semibold small">{{ note.title }}</div>
            <div class="text-muted xsmall">{{ note.priority }} priority</div>
          </div>
          <button
            class="btn btn-sm p-0 px-1 note-delete-btn"
            @click.stop="deleteNote(note.id)"
          >
            <img :src="trash" alt="delete" class="note-delete-icon" />
          </button>
        </li>
      </ul>
    </div>

    <ul v-else class="list-unstyled mb-0">
      <li
        v-for="item in items"
        :key="item.id"
        class="sub-item d-flex align-items-center gap-3 mb-2"
        :class="{ active: item.label === activeSub }"
        @click="$emit('select-sub', item.label)"
        role="button"
      >
        <img :src="item.icon" alt="" class="icon-img" />
        <span>{{ item.label }}</span>
      </li>
    </ul>

    <div
      v-if="category === 'budget'"
      class="mt-4 p-3 bg-white rounded-4 shadow-sm text-center"
    >
      <h6 class="fw-bold mb-2">Budget Overview</h6>
      <div class="progress rounded-pill" style="height: 18px">
        <div
          class="progress-bar bg-navy"
          role="progressbar"
          :style="{ width: progress + '%' }"
          :aria-valuenow="progress"
          aria-valuemin="0"
          aria-valuemax="100"
        ></div>
      </div>
      <div class="mt-2 small text-muted">
        <div>
          Planned: <strong>{{ planned }} €</strong>
        </div>
        <div>
          Spent: <strong>{{ spent }} €</strong>
        </div>
        <div>
          Remaining: <strong>{{ remaining }} €</strong>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import bride from "@/assets/bride.png";
import ring from "@/assets/ring.png";
import hairdresser from "@/assets/hairdresser.png";
import heels from "@/assets/heels.png";
import nails from "@/assets/nails.png";
import maid from "@/assets/maid.png";
import tai from "@/assets/tai.png";
import groom from "@/assets/groom.png";
import shirt from "@/assets/shirt.png";
import suit from "@/assets/suit.png";
import shoes from "@/assets/shoes.png";
import car from "@/assets/car.png";
import barber from "@/assets/barber.png";
import time from "@/assets/time.png";
import church from "@/assets/church.png";
import flowers from "@/assets/flowers.png";
import lapel from "@/assets/lapel.png";
import bouquet from "@/assets/bouquet.png";
import engagement from "@/assets/engagement.png";
import music from "@/assets/music.png";
import priest from "@/assets/priest.png";
import note from "@/assets/note.png";
import readers from "@/assets/readers.png";
import confetti from "@/assets/confetti.png";
import band from "@/assets/band.png";
import menu from "@/assets/menu.png";
import halla from "@/assets/halla.png";
import cake from "@/assets/cake.png";
import dance from "@/assets/dance.png";
import trash from "@/assets/trash.png";

export default {
  name: "SubMenu",
  props: {
    category: String,
    activeSub: String,
    notes: {
      type: Array,
      default: () => [],
    },
    activeNoteId: Number,
  },
  data() {
    return {
      planned: 5000,
      spent: 3200,
      notes: [
        {
          id: 1,
          title: "Checklist for photographer",
          priority: "high",
          done: false,
        },
        {
          id: 2,
          title: "Things to ask the hall",
          priority: "medium",
          done: true,
        },
        { id: 3, title: "Guests to call", priority: "low", done: false },
      ],
      trash,
    };
  },
  computed: {
    items() {
      const map = {
        bride: [
          {
            label: "Wedding dress",
            icon: bride,
          },
          { label: "Rings", icon: ring },
          { label: "Shoes", icon: heels },
          { label: "Hairdresser", icon: hairdresser },
          { label: "Nails", icon: nails },
          {
            label: "Maid of Honor",
            icon: maid,
          },
        ],
        groom: [
          { label: "Tie", icon: tai },
          { label: "Shoes", icon: shoes },
          { label: "Shirt", icon: shirt },
          { label: "Suit", icon: suit },
          { label: "Hairdresser", icon: hairdresser },
          { label: "Barber", icon: barber },
          { label: "Best Man", icon: groom },
        ],
        flowers: [
          { label: "Bouquet", icon: bouquet },
          { label: "Lapel", icon: lapel },
          {
            label: "Car flowers",
            icon: car,
          },
          { label: "Hall flowers", icon: flowers },
          {
            label: "Church flowers",
            icon: church,
          },
        ],
        church: [
          { label: "Date & Time", icon: time },
          {
            label: "Engagement course",
            icon: engagement,
          },
          { label: "Music", icon: music },
          { label: "A Priest", icon: priest },
          { label: "Readers", icon: readers },
          { label: "Documents", icon: note },
          { label: "Confetti", icon: confetti },
        ],
        hall: [
          { label: "Hall", icon: halla },
          { label: "Menu", icon: menu },
          { label: "Cake", icon: cake },
          { label: "Band", icon: band },
          { label: "First dance", icon: dance },
        ],
      };
      return map[this.category] || [];
    },
    remaining() {
      return this.planned - this.spent;
    },
    progress() {
      return Math.min((this.spent / this.planned) * 100, 100).toFixed(0);
    },
  },
  methods: {
    addNote() {
      const id = this.notes.length
        ? Math.max(...this.notes.map((n) => n.id)) + 1
        : 1;
      this.notes.push({
        id,
        title: "New note",
        priority: "low",
        done: false,
      });
      this.$emit("select-sub", "note-" + id);
    },
    deleteNote(id) {
      this.notes = this.notes.filter((n) => n.id !== id);
      if ("note-" + id === this.activeSub) {
        const first = this.notes[0];
        this.$emit("select-sub", first ? "note-" + first.id : "");
      }
    },
  },
};
</script>

<style scoped>
.submenu {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: calc(100vh - 130px);
}
.sub-item {
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 0.8rem;
  border-radius: 999px;
  transition: 0.2s;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}
.sub-item.active,
.sub-item:hover {
  background: #d4af37;
  color: #fff;
}
.icon-img {
  width: 52px;
  height: 52px;
  flex: 0 0 52px;
  object-fit: contain;
  filter: invert(16%) sepia(89%) saturate(894%) hue-rotate(199deg)
    brightness(90%) contrast(90%);
}
.progress {
  background-color: rgba(0, 0, 0, 0.1);
}

.bg-navy {
  background-color: rgb(13, 23, 80);
}

.note-item {
  background: rgba(255, 255, 255, 0.5);
  padding: 0.4rem 0.6rem;
  border-radius: 0.9rem;
  transition: 0.15s;
  cursor: pointer;
}
.note-item.active,
.note-item:hover {
  background: #d4af37;
  color: #fff;
}
.priority-dot {
  width: 14px;
  height: 14px;
  border-radius: 999px;
}
.priority-high {
  background: #dc3545;
}
.priority-medium {
  background: #ffc107;
}
.priority-low {
  background: #198754;
}
.xsmall {
  font-size: 0.65rem;
}

.note-delete-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}
.note-delete-btn {
  background: transparent;
  border: none;
}
</style>
