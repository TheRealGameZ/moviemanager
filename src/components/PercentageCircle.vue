<script>
import gsap from "gsap";

export default {
  props: ["percentage"],
  mounted() {
    this.animateCircle();
  },
  computed: {
    circumference() {
      return 2 * Math.PI * 22; // Aktualisierter Radius
    }
  },
  methods: {
    animateCircle() {
      var $statCircle = document.querySelectorAll(".stat-circle circle.progress");

      for (var circle of $statCircle) {
        var circumference = 2 * Math.PI * circle.r.baseVal.value;
        var off = circumference * (1 - parseFloat(circle.dataset.percentage) / 100);

        gsap.to(circle, {
          duration: 0.4,
          strokeDashoffset: off,
          rotation: -90,
          transformOrigin: "50% 50%"
        });
      }
    },
  }
}
</script>

<template>
  <svg class="stat-circle" width="50" height="50"> <!-- Aktualisierte Größe -->
    <circle class="bg" cx="25" cy="25" r="22"></circle> <!-- Aktualisierter Radius -->
    <circle class="progress" cx="25" cy="25" r="22" :stroke-dasharray="circumference" :stroke-dashoffset="circumference" :data-percentage="percentage"></circle>
    <text x="50%" y="50%" dy=".3em" style="font-size: 12px;">{{ percentage }}%</text> <!-- Aktualisierte Textgröße -->
  </svg>
</template>

<style scoped>

.stat-circle circle.bg {
  fill: none;
  stroke: rgba(87, 87, 87, 0.60);
  stroke-width: 4;
}

.stat-circle circle.progress {
  fill: none;
  stroke: #f5c014;
  stroke-width: 7;
  stroke-linecap: round;
}

.stat-circle text {
  font-size: 12px; /* Aktualisierte Textgröße */
  text-anchor: middle;
  fill: #555;
}
</style>
