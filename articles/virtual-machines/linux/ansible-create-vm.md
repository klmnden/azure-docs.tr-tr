---
title: "Temel bir Linux VM oluşturmak için Ansible kullanın | Microsoft Docs"
description: "Ansible oluşturmak ve azure'da temel bir Linux sanal makine yönetmek için nasıl kullanılacağını öğrenin"
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
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 184a30c91de0d4141d6bd8a8b9db93c539e083b5
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Azure Ansible ile temel bir sanal makine oluşturun
Ansible dağıtma ve yapılandırmanın ortamınızdaki kaynakların otomatikleştirmenizi sağlar. Azure, aynı herhangi bir kaynağa olduğu gibi sanal makineleri (VM'ler) yönetmek için Ansible kullanabilirsiniz. Bu makalede Ansible ile temel bir VM oluşturulacağını gösterir. Ayrıca öğrenebilirsiniz nasıl [Ansible ile eksiksiz bir VM ortamı oluşturma](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Önkoşullar
Ansible ile Azure kaynaklarınızı yönetmek için aşağıdakiler gerekir:

- Ansible ve ana bilgisayar sisteminizde yüklü Azure Python SDK'sını modüller.
    - Ansible yüklemek [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), ve [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure kimlik ve onları kullanmak üzere yapılandırılmış Ansible.
    - [Azure kimlik bilgileri oluşturun ve Ansible yapılandırın](ansible-install-configure.md#create-azure-credentials)
- Azure CLI Sürüm 2.0.4 veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. 
    - Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). Aynı zamanda [bulut Kabuk](/azure/cloud-shell/quickstart) tarayıcınızdan.


## <a name="create-supporting-azure-resources"></a>Azure kaynaklarını destekleyen oluşturma
Bu örnekte, var olan bir altyapısının bir VM dağıtır bir runbook oluşturun. İlk olarak, kaynak grubu ile oluşturma [az grubu oluşturma](/cli/azure/vm#create). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli
az group create --name myResourceGroup --location eastus
```

İle VM için sanal ağ oluşturma [az ağ vnet oluşturma](/cli/azure/network/vnet#create). Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* ve adlı bir alt ağ *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Oluşturma ve Ansible playbook çalıştırma
Adlı bir Ansible playbook oluşturma *azure_create_vm.yml* ve aşağıdaki içeriğini yapıştırın. Bu örnek, tek bir VM'ye oluşturur ve SSH kimlik bilgilerini yapılandırır. Kendi tam genel anahtar verilerde *key_data* gibi eşleştirin:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
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
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

VM ile Ansible oluşturmak için playbook aşağıdaki gibi çalıştırın:

```bash
ansible-playbook azure_create_vm.yml
```

Çıktı VM başarıyla oluşturulup oluşturulmadığını gösteren aşağıdaki örneğe benzer:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Sonraki adımlar
Bu örnek bir VM varolan bir kaynak grubu ve dağıtılmış bir sanal ağ oluşturur. Bir sanal ağ ve ağ güvenlik grubu kuralları gibi destekleyici kaynakları oluşturmak için Ansible kullanma hakkında daha ayrıntılı bir örnek için bkz: [Ansible ile eksiksiz bir VM ortamı oluşturma](ansible-create-complete-vm.md).
