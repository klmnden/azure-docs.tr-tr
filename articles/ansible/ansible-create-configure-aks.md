---
title: Ansible'ı kullanarak Azure'da Azure Kubernetes Service kümeleri oluşturma ve yapılandırma
description: Ansible'ı kullanarak Azure'da Azure Kubernetes Service kümesi oluşturmayı ve kullanmayı öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, Kubernetes
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/23/2018
ms.openlocfilehash: be9de3c10fdfd040b249dd8da568259c599d7215
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54050396"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Ansible'ı kullanarak Azure'da Azure Kubernetes Service kümeleri oluşturma ve yapılandırma
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Ansible'ı kullanarak Azure Kubernetes Service (AKS) örneğinizi yönetebilirsiniz. Bu makalede Ansible'ı kullanarak Azure Kubernetes Service kümesi oluşturma ve yapılandırma adımları gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- **Azure hizmet sorumlusu** - [Hizmet sorumlusunu oluştururken](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal) şu değerleri not edin: **appId**, **displayName**, **password** ve **tenant**.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.6 gerekir. 

## <a name="create-a-managed-aks-cluster"></a>Yönetilen AKS kümesi oluşturma
Aşağıdaki örnek Ansible playbook'u, bir kaynak grubu ve bu kaynak grubunun içinde bir AKS kümesi oluşturur:

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

Aşağıdaki maddeler yukarıdaki Ansible playbook kodunun açıklanmasına yardımcı olur:
- **tasks** içindeki ilk bölüm, **eastus** konumunda **myResourceGroup** adlı bir kaynak grubu oluşturur. 
- **tasks** bölümündeki ikinci bölüm, **myResourceGroup** kaynak grubunun içinde **myAKSCluster** adlı bir AKS kümesi tanımlar. 

Ansible ile AKS kümesi oluşturmak için önceki örnek playbook'u `azure_create_aks.yml` olarak kaydedin ve playbook'u şu komutla çalıştırın:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

**ansible-playbook* komutunun çıktısı, aşağıdakine benzer ve AKS kümesinin başarıyla oluşturulduğunu gösterir:

  ```Output
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

Önceki bölümde yer alan örnek playbook, iki düğüm tanımlar. Kümenizde daha az veya daha fazla kapsayıcı iş yüküne ihtiyacınız varsa düğüm sayısını kolayca ayarlayabilirsiniz. Bu bölümdeki örnek playbook, düğüm sayısını ikiden üçe çıkarır. Düğüm sayısını değiştirmek için **agent_pool_profiles** bloğundaki **count** değerini değiştirmeniz gerekir. 

**service_principal** bloğuna kendi `ssh_key`, `client_id` ve `client_secret` değerlerinizi girin:

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

Azure Kubernetes Service kümesini Ansible ile birlikte ölçeklendirmek için önceki playbook'u *azure_configure_aks.yml* olarak kaydedin ve playbook'u aşağıdaki şekilde çalıştırın:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

Aşağıdaki çıktı, AKS kümesinin başarıyla oluşturulduğunu gösterir:

  ```Output
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
## <a name="delete-a-managed-aks-cluster"></a>Yönetilen AKS kümesini silme

Aşağıdaki örnek Ansible playbook bölümü, AKS kümesinin nasıl silineceğini göstermektedir:

  ```yaml
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

Azure Kubernetes Service kümesini Ansible ile birlikte silmek için önceki playbook'u *azure_delete_aks.yml* olarak kaydedin ve playbook'u aşağıdaki şekilde çalıştırın:

  ```bash
  ansible-playbook azure_delete_aks.yml
  ```

Aşağıdaki çıktı, AKS kümesinin başarıyla silindiğini gösterir:
  ```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] ****************************

TASK [Gathering Facts] ********************************************************************
ok: [localhost]

TASK [azure_rm_aks] *********************************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
  
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Öğretici: Azure Kubernetes Service (AKS) uygulama ölçeklendirme](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale)
