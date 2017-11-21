---
title: "Tam bir Linux VM oluşturmak için Ansible kullanın | Microsoft Docs"
description: "Ansible azure'da tam bir Linux sanal makine ortamı oluşturmak ve yönetmek için nasıl kullanılacağını öğrenin"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: iainfou
ms.openlocfilehash: 8f0e2fff8ea32874729cf9c4645d547df2449089
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Ansible ile azure'da eksiksiz bir Linux sanal makine ortamı oluşturma
Ansible dağıtma ve yapılandırmanın ortamınızdaki kaynakların otomatikleştirmenizi sağlar. Azure, aynı herhangi bir kaynağa olduğu gibi sanal makineleri (VM'ler) yönetmek için Ansible kullanabilirsiniz. Bu makalede eksiksiz bir Linux ortamı ve Ansible kaynaklarla destekleme nasıl oluşturulacağı gösterilmektedir. Ayrıca öğrenebilirsiniz nasıl [Ansible ile temel bir VM oluşturma](ansible-create-vm.md).


## <a name="prerequisites"></a>Ön koşullar
Ansible ile Azure kaynaklarınızı yönetmek için aşağıdakiler gerekir:

- Ansible ve ana bilgisayar sisteminizde yüklü Azure Python SDK'sını modüller.
    - Ansible yüklemek [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), ve [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure kimlik ve onları kullanmak üzere yapılandırılmış Ansible.
    - [Azure kimlik bilgileri oluşturun ve Ansible yapılandırın](ansible-install-configure.md#create-azure-credentials)
- Azure CLI Sürüm 2.0.4 veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. 
    - Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). Aynı zamanda [bulut Kabuk](/azure/cloud-shell/quickstart) tarayıcınızdan.


## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Aşağıdaki bölümde Ansible playbook adlı bir sanal ağ oluşturur *myVnet* içinde *10.0.0.0/16* adres alanı:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

Bir alt ağı eklemek için aşağıdaki bölümde adlı bir alt ağı oluşturur *mySubnet* içinde *myVnet* sanal ağ:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Ortak IP adresi oluştur
Internet'te kaynaklara erişmek için oluşturmak ve VM'nize genel bir IP adresi atayın. Aşağıdaki bölümde Ansible playbook adlı ortak IP adresi oluşturur *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturun
Ağ güvenlik grupları, VM ve ağ trafiği akışını denetler. Aşağıdaki bölümde Ansible playbook adlı bir ağ güvenlik grubu oluşturur *myNetworkSecurityGroup* ve TCP bağlantı noktası 22 SSH trafiğine izin verecek bir kural tanımlar:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Sanal ağ arabirim kartı oluşturma
Bir sanal ağ arabirim kartı (NIC), VM verilen sanal ağ, genel IP adresi ve ağ güvenlik grubu bağlanır. Adlı bir sanal NIC Ansible playbook aşağıdaki bölümde oluşturur *myNIC* oluşturduğunuz sanal ağ kaynaklarına bağlı:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
Son adım, bir VM oluşturun ve oluşturulan tüm kaynakları kullanmaktır. Adlı bir VM'den Ansible playbook aşağıdaki bölümde oluşturur *myVM* ve adlı bir sanal NIC ekler *myNIC*. Kendi ortak anahtar verilerde *key_data* gibi eşleştirin:

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys: 
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Tam Ansible playbook
Bu bölümler araya getirmek için adlı bir Ansible playbook oluşturma *azure_create_complete_vm.yml* ve aşağıdaki içeriği yapıştırın:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Ansible içine tüm kaynaklarınızı dağıtmak için bir kaynak grubu gerekir. [az group create](/cli/azure/vm#create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ansible ile tam VM ortamı oluşturmak için playbook aşağıdaki gibi çalıştırın:

```bash
ansible-playbook azure_create_complete_vm.yml
```

Çıktı VM başarıyla oluşturulup oluşturulmadığını gösteren aşağıdaki örneğe benzer:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnek, gerekli sanal ağ kaynakları içeren tam bir VM ortamı oluşturur. Var olan ağ kaynaklarına varsayılan seçeneklerle içine bir VM oluşturmak daha doğrudan örnek için bkz [bir VM oluşturma](ansible-create-vm.md).
