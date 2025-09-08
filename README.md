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

// ----------------------
// Variáveis iniciais
// ----------------------
let clientes = JSON.parse(localStorage.getItem("clientes")) || [];
let produtos = JSON.parse(localStorage.getItem("produtos")) || [];
let vendas = JSON.parse(localStorage.getItem("vendas")) || [];

// ----------------------
// Navegação
// ----------------------
function mostrar(viewId){
    document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
    document.getElementById(viewId).classList.add('active');
}
mostrar('vendas'); // view inicial

// ----------------------
// Elementos
// ----------------------
// Clientes
const tabelaClientes = document.querySelector("#tabelaClientes tbody");
const nomeCliente = document.getElementById("nomeCliente");
const telefoneCliente = document.getElementById("telefoneCliente");
const btnCadastrarCliente = document.getElementById("btnCadastrarCliente");
const clienteSelect = document.getElementById("clienteSelect");

// Produtos
const tabelaProdutos = document.querySelector("#tabelaProdutos tbody");
const nomeProduto = document.getElementById("nomeProduto");
const quantidadeProduto = document.getElementById("quantidadeProduto");
const valorCompraProduto = document.getElementById("valorCompraProduto");
const valorVendaProduto = document.getElementById("valorVendaProduto");
const btnCadastrarProduto = document.getElementById("btnCadastrarProduto");
const produtoSelect = document.getElementById("produtoSelect");
const totalCompraEl = document.getElementById("totalCompra");
const totalVendaEl = document.getElementById("totalVenda");

// Vendas
const tabelaVendas = document.querySelector("#tabelaVendas tbody");
const quantidadeVenda = document.getElementById("quantidadeVenda");
const formaPagamento = document.getElementById("formaPagamento");
const btnVender = document.getElementById("btnVender");
const totalGeralEl = document.getElementById("totalGeral");

// Registros
const tabelaRegistros = document.querySelector("#tabelaRegistros tbody");
const totalGeralRegistros = document.getElementById("totalGeralRegistros");

// ----------------------
// Modal Edição
// ----------------------
const modalEditar = document.getElementById("modalEditar");
const modalEditarTitulo = document.getElementById("modalEditarTitulo");
const modalEditarNome = document.getElementById("modalEditarNome");
const modalEditarTelefone = document.getElementById("modalEditarTelefone");
const modalEditarQuantidade = document.getElementById("modalEditarQuantidade");
const modalEditarCompra = document.getElementById("modalEditarCompra");
const modalEditarVenda = document.getElementById("modalEditarVenda");
const btnSalvarEdicao = document.getElementById("btnSalvarEdicao");
const btnCancelarEdicao = document.getElementById("btnCancelarEdicao");

let itemEdicao = null;
let tipoEdicao = null; // 'cliente' ou 'produto'

function abrirModal(tipo, id){
    tipoEdicao = tipo;
    itemEdicao = id;

    if(tipo==='cliente'){
        const c = clientes.find(c=>c.id===id);
        modalEditarTitulo.textContent = "Editar Cliente";
        modalEditarNome.value = c.nome;
        modalEditarTelefone.value = c.telefone;
        modalEditarQuantidade.style.display = 'none';
        modalEditarCompra.style.display = 'none';
        modalEditarVenda.style.display = 'none';
        modalEditarTelefone.style.display = 'block';
    } else {
        const p = produtos.find(p=>p.id===id);
        modalEditarTitulo.textContent = "Editar Produto";
        modalEditarNome.value = p.nome;
        modalEditarQuantidade.value = p.quantidade;
        modalEditarCompra.value = p.valorCompra;
        modalEditarVenda.value = p.valorVenda;
        modalEditarTelefone.style.display = 'none';
        modalEditarQuantidade.style.display = 'block';
        modalEditarCompra.style.display = 'block';
        modalEditarVenda.style.display = 'block';
    }
    modalEditar.style.display = 'block';
}

btnSalvarEdicao.onclick = () => {
    if(tipoEdicao==='cliente'){
        const c = clientes.find(c=>c.id===itemEdicao);
        if(!modalEditarNome.value.trim()) return alert("Nome obrigatório");
        c.nome = modalEditarNome.value.trim();
        c.telefone = modalEditarTelefone.value.trim();
        localStorage.setItem("clientes", JSON.stringify(clientes));
        carregarClientes();
        carregarProdutos();
    } else {
        const p = produtos.find(p=>p.id===itemEdicao);
        if(!modalEditarNome.value.trim()) return alert("Nome obrigatório");
        p.nome = modalEditarNome.value.trim();
        p.quantidade = parseInt(modalEditarQuantidade.value) || p.quantidade;
        p.valorCompra = parseFloat(modalEditarCompra.value) || p.valorCompra;
        p.valorVenda = parseFloat(modalEditarVenda.value) || p.valorVenda;
        localStorage.setItem("produtos", JSON.stringify(produtos));
        carregarProdutos();
    }
    modalEditar.style.display = 'none';
}

btnCancelarEdicao.onclick = () => modalEditar.style.display = 'none';

// ----------------------
// Modal Exclusão
// ----------------------
const modalExcluir = document.getElementById("modalExcluir");
const btnConfirmarExcluir = document.getElementById("btnConfirmarExcluir");
const btnCancelarExcluir = document.getElementById("btnCancelarExcluir");
let acaoExcluir = null;

function abrirModalExclusao(func){
    acaoExcluir = func;
    modalExcluir.style.display = 'block';
}

btnConfirmarExcluir.onclick = () => {
    if(acaoExcluir) acaoExcluir();
    modalExcluir.style.display = 'none';
}

btnCancelarExcluir.onclick = () => modalExcluir.style.display = 'none';

// ----------------------
// Clientes
// ----------------------
btnCadastrarCliente.onclick = () => {
    if(!nomeCliente.value.trim()) return alert("Informe o nome do cliente");

    let cliente = {
        id: crypto.randomUUID(),
        nome: nomeCliente.value.trim(),
        telefone: telefoneCliente.value.trim()
    };
    clientes.push(cliente);
    localStorage.setItem("clientes", JSON.stringify(clientes));
    carregarClientes();
    nomeCliente.value = telefoneCliente.value = "";
};

function carregarClientes(){
    tabelaClientes.innerHTML = "";
    clienteSelect.innerHTML = "<option value=''>Selecione o cliente</option>";

    clientes.forEach((c,index)=>{
        let tr = document.createElement("tr");
        tr.innerHTML = `
            <td>${c.nome}</td>
            <td>${c.telefone}</td>
            <td>
                <button class="acao-btn" onclick="abrirModal('cliente','${c.id}')">Editar</button>
                <button class="acao-btn excluir" onclick="abrirModalExclusao(()=>excluirCliente('${c.id}'))">Excluir</button>
            </td>
        `;
        tabelaClientes.appendChild(tr);

        let option = document.createElement("option");
        option.value = index;
        option.textContent = c.nome;
        clienteSelect.appendChild(option);
    });
}

function excluirCliente(id){
    clientes = clientes.filter(c=>c.id!==id);
    localStorage.setItem("clientes", JSON.stringify(clientes));
    carregarClientes();
}

// ----------------------
// Produtos
// ----------------------
btnCadastrarProduto.onclick = () => {
    const nome = nomeProduto.value.trim();
    const quantidade = parseInt(quantidadeProduto.value);
    const valorCompra = parseFloat(valorCompraProduto.value);
    const valorVenda = parseFloat(valorVendaProduto.value);

    if(!nome || isNaN(quantidade) || isNaN(valorCompra) || isNaN(valorVenda)) return alert("Preencha todos os campos corretamente");

    let produto = {
        id: crypto.randomUUID(),
        nome,
        quantidade,
        valorCompra,
        valorVenda
    };

    produtos.push(produto);
    localStorage.setItem("produtos", JSON.stringify(produtos));
    carregarProdutos();
    nomeProduto.value = quantidadeProduto.value = valorCompraProduto.value = valorVendaProduto.value = "";
};

function carregarProdutos(){
    tabelaProdutos.innerHTML = "";
    produtoSelect.innerHTML = "<option value=''>Selecione o produto</option>";

    let totalCompra = 0;
    let totalVenda = 0;

    produtos.forEach((p,index)=>{
        totalCompra += p.quantidade*p.valorCompra;
        totalVenda += p.quantidade*p.valorVenda;

        let tr = document.createElement("tr");
        tr.innerHTML = `
            <td>${p.nome}</td>
            <td>${p.quantidade}</td>
            <td>R$ ${p.valorCompra.toFixed(2)}</td>
            <td>R$ ${p.valorVenda.toFixed(2)}</td>
            <td>
                <button class="acao-btn" onclick="abrirModal('produto','${p.id}')">Editar</button>
                <button class="acao-btn excluir" onclick="abrirModalExclusao(()=>excluirProduto('${p.id}'))">Excluir</button>
            </td>
        `;
        tabelaProdutos.appendChild(tr);

        let option = document.createElement("option");
        option.value = p.id;
        option.textContent = `${p.nome} (Estoque: ${p.quantidade})`;
        produtoSelect.appendChild(option);
    });

    totalCompraEl.textContent = "R$ "+totalCompra.toFixed(2);
    totalVendaEl.textContent = "R$ "+totalVenda.toFixed(2);
}

function excluirProduto(id){
    produtos = produtos.filter(p=>p.id!==id);
    localStorage.setItem("produtos", JSON.stringify(produtos));
    carregarProdutos();
}

// ----------------------
// Vendas
// ----------------------
btnVender.onclick = () => {
    const clienteIndex = clienteSelect.value;
    const produtoId = produtoSelect.value;
    const qtd = parseInt(quantidadeVenda.value);

    if(clienteIndex==="" || !produtoId || qtd<=0) return alert("Preencha todos os campos corretamente");

    const produto = produtos.find(p=>p.id===produtoId);
    if(!produto || produto.quantidade<qtd) return alert("Estoque insuficiente");

    produto.quantidade -= qtd;

    let venda = {
        data: new Date().toLocaleString(),
        cliente: clientes[clienteIndex].nome,
        produtoId: produto.id,
        nome: produto.nome,
        qtd,
        preco: produto.valorVenda,
        total: produto.valorVenda*qtd,
        pagamento: formaPagamento.value
    };

    vendas.push(venda);
    localStorage.setItem("produtos", JSON.stringify(produtos));
    localStorage.setItem("vendas", JSON.stringify(vendas));

    quantidadeVenda.value = "";
    carregarProdutos();
    montarTabelaVendas();
    montarRegistrosVendas();
};

function montarTabelaVendas(){
    tabelaVendas.innerHTML = "";
    let totalGeral = 0;
    vendas.forEach(v=>{
        totalGeral += v.total;
        let tr = document.createElement("tr");
        tr.innerHTML = `
            <td>${v.data}</td>
            <td>${v.cliente}</td>
            <td>${v.nome}</td>
            <td>${v.qtd}</td>
            <td>R$ ${v.preco.toFixed(2)}</td>
            <td>R$ ${v.total.toFixed(2)}</td>
            <td>${v.pagamento}</td>
        `;
        tabelaVendas.appendChild(tr);
    });
    totalGeralEl.textContent = "R$ "+totalGeral.toFixed(2);
}

// ----------------------
// Registros de Vendas
// ----------------------
function montarRegistrosVendas(){
    tabelaRegistros.innerHTML = "";
    let totalGeral = 0;
    vendas.forEach((v,index)=>{
        totalGeral += v.total;
        let tr = document.createElement("tr");
        tr.innerHTML = `
            <td>${v.data}</td>
            <td>${v.cliente}</td>
            <td>${v.nome}</td>
            <td>${v.qtd}</td>
            <td>R$ ${v.preco.toFixed(2)}</td>
            <td>R$ ${v.total.toFixed(2)}</td>
            <td>${v.pagamento}</td>
            <td>
                <button class="acao-btn excluir" onclick="abrirModalExclusao(()=>excluirVenda(${index}))">Excluir</button>
            </td>
        `;
        tabelaRegistros.appendChild(tr);
    });
    totalGeralRegistros.textContent = "R$ "+totalGeral.toFixed(2);
}

function excluirVenda(index){
    let venda = vendas[index];
    let produto = produtos.find(p=>p.id===venda.produtoId);
    if(produto) produto.quantidade += venda.qtd;

    vendas.splice(index,1);
    localStorage.setItem("vendas", JSON.stringify(vendas));
    localStorage.setItem("produtos", JSON.stringify(produtos));

    montarRegistrosVendas();
    montarTabelaVendas();
    carregarProdutos();
}

// Atualiza registros ao clicar na aba
document.querySelector("button[onclick=\"mostrar('registrosVendas')\"]")
    .addEventListener('click', montarRegistrosVendas);

// ----------------------
// Exportar PDF
// ----------------------
function exportarRelatorio(){
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.setFontSize(16);
    doc.text("Relatório de Vendas",10,15);

    let y = 25;
    let totalGeral = 0;
    doc.setFontSize(8);
    doc.text("Data",8,y);
    doc.text("Cliente",40,y);
    doc.text("Produto",80,y);
    doc.text("Qtd",120,y);
    doc.text("Preço",140,y);
    doc.text("Total",160,y);
    doc.text("Pagamento",180,y);
    y+=8;

    vendas.forEach(v=>{
        totalGeral += v.total;
        doc.text(v.data,10,y);
        doc.text(v.cliente,40,y);
        doc.text(v.nome,80,y);
        doc.text(String(v.qtd),120,y);
        doc.text("R$ "+v.preco.toFixed(2),140,y);
        doc.text("R$ "+v.total.toFixed(2),160,y);
        doc.text(v.pagamento,180,y);
        y+=8;
        if(y>270){doc.addPage();y=20;}
    });

    y+=10;
    doc.setFontSize(14);
    doc.text("Total Geral: R$ "+totalGeral.toFixed(2),10,y);
    doc.save("relatorio_vendas.pdf");
}

// ----------------------
// Inicialização
// ----------------------
carregarClientes();
carregarProdutos();
montarTabelaVendas();
montarRegistrosVendas();

/* Reset básico */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  background: #f5f5f5;
  color: #333;
}

header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #007BFF;
  color: #fff;
  padding: 10px 20px;
}

.header-left {
  flex: 0 0 auto;
}

.header-left .logo {
  height: 80px; /* Ajuste o tamanho conforme necessário */
}

.header-center {
  flex: 1;
  text-align: center;
}

header h1 {
  margin-bottom: 5px;
}

header nav button {
  background: #fff;
  color: #007BFF;
  border: none;
  padding: 8px 15px;
  margin-left: 5px;
  cursor: pointer;
  border-radius: 4px;
  font-weight: bold;
}

header nav button:hover {
  background: #0056b3;
  color: #fff;
}


nav button {
  background: #fff;
  color: #007BFF;
  border: none;
  padding: 8px 15px;
  margin: 0 5px;
  cursor: pointer;
  border-radius: 4px;
  font-weight: bold;
}

nav button:hover {
  background: #0056b3;
  color: #fff;
}

main {
  padding: 20px;
}

.view {
  display: none;
}

.view.active {
  display: block;
}

.form {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 15px;
}

.form input, .form select {
  padding: 8px;
  flex: 1;
  min-width: 150px;
}

.form button {
  padding: 8px 15px;
  background: #28a745;
  color: #fff;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  font-weight: bold;
}

.form button:hover {
  background: #218838;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 15px;
  background: #fff;
}

table th, table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}

table th {
  background: #007BFF;
  color: #fff;
}

table td button {
  padding: 5px 10px;
  margin: 2px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  font-size: 0.9em;
}

.acao-btn {
  background: #ffc107;
  color: #000;
}

.acao-btn:hover {
  background: #e0a800;
}

.excluir {
  background: #dc3545;
  color: #fff;
}

.excluir:hover {
  background: #c82333;
}

.total {
  font-weight: bold;
  margin-bottom: 15px;
}

/* Modal comum */
.modal {
  display: none;
  position: fixed;
  z-index: 10;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background: rgba(0,0,0,0.5);
}

.modal-content {
  background: #fff;
  margin: 10% auto;
  padding: 20px;
  border-radius: 8px;
  width: 90%;
  max-width: 400px;
  text-align: center;
}

.modal-content input {
  width: 80%;
  margin: 8px 0;
  padding: 8px;
}

.modal-content button {
  padding: 8px 15px;
  margin: 5px;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  font-weight: bold;
}

#btnSalvarEdicao {
  background: #28a745;
  color: #fff;
}

#btnCancelarEdicao, #btnCancelarExcluir {
  background: #6c757d;
  color: #fff;
}

#btnConfirmarExcluir {
  background: #dc3545;
  color: #fff;
}

.modal-content button:hover {
  opacity: 0.9;
}

