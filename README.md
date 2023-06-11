# homelab-bootstrap

## Descrição

Esse repositório contém instruções ansible com as configurações padrões dos servidores no meu homelab. Por padrão, estes servidores rodarão containers lxd.

## Utilização

Para configurar um servidor usando ansible, primeiro é necessário instalar o Ubuntu Server minimal. O particionamento do disco na instalação deve conter 4 partições. A primeira de boot, a segunda para `/` com 30GB de espaço, a terceira de `swap` e a quarta com o restante do disco deve ser criada como não formatada. Uma vez que a máquina for inicializada, deve-se logar na máquina e rodar `ip a` para identificar o IP atual. Em seguida, é necessário editar o arquivo `hosts/homelab.yml` para inscluir o IP atual da máquina na variável `ansible_host`, o IP fixo utilizado pelo bridge `br0` na variável `target_ip` e a interface de rede conectada a este bridge na variável `interface`.

A primeira execução deve ocorrer com o comando seguinte, onde deve ser fornecido a senha do usuário `victoitor`.

```bash
ansible-playbook playbooks/00-bootstrap-homelab.yml --ask-become-pass
```

Para as execuções posteriores, é necessário trocar a variável `ansible_host` para o mesmo valor que `target_ip` no arquivo `hosts/homelab.yml`.
As execuções posteriores podem ser feitas com o seguinte comando.

```bash
ansible-playbook playbooks/00-bootstrap-homelab.yml
```

Depois de rodar o playbook de bootstrap, é necessário iniciar o `lxd` manualmente na máquina usando `sudo lxd init`. Em seguida estão as respostas padrão, onde o endereço da partição fornecida para storage é obtido com o comando `lsblk` como a partição não formatada na instalação do Ubuntu Server.

```
Would you like to use LXD clustering? (yes/no) [default=no]: 
Do you want to configure a new storage pool? (yes/no) [default=yes]: 
Name of the new storage pool [default=default]: 
Name of the storage backend to use (zfs, btrfs, ceph, dir, lvm) [default=zfs]: btrfs
Create a new BTRFS pool? (yes/no) [default=yes]: 
Would you like to use an existing empty block device (e.g. a disk or partition)? (yes/no) [default=no]: yes
Path to the existing block device: /dev/sda4
Would you like to connect to a MAAS server? (yes/no) [default=no]: 
Would you like to create a new local network bridge? (yes/no) [default=yes]: no
Would you like to configure LXD to use an existing bridge or host interface? (yes/no) [default=no]: yes
Name of the existing bridge or host interface: br0
Would you like the LXD server to be available over the network? (yes/no) [default=no]: yes
Address to bind LXD to (not including port) [default=all]: 
Port to bind LXD to [default=8443]: 
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]: 
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]: 

```