---
title: Öğretici - otomatik ölçeklendirme sanal makine ölçek kümeleri Azure'da Ansible kullanarak | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri ile otomatik ölçeklendirme ölçeğini Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, playbook, Ölçek, otomatik ölçeklendirme, sanal makine, sanal makine ölçek kümesi, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 4f2cd66b7460fc6fe48cb55f45bf4bc309ae054c
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231272"
---
# <a name="tutorial-autoscale-virtual-machine-scale-sets-in-azure-using-ansible"></a>Öğretici: Otomatik ölçeklendirme sanal makine ölçek kümeleri Azure'da ansible'ı kullanma

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

Sanal makine örneği sayısını otomatik olarak ayarlama özelliği adlı [otomatik ölçeklendirme](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview). Otomatik ölçeklendirme, izlemek ve uygulamanızın performansını en iyi duruma getirmek için yönetim yükünü azaltır avantajdır. Otomatik ölçeklendirme, yanıt, tanımlanmış bir zamanlamaya göre veya isteğe bağlı şekilde yapılandırılabilir. Ansible'ı kullanarak, pozitif bir müşteri deneyimi için kabul edilebilir performans tanımlayan otomatik ölçeklendirme kuralları belirtebilirsiniz.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Otomatik ölçeklendirme profilini tanımlama
> * Yinelenen bir zamanlamaya göre otomatik ölçeklendirme
> * Uygulama performansına dayalı otomatik ölçeklendirme
> * Otomatik ölçeklendirme ayarları bilgilerini alma 
> * Bir otomatik ölçeklendirme ayarı devre dışı bırak

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 
[!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]

## <a name="autoscale-based-on-a-schedule"></a>Bir zamanlamaya göre otomatik ölçeklendirme

Bir ölçek kümesinde otomatik ölçeklendirmeyi etkinleştirmek için ilk olarak bir otomatik ölçeklendirme profili tanımlamanız gerekir. Bu profil varsayılan, en düşük ve en yüksek ölçek kümesi kapasitesini tanımlar. Bu sınırlar değil, sürekli olarak VM örnekleri oluşturarak maliyet denetlemek ve en az bir ölçeğini olayında kalan örnek sayısı kabul edilebilir performans Bakiye belirlemenizi sağlar. 

Ansible, belirli bir tarih veya yinelenen bir zamanlama, Ölçek kümelerinde ölçeklendirmenize olanak tanır.

Bu bölümdeki playbook kod üç saat 10:00 her Pazartesi VM örneği sayısını artırır.

Aşağıdaki playbook'u `vmss-auto-scale.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks: 
    - name: Create autoscaling
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

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="autoscale-based-on-performance-data"></a>Performans verilerini temel alarak otomatik ölçeklendirme

Uygulamanızın talebi artarsa, Ölçek kümenizdeki sanal makine örneklerine üzerindeki yük arttıkça ayarlar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. Ansible, hangi ölçümlerin izleneceğini, CPU kullanımı, disk kullanımı ve uygulama yükleme süresi gibi denetlemenize olanak tanır. Ölçeği daraltma ve ölçek genişletme göre performans ölçüm eşikleri, yinelenen bir zamanlamaya göre veya belirli bir tarihe göre ayarlar. 

Bu bölümdeki playbook kod, önceki 10 dakika 18:00 her Pazartesi CPU yükünü denetler. 

Playbook'u CPU yüzdesi ölçümlere göre aşağıdaki eylemlerden birini gerçekleştirir:

- Dört VM örneklerinin sayısını ölçeklendirir
- Bir sanal makine örneği sayısını ölçeklendirir

Aşağıdaki playbook'u `vmss-auto-scale-metrics.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get scale set resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create autoscaling
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

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-autoscale-settings-information"></a>Otomatik ölçeklendirme ayarları bilgilerini alma 

Bu bölümdeki playbook kod `azure_rm_autoscale_facts` otomatik ölçeklendirme ayarı ayrıntılarını alma modülü.

Aşağıdaki playbook'u `vmss-auto-scale-get-settings.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve autoscale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook vmss-auto-scale-get-settings.yml
```

## <a name="disable-autoscale-settings"></a>Otomatik ölçeklendirme ayarlarını devre dışı bırak

Otomatik ölçeklendirme ayarları devre dışı bırakmak için iki yolu vardır. Tek yönlü değiştirmektir `enabled` anahtar `true` için `false`. İkinci yol ayarı silmektir.

Bu bölümdeki playbook kod otomatik ölçeklendirme ayarının siler. 

Aşağıdaki playbook'u `vmss-auto-scale-delete-setting.yml` olarak kaydedin:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete autoscaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
vmss-auto-scale-delete-setting.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Öğretici: Ansible'ı kullanarak Azure sanal makine ölçek özel görüntüsünü güncelleştirme ayarlar](./ansible-vmss-update-image.md)