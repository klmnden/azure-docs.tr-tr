---
title: Ansible (Önizleme) kullanarak Azure Application Gateway ile web trafiğini yönetme
description: Web trafiğini yönetmek üzere bir Azure Application Gateway oluşturup yapılandırmak için Ansible kullanmayı öğrenme
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, azure application gateway, yük dengeleyici, web trafiği
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 02b98cb22d897fc9599f6e44ddc57ef4211b0893
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410866"
---
# <a name="manage-web-traffic-with-azure-application-gateway-using-ansible-preview"></a>Ansible (Önizleme) kullanarak Azure Application Gateway ile web trafiğini yönetme
[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/), web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. 

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, bir Azure Application Gateway oluşturmak için Ansible’ın nasıl kullanılacağı ve Azure kapsayıcı örneklerinde çalışan iki web sunucusunun trafiğini yönetmede nasıl kullanılacağı gösterilmektedir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağı ayarlama
> * Httpd görüntüsüyle iki Azure kapsayıcı örneği oluşturma
> * Arka uç havuzundaki Azure kapsayıcı örneklerinin üzerinde bir application gateway oluşturun


## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir. Ansible 2.7 RC sürümünü çalıştırarak yükleyebilirsiniz`sudo pip install ansible[azure]==2.7.0rc2`. Ansible 2.7, Ekim 2018'de kullanıma sunulacaktır. Bundan sonra, varsayılan sürümü 2.7 olacağı için sürümü burada belirtmeniz gerekmez. 

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

Yukarıdaki playbook’u *rg.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 
Diğer kaynaklarla iletişim kurmak için uygulama ağ geçidine yönelik bir sanal makine oluşturmanız gerekir. 

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

Yukarıdaki playbook’u *vnet_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook vnet_create.yml
```

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma
Bu örnekte, application gateway için arka uç sunucuları olarak kullanılacak, httpd görüntüsüne sahip iki Azure kapsayıcı örneği oluşturacaksınız.  

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

Yukarıdaki playbook’u *aci_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Şimdi bir application gateway oluşturalım. Aşağıdaki örnek; arka uç, ön uç ve http yapılandırmasına sahip, **myAppGateway** adında bir application gateway oluşturur.  

> [!div class="checklist"]
> * **gateway_ip_configurations** bloğunda tanımlanan **appGatewayIP** - Ağ geçidinin ip yapılandırması için alt ağ başvurusu gereklidir. 
> * **backend_address_pools** bloğunda tanımlanan **appGatewayBackendPool** - bir application gateway en az bir arka uç adres havuzuna sahip olmalıdır. 
> * **backend_http_settings_collection** bloğunda tanımlanan **appGatewayBackendHttpSettings** - İletişim için 80 numaralı bağlantı noktasının ve bir HTTP protokolünün kullanıldığını tanımlar. 
> * **backend_http_settings_collection** bloğunda tanımlanan **appGatewayHttpListener** - appGatewayBackendPool ile ilişkili varsayılan dinleyici. 
> * **frontend_ip_configurations** bloğunda tanımlanan **appGatewayFrontendIP** - appGatewayHttpListener’a myAGPublicIPAddress’i atar. 
> * **request_routing_rules** bloğunda tanımlanan **rule1** - appGatewayHttpListener ile ilişkili varsayılan yönlendirme kuralı. 

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

Yukarıdaki playbook’u *appgw_create.yml* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook appgw_create.yml
```

Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. 

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Ağ kaynakları için yukarıdaki örnek playbook’ta, **mydomain** adlı etki alanı **eastus**’ta oluşturulmuştur. Artık tarayıcıya gidip, `http://mydomain.eastus.cloudapp.azure.com` yazabilir ve Application Gateway’in beklendiği gibi çalıştığını onaylayan bir sonraki sayfayı görebilirsiniz.

![Application Gateway’e Erişme](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara ihtiyacınız yoksa, aşağıdaki örneği çalıştırarak silebilirsiniz. **myResourceGroup** adlı bir kaynak grubunu siler. 

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

Yukarıdaki playbook’u *rg_delete* olarak kaydedin. Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure Üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)
