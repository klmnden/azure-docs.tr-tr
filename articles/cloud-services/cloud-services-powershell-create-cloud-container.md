---
title: PowerShell ile bir bulut hizmeti kapsayıcısı oluşturma | Microsoft Docs
description: Bu makalede PowerShell ile bir bulut hizmeti kapsayıcısı oluşturma açıklanmaktadır. Kapsayıcı web ve çalışan rollerini barındırır.
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
ms.openlocfilehash: 771f93edfee8f7b48fb7d0d2c98419f9427f6338
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60527322"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Boş bir bulut hizmeti kapsayıcısı oluşturmak için bir Azure PowerShell komutunu kullanın

Bu makalede hızlı bir şekilde Azure PowerShell cmdlet'lerini kullanarak bir bulut Hizmetleri kapsayıcının nasıl oluşturulacağını açıklar. Lütfen aşağıdaki adımları izleyin:

1. Microsoft Azure PowerShell cmdlet'inden yükleme [Azure PowerShell indirir](https://aka.ms/webpi-azps) sayfası.
2. PowerShell komut istemini açın.
3. Kullanım [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-4.0.0) oturum açmak için.

   > [!NOTE]
   > Azure PowerShell cmdlet yükleme ve Azure aboneliğinize bağlanma hakkında daha fazla yönerge için başvurmak [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).
   >
   >
4. Kullanım **New-AzureService** boş bir Azure bulut hizmeti kapsayıcısı oluşturmak için cmdlet'i.

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. Bu örnek cmdlet çağrılacak izleyin:

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Azure bulut hizmeti oluşturma hakkında daha fazla bilgi için çalıştırın:

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>Sonraki adımlar

* Bulut hizmeti dağıtımı yönetmek için başvurmak [Get-AzureService](/powershell/module/servicemanagement/azure/Get-AzureService?view=azuresmps-4.0.0), [Remove-AzureService](/powershell/module/servicemanagement/azure/Remove-AzureService?view=azuresmps-4.0.0), ve [Set-AzureService](/powershell/module/servicemanagement/azure/set-azureservice?view=azuresmps-4.0.0) komutları. Ayrıca bakabilirsiniz [bulut Hizmetleri Yapılandırma](cloud-services-how-to-configure-portal.md) daha fazla bilgi için.
* Azure'da bulut hizmeti projenizi yayımlamak için başvurmak **PublishCloudService.ps1** kod örneğini [arşivlenmiş cloud services deposu](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery).