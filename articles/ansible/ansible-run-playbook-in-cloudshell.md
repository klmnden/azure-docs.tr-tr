---
title: Hızlı Başlangıç - Azure Cloud Shell'deki Bash hizmetinde aracılığıyla Çalıştır Ansible playbook'ları | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Cloud Shell'deki Bash hizmetinde ile çeşitli Ansible görevlerini gerçekleştirmenize öğrenin
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: a0d0544601bfc6ce0c7ef642f576ed780c8642ba
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763380"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Hızlı Başlangıç: Azure Cloud Shell'deki Bash hizmetinde aracılığıyla Ansible playbook'ları Çalıştır

Azure Cloud Shell'i Azure kaynaklarını yönetmek için etkileşimli ve tarayıcı erişilebilir bir kabuktur. Cloud Shell'i etkinleştirir sağlar, Bash veya Powershell komut satırını kullanın. Bu makalede, Bash Azure Cloud Shell içinde bir Ansible playbook çalıştırmak için kullanın.

## <a name="prerequisites"></a>Önkoşullar

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure Cloud Shell'i yapılandırın** - Azure Cloud Shell için yeni başladıysanız bkz [Hızlı Başlangıç için Azure Cloud Shell'deki Bash hizmetinde](https://docs.microsoft.com/azure/cloud-shell/quickstart).
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Cloud shell'e imzalandığında Ansible herhangi bir ek yapılandırma olmadan altyapısını yönetmek için Azure ile kimlik doğrulaması yapar. 

Birden çok aboneliği ile çalışırken, Ansible kullanan aktararak abonelik belirtin `AZURE_SUBSCRIPTION_ID` ortam değişkeni. 

Tüm Azure aboneliklerinizi listelemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az account list
```

Azure abonelik Kimliğinizi kullanarak `AZURE_SUBSCRIPTION_ID` gibi:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Başarılı yapılandırmasını doğrulamak için bir Azure kaynak grubu oluşturmak için ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı Başlangıç: Ansible'ı kullanarak Azure'da sanal makine yapılandırma](/azure/virtual-machines/linux/ansible-create-vm)