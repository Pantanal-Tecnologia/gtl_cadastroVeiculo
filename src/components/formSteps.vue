<template>
  <div class="text-center">
    <h4 class="">Formulário</h4>
  </div>
  <div class="q-pa-md" style="width: 100%">
    <q-stepper
      v-model="step"
      ref="stepper"
      color="primary"
      animated
      vertical
      style="max-width: 700px"
      class="q-mx-auto"
    >
      <q-step :name="1" title="Selecione tipo" icon="settings" :done="step > 1">
        <div class="q-gutter-sm q-py-md text-center">
          <q-radio
            @update:model-value="loadMarcas"
            v-model="tipo"
            val="carros"
            label="Carros"
          />
          <q-radio
            @update:model-value="loadMarcas"
            v-model="tipo"
            val="motos"
            label="Motos"
          />
          <q-radio
            @update:model-value="loadMarcas"
            v-model="tipo"
            val="caminhoes"
            label="Caminhões"
          />
          <q-radio v-model="tipo" val="maquinario" label="Maquinario" />
        </div>
      </q-step>

      <q-step
        :name="2"
        title="Selecione marca"
        caption="Optional"
        icon="create_new_folder"
        :done="step > 2"
      >
        <q-select
          class="q-mx-auto"
          outlined
          v-model="marca"
          :options="marcaOptions"
          label="Marca"
          option-label="nome"
          @update:model-value="loadModelos"
        />
      </q-step>

      <q-step
        :name="3"
        title="Selecione modelo"
        icon="assignment"
        :done="step > 3"
      >
        <q-select
          outlined
          v-model="modelo"
          :options="modeloOptions"
          label="Modelo"
          option-label="nome"
          @update:model-value="loadAnos"
        />
      </q-step>

      <q-step
        :name="4"
        title="Selecione ano"
        icon="assignment"
        :done="step > 4"
      >
        <q-select
          outlined
          v-model="ano"
          :options="anoOptions"
          label="Ano"
          option-label="ano"
          @update:model-value="loadAnos"
        />
      </q-step>

      <q-step
        :name="5"
        title="Selecione placa ou PIN"
        icon="add_comment"
        :done="step > 5"
      >
        <q-select
          outlined
          v-model="placa"
          :options="idk"
          :label="tipo == 'maquinario' ? 'PIN' : 'placa'"
          option-label="placa"
          @update:model-value="maquinas"
        />
      </q-step>

      <q-step
        :name="6"
        title="Documento e imagem do veículo"
        icon="add_comment"
        :done="step > 6"
      >
        <div class="row q-py-md">
          <q-file
            class="col-6 q-px-sm"
            filled
            v-model="docveiculo"
            label="Documento do veículo"
            accept=".pdf, image/*"
            loading="true"
          />
          <q-file
            class="col-6 q-px-sm"
            filled
            v-model="fotveiculo"
            label="Imagem do veiculo"
            accept="image/*"
          />
        </div>
      </q-step>
      <q-step :name="7" title="Seguro" icon="add_comment">
        <div class="row q-py-md">
          <q-checkbox class="col-3 q-px-sm" v-model="seguro" label="Seguro" />

          <q-file
            :class="seguro == true ? 'q-px-sm' : 'hidden'"
            style="max-width: 800px"
            filled
            v-model="model"
            label="Upload"
            accept=".pdf, image/*"
          />
        </div>
      </q-step>

      <template v-slot:navigation>
        <q-stepper-navigation>
          <q-btn
            @click="$refs.stepper.next()"
            color="primary"
            :label="step === 7 ? 'Finish' : 'Continue'"
          />
          <q-btn
            v-if="step > 1"
            flat
            color="primary"
            @click="$refs.stepper.previous()"
            label="Back"
            class="q-ml-sm"
          />
        </q-stepper-navigation>
      </template>
    </q-stepper>
    <!-- <apexchart type="line" height="700" :options="apexOptions" :series="serie"></apexchart> -->
  </div>
</template>

<script>
import { ref, defineComponent } from "vue";
import axios from "axios";
export default defineComponent({
  name: "formSteps",
  setup() {
    const tipo = ref(null);
    const marca = ref(null);
    const marcaOptions = ref([]);
    const modelo = ref(null);
    const modeloOptions = ref([]);
    const ano = ref(null);
    const anoOptions = ref([]);

    const loadMarcas = (value) => {
      console.log(value, "é essa peça?");
      axios
        .get(`https://parallelum.com.br/fipe/api/v1/${value}/marcas`)
        .then((res) => (marcaOptions.value = res.data));
    };

    const loadModelos = (value) => {
      console.log(marca.value.codigo);
      axios
        .get(
          `https://parallelum.com.br/fipe/api/v1/${tipo.value}/marcas/${value.codigo}/modelos`
        )
        .then((res) => {
          modeloOptions.value = res.data.modelos;
        });
    };

    const loadAnos = (value) => {
      console.log("laskdsalkd" + modeloOptions.value);
      axios
        .get(
          `https://parallelum.com.br/fipe/api/v1/${tipo.value}/marcas/${marca.value.codigo}/modelos/${value.codigo}/anos`
        )
        .then((res) => {
          anoOptions.value = res.data;
        });
    };
    // const LoadMaquinario = () => {};

    const Maquinas = () => {};

    return {
      tipo,
      marca,
      marcaOptions,
      modelo,
      modeloOptions,
      ano,
      anoOptions,
      loadModelos,
      loadMarcas,
      loadAnos,
      Maquinas,
      docveiculo: ref(true),
      fotveiculo: ref(true),
      seguro: ref(false),
      model: ref(null),
      val: ref(true),
      step: ref(1),
    };
  },
});
</script>

<style scoped></style>
