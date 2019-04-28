---
title: Öğretici - için ansible'ı kullanarak Redis önbellekleri Azure önbelleğinde önbellek yapılandırma | Microsoft Docs
description: Oluşturma, ölçeklendirme ve Azure Cache, Redis için yeniden başlatma için Ansible'ı kullanmayı öğrenin
keywords: ansible'ı, azure, devops, bash, playbook, önbellek, redis
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 453a54bd8d81b541cdc42cb09163be9e0b6b6ac2
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767183"
---
# <a name="tutorial-configure-caches-in-azure-cache-for-redis-using-ansible"></a>Öğretici: Ansible'ı kullanarak Redis önbellekleri Azure önbelleğinde önbellek yapılandırma

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[Azure önbelleği için Redis](/azure/azure-cache-for-redis/) verilere hızlı erişim sağlayarak hızlı yanıt veren uygulamalar oluşturmanıza olanak tanıyan bir açık kaynak uyumlu hizmetidir. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Bir önbellek oluşturma
> * Önbellek ölçeklendirme
> * Önbellek yeniden başlatma
> * Önbellek için bir güvenlik duvarı kuralı ekleyin
> * Önbelleği Sil

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-cache"></a>Bir önbellek oluşturma

Bir Azure önbelleği için Redis içinde yeni bir kaynak grubu oluşturun.

```yml
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: basic
        size: C1 
```

Uygulamanın bir önbellek sağlanması birkaç dakika sürebilir. Aşağıdaki kod, işlemin tamamlanmasını bekle Ansible bildirir:

```yml
  - name: Wait for Redis provisioning to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60
```

Uzun sağlama işlemi sırasında birkaç "error" iletisi görüntülenir. Bu ileti güvenle yoksayılabilir. Son ileti önemli iletisidir. Aşağıdaki örnekte, son ("Tamam") ileti kadar çok sayıda hata iletisi vardır.

```Output
FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
ok: [localhost]
```

## <a name="scale-the-cache"></a>Önbellek ölçeklendirme

Azure önbelleği için Redis, uygulamanızın ihtiyaçlarına bağlı olarak farklı bir önbellek teklifleri sahiptir. Bu önbellek seçenekleri, seçtiğiniz önbellek boyutunu ve özelliklerini esneklik sağlar. Önbellek oluşturulduktan sonra uygulama gereksinimleriniz değiştikçe, önbellek gerektiği şekilde ölçekleyebilirsiniz. Ölçeklendirme hakkında daha fazla bilgi için bkz. [ölçek Azure Cache, Redis için nasıl](/azure/azure-cache-for-redis/cache-how-to-scale).

Aşağıdaki örnek kod önbelleğine ölçeklendirir **standart**:

```yml
- name: Scale up Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: standard
        size: C1
```

Bu önbellek ölçeklendirme birkaç dakika sürebilir. Aşağıdaki kod, işlemin tamamlanmasını bekle Ansible bildirir:

```yml
  - name: Wait for Redis scaling up to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60
```

Azure önbelleği için Redis sağlamak için görev, şu iletiyi gibi bir çıktı normal benzer:

```Ouput
**FAILED - RETRYING: Get facts (100 retries left)** is normal.
```

## <a name="reboot-the-cache"></a>Önbellek yeniden başlatma

Aşağıdaki kod, önceki bölümde oluşturulan önbelleği yeniden başlatır.

```yml
  - name: Reboot Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      reboot:
        reboot_type: all
```

### <a name="add-firewall-rule"></a>Güvenlik duvarı kuralı ekleyin

Aşağıdaki kod, bir güvenlik duvarı kuralı önbelleğe ekler:

```yml
  - name: Add Firewall rule
    azure_rm_rediscachefirewallrule:
      resource_group: "{{ resource_group }}"
      cache_name: "{{ redis_name }}"
      name: rule1
      start_ip_address: 168.1.1.1
      end_ip_address: 168.1.1.4
```

## <a name="delete-the-cache"></a>Önbelleği Sil

Aşağıdaki kod önbelleğini siler:

```yml
  - name: Delete Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      state: absent
```

## <a name="get-the-sample-playbook"></a>Örnek playbook Al

Tam örnek playbook almanın iki yolu vardır:
- [Playbook'u indirin](https://github.com/Azure-Samples/ansible-playbooks/blob/master/rediscache.yml) ve kaydetmesi `rediscache.yml`.
- Adlı yeni bir dosya oluşturun `rediscache.yml` ve aşağıdaki içeriği dosyaya kopyalayın:

```yml
- name: Manage Azure Cache for Redis
  hosts: localhost
  connection: local
  vars:
    resource_group: "{{ resource_group_name }}"
    redis_name: "redis{{ resource_group_name }}"
    location: eastus2

  roles:
    - azure.azure_preview_modules

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: basic
        size: C1

  - name: Wait for Redis provisioning to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60

  - name: Scale up Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      sku:
        name: standard
        size: C1

  - name: Wait for Redis scaling up to complete
    azure_rm_rediscache_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
    register: facts
    until: "{{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}"
    retries: 100
    delay: 60

  - name: Reboot Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      reboot:
        reboot_type: all

  - name: Add Firewall rule
    azure_rm_rediscachefirewallrule:
      resource_group: "{{ resource_group }}"
      cache_name: "{{ redis_name }}"
      name: rule1
      start_ip_address: 168.1.1.1
      end_ip_address: 168.1.1.4

  - name: Delete Azure Cache for Redis
    azure_rm_rediscache:
      resource_group: "{{ resource_group }}"
      name: "{{ redis_name }}"
      state: absent
```

## <a name="run-the-sample-playbook"></a>Örnek playbook çalıştırın

Bu bölümde, bu makalede gösterilen çeşitli özelliklerini test etmek için playbook çalıştırın.

İçinde `vars` bölümünde, değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook rediscache.yml
```

Çıktı aşağıdaki sonuçları gibi görünür:

```Output
TASK [create resource group] 
Tuesday 12 March 2019  16:21:07 +0800 (0:00:00.054)       0:00:01.503 
ok: [localhost]

TASK [Create Azure Cache for Redis] 
Tuesday 12 March 2019  16:21:09 +0800 (0:00:01.950)       0:00:03.454 
 [WARNING]: Azure API profile latest does not define an entry for RedisManagementClient

changed: [localhost]

TASK [Dump host name] 
Tuesday 12 March 2019  16:21:49 +0800 (0:00:40.125)       0:00:43.580 
ok: [localhost] =>
  output['host_name']: redis0312.redis.cache.windows.net

TASK [Get facts] 
Tuesday 12 March 2019  16:21:49 +0800 (0:00:00.056)       0:00:43.636 
 [WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: {{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}

FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
FAILED - RETRYING: Get facts (90 retries left).
ok: [localhost]

TASK [Scale up Azure Cache for Redis] 
Tuesday 12 March 2019  16:33:20 +0800 (0:11:31.296)       0:12:14.933 
changed: [localhost]

TASK [Get facts] 
Tuesday 12 March 2019  16:33:29 +0800 (0:00:09.164)       0:12:24.097 
 [WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: {{ facts.rediscaches[0]['provisioning_state'] == 'Succeeded' }}

FAILED - RETRYING: Get facts (100 retries left).
FAILED - RETRYING: Get facts (99 retries left).
FAILED - RETRYING: Get facts (98 retries left).
FAILED - RETRYING: Get facts (97 retries left).
FAILED - RETRYING: Get facts (96 retries left).
FAILED - RETRYING: Get facts (95 retries left).
FAILED - RETRYING: Get facts (94 retries left).
FAILED - RETRYING: Get facts (93 retries left).
FAILED - RETRYING: Get facts (92 retries left).
FAILED - RETRYING: Get facts (91 retries left).
ok: [localhost]

TASK [Reboot Azure Cache for Redis] 
Tuesday 12 March 2019  16:43:57 +0800 (0:10:27.740)       0:22:51.838 
ok: [localhost]

TASK [Add Firewall rule] 
Tuesday 12 March 2019  16:44:02 +0800 (0:00:05.432)       0:22:57.271 
changed: [localhost]

TASK [Delete Azure Cache for Redis] 
Tuesday 12 March 2019  16:44:08 +0800 (0:00:05.137)       0:23:02.409 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=10   changed=4    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

Tuesday 12 March 2019  16:44:14 +0800 (0:00:06.217)       0:23:08.626 

```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturduğunuz kaynakları silin. 

Aşağıdaki kod olarak Kaydet `cleanup.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

İçinde `vars` bölümünde, değiştirin `{{ resource_group_name }}` yer tutucusu yerine kaynak grubunuzun adını.

Kullanarak playbook çalıştırma `ansible-playbook` komutu:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure üzerinde Ansible](https://docs.microsoft.com/azure/ansible/)