---
title: Azure Blockchain sorun giderme çalışma ekranı
description: Bir Azure Blockchain çalışma ekranı uygulama sorunlarını giderme.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: df7fd71bee287a063e8d586de38522f3b71e14d9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain sorun giderme çalışma ekranı

Bir PowerShell komut dosyası hata ayıklama developer ile yardımcı veya desteklemek kullanılabilir. Betik bir Özet oluşturur ve sorun giderme için ayrıntılı günlük toplar. Toplanan günlükleri şunlardır:

* Ethereum gibi Blockchain ağ
* Blockchain çalışma ekranı mikro
* Application Insights
* Azure (OMS) izleme

Sonraki adımlar belirlemek ve sorunları kök nedenini belirlemek için bilgileri kullanın. 

## <a name="troubleshooting-script"></a>Betik sorunlarını giderme

Betik sorunlarını giderme PowerShell Github'da kullanılabilir. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/blockchain/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Komut dosyasını çalıştır
[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

Çalıştırma `collectBlockchainWorkbenchTroubleshooting.ps1` günlükleri toplamak ve sorun giderme bilgileri, bir klasör içeren bir ZIP dosyası oluşturmak için komut dosyası. Örneğin:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Betiği aşağıdaki parametreleri kabul eder:

| Parametre  | Açıklama | Gerekli |
|---------|---------|----|
| Subscriptionıd | Oluşturmak veya tüm kaynaklarını bulmak için Subscriptionıd. | Evet |
| ResourceGroupName | Blockchain çalışma ekranı bir yere dağıtılan Azure kaynak grubu adı. | Evet |
| Çıktıdizini | Çıktı oluşturmak için yolu. ZIP dosyası. Belirtilmezse, geçerli dizine varsayılan olur. | Hayır
| OmsSubscriptionId | OMS dağıtıldığı abonelik kimliği. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin.| Hayır |
| OmsResourceGroup |OMS dağıtıldığı kaynak grubu. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin.| Hayır |
| OmsWorkspaceName | OMS çalışma alanı adı. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin | Hayır |

## <a name="what-is-collected"></a>Ne toplanır?

Çıktı ZIP dosyası aşağıdaki klasör yapısını içerir:

| Klasör \ dosya | Açıklama  |
|---------|---------|
| \Summary.txt | Sistem Özeti |
| \metrics\blockchain | Ölçümleri blockchain hakkında |
| \metrics\workbench | Çalışma ekranı hakkında ölçümleri |
| \details\blockchain | Blockchain hakkında ayrıntılı Günlükler |
| \details\workbench | Çalışma ekranı hakkında ayrıntılı Günlükler |

Özet dosyası, uygulamanın genel durumu ve sistem durumu uygulamanın bir anlık görüntüsünü sunar. Özet üst hataları ve meta verileri, hizmetleri çalıştırma hakkında önemli noktalar, önerilen eylemleri sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain çalışma ekranı mimarisi](blockchain-workbench-architecture.md)