<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Alves Personalizados</title>
<link rel="stylesheet" href="programa.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</head>
<body>
<header>
  <div class="header-left">
    <img src="logo.png" alt="logo" class="logo">
  </div>
  <div class="header-center">
  <h1>Alves Personalizados</h1>
  <nav>
    <button onclick="mostrar('clientes')">Clientes</button>
    <button onclick="mostrar('produtos')">Produtos</button>
    <button onclick="mostrar('vendas')">Vendas</button>
    <button onclick="mostrar('registrosVendas')">Registros</button>
  </nav>
</header>

<main>
  <!-- Clientes -->
  <section id="clientes" class="view">
    <h2>Cadastro de Clientes</h2>
    <div class="form">
      <input type="text" id="nomeCliente" placeholder="Nome *">
      <input type="text" id="telefoneCliente" placeholder="Telefone">
      <button id="btnCadastrarCliente">Cadastrar Cliente</button>
    </div>
    <h3>Lista de Clientes</h3>
    <table id="tabelaClientes">
      <thead>
        <tr>
          <th>Nome</th>
          <th>Telefone</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </section>

  <!-- Produtos -->
  <section id="produtos" class="view">
    <h2>Cadastro de Produtos</h2>
    <div class="form">
      <input type="text" id="nomeProduto" placeholder="Nome do Produto *">
      <input type="number" id="quantidadeProduto" placeholder="Quantidade *">
      <input type="number" id="valorCompraProduto" placeholder="Valor de Compra (R$) *" step="0.01">
      <input type="number" id="valorVendaProduto" placeholder="Valor de Venda (R$) *" step="0.01">
      <button id="btnCadastrarProduto">Cadastrar Produto</button>
    </div>
    <h3>Lista de Produtos</h3>
    <table id="tabelaProdutos">
      <thead>
        <tr>
          <th>Produto</th>
          <th>Qtd</th>
          <th>Valor Compra</th>
          <th>Valor Venda</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <p class="total">Total Compra: <span id="totalCompra">R$ 0,00</span> | Total Venda: <span id="totalVenda">R$ 0,00</span></p>
  </section>

  <!-- Vendas -->
  <section id="vendas" class="view active">
    <h2>Registrar Venda</h2>
    <div class="form">
      <select id="clienteSelect"></select>
      <select id="produtoSelect"></select>
      <input type="number" id="quantidadeVenda" placeholder="Quantidade">
      <select id="formaPagamento">
        <option value="Dinheiro">Dinheiro</option>
        <option value="Cartão">Cartão</option>
        <option value="Pix">Pix</option>
      </select>
      <button id="btnVender">Vender</button>
    </div>
    <h3>Resumo das Vendas</h3>
    <table id="tabelaVendas">
      <thead>
        <tr>
          <th>Data</th>
          <th>Cliente</th>
          <th>Produto</th>
          <th>Qtd</th>
          <th>Preço</th>
          <th>Total</th>
          <th>Pagamento</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <p class="total">Total Geral: <span id="totalGeral">R$ 0,00</span></p>
  </section>

  <!-- Registros -->
  <section id="registrosVendas" class="view">
    <h2>Registros de Vendas</h2>
    <table id="tabelaRegistros">
      <thead>
        <tr>
          <th>Data</th>
          <th>Cliente</th>
          <th>Produto</th>
          <th>Qtd</th>
          <th>Preço</th>
          <th>Total</th>
          <th>Pagamento</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <p class="total">Total Geral: <span id="totalGeralRegistros">R$ 0,00</span></p>
    <button onclick="exportarRelatorio()">Exportar PDF</button>
  </section>
</main>

<!-- Modal Edição -->
<div id="modalEditar" class="modal">
  <div class="modal-content">
    <h3 id="modalEditarTitulo">Editar</h3>
    <input type="text" id="modalEditarNome" placeholder="Nome">
    <input type="text" id="modalEditarTelefone" placeholder="Telefone">
    <input type="number" id="modalEditarQuantidade" placeholder="Quantidade">
    <input type="number" id="modalEditarCompra" placeholder="Valor de Compra" step="0.01">
    <input type="number" id="modalEditarVenda" placeholder="Valor de Venda" step="0.01">
    <button id="btnSalvarEdicao">Salvar</button>
    <button id="btnCancelarEdicao">Cancelar</button>
  </div>
</div>

<!-- Modal Exclusão -->
<div id="modalExcluir" class="modal">
  <div class="modal-content">
    <p>Deseja realmente excluir?</p>
    <button id="btnConfirmarExcluir">Sim</button>
    <button id="btnCancelarExcluir">Não</button>
  </div>
</div>

<script src="programa.js"></script>
</body>
</html>

