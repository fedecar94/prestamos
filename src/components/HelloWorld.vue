<template>
  <div>
    <h2 style="text-align: center;">Ingrese los datos para calcular el prestamo</h2>
    <br>
    <div class="container">
      <form>
        <div class="form-group row">
          <label for="monto" class="col-3 col-form-label">Monto</label>
          <div class="col-9">
            <input type="number" class="form-control" name="monto" id="monto" v-model="monto">
          </div>
        </div>
        <div class="form-group row">
          <label for="interes" class="col-3 col-form-label">Intereses</label>
          <div class="col-4">
            <input type="number" class="form-control" id="interes" v-model="interes">
          </div>
          <div class="col-5">
            <select v-model="interesTipo" class="form-control">
              <option>Anual</option>
              <option>Total</option>
            </select>
          </div>
        </div>
        <div class="form-group row">
          <label for="plazo" class="col-3 col-form-label">Plazo</label>
          <div class="col-4">
            <input type="number" class="form-control" id="plazo" v-model="plazo">
          </div>
          <div class="col-5">
            <select v-model="plazoTipo" class="form-control">
              <option>Años</option>
              <option>Meses</option>
              <option>Semanas</option>
            </select>
          </div>
        </div>
        <div class="form-group row">
          <label for="pago" class="col-3 col-form-label">Periodicidad de pagos</label>
          <div class="col-9">
            <select v-model="pago" class="form-control">
              <option>Semanal</option>
              <option>Mensual</option>
              <option>Bimensual</option>
              <option>Trimestral</option>
              <option>Semestral</option>
              <option>Anual</option>
            </select>
          </div>
        </div>
        <div class="form-group row">
          <label for="tipoPrestamo" class="col-3 col-form-label">Tipo de prestamo</label>
          <div class="col-9">
            <select v-model="tipoPrestamo" class="form-control">
              <option>Frances</option>
              <option>Aleman</option>
              <option>Americano</option>
            </select>
          </div>
        </div>
      </form>
    </div>
    <div style="text-align: center;">
      <br>
      <p>
        El prestamo es por Gs. {{ monto }} que se pagara en {{ plazo }} {{ plazoTipo }} de manera {{ pago }}, con un interes del {{ interes }}% {{ interesTipo }}
      </p>
      <br>
    </div>
    <div class="container">
      <table class="table table-sm">
        <thead style="text-align: center;">
          <tr>
            <th scope="col">Cuota</th>
            <th scope="col">Amortizacion</th>
            <th scope="col">Interes</th>
            <th scope="col">A pagar</th>
            <th scope="col">Deuda restante</th>
          </tr>
        </thead>
        <tbody style="text-align: right;">
          <tr v-for="item in tablaCuotas" v-bind:key="item.numero">
            <th scope="row">{{item.numero}}</th>
            <td>Gs. {{item.monto}}</td>
            <td>Gs. {{item.interes}}</td>
            <td>Gs. {{item.total}}</td>
            <td>Gs. {{item.restante}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <h2>Debug</h2>
      total años: {{ totalAnhos }}
      <br>
      total cuotas: {{ totalCuotas }}
      <br>
      interes anual: {{ interesAnual }}
      <br>
      interesEfectivo: {{ interesEfectivo }}
      <br>
      multiplicador de cuota: 
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      monto: 0,
      plazo: 0,
      plazoTipo: 'Años',
      pago: 'Mensual',
      interes: 0,
      interesTipo: 'Anual',
      tipoPrestamo: 'Frances'
    }
  },
  computed: {
    totalAnhos: function () {
      var plazo = this.plazo
      if (plazo === 0) {
        plazo = 1
      }
      if (this.plazoTipo === 'Años') {
        return plazo
      }
      if (this.plazoTipo === 'Meses') {
        return plazo / 12
      }
      return plazo / 52
    },
    totalCuotas: function () {
      if (this.plazo === 0) {
        return 0
      }
      if (this.plazoTipo === 'Semanas') {
        if (this.pago === 'Semanal') {
          return this.totalAnhos * 52
        }
      } else {
        if (this.pago === 'Mensual') {
          return this.totalAnhos * 12
        }
        if (this.pago === 'Bimensual') {
          return this.totalAnhos * 6
        }
        if (this.pago === 'Trimestral') {
          return this.totalAnhos * 4
        }
        if (this.pago === 'Semestral') {
          return this.totalAnhos * 2
        }
      }
      return this.totalAnhos
    },
    interesAnual: function () {
      if (this.interesTipo === 'Anual') {
        return this.interes
      }
      if (this.plazoTipo === 'Semanas') {
        if (this.pago === 'Semanal') {
          return (this.interes / this.totalCuotas) * 52
        }
      } else {
        if (this.pago === 'Mensual') {
          return (this.interes / this.totalCuotas) * 12
        }
        if (this.pago === 'Bimensual') {
          return (this.interes / this.totalCuotas) * 6
        }
        if (this.pago === 'Trimestral') {
          return (this.interes / this.totalCuotas) * 4
        }
        if (this.pago === 'Semestral') {
          return (this.interes / this.totalCuotas) * 2
        }
      }
      return 0
    },
    interesEfectivo: function () {
      var plazo = 1
      if (this.plazoTipo === 'Semanas') {
        if (this.pago === 'Semanal') {
          plazo = 52
        }
      } else {
        if (this.pago === 'Mensual') {
          plazo = 12
        }
        if (this.pago === 'Bimensual') {
          plazo = 6
        }
        if (this.pago === 'Trimestral') {
          plazo = 4
        }
        if (this.pago === 'Semestral') {
          plazo = 2
        }
      }
      return (Math.round((this.interesAnual / plazo)*1000000000))/100000000000
    },
    tablaCuotas: function () {
      var array = []
      var cuota = 0
      var restante = this.monto
      var prestamo = restante
      var interes = 0
      var amortiza = 0
      if (this.tipoPrestamo === 'Frances') {
        var numerador = this.interesEfectivo * Math.pow((1 + this.interesEfectivo), this.totalCuotas)
        var denominador = Math.pow((1 + this.interesEfectivo), this.totalCuotas) - 1
        cuota = Math.round(this.monto * (numerador / denominador))
        restante = cuota * this.totalCuotas
        array.push({
          'numero': 0,
          'monto': 0,
          'interes': 0,
          'total': 0,
          'restante': restante
        })
        for (let i = 0; i < this.totalCuotas; i++) {
          restante = restante - cuota
          interes = Math.round(prestamo * this.interesEfectivo)
          amortiza = cuota - interes
          prestamo = prestamo - amortiza
          array.push({
            'numero': i + 1,
            'monto': amortiza,
            'total': cuota,
            'interes': interes,
            'restante': restante
          })
        }
      }
      return array
    }
  }
}
</script>
