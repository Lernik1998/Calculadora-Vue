<template>
  <table class="calculadora">
    <tr>
      <!-- Mostramos los operandos y el resultado  -->
      <td colspan="4">
        <span id="valor">Operación: {{ op1 }} {{ operacion }} {{ op2 }}</span>
        <span id="resultado">{{ resultado || 0 }}</span>
      </td>
    </tr>
    <tr>
      <td><button id="siete" value="7" @click="calcular(7)">7</button></td>
      <td><button id="ocho" value="8" @click="calcular(8)">8</button></td>
      <td><button id="nueve" value="9" @click="calcular(9)">9</button></td>
      <td>
        <button id="division" value="/" class="op" @click="calcular('/')">
          /
        </button>
      </td>
    </tr>
    <tr>
      <td><button id="cuatro" value="4" @click="calcular(4)">4</button></td>
      <td><button id="cinco" value="5" @click="calcular(5)">5</button></td>
      <td><button id="seis" value="6" @click="calcular(6)">6</button></td>
      <td>
        <button id="multiplicacion" value="*" class="op" @click="calcular('*')">
          *
        </button>
      </td>
    </tr>
    <tr>
      <td><button id="uno" value="1" @click="calcular(1)">1</button></td>
      <td><button id="dos" value="2" @click="calcular(2)">2</button></td>
      <td><button id="tres" value="3" @click="calcular(3)">3</button></td>
      <td>
        <button id="resta" value="-" class="op" @click="calcular('-')">
          -
        </button>
      </td>
    </tr>
    <tr>
      <td>
        <button id="igual" value="=" class="op" @click="calcular('=')">
          =
        </button>
      </td>
      <td>
        <button
          id="reset"
          value="C"
          @click="calcular('C')"
          :disabled="!hayNums"
        >
          C
        </button>
      </td>
      <td><button id="cero" value="0" @click="calcular(0)">0</button></td>
      <td>
        <button id="suma" value="+" class="op" @click="calcular('+')">+</button>
      </td>
    </tr>
  </table v-bind:>
</template>

<script setup>
// Importaciones
import { ref, computed } from "vue";

// Variablas para almacenar los operandos y la operacion
var op1 = ref(null);
var op2 = ref(null);
var operacion = ref(null);
var resultado = ref(null);
// Hago un array de operaciones, por si quisieramos ampliar el codigo
let arrayOperaciones = ["/", "*", "-", "+"];

// Si no hay números que estén los botones de C o Delete deshabilidatos.
let hayNums = computed(() => op1.value || op2.value);

// Funcion para calcular
function calcular(valor) {
  // Si es número
  if (!isNaN(valor)) {
    // Si no hay operación todavia, lo almaceno en op1
    if (operacion.value === null) {
      op1.value = (op1.value || "") + valor;
    } else {
      // Si ya hay operación, lo almaceno en op2
      op2.value = (op2.value || "") + valor;
    }

  } else if (arrayOperaciones.includes(valor)) {  // Es una operación
   
    if (op1.value !== null) {
      operacion.value = valor;
    }

  } else if (valor === "C") {
    borrarTodo();
  } else if (valor === "=" || valor === "Enter") {
    // Calculo el resultado con el eval
    try {
        // Construir eval
        resultado.value = eval(`${op1.value} ${operacion.value} ${op2.value}`);
        // Actualizar op1 con el resultado para continuar haciendo calculos
        op1.value = resultado.value;
        // Actualizo valores de op2 y resultado a null para poder seguir calculando
        operacion.value = null;
        op2.value = null;
      } catch (error) {
        alert("Debe de indicar valores para realizar la operación");
        borrarTodo();
      }
  }
}

function borrarTodo() {
  // Borrar todos los datos
  op1.value = null;
  op2.value = null;
  operacion.value = null;
  resultado.value = null;
}

// CAPTURAR MEDIANTE TECLADO

// Para detectar teclas presionadas, para que detecte el supr, ya que keypress falla
window.addEventListener("keydown", botonPresionado);

// Manejo los eventos de teclado
function botonPresionado(event) {
  // Capturo la tecla pulsada
  let teclaPulsada = event.key;

  // Verifico si es Enter
  if (teclaPulsada === "Enter") {
    event.preventDefault(); // Bloquear función por defecto
    calcular(teclaPulsada);
    // Si es Delete
  } else if (teclaPulsada === "Delete") {
    event.preventDefault();
    borrarTodo();
  } else {
    calcular(teclaPulsada);
  }
}

</script>

<style scoped>
.calculadora {
  /* display: block; */
  margin: 0 auto;
  padding: 20px;
  background-color: red;
  width: 345px;
  height: 500px;
  border-radius: 25px;
}
.calculadora td button {
  display: block;
  width: 70px;
  height: 70px;
  font-size: 25px;
}
#creditos {
  display: block;
  padding-top: 20px;
  color: #0d0c0c;
  text-align: center;
  width: 300px;
}
#resultado {
  display: block;
  text-align: center;
  font-size: 40px;
  margin-bottom: 50px;
  width: 300px;
  height: 100px;
  line-height: 100px;
  background-color: #fff;
  border-radius: 25px;
  color: black;
}
#valor {
  display: block;
  text-align: right;
  font-size: 15px;
  margin-bottom: 10px;
  background-color: #fff;
  border-radius: 5px;
  color: black;
  padding: 10px;
}
</style>