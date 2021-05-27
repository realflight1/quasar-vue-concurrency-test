<template>
  <q-page>
    <div>{{ number }}</div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { usePipeTask, useTask } from 'vue-concurrency';

export default defineComponent({
  name: 'PageIndex',
  async setup() {
    const addTask = useTask(function*(signal, a: number, b: number) {
      return a + b;
    });
    const add10Task = useTask(function*(signal, a: number) {
      return a + 10;
    });
    const add20Task = useTask(function*(signal, a: number) {
      return a + 20;
    });

    const pipeTask = usePipeTask(addTask, add10Task, add20Task);
    const number = await pipeTask.perform(100, 100);

    return { 
      number 
    };
  }
});
</script>
