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
      <table class="table">
        <thead>
          <tr>
            <td>Numero de cuota</td>
            <td>Pago cuota</td>
            <td>Pago interes</td>
            <td>Pago total</td>
            <td>Monto restante</td>
          </tr>
        </thead>
        <tbody style="text-align: right;">
          <tr v-for="item in tablaCuotas" v-bind:key="item.numero">
            <td>{{item.numero}}</td>
            <td>{{item.monto}}</td>
            <td>{{item.interes}}</td>
            <td>{{item.total}}</td>
            <td>{{item.restante}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <h2>Debug</h2>
      {{ totalAnhos }}
      <br>
      {{ totalCuotas }}
      <br>
      {{ montoCuotas }}
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
        return plazo/12
      }
      return plazo/52
    },
    totalCuotas: function () {
      if (this.plazo === 0) {
        return 1
      }
      if (this.pago === 'Semanal') {
        return this.totalAnhos*52
      }
      if (this.pago === 'Mensual') {
        return this.totalAnhos*12
      }
      if (this.pago === 'Bimensual') {
        return this.totalAnhos*6
      }
      if (this.pago === 'Trimestral') {
        return this.totalAnhos*4
      }
      if (this.pago === 'Semestral') {
        return this.totalAnhos*2
      }
      return this.totalAnhos
    },
    montoCuotas: function () {
      if (this.tipoPrestamo === 'Americano') {
        return 0
      }
      return this.monto / this.totalCuotas
    },
    tablaCuotas: function () {
      var array = []
      var totalPago = 0
      var restante = this.monto
      for (let id = 0; id < 12; id++) {
        totalPago = this.montoCuotas
        restante = restante - totalPago
        array.push(
          {
            'numero': id+1,
            'monto': this.montoCuotas,
            'interes': 1,
            'total': totalPago,
            'restante': restante
          }
        )      
      }
      return array
    }
  }
}
</script>
