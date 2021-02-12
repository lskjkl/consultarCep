<template>
  <div class="content">
      <h1 class="title">Resultado para o CEP: {{cep}}</h1>
      <table class="table-custom table-responsive">
        <thead class="head">
            <th class="thead">Bairro</th>
            <th class="thead">Rua</th>
            <th class="thead">Cidade</th>
            <th class="thead">Estado</th>
        </thead>
      <tbody class="body">
          <td class="tbody">{{bairro}}</td>
          <td class="tbody">{{rua}}</td>
          <td class="tbody">{{cidade}}</td>
          <td class="tbody">{{estado}}</td>
      </tbody>
    </table>
    </div>
</template>

<script>
export default {
    props: {
        bairro: String,
        rua: String,
        cidade: String,
        estado: String,
        cep: String
    }
}
</script>

<style>
.table-custom{
    width: 100%;
    max-width: 500px;
    background-color: #f2f4f5;
}

.table-custom .head{
  background: #bd07ca;
}
.table-custom .head .thead{
  color: #f2f4f5;
}

table td, table th {
    border-width: 0 0 1px;
    padding: 4px 5px;
}

@media (max-width: 991px){ 
  .table-responsive{
    display: block;
    position: relative;
    width: 100%;
    max-width: 500px;
    margin:15px 5px 20px;
  }

  .table-responsive thead,
  .table-responsive tbody,
  .table-responsive th,
  .table-responsive td,
  .table-responsive tr{
    display: block;
  }

  .table-responsive thead{
    float: left;
  }

  .table-responsive tbody{
    width: auto;
    position: relative;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    white-space: nowrap;
  }

  .table-responsive tbody tr{
    display: inline-block;
  }

  .table td:last-child{
     border-right: #999999 solid 1px;
  }

  .content table thead td, .content table thead th{
    border-width: 0 0 1px;
  }
}
</style>