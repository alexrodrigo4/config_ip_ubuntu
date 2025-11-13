## 📘 README.md — Configuração de IP Estático no Ubuntu Server

markdown
# 🌐 Configuração de IP Estático no Ubuntu Server

Guia completo e direto ao ponto para **visualizar, editar e configurar IP fixo** em servidores Ubuntu.

---

## 🧭 1️⃣ Verificando as configurações de rede atuais

### 🔹 Listar interfaces e endereços IP
bash
ip addr show


ou simplesmente:

bash
ip a


### 🔹 Mostrar rotas e gateway

bash
ip route show


### 🔹 Listar interfaces de rede

bash
ls /sys/class/net/


*(Use isso para descobrir o nome da interface, como ens33, eth0, enp0s3, etc.)*

---

## ⚙️ 2️⃣ Editando as configurações do Netplan

No Ubuntu Server (18.04 ou superior), a rede é configurada via **Netplan**.
Os arquivos de configuração ficam em:


/etc/netplan/


Exemplo comum:


/etc/netplan/00-installer-config.yaml


Abra o arquivo com um editor de texto:

bash
sudo nano /etc/netplan/00-installer-config.yaml


---

## 📝 3️⃣ Exemplo de configuração de IP estático

Substitua ens33 pelo nome da sua interface e ajuste IP, gateway e DNS conforme sua rede:

yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - 172.16.0.252/24
      gateway4: 172.16.0.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1


⚠️ **Atenção:** O arquivo YAML é sensível à indentação.
Use **somente espaços**, **nunca TAB**.

---

## 🚀 4️⃣ Aplicando as mudanças

Após salvar o arquivo (Ctrl + O, Enter, Ctrl + X), execute:

bash
sudo netplan apply


Se quiser testar antes de aplicar definitivamente:

bash
sudo netplan try


(Se algo der errado, ele reverte automaticamente em 120 segundos.)

---

## ✅ 5️⃣ Verificando a nova configuração

Confirme o IP:

bash
ip a


Teste a conectividade:

bash
ping -c 4 8.8.8.8
ping -c 4 google.com


---

## 🧩 6️⃣ Dicas extras

* Para reiniciar completamente o serviço de rede:

  bash
  sudo systemctl restart systemd-networkd
  
* Para verificar logs da rede:

  bash
  sudo journalctl -u systemd-networkd --since "5 minutes ago"
  
* Para listar todas as configurações Netplan aplicadas:

  bash
  sudo netplan get
  

---

## 🧠 7️⃣ Exemplo completo de ambiente local

| Interface | IP Estático  | Gateway    | DNS Primário | DNS Secundário |
| --------- | ------------ | ---------- | ------------ | -------------- |
| ens33     | 172.16.0.252 | 172.16.0.1 | 8.8.8.8      | 1.1.1.1        |

---

## 🧾 Autor e Créditos

**Autor:** [Rodrigo](https://github.com/seuusuario)
**Função:** SysAdmin & DevOps
**Versões Suportadas:** Ubuntu Server 18.04, 20.04, 22.04, 24.04
**Licença:** MIT

---

💡 *Este guia pode ser incluído em qualquer repositório de scripts de infraestrutura (Zabbix, Bind9, Proxmox, etc.).*



---

## 💾 Como adicionar no GitHub

1️⃣ Crie o arquivo:
bash
nano README.md


2️⃣ Cole o conteúdo acima e salve (Ctrl + O, Enter, Ctrl + X).

3️⃣ Suba pro seu repositório:

bash
git add README.md
git commit -m "Adicionado guia de IP estático no Ubuntu Server"
git push origin main



