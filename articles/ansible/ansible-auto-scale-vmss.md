---
title: Sanal makine ölçek kümesi ansible'ı kullanarak Azure'da otomatik ölçeklendirme
description: Sanal makine ölçek kümesini Azure otomatik ölçeklendirme ile ölçeklendirme için Ansible'ı kullanmayı öğrenin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, playbook, Ölçek, otomatik ölçeklendirme, sanal makine, sanal makine ölçek kümesi, vmss
author: tomarcher
manager: jeconnoc
ms.author: yuwzho, kyliel
ms.topic: tutorial
ms.date: 12/10/2018
ms.openlocfilehash: c6678d6df3a695d3a0471e5779bc3af4b6ba6c84
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410924"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-azure-using-ansible"></a>Sanal makine ölçek kümesi ansible'ı kullanarak Azure'da otomatik ölçeklendirme
Ansible, ortamınızdaki kaynakların dağıtımını ve yapılandırılmasını otomatikleştirmenizi sağlar. Azure'da sanal makine ölçek kümenizi (VMSS) yönetmek için, tıpkı diğer Azure kaynaklarını yönettiğiniz gibi Ansible kullanabilirsiniz. 

Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz sanal makine örneği sayısını tanımlarsınız. Uygulamanızın talebi değiştikçe, sanal makine örneklerinin sayısını otomatik olarak artırabilir veya azaltabilirsiniz. Otomatik ölçeklendirme özelliği, uygulamanızın yaşam döngüsü boyunca uygulama performansındaki değişikliklere veya müşteri taleplerine ayak uydurmanıza olanak tanır. Bu makalede, bir otomatik ölçeklendirme ayarı oluşturma ve mevcut bir sanal makine ölçek kümesine ilişkilendirin. Otomatik ölçeklendirme ayarında, ölçeği genişletme ya da istediğiniz kadar ölçeklendirme için bir kural yapılandırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- Mevcut bir Azure sanal makine ölçek kümesi. -Bir tane yoksa, [Oluştur sanal makine ölçek kümeleri Azure'da Ansible kullanarak](https://docs.microsoft.com/azure/ansible/ansible-create-configure-vmss).

> [!Note]
> Bu öğreticideki örnek playbook'ları çalıştırmak için Ansible 2.7 gerekir. 

## <a name="auto-scale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme   
Bir ölçek kümesinde otomatik ölçeklendirmeyi etkinleştirmek için ilk olarak bir otomatik ölçeklendirme profili tanımlamanız gerekir. Bu profil varsayılan, en düşük ve en yüksek ölçek kümesi kapasitesini tanımlar. Bu limitler, sürekli VM örnekleri oluşturmayarak maliyeti kontrol etmenize ve bir ölçeklendirme olayında kalan en düşük örnek sayısı ile kabul edilebilir performansı dengelemenize olanak tanır. 

Ölçeklendirme ve ölçek dışı sanal makine ölçek kümeleri, yinelenen bir zamanlamaya göre veya belirli bir tarihe göre. Bu bölümde, Ölçek kümelerinizdeki 10:00 her Pazartesi, Pasifik saati dilimindeki üç sanal makine örneği sayısını artıran bir otomatik ölçeklendirme ayarı oluşturan bir örnek Ansible playbook sunar. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks: 
    - name: Create auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         target:
           namespace: "Microsoft.Compute"
           types: "virtualMachineScaleSets"
           name: "{{ vmss_name }}"
         enabled: true
         profiles:
         - count: '3'
           min_count: '3'
           max_count: '3'
           name: Auto created scale condition
           recurrence_timezone: Pacific Standard Time
           recurrence_frequency: Week
           recurrence_days:
              - Monday
           recurrence_mins:
              - '0'
           recurrence_hours:
              - '10'
```

Playbook'u olarak Kaydet *vmss otomatik scale.yml*. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="auto-scale-based-on-performance-data"></a>Performans verilerini temel alarak otomatik ölçeklendirme
Uygulamanızın talebi artarsa, Ölçek kümenizdeki sanal makine örneklerine üzerindeki yük arttıkça ayarlar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya disk gibi hangi ölçümlerin izleneceğini, uygulama yükünün belirli bir eşiği ne kadar süre karşılaması gerektiği ve ölçek kümesine kaç sanal makine örneği ekleneceğini denetlersiniz.

Ölçeklendirme ve ölçek sanal makine ölçek kümeleri, yinelenen bir zamanlamaya göre veya belirli bir tarihe göre performans ölçüm eşiklere dayanarak çıkar. Bu bölüm her Pazartesi, Pasifik Saat dilimi 18:00 üzerinde son 10 dakika içinde iş yükü denetler ve ölçek kümeleriniz için dört VM örneği sayısını Ölçeklendirmesi eşitlenene ya da bir örneğine göre CPU yüzdesi olarak ölçeklenen bir Ansible playbook sunar. Ölçümleri. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get VMSS resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create auto scaling
    azure_rm_autoscale:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      target: "{{ vmss_id }}"
      enabled: true
      profiles:
      - count: '1'
        max_count: '1'
        min_count: '1'
        name: 'This scale condition is executed when none of the other scale condition(s) match'
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
      - count: '1'
        min_count: '1'
        max_count: '4'
        name: Auto created scale condition
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
        rules:
          - cooldown: 5
            direction: Increase
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: GreaterThan
            statistic: Average
            threshold: 70
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
          - cooldown: 5
            direction: Decrease
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: LessThan
            statistic: Average
            threshold: 30
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
```

Playbook'u olarak Kaydet *otomatik ölçek metrics.yml vmss*. Ansible playbook'u çalıştırmak için **ansible-playbook** komutunu aşağıdaki gibi kullanın:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-information-for-existing-autoscale-settings"></a>Mevcut otomatik ölçeklendirme ayarları hakkında bilgi alın
Aracılığıyla bir otomatik ölçeklendirme ayarının ayrıntısı alabilirsiniz *azure_rm_autoscale_facts* modülü ile aşağıdaki gibi playbook:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve auto scale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

## <a name="disable-the-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını devre dışı bırak
Değiştirerek otomatik ölçeklendirme ayarı devre dışı bırakabilirsiniz `enabled: true` için `enabled: false`, veya playbook'u ile otomatik ölçeklendirme ayarları gibi siliniyor:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Ansible'ı örnek playbook sanal makine ölçek için ayarlar](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)