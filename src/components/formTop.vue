<template>
  <div>
    <h4>FORMULARIO SEM TITULO</h4>
    <div class="q-gutter-sm q-py-md">
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
    <!-- <q-select  outlined v-model="porte" :options="tipo" label="Porte" /> -->
    <div class="row">
      <!-- da pra fazer um v-for? -->
      <div class="col-3 q-px-sm">
        <q-select
          outlined
          v-model="marca"
          :options="marcaOptions"
          label="Marca"
          option-label="nome"
          @update:model-value="loadModelos"
        />
      </div>
      <div class="col-3 q-px-sm">
        <q-select
          outlined
          v-model="modelo"
          :options="modeloOptions"
          label="Modelo"
          option-label="nome"
          @update:model-value="loadAnos"
        />
      </div>
      <div class="col-3 q-px-sm">
        <q-select
          outlined
          v-model="ano"
          :options="anoOptions"
          label="Ano"
          option-label="nome"
        />
      </div>
      <div class="col-3 q-px-sm">
        <q-select
          outlined
          v-model="ano"
          :options="anoOptions"
          :label="tipo == 'maquinario' ? 'PIN' : 'placa'"
          option-label="placa"
          @update:model-value="maquinas"
        />
      </div>
    </div>
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
    <div class="row q-py-md">
      <q-checkbox class="col-3 q-px-sm" v-model="seguro" label="Seguro" />

      <q-file
        :class="seguro == true ? 'col-3 q-px-sm' : 'hidden'"
        style="max-width: 400px"
        filled
        v-model="model"
        label="Upload"
        accept=".pdf, image/*"
      />
    </div>
  </div>
</template>

<script>
import { ref, defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  name: "formTop",
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
    };
  },
});
</script>

<style scoped></style>
