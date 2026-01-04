# 🖧 Configuração de IP Fixo e DNS Manual no Ubuntu Server 24.04

Este tutorial mostra **como configurar IP fixo e DNS manualmente no Ubuntu Server 24.04**, **sem interface gráfica**, utilizando o **Netplan**. Ideal para ambientes de servidor, laboratórios e redes internas.

---

## 📌 Cenário

| Item      | Valor                                     |
| --------- | ----------------------------------------- |
| IPs Fixos | 172.16.0.253 e 172.16.0.254           |
| Máscara   | /24 (255.255.255.0)                   |
| Gateway   | 172.16.0.254                            |
| DNS       | 172.16.0.254, 172.16.0.253, 8.8.8.8 |

---

## 🔍 1. Identificar a interface de rede

Execute:

bash
ip a


Você verá algo semelhante a:

* enp0s3
* ens33
* eth0

> 📌 **Anote o nome da interface**, pois será usado no arquivo de configuração.

---

## 🗂️ 2. Localizar o arquivo do Netplan

Liste os arquivos disponíveis:

bash
ls /etc/netplan/


Normalmente o arquivo será algo como:

* 00-installer-config.yaml

Edite o arquivo:

bash
sudo nano /etc/netplan/00-installer-config.yaml


---

## ✍️ 3. Configuração de IP Fixo + DNS Manual

> ⚠️ **Atenção:** YAML é sensível à indentação. Use **apenas espaços**, nunca TAB.

yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 172.16.0.253/24
        - 172.16.0.254/24
      routes:
        - to: default
          via: 172.16.0.254
      nameservers:
        addresses:
          - 172.16.0.254
          - 172.16.0.253
          - 8.8.8.8


🔁 **Substitua enp0s3 pelo nome real da sua interface**.

---

## 💾 4. Salvar e aplicar a configuração

No editor **nano**:

* Ctrl + O → Enter (salvar)
* Ctrl + X → sair

Aplicar as configurações:

bash
sudo netplan apply


Para depuração:

bash
sudo netplan apply --debug


---

## ✅ 5. Testes e validação

### 📡 Verificar IP configurado

bash
ip a


---

### 🌐 Testar conectividade com o gateway

bash
ping 172.16.0.254


---

### 🌎 Testar resolução DNS

bash
ping google.com


---

### 🔎 Verificar DNS ativos

bash
resolvectl status


---

## ⚠️ Observações Importantes

* ❌ **Não edite** /etc/resolv.conf manualmente
* ✔️ O Netplan gerencia automaticamente a rede
* 🔐 Em servidores remotos, cuidado para não perder acesso SSH

---

## 📚 Referências

* Documentação oficial Netplan: [https://netplan.io/](https://netplan.io/)
* Ubuntu Server 24.04 LTS

---

🚀 **Configuração concluída com sucesso!**

Se este guia te ajudou, considere deixar uma ⭐ no repositório.
