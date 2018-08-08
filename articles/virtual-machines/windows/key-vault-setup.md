---
title: Windows VM'ler için anahtar kasası Azure Resource Manager'daki ayarlama | Microsoft Docs
description: Bir Azure Resource Manager sanal makinesi ile kullanmak için anahtar kasası ayarlama yapma.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: 599b16f633d9a0de5165bdf5cb3d7b82abca655b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597719"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Azure Resource Manager'da sanal makineler için anahtar kasası ayarlama

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Resource Manager yığınında gizli dizileri/sertifikaları Key Vault kaynak sağlayıcısı tarafından sağlanan kaynaklar olarak modellenir. Key Vault hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Key Vault, Azure Resource Manager sanal makineler ile kullanılacak sırada **EnabledForDeployment** Key Vault özelliği ayarlanmalıdır true. Çeşitli istemciler, bunu yapabilirsiniz.
> 2. Key Vault aynı abonelik ve konumdaki sanal makine olarak oluşturulması gerekir.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Anahtar kasası ayarlama için PowerShell kullanma
PowerShell kullanarak bir anahtar kasası oluşturmak için bkz [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md#vault).

Yeni anahtar kasası için bu PowerShell cmdlet'ini kullanabilirsiniz:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Varolan anahtar kasası için bu PowerShell cmdlet'ini kullanabilirsiniz:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Anahtar kasası ayarlama için CLI kullanma
Komut satırı arabirimi (CLI) kullanarak bir anahtar kasası oluşturmak için bkz [yönetme CLI ile anahtar kasası](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

CLI için dağıtım ilkesi atamadan önce anahtar kasası oluşturmanız gerekir. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Anahtar kasası ayarlama için şablonları kullanma
Bir şablon kullanırken ayarlamanız gerekir `enabledForDeployment` özelliğini `true` Key Vault kaynak için.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Şablonları kullanarak bir anahtar kasası oluşturduğunuzda, yapılandırabileceğiniz diğer seçenekleri için bkz: [anahtar kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
