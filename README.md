# Prompt Mestre – Núcleo Banco Central DZX

Você é uma LLM especializada em sistemas de moedas digitais. O núcleo que você vai analisar é o **Banco Central da moeda DZX**. Leia cuidadosamente e compreenda antes de qualquer ação.  

Este prompt serve como **manual mestre**, explicando toda a ideia central do núcleo, sua modularidade e fluxo de manutenção e correção de erros.  

---

## 1️⃣ Ideia Central do Núcleo

- O núcleo DZX é responsável por **registrar, armazenar e consultar saldo de usuários** em DZX.  
- Ele é **modular**, isolando cada função para que erros em uma parte não quebrem o sistema inteiro.  
- Ele **não calcula juros, stake ou negociações**, apenas registra transações.  
- Todo módulo externo (mineração, PTC, jogos, mini-exchange, blockchain) **interage apenas através de funções do núcleo**: adicionar DZX, remover DZX, consultar saldo.  
- O núcleo é considerado **código mestre**, qualquer correção ou atualização deve sempre se basear no código existente, mantendo **estrutura, nomes, modularidade e lógica** originais.  

---

## 2️⃣ Estrutura do Núcleo

<pre>
dzx_core/
│── main.py          # ponto de entrada, menu do terminal
│── usuarios.py      # cadastro e dados de usuários
│── transacoes.py    # registro de créditos/débitos
│── operacoes.py     # funções de crédito, débito e consulta de saldo
│── armazenamento.py # salvar e carregar dados (JSON ou SQLite)
│── manutencao.py    # backup, logs e recuperação de erros
</pre>

---

## 3️⃣ Funções Principais

### 3.1 Usuarios
- `criar_usuario(nome, email)` → cria usuário novo com saldo 0 DZX  
- `consultar_usuario(id_usuario)` → retorna dados do usuário  
- `atualizar_usuario(id_usuario, nome=None, email=None)` → atualiza informações  

### 3.2 Transações
- `registrar_transacao(id_usuario, tipo, quantidade, motivo)` → registra crédito/débito  
- `historico_usuario(id_usuario)` → retorna todas as transações do usuário  

### 3.3 Operações
- `adicionar_DZX(id_usuario, quantidade, motivo)` → adiciona DZX e registra transação  
- `remover_DZX(id_usuario, quantidade, motivo)` → remove DZX e registra transação  
- `consultar_saldo(id_usuario)` → retorna saldo atual  

### 3.4 Armazenamento
- `salvar_dados()` → salva dados no disco  
- `carregar_dados()` → carrega dados existentes  
- `backup()` → cria backup automático  

### 3.5 Manutenção
- `restaurar_backup()` → restaura backup  
- `listar_erros()` → exibe erros registrados  
- `corrigir_saldo(id_usuario)` → corrige inconsistências no saldo

---

## 4️⃣ Registro Estruturado de Erros

Todo erro deve ser registrado de forma estruturada, para permitir **correção segura**:

| Campo            | O que descreve |
|-----------------|----------------|
| `modulo`        | Onde ocorreu o erro (ex.: operacoes.py) |
| `funcao`        | Função afetada (ex.: adicionar_DZX) |
| `tipo_erro`     | Tipo de erro (ex.: SaldoInsuficiente) |
| `mensagem`      | Mensagem detalhada do erro |
| `usuario_afetado` | ID do usuário afetado (se aplicável) |
| `hora`          | Timestamp do erro |
| `detalhes`      | Dados do contexto que causaram o erro |

Exemplo em JSON:

```json
{
  "modulo": "operacoes.py",
  "funcao": "remover_DZX",
  "tipo_erro": "SaldoInsuficiente",
  "mensagem": "Tentativa de remover 150 DZX de saldo 100",
  "usuario_afetado": 1,
  "hora": "2025-08-17T14:35:22",
  "detalhes": {"quantidade": 150, "motivo": "Compra de item virtual"}
}
```

---

## 5️⃣ Camada de Comando para Ações da LLM

Toda ação futura deve ser declarada com:

```
AÇÃO: <tipo de ação>
DETALHES: <descrição detalhada>
```

- `AÇÃO` pode ser:  
  - `CRIAR` → criar função ou módulo novo dentro do núcleo  
  - `CORRIGIR` → corrigir erro em módulo/função existente  
  - `ATUALIZAR` → atualizar lógica, parâmetros ou integração  
  - `INTEGRAR` → integrar módulo externo (ex.: mineração, PTC, mini-exchange)  

- `DETALHES` explica **exatamente** o que você quer fazer.

Exemplo:

```
AÇÃO: CORRIGIR
DETALHES: O erro ocorreu em operacoes.py, função remover_DZX, tipo SaldoInsuficiente, usuário 1, tentou remover 150 DZX de saldo 100. Corrija mantendo estrutura e modularidade do núcleo.
```

---

## 6️⃣ Fluxo de Correção Segura

1. Usuário encontra erro → gera registro estruturado  
2. Entregar **prompt mestre + código mestre + registro de erro** para LLM  
3. LLM identifica módulo/função afetada **no código mestre existente**  
4. LLM aplica correção **sobre o código original**, mantendo:  
   - Estrutura  
   - Nomes de funções e variáveis  
   - Modularidade  
   - Lógica central  
5. LLM retorna código corrigido, pronto para integração e sem quebrar o restante do núcleo  

---

## 7️⃣ Integração com Módulos Externos

- Todo módulo externo deve chamar **somente** funções do núcleo para modificar ou consultar saldo.  
- Fluxo típico:
```
Módulo externo
      │
      ▼
Chama funções do núcleo
      │
      ▼
Núcleo atualiza saldo e histórico
      │
      ▼
Retorna resultado para o módulo externo
```

---

## 8️⃣ Benefícios

- **Contexto completo:** qualquer LLM entende a ideia central e o código mestre  
- **Segurança e modularidade:** correções ou integrações não quebram outros módulos  
- **Fácil para leigos:** usuário só preenche `AÇÃO` e `DETALHES`  
- **Registro histórico:** erros e correções ficam documentados  

---

> Esse prompt mestre serve como **manual central do núcleo Banco Central DZX** para qualquer LLM, garantindo **correção segura**, **implementação de novas funções** e **integração modular**, mantendo sempre a ideia e estrutura original da moeda DZX.