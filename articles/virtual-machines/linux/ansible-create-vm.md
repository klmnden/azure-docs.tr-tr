---
title: Azure'da temel bir Linux VM oluşturmak için Ansible'ı kullanma | Microsoft Docs
description: Oluşturmak ve azure'da temel bir Linux sanal makinesi yönetmek için Ansible'ı kullanmayı öğrenin
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
ms.openlocfilehash: 35dfe8348718e0edf8683f7eeddf286831697d89
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931438"
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Azure'da Ansible ile temel bir sanal makine oluşturma
Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Azure, aynı diğer herhangi bir kaynağa olduğu gibi sanal makinelerinizde (VM'ler) yönetmek için Ansible'ı kullanabilirsiniz. Bu makalede Ansible ile temel bir VM oluşturma işlemini gösterir. Ayrıca edinebilirsiniz nasıl [Ansible ile eksiksiz bir VM ortamı oluşturma](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Önkoşullar
Ansible ile Azure kaynaklarını yönetmek için aşağıdakiler gerekir:

- Ansible'ı ve konak sisteminizde yüklü Azure Python SDK'sı modüller.
    - Ansible'ı yükleyin [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), ve [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure kimlik bilgileri ve bunları kullanmak üzere yapılandırılmış ansible'ı.
    - [Azure kimlik bilgileri oluşturun ve ansible'ı yapılandırma](ansible-install-configure.md#create-azure-credentials)
- Azure CLI 2.0.4 sürümü veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. 
    - Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). Ayrıca [Azure Cloud Shell](/azure/cloud-shell/quickstart) web tarayıcınızdan.


## <a name="create-supporting-azure-resources"></a>Destekleyici Azure kaynakları oluşturma
Bu örnekte, mevcut bir altyapı VM dağıtır bir runbook oluşturun. İlk olarak, kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group#az-group-create). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Sanal ağ ile sanal Makineniz için oluşturma [az ağ sanal ağ oluşturma](/cli/azure/network/vnet#az-network-vnet-create). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet* ve adlı bir alt ağ *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Oluşturun ve Ansible playbook çalıştırın
Adlı bir Ansible playbook oluşturmak *azure_create_vm.yml* ve aşağıdaki içeriği yapıştırın. Bu örnek, tek bir VM oluşturur ve SSH kimlik bilgileri yapılandırır. Kendi tam genel anahtar verilerde *key_data* şu şekilde eşleştirin:

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
        sku: '7.5'
        version: latest
```

Ansible ile sanal makine oluşturmak için şu şekilde playbook çalıştırın:

```bash
ansible-playbook azure_create_vm.yml
```

Çıktı, sanal Makinenin başarıyla oluşturulduğunu gösteren aşağıdaki örneğe benzer:

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
Bu örnekte, mevcut bir kaynak grubu ve zaten dağıtılmış bir sanal ağ ile bir VM oluşturur. Bir sanal ağ ve ağ güvenlik grubu kuralları gibi destekleyici kaynakları oluşturmak için Ansible'ı kullanma hakkında daha ayrıntılı bir örnek için bkz [Ansible ile eksiksiz bir VM ortamı oluşturma](ansible-create-complete-vm.md).
