<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Relatório Financeiro de Parceria</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN for charts -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
    </style>
</head>
<body class="bg-gray-100 p-8">

    <div class="max-w-4xl mx-auto bg-white rounded-lg shadow-xl p-8 space-y-8">
        <header class="text-center">
            <h1 class="text-3xl md:text-4xl font-bold text-gray-800">Relatório Financeiro da Parceria</h1>
            <p class="mt-2 text-gray-600">Transparência e clareza para nossos parceiros.</p>
        </header>

        <!-- Seção de Resumo e Indicadores -->
        <section class="grid grid-cols-1 md:grid-cols-3 gap-6 text-center">
            <div class="bg-blue-100 p-6 rounded-lg shadow-md">
                <p class="text-sm font-medium text-blue-800">Capital Investido</p>
                <p id="total-capital-investido" class="mt-1 text-2xl font-bold text-blue-900"></p>
            </div>
            <div class="bg-pink-100 p-6 rounded-lg shadow-md">
                <p class="text-sm font-medium text-pink-800">Despesas Totais</p>
                <p id="total-despesas" class="mt-1 text-2xl font-bold text-pink-900"></p>
            </div>
            <div class="bg-green-100 p-6 rounded-lg shadow-md">
                <p class="text-sm font-medium text-green-800">Total de Sócios</p>
                <p id="total-socios" class="mt-1 text-2xl font-bold text-green-900"></p>
            </div>
        </section>

        <!-- Gráfico de Despesas -->
        <section class="bg-gray-50 p-6 rounded-lg shadow-md">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4">Análise de Despesas</h2>
            <canvas id="despesasChart" class="w-full h-auto"></canvas>
        </section>

        <!-- Adicionar Despesas -->
        <section class="bg-gray-50 p-6 rounded-lg shadow-md">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4">Adicionar Nova Despesa</h2>
            <div class="flex flex-col md:flex-row items-center gap-4">
                <input type="text" id="nova-despesa-descricao" placeholder="Descrição da Despesa" class="flex-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500">
                <input type="number" id="nova-despesa-valor" placeholder="Valor" class="flex-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500">
                <button id="adicionar-despesa-btn" class="bg-purple-600 text-white font-semibold py-2 px-4 rounded-lg shadow-lg hover:bg-purple-700 transition duration-300">
                    Adicionar Despesa
                </button>
            </div>
        </section>

        <!-- Calculadora de Retorno por Sócio -->
        <section class="bg-gray-50 p-6 rounded-lg shadow-md">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4">Simulação de Retorno</h2>
            
            <div class="space-y-6">
                <!-- Opção 1: Calcular Retorno a Partir do Valor de Venda -->
                <div>
                    <h3 class="text-lg font-medium text-gray-700 mb-2">Calcular Retorno a Partir do Valor de Venda</h3>
                    <div class="flex flex-col md:flex-row items-center gap-4">
                        <label for="valor-venda" class="text-gray-700 font-medium whitespace-nowrap">Valor de Venda:</label>
                        <input type="number" id="valor-venda" placeholder="Ex: 450000" class="flex-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <button id="calcular-venda-btn" class="bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg shadow-lg hover:bg-blue-700 transition duration-300">
                            Calcular Retorno
                        </button>
                    </div>
                </div>

                <hr class="border-t border-gray-300">

                <!-- Opção 2: Calcular Valor de Venda a Partir de um Retorno Desejado -->
                <div>
                    <h3 class="text-lg font-medium text-gray-700 mb-2">Calcular Valor de Venda para Retorno Desejado</h3>
                    <div class="flex flex-col md:flex-row items-center gap-4">
                        <label for="porcentagem-retorno" class="text-gray-700 font-medium whitespace-nowrap">Retorno Desejado (%):</label>
                        <input type="number" id="porcentagem-retorno" placeholder="Ex: 20" class="flex-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500">
                        <button id="calcular-porcentagem-btn" class="bg-green-600 text-white font-semibold py-2 px-4 rounded-lg shadow-lg hover:bg-green-700 transition duration-300">
                            Calcular Valor de Venda
                        </button>
                    </div>
                </div>
            </div>
            
            <div id="retorno-socios-container" class="mt-6 space-y-4">
                <!-- Resultados das simulações serão injetados aqui via JS -->
            </div>
        </section>

        <!-- Tabela de Despesas Detalhadas -->
        <section class="bg-gray-50 p-6 rounded-lg shadow-md">
            <h2 class="text-2xl font-semibold text-gray-800 mb-4">Despesas Detalhadas</h2>
            <div class="overflow-x-auto rounded-lg border border-gray-200">
                <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-100">
                        <tr>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Descrição</th>
                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Valor</th>
                            <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">Ação</th>
                        </tr>
                    </thead>
                    <tbody id="tabela-despesas-corpo" class="bg-white divide-y divide-gray-200">
                        <!-- Linhas da tabela serão injetadas aqui via JS -->
                    </tbody>
                </table>
            </div>
        </section>

    </div>

    <script>
        // Dados do projeto fornecidos pelo usuário
        const dados = {
            // Dados de despesas atualizados
            despesas: [
                { descricao: 'Corretagem para futura venda', valor: 12090.00, categoria: 'Comissão' },
                { descricao: 'Débito IPTU', valor: 964.60, categoria: 'IPTU' },
                { descricao: 'Débito Condomínio futuro', valor: 1000.00, categoria: 'Condomínio' },
                { descricao: 'Despesas cartorárias e impostos', valor: 30000.00, categoria: 'Impostos e Cartório' },
            ],
            
            // Dados dos sócios com valores investidos
            socios: [
                { nome: 'Investidor 01', valorInvestido: 120570.00, cotas: 4 },
                { nome: 'Investidor 02', valorInvestido: 60000.00, cotas: 2 },
                { nome: 'Investidor 03', valorInvestido: 30000.00, cotas: 1 },
                { nome: 'Investidor 04', valorInvestido: 5330.00, cotas: 1 },
                { nome: 'Investidor 05', valorInvestido: 30000.00, cotas: 1 },
                { nome: 'MBM IMÓVEIS (Gestora)', valorInvestido: 0.00, cotas: 1, isGestora: true, valorReferencia: 30000.00 },
            ],
        };

        // Função utilitária para formatar valores monetários
        function formatarMoeda(valor) {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(valor);
        }

        // 1. Carregar Resumo e Tabela de Despesas
        function carregarDadosIniciais() {
            const totalCapitalInvestido = dados.socios.filter(s => !s.isGestora).reduce((soma, socio) => soma + socio.valorInvestido, 0);
            const totalDespesas = dados.despesas.reduce((soma, despesa) => soma + despesa.valor, 0);
            const totalSocios = dados.socios.length;

            document.getElementById('total-capital-investido').textContent = formatarMoeda(totalCapitalInvestido);
            document.getElementById('total-despesas').textContent = formatarMoeda(totalDespesas);
            document.getElementById('total-socios').textContent = `${totalSocios} sócios`;

            const tabelaDespesasCorpo = document.getElementById('tabela-despesas-corpo');
            tabelaDespesasCorpo.innerHTML = '';
            dados.despesas.forEach((despesa, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">${despesa.descricao}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${formatarMoeda(despesa.valor)}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                        <button class="remover-despesa-btn text-red-600 hover:text-red-900" data-index="${index}">Remover</button>
                    </td>
                `;
                tabelaDespesasCorpo.appendChild(row);
            });
            
            document.querySelectorAll('.remover-despesa-btn').forEach(button => {
                button.addEventListener('click', (event) => {
                    const index = event.target.dataset.index;
                    dados.despesas.splice(index, 1);
                    carregarDadosIniciais();
                    // Recalcula o resultado caso a despesa removida altere o valor de venda ou porcentagem
                    const valorVenda = parseFloat(document.getElementById('valor-venda').value);
                    if (!isNaN(valorVenda)) {
                        calcularPorValorDeVenda(valorVenda);
                    }
                    const porcentagemRetorno = parseFloat(document.getElementById('porcentagem-retorno').value);
                    if (!isNaN(porcentagemRetorno)) {
                        calcularPorPorcentagem(porcentagemRetorno);
                    }
                });
            });

            // 2. Criar o Gráfico de Despesas
            const categorias = {};
            dados.despesas.forEach(despesa => {
                categorias[despesa.categoria] = (categorias[despesa.categoria] || 0) + despesa.valor;
            });

            const ctx = document.getElementById('despesasChart').getContext('2d');
            
            // Destruir o gráfico anterior se ele existir para evitar sobreposições
            if (window.despesasChartInstance) {
                window.despesasChartInstance.destroy();
            }

            window.despesasChartInstance = new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: Object.keys(categorias),
                    datasets: [{
                        label: 'Despesas por Categoria',
                        data: Object.values(categorias),
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.8)',
                            'rgba(54, 162, 235, 0.8)',
                            'rgba(255, 206, 86, 0.8)',
                            'rgba(75, 192, 192, 0.8)',
                            'rgba(153, 102, 255, 0.8)',
                            'rgba(255, 159, 64, 0.8)'
                        ],
                        borderColor: [
                            'rgba(255, 99, 132, 1)',
                            'rgba(54, 162, 235, 1)',
                            'rgba(255, 206, 86, 1)',
                            'rgba(75, 192, 192, 1)',
                            'rgba(153, 102, 255, 1)',
                            'rgba(255, 159, 64, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed !== null) {
                                        label += formatarMoeda(context.parsed);
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });
        }

        // Lógica para adicionar nova despesa
        document.getElementById('adicionar-despesa-btn').addEventListener('click', () => {
            const descricaoInput = document.getElementById('nova-despesa-descricao');
            const valorInput = document.getElementById('nova-despesa-valor');
            const descricao = descricaoInput.value.trim();
            const valor = parseFloat(valorInput.value);

            if (descricao === '' || isNaN(valor) || valor <= 0) {
                const errorMessage = "Por favor, insira uma descrição e um valor válido para a despesa.";
                document.getElementById('retorno-socios-container').innerHTML = `<p class="text-red-500">${errorMessage}</p>`;
                return;
            }

            dados.despesas.push({
                descricao: descricao,
                valor: valor,
                categoria: 'Outros'
            });

            // Limpa os campos do formulário
            descricaoInput.value = '';
            valorInput.value = '';

            // Recarrega todos os dados da interface para refletir a nova despesa
            carregarDadosIniciais();
            document.getElementById('retorno-socios-container').innerHTML = '';
        });

        // Funções de cálculo
        function calcularPorValorDeVenda(valorVenda) {
            const totalCapitalInvestido = dados.socios.filter(s => !s.isGestora).reduce((soma, socio) => soma + socio.valorInvestido, 0);
            const totalBaseCapital = totalCapitalInvestido + dados.socios.find(s => s.isGestora).valorReferencia;
            const totalDespesas = dados.despesas.reduce((soma, despesa) => soma + despesa.valor, 0);
            
            const lucroTotalProjeto = valorVenda - totalCapitalInvestido - totalDespesas;
            const retornoPercentualTotal = (lucroTotalProjeto / totalBaseCapital) * 100;

            const container = document.getElementById('retorno-socios-container');
            container.innerHTML = `
                <div class="bg-blue-100 p-4 rounded-lg shadow-sm border border-blue-200">
                    <p class="text-sm font-medium text-blue-700">Retorno Percentual do Investimento:</p>
                    <p class="text-2xl font-bold text-blue-800">${retornoPercentualTotal.toFixed(2)}%</p>
                    <p class="text-sm font-medium text-blue-700">Lucro Total do Projeto: ${formatarMoeda(lucroTotalProjeto)}</p>
                </div>
            `;
            
            dados.socios.forEach(socio => {
                const socioDiv = document.createElement('div');
                socioDiv.className = 'bg-white p-4 rounded-lg shadow-sm border border-gray-200';
                
                let totalRecebido = 0;
                let lucroRecebido = 0;

                if (socio.isGestora) {
                    lucroRecebido = socio.valorReferencia * (retornoPercentualTotal / 100);
                    totalRecebido = socio.valorReferencia + lucroRecebido;
                    
                    socioDiv.innerHTML = `
                        <p class="text-lg font-semibold text-gray-800">${socio.nome}</p>
                        <p class="text-sm text-gray-500">Participação: Serviços (${socio.cotas} cotas)</p>
                        <p class="mt-1 text-xl font-bold text-blue-600">Retorno Total: ${formatarMoeda(totalRecebido)}</p>
                        <p class="text-sm text-gray-500">Lucro Líquido: ${formatarMoeda(lucroRecebido)}</p>
                        <p class="text-sm text-gray-500 font-semibold">Retorno (%): ${retornoPercentualTotal.toFixed(2)}%</p>
                    `;
                } else {
                    lucroRecebido = socio.valorInvestido * (retornoPercentualTotal / 100);
                    totalRecebido = socio.valorInvestido + lucroRecebido;
                    
                    socioDiv.innerHTML = `
                        <p class="text-lg font-semibold text-gray-800">${socio.nome}</p>
                        <p class="text-sm text-gray-500">Capital Investido: ${formatarMoeda(socio.valorInvestido)} (${socio.cotas} cotas)</p>
                        <p class="mt-1 text-xl font-bold text-blue-600">Retorno Total: ${formatarMoeda(totalRecebido)}</p>
                        <p class="text-sm text-gray-500">Lucro Líquido: ${formatarMoeda(lucroRecebido)}</p>
                        <p class="text-sm text-gray-500 font-semibold">Retorno (%): ${retornoPercentualTotal.toFixed(2)}%</p>
                    `;
                }
                
                container.appendChild(socioDiv);
            });
        }

        function calcularPorPorcentagem(porcentagemDesejada) {
            const totalCapitalInvestido = dados.socios.filter(s => !s.isGestora).reduce((soma, socio) => soma + socio.valorInvestido, 0);
            const totalBaseCapital = totalCapitalInvestido + dados.socios.find(s => s.isGestora).valorReferencia;
            const totalDespesas = dados.despesas.reduce((soma, despesa) => soma + despesa.valor, 0);
            
            const totalLucroDesejado = totalBaseCapital * (porcentagemDesejada / 100);
            const valorVendaNecessario = totalCapitalInvestido + totalDespesas + totalLucroDesejado;
            
            const container = document.getElementById('retorno-socios-container');
            container.innerHTML = '';
            const resumoDiv = document.createElement('div');
            resumoDiv.className = 'bg-green-100 p-4 rounded-lg shadow-sm border border-green-200';
            resumoDiv.innerHTML = `
                <p class="text-sm font-medium text-green-700">Para obter um retorno de ${porcentagemDesejada.toFixed(2)}% sobre o capital, o valor de venda necessário é:</p>
                <p class="text-2xl font-bold text-green-800">${formatarMoeda(valorVendaNecessario)}</p>
                <p class="text-sm font-medium text-green-700">O lucro total do projeto seria de: ${formatarMoeda(totalLucroDesejado)}</p>
            `;
            container.prepend(resumoDiv);
        }

        // Listeners para os botões de calcular
        document.getElementById('calcular-venda-btn').addEventListener('click', () => {
            const valor = parseFloat(document.getElementById('valor-venda').value);
            if (!isNaN(valor) && valor > 0) {
                calcularPorValorDeVenda(valor);
            } else {
                document.getElementById('retorno-socios-container').innerHTML = '';
            }
        });

        document.getElementById('calcular-porcentagem-btn').addEventListener('click', () => {
            const porcentagem = parseFloat(document.getElementById('porcentagem-retorno').value);
            if (!isNaN(porcentagem) && porcentagem >= 0) {
                calcularPorPorcentagem(porcentagem);
            } else {
                document.getElementById('retorno-socios-container').innerHTML = '';
            }
        });

        // Chama a função para carregar os dados iniciais quando a página carregar
        document.addEventListener('DOMContentLoaded', carregarDadosIniciais);
    </script>

</body>
</html>
