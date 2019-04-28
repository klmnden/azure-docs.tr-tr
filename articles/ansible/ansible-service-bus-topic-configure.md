---
title: Öğretici - ansible'ı kullanarak Azure Service Bus konularını yapılandırın | Microsoft Docs
description: Bir Azure Service Bus konu oluşturmak için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, playbook, hizmet veri yolu, konular, abonelikler
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: f7c6df05c19683fe739520f622e6bcab8478cbd9
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767141"
---
# <a name="tutorial-configure-topics-in-azure-service-bus-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure Service Bus konularını yapılandırın

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Konu başlığı oluşturma
> * Abonelik oluşturma
> * SAS ilkesi oluşturma
> * Ad alanı bilgileri alınamıyor
> * Konu ve abonelik bilgileri alınamıyor
> * SAS İlkesi iptal et

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-topic"></a>Service Bus konusu oluşturun

Örnek playbook kod, aşağıdaki kaynakları oluşturur:
- Azure kaynak grubu
- Kaynak grubu içinde Service Bus ad alanı
- Ad alanı ile Service Bus konusu

Aşağıdaki playbook'u `servicebus_topic.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: topic
    - debug:
          var: topic
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_topic.yml
```

## <a name="create-the-subscription"></a>Abonelik oluşturma

Service Bus konu aboneliğinde örnek playbook kod oluşturur. Azure Service Bus konuları, birden fazla aboneliğiniz olabilir. Bir konuya abone konu başlığına gönderilen her iletinin bir kopyasını alır. Abonelikleri arızaya oluşturulur, ancak isteğe bağlı olarak süresinin dolmasını sağlayabilir, varlıklar olarak adlandırılır.

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Create a subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs
    - debug:
          var: subs
```

Aşağıdaki playbook'u `servicebus_subscription.yml` olarak kaydedin:

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_subscription.yml
```

## <a name="create-the-sas-policy"></a>SAS ilkesi oluşturma

A [paylaşılan erişim imzası (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) belirteçleri kullanarak bir beyana dayalı yetkilendirme mekanizması. 

Örnek playbook kod farklı ayrıcalıklara sahip bir Service Bus kuyruğu için iki SAS ilkeleri oluşturur.

Aşağıdaki playbook'u `servicebus_topic_policy.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Create a policy with send and listen privilege
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-{{ item }}"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: "{{ item }}"
      with_items:
        - send
        - listen
      register: policy
    - debug:
          var: policy
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_topic_policy.yml
```

## <a name="retrieve-namespace-information"></a>Ad alanı bilgileri alınamıyor

Örnek playbook kod ad alanı bilgilerini sorgular.

Aşağıdaki playbook'u `servicebus_namespace_info.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
  tasks:
    - name: Get a namespace's information
      azure_rm_servicebus_facts:
          type: namespace
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: ns
    - debug:
          var: ns
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:
- `show_sas_policies` Değeri belirtilen ad alanı altında SAS ilkeleri gösterilip gösterilmeyeceğini belirtir. Varsayılan değer olan `False` ek ağ yükünü önlemek için.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_namespace_info.yml
```

## <a name="retrieve-topic-and-subscription-information"></a>Konu ve abonelik bilgileri alınamıyor

Aşağıdaki bilgileri için örnek playbook kod sorgular:
- Service Bus konusu bilgileri
- Konu için Abonelik Ayrıntıları listesi
 
Aşağıdaki playbook'u `servicebus_list.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Get a topic's information
      azure_rm_servicebus_facts:
          type: topic
          name: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: topic_fact
    - name: "List subscriptions under topic {{ topic }}"
      azure_rm_servicebus_facts:
          type: subscription
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: subs_fact
    - debug:
          var: "{{ item }}"
      with_items:
        - topic_fact.servicebuses[0]
        - subs_fact.servicebuses
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:
- `show_sas_policies` Değeri altında belirtilen sıraya SAS ilkeleri gösterilip gösterilmeyeceğini belirtir. Varsayılan olarak, bu değeri ayarlamak `False` ek ağ yükünü önlemek için.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_list.yml
```

## <a name="revoke-the-queue-sas-policy"></a>Kuyruk SAS İlkesi iptal et

Örnek playbook kod bir kuyruk SAS İlkesi siler.

Aşağıdaki playbook'u `servicebus_queue_policy_delete.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
  tasks:
    - name: Delete a policy
      azure_rm_servicebussaspolicy:
          name: "{{ topic }}-policy"
          topic: "{{ topic }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_topic_policy_delete.yml
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      topic: servicebustesttopic
      subscription: servicebustestsubs
  tasks:
    - name: Delete subscription
      azure_rm_servicebustopicsubscription:
          name: "{{ subscription }}"
          topic: "{{ topic }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete topic
      azure_rm_servicebustopic:
          name: "{{ topic }}"
          resource_group: "{{ resource_group }}"
          namespace: "{{ namespace }}"
          state: absent
    - name: Delete namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
    - name: Delete resource group
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          state: absent
          force_delete_nonempty: yes
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](/azure/ansible/)