<script setup lang="ts">
import { reactive, toRefs, computed } from "vue";

// declare a reactive object
const character = reactive({
  name: "Mario",
  stats: {
    hp: 10,
    mp: 6,
    strength: 8,
  },
  inventory: ["portion", "sword"],
});

// destructure a reactive object but retain reactivity with the toRefs function
const { name, stats, inventory } = toRefs(character);

// function to the strength property by 1
function levelup() {
  stats.value.strength++;
}

// take Damage function to reduce hp by 1
function takeDamage() {
  if (stats.value.hp > 0) {
    stats.value.hp--;
  }
}

/* check if the inventory function has potion & 
    increment hp by 20 and also mutate the inventory 
    array and remove it
*/
function usePortion(inventory: string[]) {
  const portionIndex = inventory.findIndex((item) =>
    item.toLowerCase().includes("portion")
  );

  if (portionIndex !== -1) {
    stats.value.hp += 20;

    inventory.splice(portionIndex, 1);
  }
}

// computed value returns true if hp is > 0 and false if not
const isAlive = computed(() => (stats.value.hp > 0 ? true : false));

const characterLevel = computed(() =>
  stats.value.strength > 10 ? "Veteran" : "Rookie"
);
</script>

<template>
  <div class="card">
    <div class="card-wrapper">
      <!-- CHARACTER CARD -->
      <div class="character-card">
        <div class="about">
          <h1 class="name">
            {{ name }}
          </h1>

          <p>{{ characterLevel }}</p>
        </div>

        <section class="stats">
          <h2>Stats</h2>
          <ul>
            <li>
              <span>❤️ HP</span><strong>{{ stats.hp }}</strong>
            </li>
            <li>
              <span>🔮 MP</span><strong>{{ stats.mp }}</strong>
            </li>
            <li>
              <span>💪 STR</span><strong>{{ stats.strength }}</strong>
            </li>
          </ul>
        </section>

        <section class="inventory">
          <h2>Inventory</h2>
          <ul>
            <li v-for="(item, i) in inventory" :key="i">
              {{ item }}
            </li>
          </ul>
        </section>

        <section v-if="isAlive" class="actions">
          <button @click="levelup">Level Up</button>
          <button @click="takeDamage">Take Damage</button>
          <button @click="usePortion(inventory)">Use Potion</button>
        </section>
      </div>

      <!--overlay-->
      <div v-if="!isAlive" class="game-over-overlay">
        <h2>GAME OVER</h2>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  background-color: cornsilk;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* about */
.about {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* shared footprint */
.card-wrapper {
  position: relative;
  width: 320px;
  height: 400px;
}

/* character card */
.character-card {
  position: absolute;
  inset: 0;
  padding: 10px;
  background-color: aliceblue;
  border: 2px solid #222;
  border-radius: 8px;
  font-family: system-ui, sans-serif;
  z-index: 1;
}

/* inventory */
.inventory h2 {
  text-align: center;
}

.inventory ul {
  display: flex;
  justify-content: space-around;
}

/* game over screen */
.game-over-overlay {
  position: absolute;
  inset: 0;
  background-color: aliceblue;
  border: 2px solid #222;
  border-radius: 8px;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.game-over-overlay h2 {
  font-size: 1.6rem;
  color: crimson;
}
</style>
