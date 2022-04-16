<template>
  <v-container>
    <v-row>
      <v-col>
        Choose a monster ! :
        <!-- This should not report an error, yet vmodel and items will both reports typescript errors.
              For V-Model : Type 'Monster | undefined' is not assignable to type 'string | number | unknown[] | undefined'.
              Type 'Monster' is not assignable to type 'string | number | unknown[] | undefined'.ts(2322)
              index.d.ts(477, 13): The expected type comes from property 'modelValue' which is declared here on type 'IntrinsicAttributes & Partial<{ transition: string | boolean | (TransitionProps & { component?: Component<any, any, any, ComputedOptions, MethodOptions> | undefined; }); ... 10 more ...; openOnClear: boolean; }> & Omit<...>' 
              For items :
              Type '{ title: string; value: { name: string; shouts: string; lethal: boolean; }; }[]' is not assignable to type 'SelectItem[]'.
              Type '{ title: string; value: { name: string; shouts: string; lethal: boolean; }; }' is not assignable to type 'SelectItem'.ts(2322)
              index.d.ts(475, 13): The expected type comes from property 'items' which is declared here on type 'IntrinsicAttributes & Partial<{ transition: string | boolean | (TransitionProps & { component?: Component<any, any, any, ComputedOptions, MethodOptions> | undefined; }); ... 10 more ...; openOnClear: boolean; }> & Omit<...>'-->
        <v-select v-model="selectedMonster" :items="items" />
      </v-col>
      <v-col v-if="selectedMonster">
        <v-card :color="cardColor">
          <v-card-title>{{ selectedMonster.name }}</v-card-title>
          <v-card-header
            ><v-card-header-text
              >This monster shouts :
              <span class="font-weight-bold">{{
                selectedMonster.shouts
              }}</span></v-card-header-text
            ></v-card-header
          >
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts" setup>
import { ref, reactive, computed } from "vue";

interface Monster {
  name: string;
  shouts: string;
  lethal: boolean;
}

const monsters = reactive<Monster[]>([
  {
    name: "Piplup",
    shouts: "Tweet",
    lethal: false,
  },
  {
    name: "Pingu",
    shouts: "NOOT!",
    lethal: true,
  },
]);

const items = computed(() =>
  monsters.map((m) => ({
    title: m.name,
    value: m,
  }))
);
const selectedMonster = ref<Monster>();

const cardColor = computed(() => {
  if (selectedMonster.value && selectedMonster.value.lethal) {
    return "error";
  } else {
    return "primary";
  }
});
</script>
