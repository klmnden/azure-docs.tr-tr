---
title: "PowerShell ile bir bulut hizmeti kapsayıcısını oluşturma | Microsoft Docs"
description: "Bu makalede PowerShell ile bir bulut hizmeti kapsayıcısını oluşturma açıklanmaktadır. Kapsayıcı web ve çalışan rollerini barındırır."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 2023fa7b318f9f76ce1e1ea0a46110297be9a001
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Boş bir bulut hizmeti kapsayıcısını oluşturmak için bir Azure PowerShell komutunu kullanın
Bu makalede hızlı bir şekilde Azure PowerShell cmdlet'lerini kullanarak bulut Hizmetleri kapsayıcısı oluşturma açıklanmaktadır. Lütfen aşağıdaki adımları izleyin:

1. Microsoft Azure PowerShell cmdlet'inden yükleme [Azure PowerShell indirmeleri](http://aka.ms/webpi-azps) sayfası.
2. PowerShell komut istemi açın.
3. Kullanım [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) oturum açmak için.

   > [!NOTE]
   > Azure PowerShell cmdlet yükleme ve Azure aboneliğinize bağlanma hakkında daha fazla yönerge için başvurmak [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
   >
   >
4. Kullanım **New-AzureService** boş Azure bulut hizmeti kapsayıcısını oluşturmak için cmdlet'i.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Bu örnek cmdlet çağrılacak izleyin:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Azure bulut hizmeti oluşturma hakkında daha fazla bilgi için çalıştırın:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Sonraki adımlar
* Bulut hizmeti dağıtımı yönetmek için başvurmak [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), ve [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) komutları. Ayrıca bakabilirsiniz [bulut hizmetlerin nasıl yapılandırılacağı](cloud-services-how-to-configure.md) daha fazla bilgi için.
* İçin Azure bulut hizmeti projenizi yayımlamak için başvurmak **PublishCloudService.ps1** kod örnekten [Azure'daki bulut hizmeti için kesintisiz teslim](cloud-services-dotnet-continuous-delivery.md).
