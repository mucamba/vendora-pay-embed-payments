# Documentação para Pagamentos Embutidos - VendoraPay

## 📋 Índice
1. [Visão Geral](#visão-geral)
2. [Integração Rápida](#integração-rápida)
3. [Métodos de Integração](#métodos-de-integração)
4. [API Reference](#api-reference)
5. [Exemplos Completos](#exemplos-completos)
6. [Customização](#customização)
7. [Callback e Webhooks](#callback-e-webhooks)
8. [FAQ](#faq)

## 🚀 Visão Geral

A VendoraPay oferece uma solução de pagamentos embutidos que permite integrar checkout de pagamento diretamente no seu site através de widgets e iframes.

### Características Principais
- ✅ **Checkout Embutido** - Integre diretamente no seu site
- ✅ **Múltiplos Métodos** - Mpesa, e-Mola, Cartão
- ✅ **Responsivo** - Adapta-se a qualquer dispositivo
- ✅ **Seguro** - Processamento PCI Compliant
- ✅ **Fácil Integração** - Apenas algumas linhas de código

## ⚡ Integração Rápida

### 1. Inclua a Biblioteca
```html
<script src="https://vendorapay.com/cdn/embed-widget.js"></script>
```

### 2. Adicione um Botão de Pagamento
```html
<button class="vendorapay-trigger"
        data-widget-key="SEU_WIDGET_KEY"
        data-amount="500"
        data-context="Descrição do pagamento">
    Pagar Agora
</button>
```

### 3. Pronto! 🎉
O modal de pagamento será aberto automaticamente quando o usuário clicar no botão.

## 🔧 Métodos de Integração

### Método 1: Auto-inicialização (Recomendado)

Use data attributes para inicialização automática:

```html
<!-- Botão simples -->
<button class="vendorapay-trigger"
        data-widget-key="widget_abc123..."
        data-amount="1000"
        data-context="Compra na Loja XYZ">
    Pagar 1000 MT
</button>

<!-- Link de pagamento -->
<a href="#" class="vendorapay-trigger"
   data-widget-key="widget_abc123..."
   data-amount="500"
   data-context="Doação"
   data-theme="dark">
    Fazer Doação
</a>

<!-- Produto em lista -->
<div class="product">
    <h3>Produto A - 750 MT</h3>
    <button class="vendorapay-trigger"
            data-widget-key="widget_abc123..."
            data-amount="750"
            data-context="Produto A - Loja XYZ">
        Comprar Produto A
    </button>
</div>
```

### Método 2: JavaScript Programático

Para mais controle, use a API JavaScript:

```javascript
// Inicialização básica
const widget = new VendoraPayWidget('SEU_WIDGET_KEY', {
    amount: 1500,
    context: 'Pagamento Personalizado',
    theme: 'light' // ou 'dark'
});
widget.show();

// Com callback de sucesso
const widget = new VendoraPayWidget('SEU_WIDGET_KEY', {
    onPaymentComplete: function(data) {
        console.log('Pagamento concluído!', data);
        // Atualizar UI, redirecionar, etc.
        alert('Obrigado pelo seu pagamento!');
    }
});
widget.show(2000, 'Compra Especial');
```

### Método 3: Integração Dinâmica

Para valores dinâmicos baseados em carrinho ou seleção do usuário:

```javascript
function processarPagamentoDinamico(produtos) {
    const total = calcularTotal(produtos);
    const descricao = gerarDescricao(produtos);
    
    const widget = new VendoraPayWidget('SEU_WIDGET_KEY');
    widget.show(total, descricao);
}

// Exemplo de uso
document.getElementById('finalizar-compra').addEventListener('click', function() {
    const produtosSelecionados = obterProdutosDoCarrinho();
    processarPagamentoDinamico(produtosSelecionados);
});
```

## 📚 API Reference

### Classe VendoraPayWidget

#### Construtor
```javascript
new VendoraPayWidget(widgetKey, options)
```

**Parâmetros:**
- `widgetKey` (String, obrigatório) - Chave do widget obtida no dashboard
- `options` (Object, opcional) - Configurações do widget

**Opções:**
```javascript
{
    amount: null,           // Valor em MT (opcional)
    context: '',           // Descrição do pagamento
    theme: 'light',        // 'light' ou 'dark'
    baseUrl: 'https://vendorapay.com/api/widget/create',
    onPaymentComplete: function(data) {
        // Callback quando pagamento é concluído
    }
}
```

#### Métodos

##### `show(amount, context)`
Abre o modal de pagamento.

```javascript
widget.show(1000, 'Descrição do pagamento');
```

##### `close()`
Fecha o modal de pagamento.

```javascript
widget.close();
```

### Data Attributes para Auto-inicialização

| Attribute | Obrigatório | Descrição | Exemplo |
|-----------|-------------|-----------|---------|
| `data-widget-key` | ✅ | Chave do widget | `widget_abc123...` |
| `data-amount` | ❌ | Valor em MT | `500` |
| `data-context` | ❌ | Descrição | `Compra na Loja` |
| `data-theme` | ❌ | Tema do modal | `light` ou `dark` |

## 💡 Exemplos Completos

### Exemplo 1: Loja E-commerce
```html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minha Loja</title>
    <style>
        .products {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            padding: 20px;
        }
        .product-card {
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }
        .buy-btn {
            background: #4f46e5;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Minha Loja Online</h1>
    
    <div class="products">
        <div class="product-card">
            <h3>Produto A</h3>
            <p>500 MT</p>
            <button class="vendorapay-trigger buy-btn"
                    data-widget-key="SEU_WIDGET_KEY"
                    data-amount="500"
                    data-context="Produto A - Minha Loja">
                Comprar Agora
            </button>
        </div>
        
        <div class="product-card">
            <h3>Produto B</h3>
            <p>750 MT</p>
            <button class="vendorapay-trigger buy-btn"
                    data-widget-key="SEU_WIDGET_KEY"
                    data-amount="750"
                    data-context="Produto B - Minha Loja">
                Comprar Agora
            </button>
        </div>
    </div>

    <script src="https://cdn.vendorapay.com/widget/v1/vendorapay-widget.js"></script>
</body>
</html>
```

### Exemplo 2: Sistema de Doações
```html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Campanha de Doações</title>
</head>
<body>
    <div class="donation-campaign">
        <h1>Ajude Nossa Causa</h1>
        <p>Escolha um valor para doar:</p>
        
        <div class="donation-options">
            <button class="vendorapay-trigger"
                    data-widget-key="SEU_WIDGET_KEY"
                    data-amount="100"
                    data-context="Doação - Valor Básico">
                Doar 100 MT
            </button>
            
            <button class="vendorapay-trigger"
                    data-widget-key="SEU_WIDGET_KEY"
                    data-amount="500"
                    data-context="Doação - Valor Médio">
                Doar 500 MT
            </button>
            
            <button class="vendorapay-trigger"
                    data-widget-key="SEU_WIDGET_KEY"
                    data-amount="1000"
                    data-context="Doação - Valor Generoso">
                Doar 1000 MT
            </button>
        </div>
        
        <div class="custom-donation">
            <h3>Ou digite um valor personalizado:</h3>
            <input type="number" id="custom-amount" placeholder="Valor em MT" min="10">
            <button onclick="processCustomDonation()">Doar Valor Personalizado</button>
        </div>
    </div>

    <script src="https://vendorapay.com/cdn/embed-widget.js"></script>
    <script>
        function processCustomDonation() {
            const amount = document.getElementById('custom-amount').value;
            if (amount && amount >= 10) {
                const widget = new VendoraPayWidget('SEU_WIDGET_KEY', {
                    onPaymentComplete: function(data) {
                        alert('Obrigado pela sua doação!');
                        // Atualizar contador de doações, etc.
                    }
                });
                widget.show(parseFloat(amount), 'Doação Personalizada - Nossa Causa');
            } else {
                alert('Por favor, insira um valor válido (mínimo 10 MT)');
            }
        }
    </script>
</body>
</html>
```

### Exemplo 3: Sistema de Assinaturas
```html
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planos de Assinatura</title>
</head>
<body>
    <div class="pricing-plans">
        <div class="plan">
            <h3>Plano Básico</h3>
            <p class="price">500 MT/mês</p>
            <button onclick="subscribeToPlan('basic', 500)">Assinar Agora</button>
        </div>
        
        <div class="plan">
            <h3>Plano Pro</h3>
            <p class="price">1000 MT/mês</p>
            <button onclick="subscribeToPlan('pro', 1000)">Assinar Agora</button>
        </div>
        
        <div class="plan">
            <h3>Plano Enterprise</h3>
            <p class="price">2000 MT/mês</p>
            <button onclick="subscribeToPlan('enterprise', 2000)">Assinar Agora</button>
        </div>
    </div>

    <script src="https://vendorapay.com/cdn/embed-widget.js"></script>
    <script>
        function subscribeToPlan(plan, amount) {
            const widget = new VendoraPayWidget('SEU_WIDGET_KEY', {
                onPaymentComplete: function(data) {
                    // Ativar assinatura no backend
                    activateSubscription(plan, data.transactionId);
                }
            });
            widget.show(amount, `Assinatura ${plan} - Mensal`);
        }
        
        function activateSubscription(plan, transactionId) {
            // Chamar sua API para ativar a assinatura
            fetch('/api/activate-subscription', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    plan: plan,
                    transactionId: transactionId
                })
            }).then(() => {
                alert('Assinatura ativada com sucesso!');
                window.location.href = '/dashboard';
            });
        }
    </script>
</body>
</html>
```

## 🎨 Customização

### Temas Disponíveis

**Tema Claro (padrão):**
```html
<button class="vendorapay-trigger"
        data-theme="light">
    Pagar Agora
</button>
```

**Tema Escuro:**
```html
<button class="vendorapay-trigger"
        data-theme="dark">
    Pagar Agora
</button>
```

### Estilização dos Botões

Você pode estilizar os botões livremente:

```css
/* Estilo personalizado para botões VendoraPay */
.vendorapay-trigger {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 15px 30px;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.vendorapay-trigger:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

/* Para links */
a.vendorapay-trigger {
    text-decoration: none;
    display: inline-block;
    text-align: center;
}
```

## 🔄 Callback e Webhooks

### Callback no Frontend

```javascript
const widget = new VendoraPayWidget('SEU_WIDGET_KEY', {
    onPaymentComplete: function(data) {
        // data contém informações da transação
        console.log('Pagamento concluído:', data);
        
        // Exemplo de ações pós-pagamento:
        if (data.success) {
            // Redirecionar para página de sucesso
            window.location.href = '/obrigado';
            
            // Ou atualizar interface
            document.getElementById('status-pagamento').innerHTML = 
                '<span style="color: green;">✓ Pagamento Confirmado</span>';
            
            // Ou enviar para analytics
            gtag('event', 'purchase', {
                transaction_id: data.transactionId,
                value: data.amount
            });
        }
    }
});
```

### Webhooks no Backend
Receba notificações instantâneas quando pagamentos forem confirmados.

Notificação Instantânea
O webhook é acionado imediatamente após a confirmação do pagamento.

Cabeçalhos de Segurança
Para evitar phishing, todas as requisições de webhook incluem um cabeçalho de autenticação:

 Copiar
secretKey: SUA_CHAVE_SECRETA
Corpo da Requisição
```json
{
  "id": "txn_123456789",
  "productId": "prod_abc123",
  "method": "mpesa",
  "paid": 1500,
  "received": 1425,
  "fee": 75,
  "context": "Pagamento do curso de programação"
}
```
Obtendo sua Chave Secreta
Sua chave secreta pode ser encontrada na mesma seção do dashboard onde você encontrou sua chave API.
## ❓ FAQ

### Como obtenho minha widget key?
Acesse o dashboard da VendoraPay, vá em "Widgets" e crie um novo widget. Copie a chave gerada.

### Quais métodos de pagamento são suportados?
- ✅ Mpesa
- ✅ e-Mola  
- ✅ Cartão de Crédito/Débito (Visa, Mastercard)

### Posso usar valores dinâmicos?
Sim! Use a API JavaScript para definir valores dinamicamente:

```javascript
const widget = new VendoraPayWidget('SEU_WIDGET_KEY');
widget.show(valorCalculado, descricaoDinamica);
```

### O widget é responsivo?
Sim! O modal se adapta automaticamente a dispositivos móveis e desktop.

### Posso customizar a aparência?
Pode customizar completamente os botões de trigger. O modal interno mantém a identidade da VendoraPay para segurança.

### Como lido com pagamentos falhados?
O usuário pode tentar novamente diretamente no modal. Você também recebe webhooks para pagamentos falhados.

### É seguro?
Sim! Todo processamento de cartão é PCI DSS compliant. Dados sensíveis nunca passam pelo seu servidor.

### Há limites de valor?
O valor mínimo é 1 MT e máximo é 5000 MT por transação.

## 🆘 Suporte

Precisa de ajuda?
- 📧 Email: mozinovati@gmail.com
- 📚 Documentação: https://vendorapay.com/api-docs

---

**Versão:** 1.0  
**Última atualização:** ${new Date().toLocaleDateString('pt-MZ')}
