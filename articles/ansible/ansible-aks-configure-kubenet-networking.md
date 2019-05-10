---
title: Öğretici - Azure Kubernetes Service (AKS) ansible'ı kullanarak ağ kubernetes yapılandırma | Microsoft Docs
description: Azure Kubernetes Service (AKS) kümesi içinde ağ kubernetes yapılandırmak için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, aks, kapsayıcı, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: cd32347f9de87ea6272be922d0359f1cc7f6f758
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231307"
---
# <a name="tutorial-configure-kubenet-networking-in-azure-kubernetes-service-aks-using-ansible"></a>Öğretici: Kubernetes Azure Kubernetes Service (AKS) ansible'ı kullanarak ağ yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS kullanarak, aşağıdaki ağ modelleri kullanarak bir kümeyi dağıtabilirsiniz:

- [Kubernetes ağ](/azure/aks/configure-kubenet) -ağ kaynaklarını genellikle oluşturulur ve AKS kümesi dağıtılır olarak yapılandırılmış.
- [Azure kapsayıcı ağ arabirimi (CNI) ağ](/azure/aks/configure-azure-cni) -AKS kümesini mevcut sanal ağ kaynakları ve yapılandırmaların bağlı.

Uygulamalarınıza aks'deki ağ hakkında daha fazla bilgi için bkz. [kavramları aks'deki uygulamalar için ağ](/azure/aks/concepts-network).

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * AKS kümesi oluşturma
> * Azure kubernetes ağı yapılandırma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Bu bölümde playbook kodda aşağıdaki Azure kaynakları oluşturur:

- Sanal ağ
- Sanal ağ içindeki alt ağ

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

Bu bölümdeki playbook kod, bir sanal ağ içindeki bir AKS kümesi oluşturur. 

Aşağıdaki playbook'u `aks.yml` olarak kaydedin:

```yml
- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster with vnet
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
          network_plugin: kubenet
          pod_cidr: 192.168.0.0/16
          docker_bridge_cidr: 172.17.0.1/16
          dns_service_ip: 10.0.0.10
          service_cidr: 10.0.0.0/16
  register: aks
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Kullanım `azure_rm_aks_version` desteklenen sürümü bulmak için modül.
- `vnet_subnet_id` Önceki bölümde oluşturduğunuz alt ağı.
- `network_profile` Kubernetes ağ eklentisi için özellikleri tanımlar.
- `service_cidr` İç Hizmetleri'nde bir AKS kümesi için bir IP adresi atamak için kullanılır. Bu IP adresi aralığı, başka bir yerde ağınızda kullanılmayan adres alanı olmalıdır. 
- `dns_service_ip` Adresi olmalıdır ". 10" hizmeti IP adresi aralığınızı adresidir.
- `pod_cidr` , Ağ ortamınızda başka bir yerde kullanımda olmayan bir geniş adres alanı olmalıdır. Adres aralığı'nın ölçeği beklediğiniz düğüm sayısını tutabilecek kadar büyük olmalıdır. Küme dağıtıldıktan sonra bu adres aralığını değiştiremezsiniz.
- Pod IP adres aralığı bir/24 atamak için kullanılan adres alanı her küme düğümünde için. Aşağıdaki örnekte, `pod_cidr` 192.168.0.0/16 ilk düğümü 192.168.0.0/24, İkinci düğüm 192.168.1.0/24 ve üçüncü düğüm 192.168.2.0/24 atar.
- Küme ölçek veya yükseltme olarak Azure pod IP adresi aralığı her yeni düğüme atamak devam eder.
- Playbook'u yükler `ssh_key` gelen `~/.ssh/id_rsa.pub`. Değiştirirseniz, tek satırlı biçimde (tırnak işaretleri olmadan) "ssh-rsa" ile başlayan - kullanın.
- `client_id` Ve `client_secret` gelen yüklenen değerler `~/.azure/credentials`, varsayılan kimlik bilgileri dosyası olduğu. Bu değerleri hizmetinize asıl ayarlayabilir veya bu değerleri ortam değişkenlerinden yükleyin:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

## <a name="associate-the-network-resources"></a>Ağ kaynaklarını ilişkilendirebilirsiniz.

Bir AKS kümesi oluşturduğunuzda, bir ağ güvenlik grubu ve rota tablosu oluşturulur. Bu kaynaklar AKS tarafından yönetilen ve oluştururken ve hizmetlerini güncelleştirildi. Ağ güvenlik grubu ve rota tablosu gibi sanal ağ alt ağı ile ilişkilendirin. 

Aşağıdaki playbook olarak Kaydet `associate.yml`.

```yml
- name: Get route table
  azure_rm_routetable_facts:
      resource_group: "{{ node_resource_group }}"
  register: routetable

- name: Get network security group
  azure_rm_securitygroup_facts:
      resource_group: "{{ node_resource_group }}"
  register: nsg

- name: Parse subnet id
  set_fact:
      subnet_name: "{{ vnet_subnet_id | regex_search(subnet_regex, '\\1') }}"
      subnet_rg: "{{ vnet_subnet_id | regex_search(rg_regex, '\\1') }}"
      subnet_vn: "{{ vnet_subnet_id | regex_search(vn_regex, '\\1') }}"
  vars:
      subnet_regex: '/subnets/(.+)'
      rg_regex: '/resourceGroups/(.+?)/'
      vn_regex: '/virtualNetworks/(.+?)/'

- name: Associate network resources with the node subnet
  azure_rm_subnet:
      name: "{{ subnet_name[0] }}"
      resource_group: "{{  subnet_rg[0] }}"
      virtual_network_name: "{{ subnet_vn[0] }}"
      security_group: "{{ nsg.ansible_facts.azure_securitygroups[0].id }}"
      route_table: "{{ routetable.route_tables[0].id }}"
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- `node_resource_group` AKS düğümleri oluşturulur kaynak grubu adı.
- `vnet_subnet_id` Önceki bölümde oluşturduğunuz alt ağı.


## <a name="run-the-sample-playbook"></a>Örnek playbook çalıştırın

Bu bölümde, bu makaledeki oluşturma görevleri çağıran tam örnek playbook'u listelenir. 

Aşağıdaki playbook'u `aks-kubenet.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exist
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create vnet
       include_tasks: vnet.yml

     - name: Create AKS
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
       include_tasks: aks.yml

     - name: Associate network resources with the node subnet
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
           node_resource_group: "{{ aks.node_resource_group }}"
       include_tasks: associate.yml

     - name: Get details of the AKS
       azure_rm_aks_facts:
           name: "{{ name }}"
           resource_group: "{{ resource_group }}"
           show_kubeconfig: user
       register: output

     - name: Show AKS cluster detail
       debug:
           var: output.aks[0]
```

İçinde `vars` bölümünde, aşağıdaki değişiklikleri yapın:

- İçin `resource_group` anahtar, değişikliği `aksansibletest` kaynak grubunuzun adı değeri.
- İçin `name` anahtar, değişikliği `aksansibletest` AKS adınızla değeri.
- İçin `Location` anahtar, değişikliği `eastus` değeri, kaynak grubu konumu.

Kullanarak tam playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook aks-kubenet.yml
```

Playbook'u çalıştırdıktan sonuç aşağıdaki çıktıya benzer gösterir:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Ensure resource group exist] 
ok: [localhost]

TASK [Create vnet] 
included: /home/devops/aks-kubenet/vnet.yml for localhost

TASK [Create vnet] 
ok: [localhost]

TASK [Create subnet] 
ok: [localhost]

TASK [Create AKS] 
included: /home/devops/aks-kubenet/aks.yml for localhost

TASK [List supported kubernetes version from Azure] 
 [WARNING]: Azure API profile latest does not define an entry for
ContainerServiceClient

ok: [localhost]

TASK [Create AKS cluster with vnet] 
changed: [localhost]

TASK [Associate network resources with the node subnet] 
included: /home/devops/aks-kubenet/associate.yml for localhost

TASK [Get route table] 
ok: [localhost]

TASK [Get network security group] 
ok: [localhost]

TASK [Parse subnet id] 
ok: [localhost]

TASK [Associate network resources with the node subnet] 
changed: [localhost]

TASK [Get details of the AKS] 
ok: [localhost]

TASK [Show AKS cluster detail] 
ok: [localhost] => {
    "output.aks[0]": {
        "id": /subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourcegroups/aksansibletest/providers/Microsoft.ContainerService/managedClusters/aksansibletest",
        "kube_config": "apiVersion: ...",
        "location": "eastus",
        "name": "aksansibletest",
        "properties": {
            "agentPoolProfiles": [
                {
                    "count": 3,
                    "maxPods": 110,
                    "name": "nodepool1",
                    "osDiskSizeGB": 100,
                    "osType": "Linux",
                    "storageProfile": "ManagedDisks",
                    "vmSize": "Standard_D2_v2",
                    "vnetSubnetID": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourceGroups/aksansibletest/providers/Microsoft.Network/virtualNetworks/aksansibletest/subnets/aksansibletest"
                }
            ],
            "dnsPrefix": "aksansibletest",
            "enableRBAC": false,
            "fqdn": "aksansibletest-cda2b56c.hcp.eastus.azmk8s.io",
            "kubernetesVersion": "1.12.6",
            "linuxProfile": {
                "adminUsername": "azureuser",
                "ssh": {
                    "publicKeys": [
                        {
                            "keyData": "ssh-rsa ..."
                        }
                    ]
                }
            },
            "networkProfile": {
                "dnsServiceIP": "10.0.0.10",
                "dockerBridgeCidr": "172.17.0.1/16",
                "networkPlugin": "kubenet",
                "podCidr": "192.168.0.0/16",
                "serviceCidr": "10.0.0.0/16"
            },
            "nodeResourceGroup": "MC_aksansibletest_pcaksansibletest_eastus",
            "provisioningState": "Succeeded",
            "servicePrincipalProfile": {
                "clientId": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"
            }
        },
        "type": "Microsoft.ContainerService/ManagedClusters"
    }
}

PLAY RECAP 
localhost                  : ok=15   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
```

İçinde `vars` bölümünde, değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici - AKS ansible'ı kullanarak Azure kapsayıcı ağ arabirimi (CNI) ağı yapılandırma](./ansible-aks-configure-cni-networking.md)