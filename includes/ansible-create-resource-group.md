---
author: tomarcher
ms.service: ansible
ms.topic: include
ms.date: 08/09/2018
ms.author: tarcher
ms.openlocfilehash: fa1f7fe0b4b70aae4f9165197d5d1463df1f2e3b
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40026602"
---
1. Cloud Shell'de adlı bir dosya oluşturun `rg.yml`.

    ```bash
    vi rg.yml
    ```

1. Ekleme modu seçerek girin **miyim** anahtarı.

1. Aşağıdaki kod düzenleyicisine yapıştırın:

   ```yaml
   ---
   - hosts: localhost
     connection: local
     tasks:
       - name: Create resource group
         azure_rm_resourcegroup:
           name: ansible-rg
           location: eastus
         register: rg
       - debug:
           var: rg
   ```

1. Çıkış Ekle modundan **Esc** anahtarı.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyiciden çıkın:

    ```bash
    :wq
    ```

1. Playbook'u Çalıştır `rg.yml`:

   ```bash
   ansible-playbook rg.yml
   ```

Ansible'ı komut çalıştırma sonuçları aşağıdaki çıktıya benzer görünmelidir:

```output
PLAY [localhost] *********************************************************************************

TASK [Gathering Facts] ***************************************************************************
ok: [localhost]

TASK [Create resource group] *********************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************
ok: [localhost] => {
    "rg": {
        "changed": true,
        "contains_resources": false,
        "failed": false,
        "state": {
            "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/ansible-rg",
            "location": "eastus",
            "name": "ansible-rg",
            "provisioning_state": "Succeeded",
            "tags": null
        }
    }
}

PLAY RECAP ***************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0
```