---
title: Öğretici - Azure Kubernetes Service (AKS) kümelerini ansible'ı kullanarak Azure'da yapılandırma | Microsoft Docs
description: Ansible'ı kullanarak Azure'da Azure Kubernetes Service kümesi oluşturmayı ve kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 64b8eb12c755f41cde28067b5c145c4e95066886
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688454"
---
# <a name="tutorial-configure-azure-kubernetes-service-aks-clusters-in-azure-using-ansible"></a>Öğretici: Azure'da ansible'ı kullanarak Azure Kubernetes Service (AKS) kümelerini yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS, kullanılacak yapılandırılabilir [Azure Active Directory (AD)](/azure/active-directory/) kullanıcı kimlik doğrulaması. Yapılandırıldıktan sonra AKS kümesine imzalamak için Azure AD kimlik doğrulama belirteci kullanın. RBAC, bir kullanıcının kimliğini veya directory grubu üyeliği dayanabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * AKS kümesi oluşturma
> * Bir AKS kümesi yapılandırma

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-managed-aks-cluster"></a>Yönetilen AKS kümesi oluşturma

Örnek playbook bir kaynak grubu ve kaynak grubu içinde bir AKS kümesi oluşturur.

Aşağıdaki playbook'u `azure_create_aks.yml` olarak kaydedin:

```yml
- name: Create Azure Kubernetes Service
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create a managed Azure Container Services (AKS) cluster
    azure_rm_aks:
      name: "{{ aks_name }}"
      location: "{{ location }}"
      resource_group: "{{ resource_group }}"
      dns_prefix: "{{ aks_name }}"
      linux_profile:
        admin_username: "{{ username }}"
        ssh_key: "{{ ssh_key }}"
      service_principal:
        client_id: "{{ client_id }}"
        client_secret: "{{ client_secret }}"
      agent_pool_profiles:
        - name: default
          count: 2
          vm_size: Standard_D2_v2
      tags:
        Environment: Production
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

- İlk bölümde `tasks` adlı bir kaynak grubu tanımlar `myResourceGroup` içinde `eastus` konumu.
- İkinci kısımda `tasks` adlı bir AKS kümesi tanımlar `myAKSCluster` içinde `myResourceGroup` kaynak grubu.
- İçin `your_ssh_key` yer tutucusu, tek satırlı biçimde (tırnak işaretleri olmadan) "ssh-rsa" ile başlayan - RSA ortak anahtarınızı girin.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook azure_create_aks.yml
```

Playbook'u çalıştırdıktan sonuç aşağıdaki çıktıya benzer gösterir:

```Output
PLAY [Create AKS] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create resource group] 
changed: [localhost]

TASK [Create an Azure Container Services (AKS) cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="scale-aks-nodes"></a>AKS düğümlerini ölçeklendirme

Önceki bölümde yer alan örnek playbook, iki düğüm tanımlar. Düğüm sayısını değiştirerek ayarlamak `count` değerini `agent_pool_profiles` blok.

Aşağıdaki playbook'u `azure_configure_aks.yml` olarak kaydedin:

```yml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

- İçin `your_ssh_key` yer tutucusu, tek satırlı biçimde (tırnak işaretleri olmadan) "ssh-rsa" ile başlayan - RSA ortak anahtarınızı girin.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook azure_configure_aks.yml
```

Playbook'u çalıştırdıktan sonuç aşağıdaki çıktıya benzer gösterir:

```Output
PLAY [Scale AKS cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Scaling an existed AKS cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="delete-a-managed-aks-cluster"></a>Yönetilen AKS kümesini silme

Örnek playbook AKS kümesini siler.

Aşağıdaki playbook'u `azure_delete_aks.yml` olarak kaydedin:


```yml
- name: Delete a managed Azure Container Services (AKS) cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    aks_name: myAKSCluster
  tasks:
  - name:
    azure_rm_aks:
      name: "{{ aks_name }}"
      resource_group: "{{ resource_group }}"
      state: absent
  ```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook azure_delete_aks.yml
```

Playbook'u çalıştırdıktan sonuç aşağıdaki çıktıya benzer gösterir:

```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [azure_rm_aks] 

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Kubernetes Service (AKS) uygulama ölçeklendirme](/azure/aks/tutorial-kubernetes-scale)