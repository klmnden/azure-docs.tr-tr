---
title: PowerShell ile bir bulut hizmeti kapsayıcısını oluşturma | Microsoft Docs
description: Bu makalede PowerShell ile bir bulut hizmeti kapsayıcısını oluşturma açıklanmaktadır. Kapsayıcı web ve çalışan rollerini barındırır.
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: ''
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: b55a0dd7800448c50897af784092b4a60fa7a25e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29376142"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Boş bir bulut hizmeti kapsayıcısını oluşturmak için bir Azure PowerShell komutunu kullanın
Bu makalede hızlı bir şekilde Azure PowerShell cmdlet'lerini kullanarak bulut Hizmetleri kapsayıcısı oluşturma açıklanmaktadır. Lütfen aşağıdaki adımları izleyin:

1. Microsoft Azure PowerShell cmdlet'inden yükleme [Azure PowerShell indirmeleri](http://aka.ms/webpi-azps) sayfası.
2. PowerShell komut istemi açın.
3. Kullanım [Add-AzureAccount](/powershell/module/Azure/Add-AzureAccount?view=azuresmps-4.0.0) oturum açmak için.

   > [!NOTE]
   > Azure PowerShell cmdlet yükleme ve Azure aboneliğinize bağlanma hakkında daha fazla yönerge için başvurmak [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
   >
   >
4. Kullanım **New-AzureService** boş Azure bulut hizmeti kapsayıcısını oluşturmak için cmdlet'i.

   ```powershell
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Bu örnek cmdlet çağrılacak izleyin:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Azure bulut hizmeti oluşturma hakkında daha fazla bilgi için çalıştırın:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Sonraki adımlar
* Bulut hizmeti dağıtımı yönetmek için başvurmak [Get-AzureService](/powershell/module/Azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/Azure/Remove-AzureService?view=azuresmps-4.0.0), ve [Set-AzureService](/powershell/module/azure/set-azureservice?view=azuresmps-4.0.0) komutları. Ayrıca bakabilirsiniz [bulut hizmetlerin nasıl yapılandırılacağı](cloud-services-how-to-configure-portal.md) daha fazla bilgi için.
* İçin Azure bulut hizmeti projenizi yayımlamak için başvurmak **PublishCloudService.ps1** kod örnekten [arşivlenmiş bulut Hizmetleri havuzunu](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).
