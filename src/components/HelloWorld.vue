<template>
  <v-container>

    <template>
      <v-row class="pa-2">
        <v-col md="5">
          <v-row justify="center">
            <v-col md="12">

              <v-dialog
                  v-model="dialog"
                  width="600px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                      color="primary"
                      dark
                      v-bind="attrs"
                      v-on="on"
                  >
                    1. Instrucciones
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="text-h5">Distribución de bombillos</span>
                  </v-card-title>
                  <v-card-text>
                    <p>
                      Un electricista muy cuidadoso está tratando de iluminar al más bajo costo posible las
                      habitaciones de sus clientes. Las habitaciones que él ilumina, siempre son habitaciones
                      en forma de matriz (Ver figura 1). Como los bombillos son muy costosos, él trata de
                      iluminar toda la habitación utilizando la menor cantidad de los mismos.
                    </p>
                    <p>
                      Los bombillos sólo tiene alcance de iluminar la habitación de forma horizontal y de forma
                      vertical (Ver figura 2 y figura 3).
                    </p>
                    <p>
                      Lo único malo es que las habitaciones pueden tener paredes dentro de ellas, en cuyo
                      caso, se interrumpiría el alcance de la luz de un bombillo determinado (Ver figura 4).
                    </p>
                    <v-spacer></v-spacer>
                    <v-img
                        :src="require('../assets/figuras.png')"
                        class="my-3"
                        contain
                        height="250"
                    />
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                        color="green darken-1"
                        text
                        @click="dialog = false"
                    >
                      Cerrar
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>

            </v-col>
            <v-col md="12">
              <v-card
                  elevation="8"
                  outlined
                  shaped
              >
                <v-card-title>
                  2. Cargar matriz
                </v-card-title>
                <v-card-text>
                  <v-file-input
                      accept=".txt"
                      label="Click aquí para seleccionar archivo .txt"
                      outlined
                      v-model="chosenFile"
                  >
                  </v-file-input>
                </v-card-text>
                <v-card-actions>
                  <v-btn small right @click="importTxt">3. Leer archivo</v-btn>
                  <v-btn small class="success" right @click="calcular">4. Colocar bombillos</v-btn>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-divider
            vertical
        ></v-divider>
        <v-col md="7">
          <v-card raised>
            <v-card-text>
              <v-row>
                <v-col md="6">
                  <div v-show="mostrarHabitacion">
                    <v-card elevation="6">
                      <v-card-title>Habitación cargada</v-card-title>

                      <v-card-text>
                        <table>
                        <tbody>
                        <tr
                            v-for="(row, index) in habitacion"
                            :key="'fila'+index"
                        >
                          <td
                              style="border: 2px #AAA solid; width: 25px; height: 25px;"
                              v-for="(col, index) in row"
                              :key="'col'+index"
                          >
                            <v-icon v-if="col === 1">mdi-wall</v-icon>
                            <v-icon v-if="false" color="yellow darken-1">mdi-lightbulb-on</v-icon>
                          </td>
                        </tr>
                        </tbody>
                      </table>
                      </v-card-text>
                    </v-card>
                  </div>
                </v-col>
                <v-col md="6">
                  <div v-show="mostrarResultado">
                    <v-card elevation="6">
                      <v-card-title>Bombillos colocados</v-card-title>

                      <v-card-text>
                        <table>
                      <tbody>
                      <tr
                          v-for="(row, index) in arrayMultiple"
                          :key="'fila'+index"
                      >
                        <td
                            style="border: 2px #AAA solid; width: 25px; height: 25px;"
                            v-for="(col, index) in row"
                            :key="'col'+index"
                        >
                          <v-icon v-if="col === 1">mdi-wall</v-icon>
                          <v-icon v-else-if="col === 2" color="blue darken-3">mdi-lightbulb</v-icon>
                          <v-icon v-else-if="col === 3" color="blue lighten-4">mdi-checkbox-blank-circle-outline</v-icon>
                        </td>
                      </tr>
                      </tbody>
                    </table>
                      </v-card-text>
                    </v-card>
                  </div>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

    </template>
  </v-container>
</template>

<script>


export default {
  name: 'HelloWorld',

  data: () => ({
    arrayMultiple: [[]],
    dialog: false,
    text: "",
    chosenFile: null, // <- initialize the v-model prop
    data: null,
    row: 0,
    col: 0,
    mostrarResultado: false,
    mostrarHabitacion: false,
    habitacion: [[]],
  }),
  methods: {
    importTxt() {
      this.data = null
      this.arrayMultiple = [[]]
      this.habitacion = [[]]
      this.mostrarResultado = false
      if (!this.chosenFile) {
        this.data = "No File Chosen"
      }
      let reader = new FileReader();

      // Use the javascript reader object to load the contents
      // of the file in the v-model prop
      reader.readAsText(this.chosenFile);
      reader.onload = () => {
        this.data = reader.result;

        this.data = this.data.replaceAll(' ', '')
        let array1 = this.data.split('\r\n');
        console.log(array1);
        // This loop is for outer array
        for (let i = 0, l1 = array1.length; i < l1; i++) {
          console.log(array1[i])

          // Accessing each elements of inner-array
          let number = array1[i],
              output = [],
              sNumber = number.toString();
          output = sNumber.split("");

          this.arrayMultiple[i] = output.map(elem => parseInt(elem, 10))
        }
        this.habitacion = Array.from(this.arrayMultiple);

      }
      reader.onerror = (err) => console.log(err);


      console.log("Array multiple")
      console.log(this.arrayMultiple);
      if (Array.isArray((this.arrayMultiple))) {
        this.mostrarHabitacion = true;
        console.log("Seteando habitacion")
      } else {
        alert('NO se pudo cargar habitación')
      }

    },

    // NOTAS
    /*
    PRimero encontrar el primer 0 y evaluar si puede ilimunar en los 4 lados
    "hacer funciones para recorrer en los 4 lados validando primero si se puede recorrer
    " es decir si row es 0, no puede recorrer hacia arriba ni hacia la izquierda
    asignar 1 punto por cada lado,
    el ganador de la fila procede a ser asignado como foco y cambia los espacios que son ilumninados

     */
    procesar(lados) {
      this.row = 0
      this.col = 0
      while (this.row !== -1) {
        console.log("Aún se puede buscar")
        let ladosIluminar = 0;
        this.findZero();
        if (this.row === -1) {
          break;
        }

        ladosIluminar += this.recorrerArriba(lados);
        ladosIluminar += this.recorrerDerecha(lados);
        ladosIluminar += this.recorrerAbajo(lados);
        ladosIluminar += this.recorrerIzquierda(lados);

        if (ladosIluminar >= 2) {
          this.arrayMultiple[this.row][this.col] = 2
          this.recorrerArriba(lados, true)
          this.recorrerDerecha(lados, true)
          this.recorrerAbajo(lados, true)
          this.recorrerIzquierda(lados, true)
        } else {
          this.arrayMultiple[this.row][this.col] = -1
        }

      }
      this.reset();
    },
    calcular() {
      this.mostrarResultado = true

      // Aplicar las 3 reglas
      // Regla 1
      // Lados 2, espacios = 2
      this.procesar(2)
      // Lados 2, espacios 1
      this.procesar(1)
      // Lados 1
      this.procesar(0)
      console.log(this.arrayMultiple);
    },

    findZero() {
      let search = 0;
      this.row = this.arrayMultiple.findIndex(row => row.includes(search));
      if (this.row !== -1)
        this.col = this.arrayMultiple[this.row].indexOf(search);
      console.log("--------- Zero find -----------")
      console.log(this.row, this.col)
      if (this.row !== -1)
        console.log(this.arrayMultiple[this.row][this.col]);
      console.log("-------------------------------")
    },
    reset() {
      this.row = 0
      this.col = 0

      while (this.row !== -1) {


        let search = -1;
        this.row = this.arrayMultiple.findIndex(row => row.includes(search));
        if (this.row !== -1)
          this.col = this.arrayMultiple[this.row].indexOf(search);
        console.log("--------- -1 Found -----------")
        console.log(this.row, this.col)
        if (this.row !== -1) {

          console.log(this.arrayMultiple[this.row][this.col]);
          console.log("-------------------------------")
          this.arrayMultiple[this.row][this.col] = 0;

        }

        if (this.row === -1) {
          break;
        }
      }


    },

    recorrerArriba(regla, iluminar = false) {
      let puntosArriba = 0;
      if (this.row === 0) {
        console.log("no se puede recorrer hacia arriba")
        // No se puede recorrer
        return 0;
      } else {
        for (let i = this.row - 1; i >= 0; i--) {

          console.log("######## Punto en recorrido arriba ###########")
          console.log(this.row, this.col)
          console.log(this.arrayMultiple[i][this.col])

          if (this.arrayMultiple[i][this.col] > 0 && this.arrayMultiple[i][this.col] !== 3) {
            console.log("Cortando recorrido arriba")
            console.log(this.arrayMultiple[i][this.col])
            break;
          }
          if (this.arrayMultiple[i][this.col] < 1 || this.arrayMultiple[i][this.col] === 3) { // true: Se puede iluminar
            console.log("aqui se puede iluminar")
            console.log(i)
            if (iluminar) {
              this.arrayMultiple[i][this.col] = 3;
            }
            puntosArriba++;
          }

        }
        return (puntosArriba >= regla);
      }
    },

    recorrerDerecha(regla, iluminar = false) {
      let puntosDerecha = 0;
      if (this.col > this.arrayMultiple[this.row].length) {
        console.log("No se puede recorrer a la derecha")
        return 0;
      } else {
        for (let i = this.col + 1; i < this.arrayMultiple[this.row].length; i++) {

          if (this.arrayMultiple[this.row][i] > 0 && this.arrayMultiple[this.row][i] !== 3) { // true: no se puede iluminar
            break;
          }

          if (this.arrayMultiple[this.row][i] < 1 || this.arrayMultiple[this.row][i] === 3) { // true: se puede iluminar
            puntosDerecha++
            if (iluminar) {
              // Marcar iluminado
              this.arrayMultiple[this.row][i] = 3
            }
          }
        }
        return (puntosDerecha >= regla);
      }
    },

    recorrerAbajo(regla, iluminar = false) {
      let puntosAbajo = 0;
      console.log("print length en recorrer abajo")
      console.log(this.arrayMultiple.length);
      if (this.row >= this.arrayMultiple.length) {
        console.log("No se puede recorrer hacia abajo")
        return 0;
      } else {
        for (let i = this.row + 1; i < this.arrayMultiple.length; i++) {

          if (this.arrayMultiple[i][this.col] > 0 && this.arrayMultiple[i][this.col] !== 3) { // true: no se puede iluminar
            break;
          }

          if (this.arrayMultiple[i][this.col] < 1 || this.arrayMultiple[i][this.col] === 3) { // true: se puede iluminar
            puntosAbajo++
            if (iluminar) {
              this.arrayMultiple[i][this.col] = 3
            }
          }
        }
        return (puntosAbajo >= regla);
      }

    },

    recorrerIzquierda(regla, iluminar = false) {
      let puntosIzquierda = 0;
      if (this.col === 0) {
        console.log("no se puede recorrer hacia izquierda")
        // No se puede recorrer
        return 0;
      } else {
        for (let i = this.col - 1; i >= 0; i--) {
          if (this.arrayMultiple[this.row][i] > 0 && this.arrayMultiple[this.row][i] !== 3) {
            break;
          }
          if (this.arrayMultiple[this.row][i] < 1 || this.arrayMultiple[this.row][i] === 3) { // true: se puede iluminar
            console.log("aqui se puede iluminar")
            console.log(i)
            puntosIzquierda++;
            if (iluminar) {
              this.arrayMultiple[this.row][i] = 3
            }
          }

        }
        return (puntosIzquierda >= regla);
      }
    }

  },
  watch: {
    data() {
      console.log("Make array 2D");
      console.log(this.data)

    }
  },
}
</script>
