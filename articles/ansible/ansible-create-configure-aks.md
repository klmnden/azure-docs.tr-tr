---
title: Oluşturma ve Azure'da ansible'ı kullanarak Azure Kubernetes Service kümeleri yapılandırma
description: Oluşturmak ve azure'da Azure Kubernetes Service kümesini yönetmek için Ansible'ı kullanmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, Kubernetes
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 6d7c5f961256e0ae1831bd76353cadd761f4b8ac
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009206"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Oluşturma ve Azure'da ansible'ı kullanarak Azure Kubernetes Service kümeleri yapılandırma
Ansible, dağıtımını ve yapılandırmasını, ortamınızdaki kaynakları otomatikleştirmenize olanak tanır. Azure Kubernetes Service (AKS) yönetmek için Ansible'ı kullanabilirsiniz. Bu makalede Azure Kubernetes hizmeti kümesi oluşturmalı ve yapılandırmalısınız için ansible'ı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - oluşturma, bir Azure aboneliği yoksa, bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.
- **Ansible'ı yapılandırma** - [oluşturma Azure kimlik bilgileri ve ansible'ı yapılandırma](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible'ı ve Azure Python SDK'sı modüller** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Azure hizmet sorumlusu** - [hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal), aşağıdaki değerleri not edin: **AppID**, **displayName**, **parola** , ve **Kiracı**.

> [!Note]
> Ansible 2.6 aşağıdaki çalıştırılması için gerekli Bu öğreticide örnek playbook'ları. 

## <a name="create-a-managed-aks-cluster"></a>Yönetilen bir AKS kümesi oluşturma
Aşağıdaki örnek Ansible playbook bir kaynak grubu ve kaynak grubunda bulunan bir AKS kümesi oluşturur:

  ```yaml
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

Bunlar aşağıda maddeler, yukarıdaki Ansible playbook kod açıklamak için yardımcı olur:
- İlk bölümde **görevleri** adlı bir kaynak grubu tanımlar **myResourceGroup** içinde **eastus** konumu. 
- İkinci kısımda **görevleri** adlı bir AKS kümesi tanımlar **myAKSCluster** içinde **myResourceGroup** kaynak grubu. 

Ansible ile AKS kümesi oluşturmak için önceki örnek playbook olarak Kaydet `azure_create_aks.yml`, ve playbook'u ile aşağıdaki komutu çalıştırın:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

Çıkışı **ansible playbook* komutu aşağıdaki AKS kümesi başarıyla oluşturulduğunu gösteren için benzer görünür:

  ```bash
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>AKS düğümlerini ölçeklendirme

Önceki bölümdeki örnek playbook iki düğümü tanımlar. Kümenizde daha az veya daha fazla kapsayıcı iş yüklerinin gerekiyorsa, düğüm sayısını kolayca ayarlayabilirsiniz. Bu bölümdeki örnek playbook üç iki düğümden düğüm sayısını artırır. Düğüm sayısı değiştirme yapılır değiştirerek **sayısı** değerini **agent_pool_profiles** blok. 

Kendi girin `ssh_key`, `client_id`, ve `client_secret` içinde **service_principal** engelle:

```yaml
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

Ansible ile Azure Kubernetes Service kümesini ölçeklendirme için önceki playbook olarak kaydedin. *azure_configure_aks.yml*, ve playbook aşağıdaki gibi çalıştırın:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

Aşağıdaki çıktı, AKS kümesi başarıyla oluşturulduğunu gösterir:

  ```bash
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Öğretici: Azure Kubernetes Service (AKS) uygulama ölçeklendirme](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale)