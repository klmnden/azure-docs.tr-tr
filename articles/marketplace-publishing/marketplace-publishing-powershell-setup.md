---
title: Market VM oluşturmak için PowerShell'i ayarlama | Microsoft Docs
description: Azure PowerShell ayarlama ayarlama ve isteğe bağlı bir işlem olarak kullanmak için yönergeleri dağıtmak için VM görüntüleri oluşturmak için flow ve Azure Marketi'nde
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: bbcce5093d2bbd5326523063db7d0e565fe4de6d
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713644"
---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure PowerShell ' için Azure Marketi'nde teklif oluşturma ayarlayın
Azure PowerShell ayarlama konusunda ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview). Basit bir yaklaşım indirir ve kimlik doğrulaması için gerekli bir sertifika alır sertifika yöntemini kullanmaktır. Gerekli sertifikayı almak için kullanın **Get-AzurePublishSettingsFile** cmdlet'i. Sorulduğunda dosyayı kaydedin. Sertifika bir PowerShell oturumunuza aktarmak için kullanın **Import-AzurePublishSettingsFile** cmdlet'i.

Yapılandırma ve PowerShell oturumunun yaygın Microsoft Azure abonelik ayarlarını depolamak için kullanmak **Set-AzureSubscription** ve **Select-AzureSubscription** cmdlet'leri:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

İlk komut, varsayılan depolama hesabı (bazı VM sağlama işlemleri için gereklidir) aboneliği ile ilişkilendirir.  İkinci (diğer cmdlet'ler tarafından tanınan) geçerli bir abonelik yapar.

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)
* [Market'te bir sanal makine görüntüsü oluşturma](marketplace-publishing-vm-image-creation.md)

