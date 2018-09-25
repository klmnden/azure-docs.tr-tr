---
title: LUIS Azure Cloud Shell kullanım verileri
titleSuffix: Azure Cognitive Services
description: Kullanım bilgileri için LUIS Azure Cloud Shell'de almayı öğrenin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2017
ms.author: diberry
ms.openlocfilehash: b0a02d2e5e0a1f1765098e1b1e258424b6443878
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039590"
---
# <a name="usage-data-for-luis-service-from-azure-cloud-shell"></a>Azure Cloud shell'den LUIS hizmeti için kullanım verileri
Azure portalında LUIS kaynak ile çalışmak için PowerShell cmdlet'leri kullanmanıza olanak tanır. 

Bu cmdlet'ler izin [oluşturma](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) LUIS abonelik, aboneliği hakkında bilgi alın dahil olmak üzere [kullanım](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), ve [Kaldır](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) abonelik. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Cloud shell depolama hesabı ve kimlik doğrulaması
Azure portalında PowerShell kullanmak için [cloud shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell), Azure depolama hesabınız olması gerekir. Yoksa bir [depolama hesabı](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share), oluşturmanız istenir. Depolama hesabı cloud shell'de PowerShell betikleri kaydetmenize olanak tanır.  

Ayrıca, Azure'a tüm kaynaklara erişmek için cloud shell içinde kimlik doğrulaması gerekir. 

Bir depolama hesabınız var ve doğrulandıktan sonra PowerShell cmdlet'lerini çalıştırabilirsiniz.

## <a name="open-cloud-shell"></a>Cloud Shell’i açma
Azure portalında cloud shell kullandığınızda, her zaman en son PowerShell sürümüne olur. 

Kullanım **Cloud Shell'i Başlat** Cloud Shell'i açın veya bir tarayıcı ile açmak için düğmeyi [ https://shell.azure.com ](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>LUIS uç nokta kullanım bilgileri

PowerShell 6.x cmdlet'ini `Get-AzureRmCognitiveServicesAccountUsage`, Microsoft Bilişsel hizmetler LUIS dahil olmak üzere kullanım bilgilerini sağlar. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) kaynak grubu ve kaynak adı hizmeti gerektirir. 

Komut sözdizimi aşağıdaki gibidir:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

Aşağıdaki örnekte, kaynak grubu adı olan `luis-westus-rg` ve LUIS hizmeti abonelik adını `luis-westus-1`. Hem bu adlar LUIS hizmet oluşturulduğunda seçilir. 

Cmdlet, 7 Haziran bitiş süresi 30 gün içerisinde kullanılan 10.000 uç nokta isabetli okuma sayısının 16 kullanım bilgilerini döndürür:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Komut PowerShell cloud shell ile ilişkili Azure depolama hesabındaki *.ps1 dosyası olarak kaydedin ve herhangi bir zamanda yürütün. 

![Depolama alanından betiği çalıştırın](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Betik bulut sürücüsünde kaydedildikten sonra telefon uygulamanın cloud Shell'i Azure PowerShell betiğini çalıştırabilirsiniz. 

![Telefon uygulaması içinde depolamadan betiği çalıştırın](./media/luis-how-to-manage-from-powershell/phone-app.png)