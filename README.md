# homelab-bootstrap

## Descrição

Esse repositório contém instruções ansible com as configurações padrões dos servidores no meu homelab. Por padrão, estes servidores rodarão containers incus.

## Utilização

Esta configuração assume que foi instalado um servidor debian 12. Uma vez que a máquina for inicializada, deve-se logar na máquina e rodar `ip a` para identificar o IP atual. Em seguida, é necessário editar o arquivo `hosts/homelab.yml` para inscluir o IP atual da máquina na variável `ansible_host`. O IP fixo utilizado pelo bridge `br0` deve ser feito por dhcp. 


```bash
ansible-playbook playbooks/00-bootstrap-homelab.yml --ask-pass --ask-become-pass
```

Para as execuções posteriores, é necessário trocar a variável `ansible_host` para o mesmo valor que `target_ip` no arquivo `hosts/homelab.yml`.
As execuções posteriores podem ser feitas com o seguinte comando.

```bash
ansible-playbook playbooks/00-bootstrap-homelab.yml
```
