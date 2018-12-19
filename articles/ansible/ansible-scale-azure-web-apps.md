---
title: Ansible'ı kullanarak Azure App Service web uygulamaları ölçeklendirme
description: Ansible'ı kullanarak Linux'ta App Service hizmetinde Java 8 ve Tomcat kapsayıcı çalışma zamanı ile web uygulaması oluşturmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, Azure App Service, Web uygulaması, Ölçek, Java
author: tomarcher
manager: jeconnoc
ms.author: kyliel
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 740ff6d6a636377f9d58a5231692c87f935ae6d2
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601874"
---
# <a name="scale-azure-app-service-web-apps-by-using-ansible"></a>Ansible'ı kullanarak Azure App Service web uygulamaları ölçeklendirme
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (veya yalnızca Web Apps) ana web uygulamaları, REST API'leri ve mobil arka uç. .NET, .NET Core, Java, Ruby, Node.js, PHP veya Python dahil en sevdiğiniz dilde geliştirebilirsiniz.

Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Bu makalede, uygulamanızı Azure App Service'te ölçeklendirme için ansible'ı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service Web Apps** -bir Azure app service web uygulaması zaten yoksa, şunları yapabilirsiniz [Ansible'ı kullanarak Azure web uygulamaları oluşturma](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app-in-app-service"></a>App Service'te bir uygulama ölçeğini
Uygulamanızın ait olduğu App Service planının fiyatlandırma katmanını değiştirerek ölçeği artırabilirsiniz. Bu bölüm, işlemi tanımlayan bir örnek Ansible playbook sunar:
- Mevcut bir App Service planının gerçekleri öğrenin
- App service planı S2 üç çalışanları ile güncelleştirin.

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App serivce plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Playbook'u olarak Kaydet *webapp_scaleup.yml*.

Playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:
```bash
ansible-playbook webapp_scaleup.yml
```

Playbook'u çalıştırdıktan sonra App service planı başarıyla S2 üç arkadaşlarınızla güncelleştirildiğini aşağıdaki örneğe benzer bir çıktı gösterilmektedir:
```Output
PLAY [localhost] **************************************************************

TASK [Gathering Facts] ********************************************************
ok: [localhost]

TASK [Get facts of existing App serivce plan] **********************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] ******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] *******************************************
changed: [localhost]

TASK [Get facts] ***************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP **********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)