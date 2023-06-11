# homelab-bootstrap

## Descrição

Esse repositório contém instruções ansible com as configurações padrões dos servidores no meu homelab. Por padrão, estes servidores rodarão containers lxd.

## Utilização

Para configurar um servidor usando ansible, primeiro é necessário instalar o Ubuntu Server (pode ser instalação minimal) e editar o arquivo `hosts/homelab.yml` para inscluir o IP atual da máquina na variável `ansible_host`, o IP fixo utilizado pelo bridge `br0` na variável `target_ip` e a interface de rede conectada a este bridge na variável `interface`.

A primeira execução deve ocorrer com o comando seguinte, onde deve ser fornecido a senha do usuário `victoitor`.

```bash
ansible-playbook playbooks/bootstrap_homelab.yml --ask-become-pass
```

Para as execuções posteriores, é necessário trocar a variável `ansible_host` para o mesmo valor que `target_ip` no arquivo `hosts/homelab.yml`.
As execuções posteriores podem ser feitas com o seguinte comando.

```bash
ansible-playbook playbooks/bootstrap_homelab.yml
```
