---
title: Bash Azure Cloud shell'de Ansible çalıştırma
description: Azure Cloud Shell'de Bash ile çeşitli Ansible görevleri gerçekleştirme hakkında bilgi edinin
ms.service: ansible
keywords: ansible'ı, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 9928f646905dd0da4b15166ec55e5d8a183cb210
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42058280"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Bash Azure Cloud shell'de Ansible çalıştırma

Bu öğreticide, Bash Cloud Shell içinde Ansible çalışma alanınızı bir Azure aboneliği yapılandırmak için kullanmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği** - oluşturma, bir Azure aboneliği yoksa, bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Azure Cloud Shell'i yapılandırın** - Azure Cloud Shell, makale yeniyseniz [Hızlı Başlangıç için Azure Cloud Shell'deki Bash hizmetinde](https://docs.microsoft.com/azure/cloud-shell/quickstart), başlatmak ve Cloud Shell yapılandırma gösterilmektedir. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırmasını

Cloud shell'e imzalandığında Ansible herhangi bir ek yapılandırma olmadan altyapısını yönetmek için Azure ile kimlik doğrulaması yapar. Birden fazla aboneliğiniz varsa, Ansible çalışmalıdır ile vererek hangi abonelik seçebilirsiniz `AZURE_SUBSCRIPTION_ID` ortam değişkeni. Tüm Azure aboneliklerinizi listelemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az account list
```

Kullanarak **kimliği** istediğiniz çalışması, aboneliği ayarlamak **AZURE_SUBSCRIPTION_ID** gibi:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>yapılandırıldığını doğrulayın
Başarılı yapılandırmasını doğrulamak için bir kaynak grubu oluşturmak için ansible'ı kullanın.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Ansible ile temel bir sanal makine oluşturma](/azure/virtual-machines/linux/ansible-create-vm)