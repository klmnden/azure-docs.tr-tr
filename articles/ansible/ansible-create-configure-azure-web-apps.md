---
title: Öğretici - ansible'ı kullanarak Azure App Service uygulamalarını yapılandırma | Microsoft Docs
description: Bir uygulamayı Azure App Service'te Java 8 ile Tomcat kapsayıcı çalışma zamanı oluşturup öğrenin
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web Uygulaması, Java
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 357dfd9c840b0235ab9576a6448e2b5a3b89abee
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727212"
---
# <a name="tutorial-configure-apps-in-azure-app-service-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure App Service uygulamalarını yapılandırma

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir uygulamayı Azure App Service'te Java 8 ile Tomcat kapsayıcı çalışma zamanı oluşturup
> * Bir Azure Traffic Manager profili oluşturma
> * Oluşturulan uygulamayı kullanarak bir Traffic Manager uç noktası tanımlama

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-basic-app-service"></a>Temel uygulama hizmetini oluştur

Bu bölümdeki playbook kod, aşağıdaki kaynakları tanımlar:

* Azure kaynak grubu içinde dağıtılan App Service planı ve uygulama
* Java 8 ve Tomcat kapsayıcı çalışma zamanı ile Linux'ta App service

Aşağıdaki playbook'u `firstwebapp.yml` olarak kaydedin:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    webapp_name: myfirstWebApp
    plan_name: myAppServicePlan
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook firstwebapp.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="create-an-app-and-use-azure-traffic-manager"></a>Bir uygulama oluşturun ve Azure Traffic Manager'ı kullanın

[Azure Traffic Manager](/azure/app-service/web-sites-traffic-manager) web istemcilerinden gelen istekleri için Azure App Service'te uygulamaları nasıl dağıtıldığını denetlemenize olanak verir. App Service uç noktaları bir Azure Traffic Manager profiline eklendiğinde, Traffic Manager, App Service uygulamalarınızın durumunu izler. Durumlar arasında çalışıyor, durduruldu ve silindi yer alır. Traffic Manager, hangi uç noktaları trafik alması gereken karar vermek için kullanılır.

App Service'teki uygulamalar bir [App Service planında](/azure/app-service/overview-hosting-plans) çalışır. Bir App Service planı, bilgi işlem kaynakları için bir uygulamanın çalışmasını kümesi tanımlar. App Service planınızı ve web uygulamanızı farklı gruplarda yönetebilirsiniz.

Bu bölümdeki playbook kod, aşağıdaki kaynakları tanımlar:

* App Service planı içinde dağıtıldığı azure kaynak grubu
* App Service planı
* Uygulamanın dağıtıldığı azure kaynak grubu
* Java 8 ve Tomcat kapsayıcı çalışma zamanı ile Linux'ta App service
* Traffic Manager profili
* Oluşturulan uygulamayı kullanarak bir traffic Manager uç noktası

Aşağıdaki playbook'u `webapp.yml` olarak kaydedin:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group_webapp: myResourceGroupWebapp
    resource_group: myResourceGroup
    webapp_name: myLinuxWebApp
    plan_name: myAppServicePlan
    location: eastus
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group_webapp }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group_webapp }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ webapp_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group_webapp }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook webapp.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create resource group] 
changed: [localhost]

TASK [Create resource group for app service plan] 
changed: [localhost]

TASK [Create App Service Plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] 
changed: [localhost]

TASK [Get web app facts] 
ok: [localhost]

TASK [Create Traffic Manager Profile] 
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] 
changed: [localhost]

TASK [Get Traffic Manager Profile facts] 
ok: [localhost]

PLAY RECAP 
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: Ansible'ı kullanarak Azure App Service'te ölçeklendirme uygulamaları](/azure/ansible/ansible-scale-azure-web-apps)