---
title: Öğretici - ansible'ı kullanarak Azure rota tablolarını yapılandırmak | Microsoft Docs
description: Ansible'ı kullanarak Azure rota tabloları Sil oluşturma ve değiştirme hakkında bilgi edinin
keywords: ansible'ı, azure, devops, bash, playbook, ağ, yol, yol tablosu
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 846ff510603c0ed0888ec92ece8b86fad0354c19
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230876"
---
# <a name="tutorial-configure-azure-route-tables-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure rota tablolarını yapılandırmak

[!INCLUDE [ansible-27-note.md](../../includes/ansible-28-note.md)]

Azure otomatik olarak Azure alt ağlar, sanal ağlar arasındaki trafiği yönlendirir ve şirket içi ağlara. Ortamınızın yönlendirme hakkında daha fazla denetime ihtiyacınız varsa, oluşturabileceğiniz bir [yol tablosu](/azure/virtual-network/virtual-networks-udr-overview). 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> Bir yol tablosu bir yol tablosu silme sorgusu Oluştur bir yol tablosu oluşturma bir sanal ağ ve alt ilişkisini oluşturma bir alt ağdan bir yol tablosu bir alt ağ ile bir yönlendirme tablosunu ilişkilendirme ve silme yönlendirir

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bu bölümdeki playbook kod, bir rota tablosu oluşturur. Route-table sınırları hakkında daha fazla bilgi için bkz: [Azure sınırları](/azure/azure-subscription-service-limits#azure-resource-manager-virtual-networking-limits). 

Aşağıdaki playbook'u `route_table_create.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bu bölümdeki playbook kodu:

* Bir sanal ağ oluşturur
* Sanal ağ içindeki bir alt ağ oluşturur
* Bir alt ağın yol tablosuna ilişkilendirir

Rota tabloları sanal ağlara ilişkili değildir. Bunun yerine, rota tabloları sanal ağ alt ağı ile ilişkili değildir.

Sanal ağ ve rota tablosunu aynı Azure konumunda ve aboneliğinde içinde bir arada gerekir.

Alt ağlar ve rota tablolarını bire çok ilişkisi vardır. Bir alt ağ, herhangi bir ilişkili rota tablosu veya bir yol tablosu ile tanımlanabilir. Rota tabloları, none, bir veya birçok alt ağlar ile ilişkilendirilebilir. 

Alt ağından gelen trafiği göre yönlendirilir:

- Rota tabloları'içinde tanımlanan yollar
- [Varsayılan yollar](/azure/virtual-network/virtual-networks-udr-overview#default)
- bir şirket içi ağ üzerinden yayılan yolları

Sanal ağ için bir Azure sanal ağ geçidi bağlı olması gerekir. ExpressRoute veya VPN ağ geçidi BGP ile VPN ağ geçidi kullanıyorsanız olabilir.

Aşağıdaki playbook'u `route_table_associate.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create virtual network
      azure_rm_virtualnetwork:
        name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
        - 10.1.0.0/16
        - 172.100.0.0/16
        dns_servers:
        - 127.0.0.1
        - 127.0.0.3
    - name: Create a subnet with route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
        route_table: "{ route_table_name }"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir yol tablosu bir alt ağdan ilişkilendirmesini Kaldır

Bu bölümdeki playbook kod bir yol tablosu bir alt ağdan dissociates.

Bir yol tablosu bir alt ağdan kaldırdıktan sonra ayarlanmış `route_table` için alt ağa `None`. 

Aşağıdaki playbook'u `route_table_dissociate.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    resource_group: myResourceGroup
  tasks:
    - name: Dissociate a route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

Bu bölümde bir rota tablosu içindeki rota playbook kodu. 

Aşağıdaki playbook'u `route_create.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        next_hop_type: virtual_network_gateway
        address_prefix: "10.1.0.0/16"
        route_table_name: "{{ route_table_name }}"
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:

* `virtual_network_gateway` olarak tanımlanan `next_hop_type`. Azure yolların nasıl seçtiği hakkında daha fazla bilgi için bkz. [yönlendirmeye genel bakış](/azure/virtual-network/virtual-networks-udr-overview).
* `address_prefix` olarak tanımlanan `10.1.0.0/16`. Rota tablosu içindeki önek yinelenemez.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Bir rota Sil

Bu bölümdeki playbook kod bir rota tablosundan siler.

Aşağıdaki playbook'u `route_delete.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Remove route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        route_table_name: "{{ route_table_name }}"
        state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-route-table-information"></a>Rota tablosu bilgilerini alma

Bu bölümdeki playbook kod Ansible modülü kullanır `azure_rm_routetable_facts` rota tablosu bilgileri alınamıyor.

Aşağıdaki playbook'u `route_table_facts.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks: 
    - name: Get route table information
      azure_rm_routetable_facts:
         resource_group: "{{ resource_group }}"
         name: "{{ route_table_name }}"
      register: query
    
    - debug:
         var: query.route_tables[0]
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Rota tablosunu sil

Bu bölümde bir yol tablosu playbook kodu.

Bir yol tablosu silindiğinde, yolların da silinir.

Yönlendirme tablosunu bir alt ağ ile ilişkili değilse silinemiyor. [Rota tablosunda hiçbir alt ağ ilişkilendirmesi](#dissociate-a-route-table-from-a-subnet) önce yol tablosu silinmeye çalışılıyor. 

Aşağıdaki playbook'u `route_table_delete.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)