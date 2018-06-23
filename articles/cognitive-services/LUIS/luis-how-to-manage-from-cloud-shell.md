---
title: Azure bulut Kabuğu'ndan HALUK kullanımı görüntülemek | Microsoft Docs
description: HALUK için Azure bulut Kabuğu'nda kullanım bilgileri almak öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2017
ms.author: v-geberr
ms.openlocfilehash: 2de25645e5377efdd53bcc980695804d34db5ee2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354568"
---
# <a name="manage-luis-service-from-azure-cloud-shell"></a>Azure bulut Kabuğu'ndan HALUK hizmetini yönetme
Azure portalı, HALUK kaynakları ile çalışmak için PowerShell cmdlet'leri kullanmanıza olanak sağlar. 

Bu cmdlet'ler izin [oluşturma](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) HALUK abonelik, abonelik ilgili bilgileri Al dahil olmak üzere [kullanım](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), ve [kaldırmak](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) abonelik. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Bulut Kabuk depolama hesabı ve kimlik doğrulaması
Azure portalında PowerShell kullanmak için [bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell), bir Azure depolama hesabınızın olması gerekir. Yoksa bir [depolama hesabı](https://docs.microsoft.com/en-us/azure/cloud-shell/persisting-shell-storage#set-up-a-clouddrive-file-share), oluşturmanız istenir. Depolama hesabı PowerShell komut dosyalarını bulut Kabuğu'nda kaydetmenize olanak sağlar.  

Azure için bulut kabuğunda herhangi bir kaynağa erişmek için kimlik doğrulaması gerekir. 

Bir depolama hesabınız varsa ve doğrulandıktan sonra PowerShell cmdlet'lerini çalıştırabilirsiniz.

## <a name="open-cloud-shell"></a>Cloud Shell’i açma
Azure portal bulut Kabuğu'nu kullandığınızda, her zaman en güncel PowerShell sürümü olur. 

Kullanım **başlatma bulut Kabuk** bulut Kabuğu'nu açın veya bir tarayıcı ile açmak için düğmesini [ https://shell.azure.com ](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>HALUK uç noktası kullanım bilgileri

PowerShell 6.x cmdlet'ini `Get-AzureRmCognitiveServicesAccountUsage`, Microsoft Bilişsel HALUK gibi hizmetler için kullanım bilgileri sağlar. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) kaynak grubu ve hizmetin kaynak adı gerektirir. 

Komut sözdizimi aşağıdaki gibidir:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

Aşağıdaki örnekte, kaynak grubu adı olan `luis-westus-rg` ve HALUK hizmeti abonelik adını `luis-westus-1`. Bu iki adları HALUK hizmet oluşturulduğunda seçilir. 

Cmdlet 30 günlük süre 7 Haziran bitiş süresi ile kullanılan 10.000 uç nokta isabetli okuma sayısının 16 kullanım bilgilerini döndürür:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

PowerShell dosyası, bulut kabuğu ile ilişkili Azure depolama hesabındaki *.ps1 olarak Kaydet komutu ve herhangi bir zamanda yürütün. 

![Depolama biriminden betiği çalıştırma](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Komut dosyası Bulut sürücüsünde kaydedildikten sonra telefon uygulamanın bulut Kabuk Azure PowerShell betiğini çalıştırabilirsiniz. 

![Telefon uygulaması depoda betiği çalıştırın](./media/luis-how-to-manage-from-powershell/phone-app.png)