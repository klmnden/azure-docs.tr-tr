---
title: Öğretici - ansible'ı kullanarak Azure Service Bus kuyrukları yapılandırma | Microsoft Docs
description: Bir Azure Service Bus kuyruğuna oluşturmak için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, playbook, hizmet veri yolu, kuyruk
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: a73519e2a51e0f8847e262e315f3c13057eb2ac9
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767176"
---
# <a name="tutorial-configure-queues-in-azure-service-bus-using-ansible"></a>Öğretici: Ansible'ı kullanarak Azure Service Bus kuyrukları yapılandırın

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-servicebus.md](../../includes/open-source-devops-intro-servicebus.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir kuyruk oluşturma
> * Bir SAS plicy oluşturma
> * Ad alanı bilgileri alınamıyor
> * Kuyruk bilgileri alınamıyor
> * Kuyruk SAS İlkesi iptal et

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-service-bus-queue"></a>Service Bus kuyruğu oluşturma

Örnek playbook kod, aşağıdaki kaynakları oluşturur:
- Azure kaynak grubu
- Kaynak grubu içinde Service Bus ad alanı
- Ad alanı ile Service Bus kuyruğu

Aşağıdaki playbook'u `servicebus_queue.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      location: eastus
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Ensure resource group exist
      azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
    - name: Create a namespace
      azure_rm_servicebus:
          name: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
    - name: Create a queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
      register: queue
    - debug:
          var: queue
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_queue.yml
```

## <a name="create-the-sas-policy"></a>SAS ilkesi oluşturma

A [paylaşılan erişim imzası (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) belirteçleri kullanarak bir beyana dayalı yetkilendirme mekanizması. 

Örnek playbook kod farklı ayrıcalıklara sahip bir Service Bus kuyruğu için iki SAS ilkeleri oluşturur.

Aşağıdaki playbook'u `servicebus_queue_policy.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen priviledge
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          rights: listen_send
      register: policy
    - debug:
          var: policy
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:
- `rights` Değeri, bir kullanıcının kuyruğuyla ayrıcalık temsil eder. Aşağıdaki değerlerden birini belirtin: `manage`, `listen`, `send`, veya `listen_send`.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_queue_policy.yml
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

## <a name="retrieve-queue-information"></a>Kuyruk bilgileri alınamıyor

Örnek playbook kod sorgular bilgi kuyruğa alın. 

Aşağıdaki playbook'u `servicebus_queue_info.yml` olarak kaydedin:

```yml
---
- hosts: localhost
  vars:
      resource_group: servicebustest
      namespace: servicebustestns
      queue: servicebustestqueue
  tasks:
    - name: Get a queue's information
      azure_rm_servicebus_facts:
          type: queue
          name: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          show_sas_policies: yes
      register: queue
    - debug:
          var: queue
```

Playbook'u çalıştırmadan önce aşağıdaki alan notlara bakın:
- `show_sas_policies` Değeri altında belirtilen sıraya SAS ilkeleri gösterilip gösterilmeyeceğini belirtir. Varsayılan olarak, bu değeri ayarlamak `False` ek ağ yükünü önlemek için.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_queue_info.yml
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
      queue: servicebustestqueue
  tasks:
    - name: Create a policy with send and listen priviledge
      azure_rm_servicebussaspolicy:
          name: "{{ queue }}-policy"
          queue: "{{ queue }}"
          namespace: "{{ namespace }}"
          resource_group: "{{ resource_group }}"
          state: absent
```

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook servicebus_queue_policy_delete.yml
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
      queue: servicebustestqueue
  tasks:
    - name: Delete queue
      azure_rm_servicebusqueue:
          name: "{{ queue }}"
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
> [Öğretici: Ansible'ı kullanarak Azure Service Bus içinde bir konuyu Yapılandır](/azure/ansible/ansible-service-bus-create-topics)