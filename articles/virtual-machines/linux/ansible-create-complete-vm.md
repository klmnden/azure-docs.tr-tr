---
title: Azure'da eksiksiz bir Linux VM oluşturmak için Ansible'ı kullanma | Microsoft Docs
description: Oluşturmak ve azure'da eksiksiz bir Linux sanal makine ortamı yönetmek için Ansible'ı kullanmayı öğrenin
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 63228f8bf8729f1bf3796a77516490ae7088d5ed
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930853"
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Azure'da Ansible ile eksiksiz bir Linux sanal makine ortamı oluşturma
Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Azure, aynı diğer herhangi bir kaynağa olduğu gibi sanal makinelerinizde (VM'ler) yönetmek için Ansible'ı kullanabilirsiniz. Bu makalede eksiksiz bir Linux ortamı ve destekleyen kaynaklar Ansible ile nasıl oluşturulacağı gösterilmektedir. Ayrıca edinebilirsiniz nasıl [Ansible ile temel VM oluşturma](ansible-create-vm.md).


## <a name="prerequisites"></a>Önkoşullar
Ansible ile Azure kaynaklarını yönetmek için aşağıdakiler gerekir:

- Ansible'ı ve konak sisteminizde yüklü Azure Python SDK'sı modüller.
    - Ansible'ı yükleyin [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), ve [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure kimlik bilgileri ve bunları kullanmak üzere yapılandırılmış ansible'ı.
    - [Azure kimlik bilgileri oluşturun ve ansible'ı yapılandırma](ansible-install-configure.md#create-azure-credentials)
- Azure CLI 2.0.4 sürümü veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. 
    - Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). Ayrıca [Cloud Shell](/azure/cloud-shell/quickstart) tarayıcınızdan.


## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Şimdi her bir Ansible playbook bölümüne bakın ve tek tek Azure kaynaklarını oluşturun. Tam playbook'u için bkz: [makalenin bu bölümü](#complete-ansible-playbook).

Aşağıdaki bölümde bir Ansible playbook adlı bir sanal ağ oluşturur *myVnet* içinde *10.0.0.0/16* adres alanı:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Bir alt ağı eklemek için aşağıdaki bölümde adlı bir alt ağ oluşturur *mySubnet* içinde *myVnet* sanal ağ:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Genel IP adresi oluşturma
Kaynakları Internet üzerinden erişmek için oluşturma ve sanal makinenizde bir genel IP adresi atayın. Aşağıdaki bölümde bir Ansible playbook adlı bir genel IP adresi oluşturur *Mypublicıp*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Ağ güvenlik grupları, sanal Makinenizin içine ve dışına ağ trafiği akışını denetler. Aşağıdaki bölümde bir Ansible playbook adlı bir ağ güvenlik grubu oluşturur *Vm2* ve TCP bağlantı noktası 22 SSH trafiğine izin verecek bir kural tanımlar:

```yaml
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
```


## <a name="create-virtual-network-interface-card"></a>Sanal ağ arabirim kartı oluşturun
Bir sanal ağ arabirim kartı (NIC), belirtilen sanal ağ, genel IP adresi ve ağ güvenlik grubu, sanal Makinenizin bağlanır. Aşağıdaki bölümde bir Ansible playbook adlı bir sanal NIC oluşturur *Mynıc* oluşturduğunuz sanal ağ kaynaklarına bağlı:

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
Son adım, bir VM oluşturun ve oluşturulan tüm kaynakları kullanmaktır. Aşağıdaki bölümde bir Ansible playbook adlı bir VM oluşturur *myVM* ve adlı sanal NIC'yi ekler *Mynıc*. Kendi tam genel anahtar verilerde *key_data* şu şekilde eşleştirin:

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
      sku: '7.5'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Ansible playbook tamamlayın
Bu bölüm bir araya getirip adlı bir Ansible playbook oluşturmak *azure_create_complete_vm.yml* ve aşağıdaki içeriği yapıştırın. Kendi tam genel anahtar verilerde *key_data* çifti:

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
        sku: '7.5'
        version: latest
```

Ansible'ı tüm kaynaklarınızı dağıtmak için bir kaynak grubu gerekir. [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ansible ile tam sanal makine ortamınızı oluşturmak için şu şekilde playbook çalıştırın:

```bash
ansible-playbook azure_create_complete_vm.yml
```

Çıktı, sanal Makinenin başarıyla oluşturulduğunu gösteren aşağıdaki örneğe benzer:

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
Bu örnek, gerekli sanal ağ kaynakları dahil olmak üzere tam bir VM ortamı oluşturur. Mevcut ağ kaynaklarına varsayılan seçeneklerle içine bir VM oluşturmak daha doğrudan örneği için bkz [VM oluşturma](ansible-create-vm.md).
