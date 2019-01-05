---
title: Ansible'ı kullanarak Azure web uygulamaları oluşturun
description: Ansible'ı kullanarak Linux'ta App Service hizmetinde Java 8 ve Tomcat kapsayıcı çalışma zamanı ile web uygulaması oluşturmayı öğrenin
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web Uygulaması, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 106fe11355d629e57f71b12d1125d188b98c2236
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051354"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible"></a>Ansible'ı kullanarak Azure App Service web uygulamaları oluşturun
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (veya yalnızca Web Apps) web uygulamaları, REST API'leri ve mobil arka uçlar konaklar. .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python dahil en sevdiğiniz dilde geliştirebilirsiniz.

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede Ansible'ı kullanarak Java çalışma zamanıyla bir web uygulaması oluşturma adımları gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir.

## <a name="create-a-simple-app-service"></a>Basit bir App Service örneği oluşturma
Bu bölüm, aşağıdaki kaynakları tanımlayan bir örnek Ansible playbook sunar:
- App Service planınızın ve web uygulamanızın dağıtılacağı kaynak grubu
- Linux'ta App Service hizmetinde Java 8 ve Tomcat kapsayıcı çalışma zamanı ile bir Web uygulaması

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
Önceki playbook'u **firstwebapp.yml** olarak kaydedin.

Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook firstwebapp.yml
```

Ansible playbook'un çalıştırılması sonucu oluşan çıktı web uygulamasının başarıyla oluşturulduğunu gösterir:

```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] *************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

PLAY RECAP *************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Traffic Manager’ı kullanarak uygulama hizmeti oluşturma
[Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager)'ı kullanarak web istemcilerinden gelen isteklerin Azure App Service'teki uygulamalara nasıl dağıtılacağını denetleyebilirsiniz. App Service uç noktaları bir Azure Traffic Manager profiline eklendiğinde, Traffic Manager, App Service uygulamalarınızın durumunu izler. Durumlar arasında çalışıyor, durduruldu ve silindi yer alır. Ardından Traffic Manager bu uç noktalardan hangilerinin trafik alması gerektiğine karar verebilir.

App Service'teki uygulamalar bir [App Service planında](https://docs.microsoft.com/azure/app-service/overview-hosting-plans
) çalışır. App Service planı, bir web uygulamasının birlikte çalıştırılacağı işlem kaynakları kümesini tanımlar. App Service planınızı ve web uygulamanızı farklı gruplarda yönetebilirsiniz.

Bu bölüm, aşağıdaki kaynakları tanımlayan bir örnek Ansible playbook sunar:
- App Service planınızın dağıtılacağı kaynak grubu
- App Service planı
- Web uygulamanızın dağıtılacağı ikincil kaynak grubu
- Linux'ta App Service hizmetinde Java 8 ve Tomcat kapsayıcı çalışma zamanı ile bir Web uygulaması
- Traffic Manager profili
- Oluşturulan web sitesini kullanan Traffic Manager uç noktası

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
Önceki playbook'u **webapp.yml** olarak kaydedin veya [playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook webapp.yml
```

Ansible playbook çıktısı App Service planı, web uygulaması, Traffic Manager profili ve uç noktasının başarıyla oluşturulduğunu gösterir:
```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Ansible'ı kullanarak Azure App Service web uygulamaları ölçeklendirme](https://docs.microsoft.com/azure/ansible/ansible-scale-azure-web-apps)