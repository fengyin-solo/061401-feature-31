<script setup lang="ts">
import type { StatRisk } from '@/types/game'

interface Props {
  stats: StatRisk[]
}

defineProps<Props>()

function getBarWidth(value: number, max: number): string {
  const percent = Math.max(0, Math.min(100, (value / max) * 100))
  return `${percent}%`
}

function getLevelBadge(level: StatRisk['level']): { label: string; class: string } {
  switch (level) {
    case 'critical':
      return { label: '危险', class: 'bg-red-500/20 text-red-400 border-red-500/50' }
    case 'warning':
      return { label: '警告', class: 'bg-orange-500/20 text-orange-400 border-orange-500/50' }
    case 'normal':
      return { label: '注意', class: 'bg-yellow-500/20 text-yellow-400 border-yellow-500/50' }
    case 'safe':
      return { label: '良好', class: 'bg-green-500/20 text-green-400 border-green-500/50' }
  }
}

function getLevelBorder(level: StatRisk['level']): string {
  switch (level) {
    case 'critical':
      return 'border-l-red-500'
    case 'warning':
      return 'border-l-orange-500'
    case 'normal':
      return 'border-l-yellow-500'
    case 'safe':
      return 'border-l-green-500'
  }
}
</script>

<template>
  <div class="bg-game-card rounded-2xl p-6 border border-game-border shadow-xl">
    <h2 class="text-xl font-bold text-white mb-5 flex items-center gap-2">
      <span>📊</span>
      <span>生存状态</span>
      <span class="ml-auto text-xs font-normal text-gray-400">按风险排序</span>
    </h2>
    <div class="space-y-4">
      <div
        v-for="stat in stats"
        :key="stat.key"
        :class="[
          'group rounded-xl p-3 border-l-4 bg-gray-800/50 transition-all duration-300',
          getLevelBorder(stat.level),
          stat.level === 'critical' ? 'animate-pulse-soft' : '',
        ]"
      >
        <div class="flex items-center justify-between mb-1.5">
          <div class="flex items-center gap-2">
            <span class="text-lg">{{ stat.icon }}</span>
            <span :class="[stat.color, 'font-medium text-sm']">{{ stat.label }}</span>
            <span
              :class="[
                'text-xs px-2 py-0.5 rounded-full border font-medium',
                getLevelBadge(stat.level).class,
              ]"
            >
              {{ getLevelBadge(stat.level).label }}
            </span>
          </div>
          <div class="flex items-center gap-2">
            <span class="text-xs text-gray-500">风险 {{ Math.round(stat.risk) }}%</span>
            <span
              :class="[
                stat.color,
                'font-bold text-sm tabular-nums',
              ]"
            >
              {{ Math.round(stat.value) }}
            </span>
          </div>
        </div>
        <div class="h-2.5 bg-gray-800 rounded-full overflow-hidden">
          <div
            :class="[stat.barColor, 'h-full rounded-full transition-all duration-300 ease-out']"
            :style="{ width: getBarWidth(stat.value, stat.max) }"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>
