---
title: Öğretici - ansible'ı kullanarak Azure App Service'te ölçeklendirme uygulamaları | Microsoft Docs
description: Azure uygulama Hizmeti'nde bir uygulamanın ölçeğini öğrenin
keywords: ansible'ı, azure, devops, bash, playbook, Azure App Service, Web uygulaması, Ölçek, Java
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 213c4e086db8b40fdec26ce9fb3e0be5ad055cbc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729437"
---
# <a name="tutorial-scale-apps-in-azure-app-service-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure App Service'te ölçeklendirme uygulamaları

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Mevcut bir App Service planının gerçekleri öğrenin
> * S2 ile üç çalışanları App Service planına ölçeğini

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service uygulaması** - bir Azure App Service uygulaması yoksa [ansible'ı kullanarak Azure App Service içinde bir uygulamayı yapılandırma](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app"></a>Uygulama ölçeklendirme

Ölçeklendirme için iki iş akışı: *ölçeği* ve *ölçeğini*.

**Ölçeği artırma:** Ölçeği artırma işleminin daha fazla kaynak almaya anlamına gelir. Bu kaynaklar CPU, bellek, disk alanı, Vm'leri ve daha fazlasını içerir. Uygulama, uygulamanın ait olduğu App Service planının fiyatlandırma katmanını değiştirerek ölçeği. 
**Ölçeği genişletme:** Ölçeği genişletmek için uygulamanızı çalıştıran VM örneği sayısını artırmak anlamına gelir. Fiyatlandırma katmanı App Service planınıza bağlı olarak en çok 20 örneklerine ölçeği genişletebilirsiniz. [Otomatik ölçeklendirme](/azure/azure-monitor/platform/autoscale-get-started) örnek sayısı otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre ölçeklendirmenize olanak tanıyor.

Bu bölümdeki playbook kod, işlemi tanımlar:

* Mevcut bir App Service planının gerçekleri öğrenin
* App service planı S2 üç çalışanları ile güncelleştirin.

Aşağıdaki playbook'u `webapp_scaleup.yml` olarak kaydedin:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
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

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook webapp_scaleup.yml
```

Playbook'u çalıştırdıktan sonra aşağıdaki sonuçları benzer bir çıktı görürsünüz:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get facts of existing App service plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] 
changed: [localhost]

TASK [Get facts] 
ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP 
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)