<template>
  <div class="content-panel rounded-4 p-3 d-flex flex-column">
    <div class="d-flex flex-column flex-md-row gap-2 mb-3">
      <div
        v-if="
          showSalons ||
          showRings ||
          showShoes ||
          showHairdresser ||
          showCozmetic
        "
        class="flex-grow-1 position-relative"
      >
        <input
          v-model="search"
          type="text"
          class="form-control rounded-pill ps-5"
          placeholder="Search..."
        />
        <span class="search-icon">🔍</span>
      </div>

      <select
        v-if="
          showSalons ||
          showRings ||
          showShoes ||
          showHairdresser ||
          showCozmetic
        "
        class="form-select rounded-pill w-auto"
      >
        <option>Sort by</option>
        <option>Name</option>
        <option>Rating</option>
      </select>
    </div>

    <div v-if="showSalons" class="flex-grow-1 overflow-auto pe-1">
      <SalonCard
        v-for="salon in filteredSalons"
        :key="salon.id"
        :salon="salon"
      />
    </div>

    <div v-else-if="showRings" class="flex-grow-1 overflow-auto pe-1">
      <RingsCard
        v-for="store in filteredStores"
        :key="store.id"
        :store="store"
      />
    </div>

    <div v-else-if="showShoes" class="flex-grow-1 overflow-auto pe-1">
      <ShoesCard v-for="shop in filteredShops" :key="shop.id" :store="shop" />
    </div>

    <div v-else-if="showHairdresser" class="flex-grow-1 overflow-auto pe-1">
      <HairdresserCard
        v-for="salon in filteredHairSalon"
        :key="salon.id"
        :store="salon"
      />
    </div>

    <div v-else-if="showCozmetic" class="flex-grow-1 overflow-auto pe-1">
      <CozmeticCard
        v-for="salon in filteredCozmetic"
        :key="salon.id"
        :store="salon"
      />
    </div>

    <div v-else-if="showMaidOfHonor" class="flex-grow-1 overflow-auto pe-1">
      <MaidOfHonorCard />
    </div>

    <div v-else-if="showTie" class="flex-grow-1 overflow-auto pe-1">
      <TieCard v-for="item in filteredTie" :key="item.id" :tie="item" />
    </div>

    <div v-else-if="showShoesG" class="flex-grow-1 overflow-auto pe-1">
      <ShoesGCard
        v-for="item in filteredStoresG"
        :key="item.id"
        :storeg="item"
      />
    </div>

    <div v-else-if="showShirts" class="flex-grow-1 overflow-auto pe-1">
      <ShirtCard
        v-for="shirt in filteredShirts"
        :key="shirt.id"
        :shirt="shirt"
      />
    </div>

    <div v-else-if="showSuit" class="flex-grow-1 overflow-auto pe-1">
      <SuitCard v-for="item in filteredSuits" :key="item.id" :suit="item" />
    </div>

    <div v-else-if="showHairdresserG" class="flex-grow-1 overflow-auto pe-1">
      <HairdresserGCard
        v-for="hairdresser in filteredHairdresserG"
        :key="hairdresser.id"
        :hairdresser="hairdresser"
      />
    </div>

    <div v-else-if="showBarber" class="flex-grow-1 overflow-auto pe-1">
      <BarberCard
        v-for="barber in filteredBarbers"
        :key="barber.id"
        :barber="barber"
      />
    </div>
    <div v-else-if="showBestMan" class="flex-grow-1 overflow-auto pe-1">
      <BestManCard />
    </div>

    <div v-else-if="showLapel" class="flex-grow-1 overflow-auto pe-1">
      <LapelCard />
    </div>
    <div v-else-if="showBouquet" class="flex-grow-1 overflow-auto pe-1">
      <BouquetCard
        v-for="flower in flowers"
        :key="flower.id"
        :flower="flower"
      />
    </div>
    <div v-else-if="showCarFlowers" class="flex-grow-1 overflow-auto pe-1">
      <CarFlowersCard
        v-for="car in filteredCarFlowers"
        :key="car.id"
        :car="car"
      />
    </div>
    <div v-else-if="showHallFlowers" class="flex-grow-1 overflow-auto pe-1">
      <HallFlowersCard
        v-for="hall in filteredHallFlowers"
        :key="hall.id"
        :hall="hall"
      />
    </div>
    <div v-else-if="showChurchFlowers" class="flex-grow-1 overflow-auto pe-1">
      <ChurchFlowersCard
        v-for="church in filteredChurchFlowers"
        :key="church.id"
        :church="church"
      />
    </div>

    <div v-else-if="showDateAndTime" class="flex-grow-1 overflow-auto pe-1">
      <DateAndTimeCard />
    </div>

    <div v-else-if="showEngagement" class="flex-grow-1 overflow-auto pe-1">
      <EngagementCard />
    </div>

    <div v-else-if="showMusic" class="flex-grow-1 overflow-auto pe-1">
      <MusicCard
        v-for="music in filteredMusic"
        :key="music.id"
        :music="music"
      />
    </div>

    <div
      v-else-if="category === 'church' && sub === 'A Priest'"
      class="flex-grow-1 overflow-auto pe-1"
    >
      <APriestCard />
    </div>

    <div
      v-else-if="category === 'church' && sub === 'Readers'"
      class="flex-grow-1 overflow-auto pe-1"
    >
      <ReadersCard />
    </div>

    <div v-else-if="showDocuments" class="flex-grow-1 overflow-auto pe-1">
      <DocumentCard />
    </div>

    <div v-else-if="showConfetti" class="flex-grow-1 overflow-auto pe-1">
      <ConfettiCard />
    </div>

    <div v-else-if="showHalls" class="flex-grow-1 overflow-auto pe-1">
      <HallCard v-for="hall in filteredHalls" :key="hall.id" :hall="hall" />
    </div>

    <div v-else-if="showMenu" class="flex-grow-1 overflow-auto pe-1">
      <MenuCard />
    </div>

    <div v-else-if="showCakes" class="flex-grow-1 overflow-auto pe-1">
      <CakeCard v-for="cake in filteredCakes" :key="cake.id" :cake="cake" />
    </div>

    <div v-else-if="showBands" class="flex-grow-1 overflow-auto pe-1">
      <BandCard v-for="band in filteredBands" :key="band.id" :band="band" />
    </div>

    <div v-else-if="showDance" class="flex-grow-1 overflow-auto pe-1">
      <DanceCard />
    </div>

    <div v-else-if="showBudget" class="flex-grow-1 overflow-auto pe-1">
      <div class="bg-white rounded-4 shadow-sm p-3">
        <h5 class="fw-bold mb-3 text-center">Wedding Budget Items</h5>

        <div class="budget-table-wrapper">
          <table class="table table-sm align-middle text-center mb-0">
            <thead class="table-light sticky-top">
              <tr>
                <th class="text-start">Item</th>
                <th style="width: 120px">Planned (€)</th>
                <th style="width: 120px">Spent (€)</th>
                <th style="width: 180px">Document</th>
                <th style="width: 110px">Remaining</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in budgetItems" :key="item.label">
                <td class="text-start">
                  <span class="d-flex align-items-center gap-2">
                    <img
                      v-if="item.icon"
                      :src="item.icon"
                      alt=""
                      class="icon-img"
                    />
                    {{ item.label }}
                  </span>
                </td>
                <td>
                  <input
                    type="number"
                    class="form-control form-control-sm text-center"
                    v-model.number="item.planned"
                  />
                </td>
                <td>
                  <input
                    type="number"
                    class="form-control form-control-sm text-center"
                    v-model.number="item.spent"
                  />
                </td>
                <td>
                  <input type="file" class="form-control form-control-sm" />
                </td>
                <td>
                  <span
                    :class="{
                      'text-success': item.planned - item.spent > 0,
                      'text-danger': item.planned - item.spent < 0,
                    }"
                  >
                    {{ item.planned - item.spent }}
                  </span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div v-else-if="showNotes" class="flex-grow-1 p-3 d-flex flex-column">
      <div
        class="bg-white rounded-4 shadow-sm p-3 h-100 d-flex flex-column gap-3"
      >
        <div class="d-flex gap-2">
          <input
            v-model="currentNote.title"
            type="text"
            class="form-control"
            placeholder="Note title..."
          />
          <select
            v-model="currentNote.priority"
            class="form-select"
            style="max-width: 150px"
          >
            <option value="high">High</option>
            <option value="medium">Medium</option>
            <option value="low">Low</option>
          </select>
          <div class="form-check d-flex align-items-center gap-2">
            <input
              type="checkbox"
              class="form-check-input"
              v-model="currentNote.done"
              :id="'note-done-' + currentNote.id"
            />
            <label
              class="form-check-label small"
              :for="'note-done-' + currentNote.id"
            >
              Done
            </label>
          </div>
        </div>

        <textarea
          v-model="currentNote.text"
          class="form-control flex-grow-1"
          placeholder="Write your note here..."
        ></textarea>

        <div class="d-flex gap-3 align-items-center">
          <div>
            <label class="form-label small mb-1">Attach image</label>
            <input type="file" class="form-control form-control-sm" />
          </div>
          <div
            v-if="currentNote.image"
            class="border rounded p-1"
            style="width: 90px; height: 90px; overflow: hidden"
          >
            <img
              :src="currentNote.image"
              alt=""
              class="w-100 h-100 object-fit-cover"
            />
          </div>
        </div>
      </div>
    </div>

    <div
      v-else-if="showGuests"
      class="flex-grow-1 d-flex flex-column overflow-auto pe-1"
    >
      <div class="d-flex flex-wrap gap-2 mb-3">
        <select
          v-model="guestFilterStatus"
          class="form-select form-select-sm w-auto rounded-pill"
        >
          <option value="all">All statuses</option>
          <option value="invited">Invited</option>
          <option value="confirmed">Confirmed</option>
          <option value="declined">Declined</option>
          <option value="maybe">Maybe</option>
        </select>

        <select
          v-model="guestFilterSide"
          class="form-select form-select-sm w-auto rounded-pill"
        >
          <option value="all">Both sides</option>
          <option value="bride">Bride's side</option>
          <option value="groom">Groom's side</option>
        </select>

        <select
          v-model="guestSort"
          class="form-select form-select-sm w-auto rounded-pill ms-auto"
        >
          <option value="name-asc">Name A–Z</option>
          <option value="name-desc">Name Z–A</option>
          <option value="status">By status</option>
          <option value="side">By side</option>
        </select>
      </div>

      <div class="bg-white rounded-4 shadow-sm p-3 flex-grow-1 overflow-auto">
        <table class="table table-sm align-middle">
          <thead class="table-light">
            <tr>
              <th>Name</th>
              <th>Side</th>
              <th>Group</th>
              <th>Status</th>
              <th class="text-center">Seats</th>
              <th>Note</th>
              <th class="text-center" style="width: 70px">Actions</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="guest in filteredGuests" :key="guest.id">
              <td>{{ guest.name }}</td>
              <td class="text-capitalize">{{ guest.side }}</td>
              <td class="text-capitalize">{{ guest.group }}</td>
              <td>
                <select
                  v-model="guest.status"
                  class="form-select form-select-sm"
                >
                  <option value="invited">Invited</option>
                  <option value="confirmed">Confirmed</option>
                  <option value="declined">Declined</option>
                  <option value="maybe">Maybe</option>
                </select>
              </td>
              <td class="text-center">
                <input
                  type="number"
                  min="1"
                  class="form-control form-control-sm text-center"
                  v-model.number="guest.seats"
                />
              </td>
              <td>
                <input
                  type="text"
                  class="form-control form-control-sm"
                  v-model="guest.note"
                  placeholder="Note..."
                />
              </td>
              <td class="text-center">
                <button
                  class="btn btn-sm p-0 px-1 guest-delete-btn"
                  @click="deleteGuest(guest.id)"
                >
                  <img :src="trash" alt="delete" class="guest-delete-icon" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="mt-3 bg-white rounded-4 shadow-sm p-3">
        <h6 class="fw-bold mb-2">Add guest</h6>
        <div class="row g-2">
          <div class="col-md-3">
            <input
              v-model="newGuest.name"
              type="text"
              class="form-control form-control-sm"
              placeholder="Name and surname"
            />
          </div>
          <div class="col-md-2">
            <select v-model="newGuest.side" class="form-select form-select-sm">
              <option value="bride">Bride</option>
              <option value="groom">Groom</option>
            </select>
          </div>
          <div class="col-md-2">
            <select v-model="newGuest.group" class="form-select form-select-sm">
              <option value="family">Family</option>
              <option value="friends">Friends</option>
              <option value="colleagues">Colleagues</option>
              <option value="other">Other</option>
            </select>
          </div>
          <div class="col-md-2">
            <input
              v-model.number="newGuest.seats"
              type="number"
              min="1"
              class="form-control form-control-sm"
              placeholder="Seats"
            />
          </div>
          <div class="col-md-3 d-flex gap-2">
            <input
              v-model="newGuest.note"
              type="text"
              class="form-control form-control-sm"
              placeholder="Note (optional)"
            />
            <button
              class="btn btn-sm btn-primary rounded-pill"
              @click="addGuest"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-else-if="showChat" class="flex-grow-1 d-flex flex-column">
      <div class="d-flex align-items-center justify-content-between mb-2 px-2">
        <div>
          <h6 class="mb-0 fw-bold">{{ currentChatTitle }}</h6>
        </div>

        <div class="d-flex align-items-center gap-2">
          <span class="badge bg-success rounded-pill">Online</span>
        </div>
      </div>

      <div
        class="chat-messages flex-grow-1 mb-3 p-3 rounded-4 bg-white shadow-sm overflow-auto"
      >
        <div
          v-for="msg in currentChatMessages"
          :key="msg.id"
          class="d-flex mb-2"
          :class="{
            'justify-content-end': msg.from === 'me',
            'justify-content-start': msg.from === 'them',
          }"
        >
          <div
            class="chat-bubble"
            :class="{
              'chat-bubble-me': msg.from === 'me',
              'chat-bubble-them': msg.from === 'them',
            }"
          >
            <div v-if="msg.type === 'text'">
              {{ msg.text }}
            </div>
            <div v-else-if="msg.type === 'file'">📎 {{ msg.text }}</div>
            <div class="chat-time">{{ msg.time }}</div>
          </div>
        </div>
      </div>

      <div class="chat-input mt-auto">
        <div
          class="bg-white rounded-4 shadow-sm p-2 d-flex align-items-center gap-2"
        >
          <label class="btn btn-light btn-sm mb-0 rounded-circle">
            📎
            <input
              type="file"
              class="d-none"
              multiple
              @change="onChatFilesChange"
            />
          </label>

          <textarea
            v-model="newChatMessage"
            class="form-control border-0 flex-grow-1"
            rows="1"
            placeholder="Write a message..."
            @keyup.enter.exact.prevent="sendChatMessage"
          ></textarea>

          <button
            class="btn btn-primary rounded-pill btn-sm px-3"
            type="button"
            @click="sendChatMessage"
          >
            Send
          </button>
        </div>
      </div>
    </div>

    <div
      v-else
      class="d-flex flex-column justify-content-center align-items-center h-100 text-center"
    >
      <h4 class="mb-2 text-muted">This section will have its own page</h4>
      <p class="text-muted">
        Budget, notes, guests, chat ... will lead to different components.
      </p>
    </div>
  </div>
</template>

<script>
import SalonCard from "./SalonCard.vue";
import RingsCard from "./RingsCard.vue";
import ShoesCard from "./ShoesCard.vue";
import HairdresserCard from "./HairdresserCard.vue";
import CozmeticCard from "./CozmeticCard.vue";
import MaidOfHonorCard from "./MaidOfHonorCard.vue";
import TieCard from "./TieCard.vue";
import ShoesGCard from "./ShoesGCard.vue";
import ShirtCard from "./ShirtCard.vue";
import SuitCard from "./SuitCard.vue";
import HairdresserGCard from "./HairdresserGCard.vue";
import BarberCard from "./BarberCard.vue";
import BestManCard from "./BestManCard.vue";
import LapelCard from "./LapelCard.vue";
import BouquetCard from "./BouquetCard.vue";
import CarFlowersCard from "./CarFlowersCard.vue";
import HallFlowersCard from "./HallFlowersCard.vue";
import ChurchFlowersCard from "./ChurchFlowersCard.vue";
import DateAndTimeCard from "./DateAndTimeCard.vue";
import EngagementCard from "./EngagementCard.vue";
import MusicCard from "./MusicCard.vue";
import APriestCard from "./APriestCard.vue";
import ReadersCard from "./ReadersCard.vue";
import DocumentCard from "./DocumentCard.vue";
import ConfettiCard from "./ConfettiCard.vue";
import HallCard from "./HallCard.vue";
import MenuCard from "./MenuCard.vue";
import CakeCard from "./CakeCard.vue";
import BandCard from "./BandCard.vue";
import DanceCard from "./DanceCard.vue";

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
  name: "ContentPanel",
  components: {
    SalonCard,
    RingsCard,
    ShoesCard,
    HairdresserCard,
    CozmeticCard,
    MaidOfHonorCard,
    TieCard,
    ShoesGCard,
    ShirtCard,
    SuitCard,
    HairdresserGCard,
    BarberCard,
    BestManCard,
    LapelCard,
    BouquetCard,
    CarFlowersCard,
    HallFlowersCard,
    ChurchFlowersCard,
    DateAndTimeCard,
    EngagementCard,
    MusicCard,
    APriestCard,
    ReadersCard,
    DocumentCard,
    ConfettiCard,
    HallCard,
    MenuCard,
    CakeCard,
    BandCard,
    DanceCard,
  },
  props: {
    category: String,
    sub: String,
    chats: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      search: "",
      notesData: [
        {
          id: 1,
          title: "Checklist for photographer",
          priority: "high",
          done: false,
          text: "Ask about golden hour, backup, price.",
          image: "",
        },
        {
          id: 2,
          title: "Questions for the hall",
          priority: "medium",
          done: false,
          text: "How many people, cake, music until when...",
          image: "",
        },
        {
          id: 3,
          title: "Guests to call",
          priority: "low",
          done: true,
          text: "Marko, Ana, Luka...",
          image: "",
        },
      ],
      guests: [
        {
          id: 1,
          name: "Marko Markić",
          side: "bride",
          group: "family",
          status: "confirmed",
          seats: 2,
          note: "",
        },
        {
          id: 2,
          name: "Ana Aničić",
          side: "groom",
          group: "friends",
          status: "invited",
          seats: 1,
          note: "",
        },
        {
          id: 3,
          name: "Luka Lukić",
          side: "bride",
          group: "colleagues",
          status: "maybe",
          seats: 1,
          note: "",
        },
      ],

      newGuest: {
        name: "",
        side: "bride",
        group: "family",
        status: "invited",
        seats: 1,
        note: "",
      },

      guestSort: "name-asc",
      guestFilterStatus: "all",
      guestFilterSide: "all",

      budgetItems: [
        { label: "Wedding dress", icon: bride, planned: 1200, spent: 800 },
        { label: "Rings", icon: ring, planned: 800, spent: 500 },
        { label: "Shoes (bride)", icon: heels, planned: 150, spent: 0 },
        {
          label: "Hairdresser (bride)",
          icon: hairdresser,
          planned: 90,
          spent: 0,
        },
        { label: "Nails", icon: nails, planned: 50, spent: 0 },
        { label: "Maid of Honor", icon: maid, planned: 120, spent: 0 },

        { label: "Tie", icon: tai, planned: 40, spent: 0 },
        { label: "Shoes (groom)", icon: shoes, planned: 120, spent: 0 },
        { label: "Shirt", icon: shirt, planned: 60, spent: 0 },
        { label: "Suit", icon: suit, planned: 350, spent: 0 },
        {
          label: "Hairdresser (groom)",
          icon: hairdresser,
          planned: 25,
          spent: 0,
        },
        { label: "Barber", icon: barber, planned: 25, spent: 0 },
        { label: "Best Man", icon: groom, planned: 80, spent: 0 },

        { label: "Bouquet", icon: bouquet, planned: 90, spent: 0 },
        { label: "Lapel", icon: lapel, planned: 25, spent: 0 },
        { label: "Car flowers", icon: car, planned: 70, spent: 0 },
        { label: "Hall flowers", icon: flowers, planned: 180, spent: 0 },
        { label: "Church flowers", icon: church, planned: 120, spent: 0 },

        { label: "Date & Time", icon: time, planned: 0, spent: 0 },
        { label: "Engagement course", icon: engagement, planned: 50, spent: 0 },
        { label: "Music", icon: music, planned: 120, spent: 0 },
        { label: "A Priest", icon: priest, planned: 50, spent: 0 },
        { label: "Readers", icon: readers, planned: 0, spent: 0 },
        { label: "Documents", icon: note, planned: 30, spent: 0 },
        { label: "Confetti", icon: confetti, planned: 40, spent: 0 },

        { label: "Hall", icon: halla, planned: 1500, spent: 0 },
        { label: "Menu", icon: menu, planned: 900, spent: 0 },
        { label: "Cake", icon: cake, planned: 300, spent: 0 },
        { label: "Band", icon: band, planned: 700, spent: 0 },
        { label: "First dance", icon: dance, planned: 100, spent: 0 },
      ],

      salons: [
        {
          id: 1,
          name: "Angels-Wedding",
          rating: 4.9,
          address: "Zagrebačka ul. 1, Pula",
          status: "open",
          img: "https://picsum.photos/420/200?random=30",
        },
        {
          id: 2,
          name: "Vjenčanice Biba Rijeka",
          rating: 4.9,
          address: "Ul. Strossmayerova 2b, Rijeka",
          status: "open",
          img: "https://picsum.photos/420/200?random=31",
        },
        {
          id: 3,
          name: "Glamour Bridal",
          rating: 4.7,
          address: "Ilica 15, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=32",
        },
      ],
      stores: [
        {
          id: 1,
          name: "Zlatarna Dodić",
          rating: 4.9,
          address: "Ilica 46, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=40",
        },
        {
          id: 2,
          name: "Zlatarna Celje",
          rating: 4.9,
          address: "Praška ulica 8, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=41",
        },
        {
          id: 3,
          name: "Prahir",
          rating: 4.7,
          address: "Ul. Vice Vukova 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=42",
        },
      ],
      shops: [
        {
          id: 1,
          name: "Aldo",
          rating: 4.9,
          address: "Varšavska 46, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=50",
        },
        {
          id: 2,
          name: "Mass",
          rating: 4.9,
          address: "Ilica 8, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=51",
        },
        {
          id: 3,
          name: "Zooek",
          rating: 4.7,
          address: "Vukovarska ul. 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=52",
        },
      ],
      hairSalon: [
        {
          id: 1,
          name: "Sasha",
          rating: 4.9,
          address: "Dalmatinska 4, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=60",
        },
        {
          id: 2,
          name: "Vikler i Ruž",
          rating: 4.9,
          address: "Frankopanska 12",
          status: "open",
          img: "https://picsum.photos/420/200?random=61",
        },
        {
          id: 3,
          name: "Hairque",
          rating: 4.7,
          address: "Vukovarska ul. 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=62",
        },
      ],
      cozmeticSalon: [
        {
          id: 1,
          name: "Estrella Beauty & academy",
          rating: 4.9,
          address: "Cetingradska 26, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=60",
        },
        {
          id: 2,
          name: "Vikler i Ruž",
          rating: 4.9,
          address: "Frankopanska 12",
          status: "open",
          img: "https://picsum.photos/420/200?random=61",
        },
        {
          id: 3,
          name: "De luxe",
          rating: 4.7,
          address: "Ilica 35, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=62",
        },
      ],
      ties: [
        {
          id: 1,
          name: "Croata",
          rating: 4.9,
          address: "Oktogon, Ilica 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=60",
        },
        {
          id: 2,
          name: "Galileo",
          rating: 4.9,
          address: "Medvedgradska 12",
          status: "open",
          img: "https://picsum.photos/420/200?random=61",
        },
      ],
      shopsg: [
        {
          id: 1,
          name: "Aldo",
          rating: 4.9,
          address: "Varšavska 46, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=50",
        },
        {
          id: 2,
          name: "Mass",
          rating: 4.9,
          address: "Ilica 8, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=51",
        },
        {
          id: 3,
          name: "Modivo",
          rating: 4.7,
          address: "Vukovarska ul. 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=52",
        },
      ],
      shirts: [
        {
          id: 1,
          name: "Classic White Shirt",
          rating: 4.9,
          address: "Varšavska 10, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=70",
        },
        {
          id: 2,
          name: "Slim Fit Blue Shirt",
          rating: 4.7,
          address: "Ilica 25, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=71",
        },
      ],
      suits: [
        {
          id: 1,
          name: "Jeordie's",
          rating: 4.9,
          address: "Ilica 12, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=100",
        },
        {
          id: 2,
          name: "Martin Arbanas",
          rating: 4.7,
          address: "Savska 5, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=101",
        },
        {
          id: 3,
          name: "Suitbox",
          rating: 4.7,
          address: "Savska 5, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=1021",
        },
      ],
      hairSalonG: [
        {
          id: 1,
          name: "Gentlemen's Cut",
          rating: 4.8,
          address: "Ilica 10, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=730",
        },
        {
          id: 2,
          name: "Barber King",
          rating: 4.9,
          address: "Vukovarska 15, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=741",
        },
      ],
      barbers: [
        {
          id: 1,
          name: "Barber Shop One",
          rating: 4.8,
          address: "Trg bana J. Jelačića 5, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=70",
        },
        {
          id: 2,
          name: "Classic Barber",
          rating: 4.7,
          address: "Ilica 20, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=71",
        },
        {
          id: 3,
          name: "The Royal Barber Shop",
          rating: 4.7,
          address: "Froudeova ulica 9., Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=731",
        },
        {
          id: 4,
          name: "Brijačko - frizerski salon Man style",
          rating: 4.7,
          address: "Nova Cesta 171, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=571",
        },
      ],
      flowers: [
        {
          id: 1,
          name: "Cvjećarnica Ljubica",
          img: "https://example.com/flora.jpg",
          rating: 4.9,
          address: "Meštrovićev trg 2",
          status: "Open until 20:00",
        },
        {
          id: 2,
          name: "Cvjećarnica Orhideja",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Horvaćanska cesta 26",
          status: "Closes soon",
        },
        {
          id: 3,
          name: "Cvjećarnica Ljiljana",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Ilica 26",
          status: "Closes soon",
        },
        {
          id: 4,
          name: "Cvjećarnica Ruža",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Tkalčićeva 15, 10000 Zagreb",
          status: "Closes soon",
        },
      ],
      carFlowers: [
        {
          id: 1,
          name: "Cvjećarnica Ljubica",
          img: "https://example.com/flora.jpg",
          rating: 4.9,
          address: "Meštrovićev trg 2",
          status: "Open until 20:00",
        },
        {
          id: 2,
          name: "Cvjećarnica Orhideja",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Horvaćanska cesta 26",
          status: "Closes soon",
        },
      ],
      hallFlowers: [
        {
          id: 1,
          name: "Cvjećarnica Ljubica",
          img: "https://example.com/flora.jpg",
          rating: 4.9,
          address: "Meštrovićev trg 2",
          status: "Open until 20:00",
        },
        {
          id: 2,
          name: "Cvjećarnica Orhideja",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Horvaćanska cesta 26",
          status: "Closes soon",
        },
      ],
      churchFlowers: [
        {
          id: 1,
          name: "Cvjećarnica Ljubica",
          img: "https://example.com/flora.jpg",
          rating: 4.9,
          address: "Meštrovićev trg 2",
          status: "Open until 20:00",
        },
        {
          id: 2,
          name: "Cvjećarnica Orhideja",
          img: "https://example.com/rose.jpg",
          rating: 4.8,
          address: "Horvaćanska cesta 26",
          status: "Closes soon",
        },
      ],
      music: [
        {
          id: 1,
          name: "Joy",
          rating: 4.9,
          address: "Trg bana J. Jelačića 1, Zagreb",
          status: "Available",
          img: "https://picsum.photos/420/200?random=201",
        },
        {
          id: 2,
          name: "Tragovi",
          rating: 4.7,
          address: "Ilica 10, Zagreb",
          status: "Booked",
          img: "https://picsum.photos/420/200?random=202",
        },
        {
          id: 3,
          name: "Prva Liga",
          rating: 4.8,
          address: "Savska 5, Zagreb",
          status: "Available",
          img: "https://picsum.photos/420/200?random=203",
        },
      ],
      halls: [
        {
          id: 1,
          name: "Globus",
          rating: 4.9,
          address: "Ilica 12, Zagreb",
          status: "Available",
          img: "https://picsum.photos/420/200?random=301",
        },
        {
          id: 2,
          name: "River",
          rating: 4.8,
          address: "Savska 5, Zagreb",
          status: "Booked",
          img: "https://picsum.photos/420/200?random=302",
        },
      ],
      cakes: [
        {
          id: 1,
          name: "Jolie Pettite",
          img: "https://example.com/cake.jpg",
          rating: "4.9",
          address: "Magazinska 12, Zagreb",
          status: "Open now",
        },
        {
          id: 2,
          name: "Vincek",
          img: "https://example.com/cake.jpg",
          rating: "4.9",
          address: "Magazinska 12, Zagreb",
          status: "Open now",
        },
      ],
      band: [
        {
          id: 1,
          name: "Joy",
          rating: 4.9,
          address: "Trg bana J. Jelačića 1, Zagreb",
          status: "Available",
          img: "https://picsum.photos/420/200?random=201",
        },
        {
          id: 2,
          name: "Tragovi",
          rating: 4.7,
          address: "Ilica 10, Zagreb",
          status: "Booked",
          img: "https://picsum.photos/420/200?random=202",
        },
        {
          id: 3,
          name: "Prva Liga",
          rating: 4.8,
          address: "Savska 5, Zagreb",
          status: "Available",
          img: "https://picsum.photos/420/200?random=203",
        },
      ],
      trash,
      chatTitles: {
        "chat-1": "Maid of Honor – Ana",
        "chat-2": "Best Man – Marko",
        "chat-3": "Bend Joy",
      },
      chatMessages: {
        1: [
          {
            id: 1,
            from: "them",
            text: "Hej, imamo željenu haljinu, predivna je!",
            time: "18:30",
            type: "text",
          },
          {
            id: 2,
            from: "me",
            text: "Jeee, baš mi je drago 😊",
            time: "18:33",
            type: "text",
          },
          {
            id: 3,
            from: "them",
            text: "Odlično, možemo se čuti za termin",
            time: "18:36",
            type: "text",
          },
          {
            id: 4,
            from: "them",
            text: "Ok, šaljem ti fotku haljine kasnije.",
            time: "18:37",
            type: "text",
          },
        ],
        2: [
          {
            id: 1,
            from: "me",
            text: "Marko, potvrdi samo dolaziš li s partnericom.",
            time: "17:01",
            type: "text",
          },
          {
            id: 2,
            from: "them",
            text: "Da, naravno!",
            time: "18:30",
            type: "text",
          },
          {
            id: 1,
            from: "me",
            text: "Jesmo riješili prijevoz za kumove?",
            time: "18:31",
            type: "text",
          },
        ],
        3: [
          {
            id: 1,
            from: "them",
            text: "Pozdrav, tada smo slobodni i možemo svirati.",
            time: "Jučer",
            type: "text",
          },
        ],
      },

      newChatMessage: "",
      newChatFiles: [],
    };
  },
  computed: {
    currentChatTitle() {
      if (!this.sub) return "Chat";

      if (this.chatTitles && this.chatTitles[this.sub]) {
        return this.chatTitles[this.sub];
      }

      return this.sub;
    },
    showChat() {
      return this.category === "chat";
    },

    currentChatId() {
      if (!this.showChat) return null;
      const id =
        this.sub && this.sub.startsWith("chat-")
          ? Number(this.sub.replace("chat-", ""))
          : 1;
      return id;
    },

    currentChatMessages() {
      if (!this.currentChatId) return [];
      return this.chatMessages[this.currentChatId] || [];
    },
    showGuests() {
      return this.category === "guests";
    },

    filteredGuests() {
      let list = this.guests;

      if (this.search) {
        const q = this.search.toLowerCase();
        list = list.filter((g) => g.name.toLowerCase().includes(q));
      }

      if (this.guestFilterStatus !== "all") {
        list = list.filter((g) => g.status === this.guestFilterStatus);
      }

      if (this.guestFilterSide !== "all") {
        list = list.filter((g) => g.side === this.guestFilterSide);
      }

      const sorted = [...list];
      switch (this.guestSort) {
        case "name-desc":
          sorted.sort((a, b) => b.name.localeCompare(a.name));
          break;
        case "status":
          sorted.sort(
            (a, b) =>
              a.status.localeCompare(b.status) || a.name.localeCompare(b.name)
          );
          break;
        case "side":
          sorted.sort(
            (a, b) =>
              a.side.localeCompare(b.side) || a.name.localeCompare(b.name)
          );
          break;
        default:
          sorted.sort((a, b) => a.name.localeCompare(b.name));
      }

      return sorted;
    },
    showSalons() {
      return (
        this.category === "bride" &&
        !["Rings", "Shoes", "Hairdresser", "Nails", "Maid of Honor"].includes(
          this.sub
        )
      );
    },
    showRings() {
      return this.category === "bride" && this.sub === "Rings";
    },
    showShoes() {
      return this.category === "bride" && this.sub === "Shoes";
    },
    showHairdresser() {
      return this.category === "bride" && this.sub === "Hairdresser";
    },
    showCozmetic() {
      return this.category === "bride" && this.sub === "Nails";
    },
    showMaidOfHonor() {
      return this.category === "bride" && this.sub === "Maid of Honor";
    },
    showTie() {
      return this.category === "groom" && this.sub === "Tie";
    },
    showShoesG() {
      return this.category === "groom" && this.sub === "Shoes";
    },
    showShirts() {
      return this.category === "groom" && this.sub === "Shirt";
    },
    showSuit() {
      return this.category === "groom" && this.sub === "Suit";
    },
    showHairdresserG() {
      return this.category === "groom" && this.sub === "Hairdresser";
    },
    showBarber() {
      return this.category === "groom" && this.sub === "Barber";
    },
    showBestMan() {
      return this.category === "groom" && this.sub === "Best Man";
    },
    showLapel() {
      return this.category === "flowers" && this.sub === "Lapel";
    },
    showBouquet() {
      return this.category === "flowers" && this.sub === "Bouquet";
    },
    showCarFlowers() {
      return this.category === "flowers" && this.sub === "Car flowers";
    },
    showHallFlowers() {
      return this.category === "flowers" && this.sub === "Hall flowers";
    },
    showChurchFlowers() {
      return this.category === "flowers" && this.sub === "Church flowers";
    },
    showDateAndTime() {
      return this.category === "church" && this.sub === "Date & Time";
    },
    showEngagement() {
      return this.category === "church" && this.sub === "Engagement course";
    },
    showMusic() {
      return this.category === "church" && this.sub === "Music";
    },
    showReaders() {
      return this.category === "church" && this.sub === "Readers";
    },
    showDocuments() {
      return this.category === "church" && this.sub === "Documents";
    },
    showConfetti() {
      return this.category === "church" && this.sub === "Confetti";
    },

    showHalls() {
      return this.category === "hall" && this.sub === "Hall";
    },

    showMenu() {
      return this.category === "hall" && this.sub === "Menu";
    },

    showCakes() {
      return this.category === "hall" && this.sub === "Cake";
    },

    showBands() {
      return this.category === "hall" && this.sub === "Band";
    },

    showDance() {
      return this.category === "hall" && this.sub === "First dance";
    },

    filteredSalons() {
      if (!this.search) return this.salons;
      return this.salons.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredStores() {
      if (!this.search) return this.stores;
      return this.stores.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredShops() {
      if (!this.search) return this.shops;
      return this.shops.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredHairSalon() {
      if (!this.search) return this.hairSalon;
      return this.hairSalon.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredCozmetic() {
      if (!this.search) return this.cozmeticSalon;
      return this.cozmeticSalon.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredTie() {
      if (!this.search) return this.ties;
      return this.ties.filter((t) =>
        t.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredStoresG() {
      if (!this.search) return this.shopsg;
      return this.shopsg.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredShirts() {
      if (!this.search) return this.shirts;
      return this.shirts.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredSuits() {
      if (!this.search) return this.suits;
      return this.suits.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredHairdresserG() {
      if (!this.search) return this.hairSalonG;
      return this.hairSalonG.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredBarbers() {
      if (!this.search) return this.barbers;
      return this.barbers.filter((b) =>
        b.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredCarFlowers() {
      if (!this.search) return this.carFlowers;
      return this.carFlowers.filter((b) =>
        b.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredHallFlowers() {
      if (!this.search) return this.hallFlowers;
      return this.hallFlowers.filter((h) =>
        h.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredChurchFlowers() {
      if (!this.search) return this.churchFlowers;
      return this.churchFlowers.filter((c) =>
        c.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredMusic() {
      if (!this.search) return this.music;
      return this.music.filter((b) =>
        b.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredHalls() {
      if (!this.search) return this.halls;
      return this.halls.filter((h) =>
        h.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredCakes() {
      if (!this.search) return this.cakes;
      return this.cakes.filter((c) =>
        c.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredBands() {
      if (!this.search) return this.band;
      return this.band.filter((b) =>
        b.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },

    showBudget() {
      return this.category === "budget";
    },
    showNotes() {
      return this.category === "notes";
    },
    currentNote() {
      if (!this.showNotes) return {};

      const id =
        this.sub && this.sub.startsWith("note-")
          ? Number(this.sub.replace("note-", ""))
          : this.notesData[0]?.id;
      return this.notesData.find((n) => n.id === id) || this.notesData[0];
    },
  },
  methods: {
    sendChatMessage() {
      const text = this.newChatMessage.trim();
      if (!text && !this.newChatFiles.length) return;

      const id = this.currentChatId;
      if (!id) return;

      if (!this.chatMessages[id]) {
        this.$set(this.chatMessages, id, []);
      }

      if (text) {
        const newId = this.chatMessages[id].length
          ? Math.max(...this.chatMessages[id].map((m) => m.id)) + 1
          : 1;

        this.chatMessages[id].push({
          id: newId,
          from: "me",
          text,
          time: "Now",
          type: "text",
        });
      }

      this.newChatFiles.forEach((file, index) => {
        const newId = this.chatMessages[id].length
          ? Math.max(...this.chatMessages[id].map((m) => m.id)) + 1
          : 1;

        this.chatMessages[id].push({
          id: newId,
          from: "me",
          text: file.name,
          time: "Now",
          type: "file",
        });
      });

      this.newChatMessage = "";
      this.newChatFiles = [];
    },

    onChatFilesChange(event) {
      const files = Array.from(event.target.files || []);
      this.newChatFiles = files;
    },
    deleteGuest(id) {
      this.guests = this.guests.filter((g) => g.id !== id);
    },
    addGuest() {
      if (!this.newGuest.name.trim()) {
        return;
      }

      const id = this.guests.length
        ? Math.max(...this.guests.map((g) => g.id)) + 1
        : 1;

      this.guests.push({
        id,
        ...this.newGuest,
      });

      this.newGuest = {
        name: "",
        side: "bride",
        group: "family",
        status: "invited",
        seats: 1,
        note: "",
      };
    },
    onSelectNote(id) {
      this.activeNoteId = id;
    },
    onAddNote() {
      const newId = this.notes.length
        ? Math.max(...this.notes.map((n) => n.id)) + 1
        : 1;
      this.notes.push({
        id: newId,
        title: "New note",
        priority: "low",
        done: false,
        text: "",
        image: "",
      });
      this.activeNoteId = newId;
    },
  },
};
</script>

<style scoped>
.content-panel {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: calc(100vh - 200px);
  max-height: calc(100vh - 200px);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.content-panel > .flex-grow-1 {
  min-height: 0;
  overflow-y: auto;
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
}
.icon-img {
  width: 30px;
  height: 30px;
  object-fit: contain;
}

.budget-table-wrapper {
  overflow: visible;
}

textarea {
  min-height: 160px;
  resize: vertical;
}
.guest-delete-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}

.guest-delete-btn {
  background: transparent;
  border: none;
}
.chat-messages {
  max-height: calc(100vh - 260px);
}

.chat-bubble {
  max-width: 75%;
  padding: 0.5rem 0.75rem;
  border-radius: 1rem;
  font-size: 0.9rem;
  position: relative;
}
.chat-bubble-me {
  background: #0d1750;
  color: #fff;
  border-bottom-right-radius: 0.2rem;
}
.chat-bubble-them {
  background: #f1f1f1;
  color: #000;
  border-bottom-left-radius: 0.2rem;
}
.chat-time {
  font-size: 0.7rem;
  opacity: 0.7;
  margin-top: 0.15rem;
  text-align: right;
}
.chat-input textarea {
  resize: none;
}
</style>
