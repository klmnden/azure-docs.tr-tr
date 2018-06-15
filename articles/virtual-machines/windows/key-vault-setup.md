---
title: Anahtar kasası için Windows sanal makineleri Azure Kaynak Yöneticisi'nde ayarlama | Microsoft Docs
description: Anahtar kasası bir Azure Resource Manager sanal makinesi ile kullanmak için nasıl kurulur.
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
ms.openlocfilehash: f8f094bfb0f304123cbdf719bec22185431aca5a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30918396"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Sanal makineler Azure Kaynak Yöneticisi'nde için anahtar kasasını oluşturup

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Gizli/sertifikalar, anahtar kasası kaynak sağlayıcısı tarafından sağlanan kaynaklar olarak Azure Kaynak Yöneticisi yığınında modellenir. Anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Azure Resource Manager sanal makinelerle kullanılacak anahtar kasası için sırayla **EnabledForDeployment** anahtar kasası özelliğinin ayarlanması true. Çeşitli istemciler bunu yapabilirsiniz.
> 2. Anahtar kasası aynı abonelikte ve konumda sanal makine olarak oluşturulması gerekir.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Anahtar kasası ayarlamak için PowerShell kullanın
PowerShell kullanarak bir anahtar kasası oluşturmak için bkz: [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md#vault).

Yeni anahtar kasalarını için bu PowerShell cmdlet'ini kullanabilirsiniz:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Varolan anahtar kasalarını için bu PowerShell cmdlet'ini kullanabilirsiniz:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Bize CLI anahtar kasası ayarlamak için
Komut satırı arabirimi (CLI) kullanarak bir anahtar kasası oluşturmak için bkz: [anahtar kasası CLI kullanarak yönetme](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

CLI için dağıtım ilkesi atamadan önce anahtar kasası oluşturmanız gerekir. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Anahtar kasası ayarlamak için şablonlarını kullanma
Bir şablon kullanırken ayarlamanız gerekir `enabledForDeployment` özelliğine `true` anahtar kasası kaynak için.

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

Şablonları kullanarak bir anahtar kasası oluşturduğunuzda, sizin yapılandırabileceğiniz diğer seçenekleri için bkz: [bir anahtar kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
