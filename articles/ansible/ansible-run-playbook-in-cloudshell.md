---
title: Bash Azure Cloud shell'de Ansible çalıştırma
description: Azure Cloud Shell'de Bash ile çeşitli Ansible görevleri gerçekleştirme hakkında bilgi edinin
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: quickstart
ms.openlocfilehash: 61b23b5bc2620c82051b0ba1de4bb54b44a495e0
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359196"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Bash Azure Cloud shell'de Ansible çalıştırma

Bu öğreticide, Bash Cloud Shell içinde Ansible çalışma alanınızı bir Azure aboneliği yapılandırmak için kullanmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Azure Cloud Shell'i yapılandırın** - Azure Cloud Shell'i kullanmaya yeni başladıysanız [Azure Cloud Shell'de Bash için hızlı başlangıç](https://docs.microsoft.com/azure/cloud-shell/quickstart) makalesindeki Cloud Shell'i başlatma ve yapılandırma adımlarını izleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Cloud shell'e imzalandığında Ansible herhangi bir ek yapılandırma olmadan altyapısını yönetmek için Azure ile kimlik doğrulaması yapar. Birden fazla aboneliğiniz varsa, Ansible çalışmalıdır ile vererek hangi abonelik seçebilirsiniz `AZURE_SUBSCRIPTION_ID` ortam değişkeni. Tüm Azure aboneliklerinizi listelemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az account list
```

Kullanarak **kimliği** istediğiniz çalışması, aboneliği ayarlamak **AZURE_SUBSCRIPTION_ID** gibi:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Başarılı yapılandırmasını doğrulamak için bir kaynak grubu oluşturmak için ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Ansible ile temel bir sanal makine oluşturma](/azure/virtual-machines/linux/ansible-create-vm)