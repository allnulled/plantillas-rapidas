<template>
  <div class="ApplicationPage Component">
    <ztitle>Creador de plantillas</ztitle>
    <zlayout>
      <ztabslayout :tabs="['Plantilla', 'Variables', 'Resultado']">
        <ztabpanel>
          <zlayout>
            <zformtextarea :on-change="actualizar_variables" :initial-value="plantilla">Escribe la plantilla. Las { llaves curvadas } permiten las variables.</zformtextarea>
          </zlayout>
        </ztabpanel>
        <ztabpanel>
          <zlayout>
            <zpanel v-for="nombre_var in variables" v-bind:key="'variable-' + nombre_var">
              <zformfield :on-change="actualizar_valor(nombre_var)" :initial-value="valores[nombre_var]">{{ nombre_var }}</zformfield>
            </zpanel>
          </zlayout>
        </ztabpanel>
        <ztabpanel>
          <zlayout>
            <zformtextarea ref="salida" :disabled="true">El texto quedaría así:</zformtextarea>
          </zlayout>
        </ztabpanel>
      </ztabslayout>
    </zlayout>
  </div>
</template>
<script>
  export default {
    props: {},
    data() {
      return {
        regex_para_campos: /{([^}]*)}/gi,
        plantilla: "Hola, {Nombre de usuario}.",
        variables: [],
        valores: {},
        resultado: ""
      };
    },
    methods: {
      actualizar_variables(valor = this.plantilla) {
        this.plantilla = valor;
        let matches = this.plantilla.match(this.regex_para_campos);
        matches = matches ? matches.map(it => it.substr(1,it.length-2).trim()) : [];
        this.variables = matches;
      },
      actualizar_valor(nombre_var) {
        return valor => {
          this.valores[nombre_var] = valor;
          this.actualizar_resultado();
        };
      },
      actualizar_resultado() {
        const output = this.plantilla.replace(this.regex_para_campos, (token, match1) => {
          return this.valores[match1.trim()];
        });
        this.$refs.salida.value = output;
      }
    },
    watch: {},
    computed: {},
    beforeCreate() {},
    created() {},
    beforeMount() {},
    mounted() {
      this.actualizar_variables();
    },
    beforeUpdate() {},
    updated() {},
    beforeUnmount() {},
    unmounted() {},
  };
</script>
<style>
  
</style>