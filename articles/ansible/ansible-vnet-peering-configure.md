---
title: Öğretici - ansible'ı kullanarak Azure sanal ağ eşlemesi yapılandırma | Microsoft Docs
description: Sanal Ağ eşlemesi ile sanal ağları bağlamak için Ansible'ı kullanmayı öğrenin.
keywords: ansible'ı, azure, devops, bash, ağ, eşleme playbook
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: f51e7c857a22a362a3d295fbe087c54b25f85780
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230761"
---
# <a name="tutorial-configure-azure-virtual-network-peering-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure sanal ağ eşlemesi yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[Sanal ağ (VNet) eşlemesi](/azure/virtual-network/virtual-network-peering-overview) sorunsuz bir şekilde iki Azure sanal ağları bağlamanıza olanak sağlar. Eşlendikten sonra iki sanal ağ, bağlantı amacıyla tek olarak görünür. 

Özel IP adresleri üzerinden aynı sanal ağdaki sanal makineler arasındaki trafik yönlendirilir. Benzer şekilde, eşlenen sanal ağdaki sanal makineler arasındaki trafik Microsoft omurga altyapısı aracılığıyla yönlendirilir. Sonuç olarak, farklı sanal ağlarda bulunan VM'ler birbiriyle iletişim kurabilir.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * İki sanal ağ oluşturma
> * İki sanal ağı eşleme
> * İki ağ arasında eşleme Sil

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-two-resource-groups"></a>İki kaynak grubu oluşturun

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Bu bölümdeki örnek playbook kodu için kullanılır:

- İki kaynak grubu oluşturun 

```yml
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
```

## <a name="create-the-first-virtual-network"></a>İlk sanal ağ oluşturma

Bu bölümdeki örnek playbook kodu için kullanılır:

- Sanal ağ oluştur
- Sanal ağ içindeki alt ağ oluşturma

```yml
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
```

## <a name="create-the-second-virtual-network"></a>İkinci sanal ağ oluşturma

Bu bölümdeki örnek playbook kodu için kullanılır:

- Sanal ağ oluştur
- Sanal ağ içindeki alt ağ oluşturma

```yml
  - name: Ceate second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
```

## <a name="peer-the-two-virtual-networks"></a>İki sanal ağı eşleme

Bu bölümdeki örnek playbook kodu için kullanılır:

- Sanal Ağ eşlemesi Başlat
- İki sanal ağları daha önce oluşturduğunuz eş

```yml
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true
```

## <a name="delete-the-virtual-network-peering"></a>Sanal ağ eşlemesini Sil

Bu bölümdeki örnek playbook kodu için kullanılır:

- Daha önce oluşturulan sanal ağlarının iki tür arasında eşleme Sil

```yml
  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="get-the-sample-playbook"></a>Örnek playbook Al

Tam örnek playbook almanın iki yolu vardır:

- [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vnet_peering.yml) ve kaydetmesi `vnet_peering.yml`.
- Adlı yeni bir dosya oluşturun `vnet_peering.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 1000 | random }}"
      run_once: yes

- name: Connect virtual networks with virtual network peering
  hosts: localhost
  connection: local
  vars:
    resource_group: "{{ resource_group_name }}"
    resource_group_secondary: "{{ resource_group_name }}2"
    vnet_name1: "myVnet{{ rpfx }}"
    vnet_name2: "myVnet{{ rpfx }}2"
    peering_name: peer1
    location: eastus2
  tasks:
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
  - name: Ceate second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="run-the-sample-playbook"></a>Örnek playbook çalıştırın

Bu bölümdeki örnek playbook kodu, Bu öğretici boyunca gösterilen çeşitli özelliklerini test etmek için kullanılır.

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- İçinde `vars` bölümünde, değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.

Ansible playbook komutunu kullanarak playbook çalıştırın:

```bash
ansible-playbook vnet_peering.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Prepare random postfix] 
ok: [localhost]

PLAY [Connect virtual networks with virtual network peering] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create secondary resource group] 
changed: [localhost]

TASK [Create first virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Ceate second virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Initial vnet peering] 
changed: [localhost]

TASK [Connect vnet peering] 
changed: [localhost]

TASK [Delete vnet peering] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=12   changed=9    unreachable=0    failed=0    skipped=0   rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Bu bölümdeki örnek playbook kodu için kullanılır:

- İki kaynak gruplarına daha önce oluşturduğunuz Sil

Aşağıdaki playbook'u `cleanup.yml` olarak kaydedin:

```bash
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name-1 }}"
    resource_group_secondary: "{{ resource_group_name-2 }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent

    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group_secondary }}"
        force_delete_nonempty: yes
        state: absent
```

Örnek playbook'u ile çalışırken dikkate alınması gereken bazı önemli notlar şunlardır:

- Değiştirin `{{ resource_group_name-1 }}` yer tutucu adı ilk kaynak grubu oluşturulur.
- Değiştirin `{{ resource_group_name-2 }}` yer tutucusunu ikinci kaynak grubunun adıyla oluşturulur.
- İki belirtilen kaynak grubu içindeki tüm kaynaklar silinir.

Ansible playbook komutunu kullanarak playbook çalıştırın:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)