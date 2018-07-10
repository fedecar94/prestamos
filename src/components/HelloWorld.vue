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
            <input type="text" class="form-control" id="interes" v-model="interes">
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
      <h4>
        El prestamo es por {{ monto | currency('Gs. ', 0, { thousandsSeparator: '.' , decimalSeparator: ',' })}}
        <br>Que se pagara en {{ plazo }} {{ plazoTipo }} de manera {{ pago }}
        <br>Con un interes del {{ interes }}% {{ interesTipo }} utilizando el sistema {{ tipoPrestamo }}
      </h4>
      <br>
    </div>
    <div class="container">
      <table class="table table-sm table-striped table-responsive-lg">
        <thead style="text-align: center;" class="thead-light">
          <tr>
            <th scope="col-1">Cuota</th>
            <th scope="col-3">Amortizacion</th>
            <th scope="col-2">Interes del periodo</th>
            <th scope="col-3">Cuota a pagar</th>
            <th scope="col-3">Deuda restante</th>
          </tr>
        </thead>
        <tbody style="text-align: right;" v-if="mostrarTabla">
          <tr v-for="item in tablaCuotas" v-bind:key="item.numero">
            <th scope="row">{{item.numero}}</th>
            <td>{{item.monto | currency('Gs. ', 0, { thousandsSeparator: '.' , decimalSeparator: ',' })}}</td>
            <td>{{item.interes | currency('Gs. ', 0, { thousandsSeparator: '.' , decimalSeparator: ',' })}}</td>
            <td>{{item.total | currency('Gs. ', 0, { thousandsSeparator: '.' , decimalSeparator: ',' })}}</td>
            <td>{{item.restante | currency('Gs. ', 0, { thousandsSeparator: '.' , decimalSeparator: ',' })}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <footer>
        <br>
        <p>Puede visitar <a href="https://www.a1.com.py">mi pagina</a> para ver otros proyectos</p>
      </footer>
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
      if (this.pago === 'Semanal') {
        return this.totalAnhos * 52
      }
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
      return this.totalAnhos
    },
    interesAnual: function () {
      if (this.interesTipo === 'Anual') {
        return this.interes
      }
      if (this.pago === 'Semanal') {
        return (this.interes / this.totalCuotas) * 52
      }
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
      return 0
    },
    interesEfectivo: function () {
      var plazo = 1
      if (this.pago === 'Semanal') {
        plazo = 52
      }
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
      return (Math.round((this.interesAnual / plazo) * 1000000000)) / 100000000000
    },
    tablaCuotas: function () {
      var array = []
      var cuota = 0
      var restante = this.monto
      var interes = 0
      var amortiza = 0
      var total = 0
      array.push({
        'numero': 0,
        'monto': '',
        'interes': '',
        'total': '',
        'restante': restante
      })
      if (this.tipoPrestamo === 'Frances') {
        var prestamo = restante
        var numerador = this.interesEfectivo * Math.pow((1 + this.interesEfectivo), this.totalCuotas)
        var denominador = Math.pow((1 + this.interesEfectivo), this.totalCuotas) - 1
        cuota = Math.round(this.monto * (numerador / denominador))
        restante = cuota * this.totalCuotas
        total = restante
        array[0].restante = restante
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
        array.push({
          'numero': 'Total',
          'monto': '',
          'interes': '',
          'total': total,
          'restante': ''
        })
      }
      if (this.tipoPrestamo === 'Aleman') {
        amortiza = this.monto / this.totalCuotas
        for (let i = 0; i < this.totalCuotas; i++) {
          interes = restante * this.interesEfectivo
          cuota = amortiza + interes
          restante = restante - amortiza
          array.push({
            'numero': i + 1,
            'monto': amortiza,
            'interes': interes,
            'total': cuota,
            'restante': restante
          })
          total = total + cuota
        }
        array.push({
          'numero': 'Total',
          'monto': '',
          'interes': '',
          'total': total,
          'restante': ''
        })
      }
      if (this.tipoPrestamo === 'Americano') {
        interes = restante * this.interesEfectivo
        for (let i = 1; i < this.totalCuotas; i++) {
          array.push({
            'numero': i,
            'monto': amortiza,
            'interes': interes,
            'total': interes,
            'restante': restante
          })
          total = total + interes
        }
        amortiza = parseInt(restante) + parseInt(interes)
        array.push({
          'numero': this.totalCuotas,
          'monto': restante,
          'interes': interes,
          'total': amortiza,
          'restante': 0
        })
        total = total + amortiza
        array.push({
          'numero': 'Total',
          'monto': '',
          'interes': '',
          'total': total,
          'restante': ''
        })
      }
      return array
    },
    mostrarTabla: function () {
      var anhos = this.plazoTipo === 'Años'
      var meses = this.plazoTipo === 'Meses'
      var semanas = this.plazoTipo === 'Semanas'
      var semanal = this.pago === 'Semanal' && (anhos || semanas)
      var mensual = (this.pago === 'Mensual' || this.pago === 'Bimensual' || this.pago === 'Trimestral' || this.pago === 'Semestral') && (anhos || meses)
      var anual = this.pago === 'Anual' && this.plazoTipo === 'Años'
      return semanal || mensual || anual
    }
  }
}
</script>
