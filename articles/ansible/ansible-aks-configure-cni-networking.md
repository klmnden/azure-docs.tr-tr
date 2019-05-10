---
title: Öğretici - Azure Kubernetes Service (AKS) ansible'ı kullanarak Azure CNI ağ yapılandırma | Microsoft Docs
description: Azure Kubernetes Service (AKS) kümesi içinde ağ kubernetes yapılandırmak için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 2d43b1ffbb7910b16c81df2ff5b21e67dbcb0193
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231367"
---
# <a name="tutorial-configure-azure-cni-networking-in-azure-kubernetes-service-aks-using-ansible"></a>Öğretici: Azure Kubernetes Service (AKS) ansible'ı kullanarak Azure CNI ağı yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS kullanarak, aşağıdaki ağ modelleri kullanarak bir kümeyi dağıtabilirsiniz:

- [Kubernetes ağ](/azure/aks/configure-kubenet) -ağ kaynaklarını genellikle oluşturulur ve AKS kümesi dağıtılır olarak yapılandırılmış.
- [Azure CNI ağ](/azure/aks/configure-azure-cni) -AKS kümesini mevcut sanal ağ (VNET) kaynakları ve yapılandırmaların bağlı.

Uygulamalarınıza aks'deki ağ hakkında daha fazla bilgi için bkz. [kavramları aks'deki uygulamalar için ağ](/azure/aks/concepts-network).

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * AKS kümesi oluşturma
> * Azure CNI ağını yapılandırma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Bu bölümdeki örnek playbook kodu için kullanılır:

- Sanal ağ oluştur
- Sanal ağ içindeki alt ağ oluşturma

Aşağıdaki playbook'u `vnet.yml` olarak kaydedin:

```yml
- name: Create vnet
  azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefixes_cidr:
          - 10.0.0.0/8

- name: Create subnet
  azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefix_cidr: 10.240.0.0/16
      virtual_network_name: "{{ name }}"
  register: subnet
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Sanal ağda bir AKS kümesi oluşturma

Bu bölümdeki örnek playbook kodu için kullanılır:

- Bir sanal ağ içindeki bir AKS kümesi oluşturun.

Aşağıdaki playbook'u `aks.yml` olarak kaydedin:

```yml
- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster within a VNet
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
          vnet_subnet_id: "{{ vnet_subnet_id }}"
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      network_profile:
          network_plugin: azure
          docker_bridge_cidr: 172.17.0.1/16
          dns_service_ip: 10.2.0.10
          service_cidr: 10.2.0.0/24
  register: aks
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Kullanım `azure_rm_aks_version` desteklenen sürümü bulmak için modül.
- `vnet_subnet_id` Önceki bölümde oluşturduğunuz alt ağı.
- Playbook'u yükler `ssh_key` gelen `~/.ssh/id_rsa.pub`. Değiştirirseniz, tek satırlı biçimde (tırnak işaretleri olmadan) "ssh-rsa" ile başlayan - kullanın.
- `client_id` Ve `client_secret` gelen yüklenen değerler `~/.azure/credentials`, varsayılan kimlik bilgileri dosyası olduğu. Bu değerleri hizmetinize asıl ayarlayabilir veya bu değerleri ortam değişkenlerinden yükleyin:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

## <a name="run-the-sample-playbook"></a>Örnek playbook çalıştırın

Bu bölümdeki örnek playbook kodu, Bu öğretici boyunca gösterilen çeşitli özelliklerini test etmek için kullanılır.

Aşağıdaki playbook'u `aks-azure-cni.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exists
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create vnet
       include_tasks: vnet.yml

     - name: Create AKS
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
       include_tasks: aks.yml

     - name: Show AKS cluster detail
       debug:
           var: aks
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Değişiklik `aksansibletest` kaynak grubunuzun adı değeri.
- Değişiklik `aksansibletest` AKS adınızla değeri.
- Değişiklik `eastus` değeri, kaynak grubu konumu.

Ansible playbook komutunu kullanarak playbook çalıştırın:

```bash
ansible-playbook aks-azure-cni.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Ensure resource group exists] 
changed: [localhost]

TASK [Create vnet] 
included: /home/devops/aks-cni/vnet.yml for localhost

TASK [Create vnet] 
changed: [localhost]

TASK [Create subnet] 
changed: [localhost]

TASK [Create AKS] 
included: /home/devops/aks-cni/aks.yml for localhost

TASK [List supported kubernetes version from Azure] 
 [WARNING]: Azure API profile latest does not define an entry for
ContainerServiceClient

ok: [localhost]

TASK [Create AKS cluster with vnet] 
changed: [localhost]

TASK [Show AKS cluster detail] 
ok: [localhost] => {
    "aks": {
        "aad_profile": {},
        "addon": {},
        "agent_pool_profiles": [
            {
                "count": 3,
                "name": "nodepool1",
                "os_disk_size_gb": 100,
                "os_type": "Linux",
                "storage_profile": "ManagedDisks",
                "vm_size": "Standard_D2_v2",
                "vnet_subnet_id": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourceGroups/aksansibletest/providers/Microsoft.Network/virtualNetworks/aksansibletest/subnets/aksansibletest"
            }
        ],
        "changed": true,
        "dns_prefix": "aksansibletest",
        "enable_rbac": false,
        "failed": false,
        "fqdn": "aksansibletest-0272707d.hcp.eastus.azmk8s.io",
        "id": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourcegroups/aksansibletest/providers/Microsoft.ContainerService/managedClusters/aksansibletest",
        "kube_config": "..."
        },
        "location": "eastus",
        "name": "aksansibletest",
        "network_profile": {
            "dns_service_ip": "10.2.0.10",
            "docker_bridge_cidr": "172.17.0.1/16",
            "network_plugin": "azure",
            "network_policy": null,
            "pod_cidr": null,
            "service_cidr": "10.2.0.0/24"
        },
        "node_resource_group": "MC_aksansibletest_aksansibletest_eastus",
        "provisioning_state": "Succeeded",
        "service_principal_profile": {
            "client_id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"
        },
        "tags": null,
        "type": "Microsoft.ContainerService/ManagedClusters",
        "warnings": [
            "Azure API profile latest does not define an entry for ContainerServiceClient",
            "Azure API profile latest does not define an entry for ContainerServiceClient"
        ]
    }
}

PLAY RECAP 
localhost                  : ok=9    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Bu bölümdeki örnek playbook kodu için kullanılır:

- Bir kaynak grubu için başvurulan silme `vars` bölümü.

Aşağıdaki playbook'u `cleanup.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: {{ resource_group_name }}
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.
- Belirtilen kaynak grubu içindeki tüm kaynaklar silinir.

Ansible playbook komutunu kullanarak playbook çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory Ansible kullanarak AKS yapılandırın](./ansible-aks-configure-rbac.md)