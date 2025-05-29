# 🧱 Expansão de Disco em VM sem Parada

Este tutorial mostra como expandir o disco de uma máquina virtual **sem precisar reiniciar a VM**. O procedimento é feito com a máquina ligada, usando ferramentas como `parted` e `resize2fs`.

---

## 🧭 Sumário

- [Cenário](#cenário)
- [Desafio](#desafio)
- [Pré-requisitos](#pré-requisitos)
- [Passo a passo](#passo-a-passo)
  - [1. Aumentar o disco no virtualizador](#1-aumentar-o-disco-no-virtualizador)
  - [2. Fazer a VM reconhecer o novo tamanho do disco](#2-fazer-a-vm-reconhecer-o-novo-tamanho-do-disco)
  - [3. Redimensionar a partição com `parted`](#3-redimensionar-a-partição-com-parted)
  - [4. Expandir o sistema de arquivos com `resize2fs`](#4-expandir-o-sistema-de-arquivos-com-resize2fs)
- [Resultado](#resultado)
- [Referências](#referências)

---

## 📘 Cenário

- Máquina virtual com disco de 30GB (`/dev/sda`)
- Tabela de partição: **GPT**
- Sistema de arquivos: **ext4**
- Ambiente virtualizado (KVM, VMware, VirtualBox, Proxmox, etc.)

---

## ❗ Desafio

Expandir o disco da VM de **30GB para 50GB** com a máquina ligada, sem causar interrupções no serviço.

---

## ⚙️ Pré-requisitos

- Acesso root ou permissões com `sudo`
- Ferramentas instaladas:
  - `parted`
  - `resize2fs` (disponível no pacote `e2fsprogs`)
- Backup (recomendado)

---

## 🛠️ Passo a passo

### 1. Aumentar o disco no virtualizador

Expanda o disco da VM no seu hipervisor (Proxmox, VMware, etc.) de **30GB para 50GB**.

### 2. Fazer a VM reconhecer o novo tamanho do disco

Execute o comando abaixo na VM para forçar a revarredura do disco:

```bash
echo 1 > /sys/class/block/sda/device/rescan
```

### 3. Redimensionar a partição com `parted`

Inicie a ferramenta:

```bash
parted /dev/sda
```

No shell do `parted`, veja o espaço livre:

```bash
(parted) unit MB print free
```

Se o `parted` exibir um aviso dizendo que nem todo o espaço está sendo usado, digite:

```
Fix
```

Agora, redimensione a partição existente (geralmente a `2`):

```bash
(parted) resizepart
Partition number? 2
End? [digite o valor final do disco, como 50000MB ou 50GB]
```

Confirme os avisos com `Yes`, se solicitado, e saia:

```bash
(parted) quit
```

### 4. Expandir o sistema de arquivos com `resize2fs`

Execute o comando abaixo para expandir o sistema de arquivos:

```bash
resize2fs /dev/sda2
```

---

## ✅ Resultado

Após esses passos, a VM estará com o disco expandido de 30GB para 50GB **sem precisar reiniciar**. Você pode confirmar com:

```bash
df -h
```
