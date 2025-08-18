# 🌐 DIZIX: Ecossistema Modular de Valor por Esforço

> 💡 **Moeda digital gerada por esforço, reutilizada com ética, valorizada por uso.**
> 
> Um ecossistema **modular, independente, dinâmico**, onde cada componente funciona sozinho, mas se comunica quando necessário.

---

## 🧱 Arquitetura Modular (Estrutura do Projeto)

<pre><code>
/dizix-ecosystem/
├── core/
│   ├── token/               # Sistema de DIZIX (saldo interno ou blockchain futuro)
│   ├── wallet/              # Gestão de saldos internos (banco de dados)
│   └── burn-engine/         # Queima dinâmica de taxas
│
├── modules/
│   ├── ptc/                 # Paid-to-Click (ganho por anúncios)
│   ├── short-links/         # Encurtamento de links
│   ├── virtual-mining/      # Mineração virtual por tempo e upgrade
│   ├── marketplace/         # Compra/venda de itens entre usuários
│   └── work-rewards/        # Sistema central de recompensas
│
├── exchange/
│   ├── mini-exchange/       # Swap DIZIX ↔ USDT, BNB, DOGE, etc
│   ├── liquidity-pools/     # Pools DIZIX/USDT, DIZIX/BNB
│   └── p2p-market/          # Venda direta de DIZIX para a exchange
│
├── engine/
│   ├── dynamic-fee/         # Taxas dinâmicas (baseadas em demanda)
│   ├── oracle-price/        # Oráculo de preço (P2P + mercado)
│   └── tax-distribution/    # Distribuição: 40% queima, 40% pool, 20% reinvestimento
│
├── utils/
│   ├── auth/                # Autenticação de usuários
│   ├── notifications/       # Notificações internas
│   └── logs/                # Logs de transações e eventos
│
└── config/
    ├── monetary-policy.json # Regras: taxas, recompensas, limites
    └── modules.json         # Módulos ativos e endpoints
</code></pre>

---

## 🔗 Princípio de Modularidade

Cada módulo:
- ✅ **Funciona de forma independente**.
- ✅ **Não depende diretamente dos outros**.
- ✅ **Se comunica via eventos ou API REST/JSON-RPC**.
- ✅ **Pode ser atualizado, desligado ou substituído sem quebrar o sistema**.

### Exemplo de Comunicação
<pre><code>
[PTC Module] → Emite evento: "user_completed_ad: userId=123, reward=5 DIZIX"
     ↓
[Wallet Module] ← Escuta evento → Adiciona 5 DIZIX ao saldo
     ↓
[Burn Engine] ← Taxa de 0.25 DIZIX → queimada
     ↓
[Mini Exchange] ← Atualiza oráculo de preço
</code></pre>

> 🔄 Comunicação via **eventos internos** ou **fila de mensagens (ex: Redis, RabbitMQ)**.

---

## 💡 Módulos e Suas Funções

### 1. `core/token/` — Sistema de DIZIX
- Gerencia o saldo interno (antes da blockchain).
- Interface com banco de dados.
- Futuro: integração com contrato ERC-20.

### 2. `core/wallet/` — Gestão de Saldo
- Operações: `addBalance()`, `deductBalance()`, `transfer()`.
- Não acessa outros módulos diretamente.

### 3. `core/burn-engine/` — Queima Dinâmica
- Calcula taxa com base em regras.
- Queima parte da taxa (ex: 40%).
- Registra em log.

### 4. `modules/ptc/` — Paid-to-Click
- Mostra anúncios.
- Valida conclusão (CAPTCHA, tempo).
- Emite evento de recompensa.

### 5. `modules/short-links/`
- Encurta links.
- Rastreia cliques legítimos.
- Paga DIZIX por clique verificado.

### 6. `modules/virtual-mining/`
- Simula mineração por tempo.
- Upgrades melhoram taxa de geração.
- Itens obsoletos podem ser vendidos.

### 7. `modules/marketplace/`
- Venda de itens entre usuários.
- Taxa de 5% em DIZIX ou USDT.
- Integra com `wallet` e `burn-engine`.

### 8. `exchange/mini-exchange/`
- Swap automático: DIZIX ↔ USDT, BNB, DOGE.
- Interface tipo exchange real.
- Usa `liquidity-pools`.

### 9. `exchange/liquidity-pools/`
- Cria pares: DIZIX/USDT, DIZIX/BNB.
- Distribui taxas de swap.
- Pode ser integrado com Uniswap v2.

### 10. `exchange/p2p-market/`
- Usuário vende DIZIX diretamente para a Mini Exchange.
- Pagamento fora da plataforma (Pix, PayPal).
- DIZIX adquirida entra no **pool de revenda ética**.

### 11. `engine/dynamic-fee/`
- Calcula taxa com base em:
  - Volume de transações
  - Demanda por saques
  - Atividade no marketplace
- Ex: taxa entre 2% e 8%.

### 12. `engine/oracle-price/`
- Coleta preços de:
  - Swaps na mini exchange
  - Vendas P2P confirmadas
- Calcula média ponderada.
- Atualiza valor: "1 DIZIX ≈ $0,012".

### 13. `engine/tax-distribution/`
- Divide taxa cobrada:
  - 40% → Queimado
  - 40% → Pool de liquidez
  - 20% → Reinvestimento (desenvolvimento)

---

## 🔄 Regras de Comunicação

| Regra | Como Funciona |
|------|----------------|
| **Nenhum módulo importa outro diretamente** | Usa eventos ou API |
| **Comunicação via eventos internos** | Ex: `reward_issued`, `swap_executed` |
| **Configuração centralizada** | `config/monetary-policy.json` |
| **Logs obrigatórios** | Toda ação é registrada em `utils/logs/` |
| **Autenticação obrigatória** | `utils/auth/` valida antes de qualquer ação |

---

## 📊 Exemplo de Fluxo: Usuário Vende DIZIX para Mini Exchange

<pre><code>
1. Usuário A → Vende 1.000 DIZIX na `p2p-market`
2. Sistema → Confirma pagamento fora (Pix)
3. `p2p-market` → Emite evento: "dizix_acquired: amount=1000, buyer=exchange"
4. `wallet` → Deduz 1.000 DIZIX do usuário
5. `mini-exchange` → Adiciona ao pool de revenda
6. `burn-engine` → Queima 40 DIZIX (4% taxa implícita)
7. `oracle-price` → Atualiza valor médio do DIZIX
</code></pre>

---

## 🛠️ Tecnologias Sugeridas

| Camada | Tecnologia |
|-------|-----------|
| Backend | Node.js + Express ou Python + FastAPI |
| Banco de Dados | PostgreSQL ou SQLite |
| Comunicação | Redis (fila de eventos) ou WebSocket |
| Frontend | React (opcional, futuro) |
| Deploy | Servidor Linux (Hetzner) |
| Versionamento | Git + GitHub |

---

## 🚀 Próximos Passos

1. ✅ Crie a estrutura de pastas.
2. ✅ Implemente `core/wallet/` e `core/token/`.
3. ✅ Desenvolva `modules/ptc/` como primeiro módulo.
4. ✅ Conecte com `engine/dynamic-fee/` e `core/burn-engine/`.
5. ✅ Lance `exchange/mini-exchange/` com swap DIZIX/USDT.
6. ✅ Use `config/monetary-policy.json` para ajustar regras.

---

## 📦 Configuração Exemplo: `monetary-policy.json`

<pre><code>{
  "dynamic_fee": {
    "min_rate": 0.02,
    "max_rate": 0.08,
    "volume_threshold_low": 1000,
    "volume_threshold_high": 10000
  },
  "burn_ratio": 0.4,
  "liquidity_ratio": 0.4,
  "reinvestment_ratio": 0.2,
  "max_daily_withdrawal": 100,
  "kyc_required_for_large_withdrawals": true
}
</code></pre>

---

## 🏁 Conclusão

Este ecossistema:
- ✅ É **modular** — cada parte é independente.
- ✅ É **dinâmico** — taxas e valor se adaptam.
- ✅ É **ético** — só DIZIX de esforço circula.
- ✅ É **escalável** — pode ir para blockchain depois.
- ✅ É **seu** — você controla a evolução.

> 🌱 **DIZIX nasce no seu servidor, amadurece com uso, e um dia vira blockchain.**

---

💾 **Salve este arquivo como `README.md`.**  
🔧 Atualize conforme evolui.  
🚀 Compartilhe com quem ajudar.

> **DIZIX: a moeda que respeita quem trabalha.**