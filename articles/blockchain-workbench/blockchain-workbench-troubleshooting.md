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
ms.openlocfilehash: 8a2715666c4fff490f5184b7b8719b412952b9bf
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
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

> [!div class="nextstepaction"]
> [Azure Blockchain çalışma ekranı mimarisi](blockchain-workbench-architecture.md)