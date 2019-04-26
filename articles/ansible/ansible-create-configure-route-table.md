---
title: Oluşturma, değiştirme veya ansible'ı kullanarak bir Azure rota tablosunu sil
description: Oluşturma, değiştirme veya silme ansible'ı kullanarak bir yönlendirme tablosu için Ansible'ı kullanmayı öğrenin
ms.service: azure
keywords: ansible'ı, azure, devops, bash, playbook, ağ, yol, yol tablosu
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/17/2018
ms.openlocfilehash: 025a8182d32a7d0d00a48795c848d356eb1c3d4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60396827"
---
# <a name="create-change-or-delete-an-azure-route-table-using-ansible"></a>Oluşturma, değiştirme veya ansible'ı kullanarak bir Azure rota tablosunu sil
Azure otomatik olarak Azure alt ağlar, sanal ağlar arasındaki trafiği yönlendirir ve şirket içi ağlara. Azure'da varsayılan yönlendirmeyi değiştirmek istiyorsanız, oluşturarak bunu bir [yol tablosu](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede oluşturmak, değiştirmek veya bir Azure rota tablosunu sil ve yönlendirme tablosunu bir alt ağa eklemek gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7, bu öğreticide aşağıdaki örnek playbook'ları çalıştırmak için gereklidir.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma
Bu bölümde, bir rota tablosu oluşturur bir örnek Ansible playbook sunar. Kaç yönlendirme tablolarını Azure konumu ve abonelik oluşturmak için bir sınır yoktur. Ayrıntılar için [Azure limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin. 

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

Playbook'u olarak Kaydet `route_table_create.yml`. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
Bir alt ağ ile ilişkili sıfır veya bir yol tablosu olabilir. Sıfır veya birden çok alt ağa bir yol tablosu ilişkilendirilebilir. Rota tabloları sanal ağlara ilişkili olmadığından bir yol tablosu ile ilişkili yol tablosuna istediğiniz her bir alt ağa ilişkilendirmeniz gerekir. Alt ağdan çıkan tüm trafiği rota tabloları içinde oluşturulmuş rotalar göre yönlendirilir [varsayılan yolları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#default), ve yollar yayılan bir şirket içi ağdan sanal ağa bağlı olması durumunda bir Azure sanal ağ geçidi (için ExpressRoute veya VPN ağ geçidi ile BGP kullanıyorsanız, VPN). Yalnızca bir yol tablosu aynı Azure konumunda ve aboneliğinde yol tablosu olarak mevcut olan sanal ağlardaki alt ağlara ilişkilendirebilirsiniz.

Bu bölümde, bir sanal ağ ve bir alt ağ oluşturur, sonra alt ağa bir yol tablosu ilişkilendirir bir Ansible playbook sunar.

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

Playbook'u olarak Kaydet `route_table_associate.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir yol tablosu bir alt ağdan ilişkilendirmesini Kaldır
Bir yol tablosu bir alt ağdan ilişkilendirmesini Kaldır, Ayarla yeterlidir `route_table` bir alt ağda `None`. Bir örnek ansible playbook aşağıda verilmiştir. 

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

Playbook'u olarak Kaydet `route_table_dissociate.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Yönlendirme oluşturma
Bu bölümde, bir yol altında rota tablosu oluşturur bir örnek Ansible playbook sunar. Tanımladığı `virtual_network_gateway` olarak `next_hop_type` ve `10.1.0.0/16` olarak `address_prefix`. Önek içinde başka bir önek olabilir ancak rota tablosu içindeki birden fazla yol ön eki çoğaltılamaz. Azure, yollar ve tüm sonraki atlama türleri ayrıntılı bir açıklamasını nasıl seçtiği hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

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
Playbook'u olarak Kaydet `route_create.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Bir rota Sil
Bu bölümde, bir rota tablosundan bir rota silen bir örnek Ansible playbook sunar.

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

Playbook'u olarak Kaydet `route_delete.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-information-of-a-route-table"></a>Bir yol tablosu bilgilerini alma
Adlı Ansible modülü aracılığıyla bir route_table ayrıntılarını görüntüleyebilirsiniz `azure_rm_routetable_facts`. Bulguları modülü bağlı tüm yollar ile rota tablosunu bilgilerini döndürür.
Bir örnek ansible playbook aşağıda verilmiştir. 

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

Playbook'u olarak Kaydet `route_table_facts.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Rota tablosunu sil
Hiçbir alt ağ için bir yol tablosu ilişkiliyse, bu komut dosyası silinemiyor. [İlişkisini](#dissociate-a-route-table-from-a-subnet) silmeye çalışmadan önce tüm alt ağların yol tablosundan.

Rota tablosunu birlikte tüm yollar silebilirsiniz. Bir örnek ansible playbook aşağıda verilmiştir. 

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

Playbook'u olarak Kaydet `route_table_delete.yml`. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)
