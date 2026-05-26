# 🐳 Laboratório de Rede com Containerlab

> Laboratório prático de criação de uma topologia de rede simples usando Docker e Containerlab.

[![Containerlab](https://img.shields.io/badge/Containerlab-v0.50+-blue?logo=linux)](https://containerlab.dev)
[![Docker](https://img.shields.io/badge/Docker-required-blue?logo=docker)](https://www.docker.com)

---

## 📖 Visão Geral

Este laboratório demonstra como criar uma topologia de rede virtual simples utilizando o Containerlab e containers Docker.

**O que este laboratório demonstra:**

* Deploy de uma rede virtual com 2 nós usando Containerlab.
* Configuração de conectividade entre containers Linux.
* Testes básicos de comunicação utilizando ping.
* Estrutura simples para estudos de redes e automação.

---

## 🌐 Topologia

```
┌─────────────────────────────────────────┐
│               Máquina Host              │
│                                         │
│  ┌──────────┐ eth1   eth1 ┌──────────┐  │
│  │  node-a  ├─────────────┤  node-b  │  │
│  │10.0.0.1  │             │10.0.0.2  │  │
│  └──────────┘             └──────────┘  │
│                                         │
└─────────────────────────────────────────┘
```

* node-a: Container Linux usando a imagem `nicolaka/netshoot`.
* node-b: Container Linux usando a imagem `nicolaka/netshoot`.

| Nó     | Endereço IP | Função                     |
| ------ | ----------- | -------------------------- |
| node-a | `10.0.0.1`  | Origem dos testes de rede  |
| node-b | `10.0.0.2`  | Destino dos testes de rede |

---

## 🔧 Pré-requisitos

### 0. Requisitos do Sistema

Os seguintes requisitos devem ser atendidos para que a ferramenta Containerlab seja executada com sucesso:

* Um usuário com privilégios de sudo.
* Um servidor Linux ou WSL2.

Referências:

* [https://containerlab.dev/install/](https://containerlab.dev/install/)
* [https://learn.microsoft.com/pt-br/windows/wsl/install](https://learn.microsoft.com/pt-br/windows/wsl/install)

---

## 🐳 1. Instalar o Docker

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

> Saia e entre novamente na sessão após adicionar seu usuário ao grupo `docker`.

---

## 📦 2. Instalar o Containerlab

```bash
bash -c "$(curl -sL https://get.containerlab.dev)"
```

Verifique a instalação:

```bash
containerlab version
```

---

## ⏬ Obtendo o Laboratório

Clone o repositório e acesse o diretório do laboratório:

```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```

> 📁 Arquivo principal:
>
> * `lab.clab.yml` — Definição da topologia Containerlab

---

## 🚀 Passo 1 — Deploy da Topologia

```bash
sudo containerlab deploy -t lab.clab.yml --reconfigure
```

Isso irá:

* Criar dois containers Linux (`node-a` e `node-b`) com a imagem `nicolaka/netshoot`.
* Configurar os IPs nas interfaces `eth1` de cada nó.
* Criar um link virtual direto entre as interfaces `eth1` dos dois nós.

Verifique se o laboratório está rodando:

```bash
docker ps
```

---

## 📡 Passo 2 — Verificar Conectividade

Teste a comunicação entre os containers:

```bash
docker exec clab-lab-node-a ping -c 3 10.0.0.2
```

**Resultado esperado:**

```bash
0% packet loss
```

Você também pode testar no sentido contrário:

```bash
docker exec clab-lab-node-b ping -c 3 10.0.0.1
```

---

## 🧹 Limpeza

Para destruir o laboratório e remover todos os containers:

```bash
sudo containerlab destroy -t lab.clab.yml
```

---

## 📂 Estrutura do Projeto

```
projeto-lab/
├── lab.clab.yml              # Definição da topologia Containerlab
└── clab-lab/                 # Arquivos gerados pelo Containerlab
    ├── ansible-inventory.yml
    ├── nornir-simple-inventory.yml
    ├── authorized_keys
    └── topology-data.json
```

---

## 📚 Referências

* [https://containerlab.dev/quickstart/](https://containerlab.dev/quickstart/)
* [https://github.com/nicolaka/netshoot](https://github.com/nicolaka/netshoot)
* [https://docs.docker.com/](https://docs.docker.com/)
