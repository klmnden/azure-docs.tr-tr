---
title: Azure CLI 1.0 ile Linux VM'ler için anahtar kasasını oluşturup | Microsoft Docs
description: Anahtar kasası Azure CLI 1.0 ile bir Azure Resource Manager sanal makine ile kullanmak için nasıl kurulur.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: a9225429e878415334b0c8a66777902395606d63
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30911511"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-the-azure-cli-10"></a>Sanal makineler Azure Resource Manager ile Azure CLI 1.0 için anahtar kasasını oluşturup
Gizli/sertifikalar, anahtar kasası kaynak sağlayıcısı tarafından sağlanan kaynaklar olarak Azure Kaynak Yöneticisi yığınında modellenir. Azure anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md) Azure Resource Manager sanal makinelerle kullanılacak anahtar kasası için sırayla *EnabledForDeployment* anahtar kasası özelliğinin ayarlanması true. Çeşitli istemciler bunu yapabilirsiniz. Bu makalede Azure sanal makineler ile kullanmak için anahtar kasasını oluşturup gösterilmiştir.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
CLI sürümlerinden birini kullanarak görevi tamamlamak

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="use-cli-10-to-set-up-key-vault"></a>Anahtar kasası ayarlamak için CLI 1.0 kullanın
Komut satırı arabirimi (CLI) kullanarak bir anahtar kasası oluşturmak için bkz: [anahtar kasası CLI kullanarak yönetme](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

CLI 1.0 için dağıtım ilkesi atamadan önce anahtar kasası oluşturmanız gerekir. Ardından, aşağıdaki komutu kullanarak İlkesi atayabilirsiniz:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Anahtar kasası ayarlamak için şablonlarını kullanma
Bir şablonu kullandığınızda ayarlamanız gerekir `enabledForDeployment` özelliğine `true` anahtar kasası kaynak için.

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
