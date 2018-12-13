---
title: Ansible'ı kullanarak Azure Application Gateway ile Web trafiğini yönetin
description: Web trafiğini yönetmek üzere bir Azure Application Gateway oluşturup yapılandırmak için Ansible kullanmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, uygulama ağ geçidi, yük dengeleyici, web trafiği
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: af7f22ae5c289a01e6876d8ce586cb32383c8d3b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253383"
---
# <a name="manage-web-traffic-with-azure-application-gateway-by-using-ansible"></a>Ansible'ı kullanarak Azure Application Gateway ile Web trafiğini yönetin

[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/), web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir.

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenize yardımcı olur. Bu makalede Ansible'ı kullanarak uygulama ağ geçidi oluşturma adımları gösterilir. Ayrıca, Azure kapsayıcı örneklerinde çalıştırılan iki web sunucusunun trafiğini yönetmek için ağ geçidi nasıl kullanacağınız da öğretilir.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Ağı ayarlama
> * HHPD görüntüleriyle iki Azure kapsayıcı örneği oluşturma
> * Sunucu havuzundaki Azure kapsayıcı örnekleriyle bir uygulama ağ geçidi oluşturma

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir. 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek **eastus** konumunda **myResourceGroup** adlı bir kaynak grubu oluşturur.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Bu playbook’u *rg.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Uygulama ağ geçidinin diğer kaynaklarla iletişim kurabilmesini sağlamak için önce bir sanal makine oluşturun.

Aşağıdaki örnek, **myVNet** adında bir sanal ağ, **myAGSubnet** adında bir alt ağ ve etki alanı adı **mydomain** olan **myAGPublicIPAddress** adında bir genel IP adresi oluşturur.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Bu playbook’u *vnet_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook vnet_create.yml
```

## <a name="create-servers"></a>Sunucu oluşturma

Aşağıdaki örnekte, uygulama ağ geçidi için web sunucuları olarak kullanılacak, HTTPD görüntüsüne sahip iki Azure kapsayıcı örneğini nasıl oluşturacağınız gösterilir.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Bu playbook’u *aci_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Aşağıdaki örnek arka uç, ön uç ve HTTP yapılandırmaları olan **myAppGateway** adlı bir uygulama ağ geçidi oluşturur.  

* **appGatewayIP**, **gateway_ip_configurations** bloğunda tanımlanır. Ağ geçidinin IP yapılandırması için alt ağ başvurusu gerekir.
* **appGatewayBackendPool**, **backend_address_pools** bloğunda tanımlanır. Bir uygulama ağ geçidi en az bir arka uç adres havuzuna sahip olmalıdır.
* **appGatewayBackendHttpSettings**, **backend_http_settings_collection** bloğunda tanımlanır. İletişim için 80 numaralı bağlantı noktasının ve HTTP protokolünün kullanıldığını belirtir.
* **appGatewayHttpListener**, **backend_http_settings_collection** bloğunda tanımlanır. appGatewayBackendPool ile ilişkilendirilmiş varsayılan dinleyicidir.
* **appGatewayFrontendIP**, **frontend_ip_configurations** bloğunda tanımlanır. appGatewayHttpListener’a myAGPublicIPAddress’i atar.
* **rule1**, **request_routing_rules** bloğunda tanımlanır. appGatewayHttpListener ile ilişkilendirilmiş varsayılan yönlendirme kuralıdır.

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Bu playbook’u *appgw_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook appgw_create.yml
```

Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Ağ kaynaklarına yönelik örnek playbook’ta, **eastus** bölgesinde **mydomain** etki alanını oluşturdunuz. Tarayıcınızda `http://mydomain.eastus.cloudapp.azure.com` sayfasına gidin. Aşağıdaki sayfayı görürseniz, uygulama ağ geçidi beklendiği gibi çalışıyor demektir.

![Çalışan uygulama ağ geçidinin başarılı testi](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara ihtiyacınız yoksa, bunları aşağıdaki kodu çalıştırarak silebilirsiniz. **myResourceGroup** adlı kaynak grubunu siler.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Bu playbook’u *rg_delete*.yml olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)
