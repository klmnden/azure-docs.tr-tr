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
ms.openlocfilehash: 419ed6dc76101366e47ae94067f7b671a10c94e2
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34196343"
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

| Parametre  | Açıklama | Gereken |
|---------|---------|----|
| Subscriptionıd | Oluşturmak veya tüm kaynaklarını bulmak için Subscriptionıd. | Evet |
| ResourceGroupName | Blockchain çalışma ekranı bir yere dağıtılan Azure kaynak grubu adı. | Evet |
| Çıktıdizini | Çıktı oluşturmak için yolu. ZIP dosyası. Belirtilmezse, geçerli dizine varsayılan olur. | Hayır |
| LookbackHours | Telemetri çekme yapılırken kullanılacak saat sayısı. Varsayılan değer 24 saattir. En büyük değer 90 saattir | Hayır |
| OmsSubscriptionId | OMS dağıtıldığı abonelik kimliği. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin.| Hayır |
| OmsResourceGroup |OMS dağıtıldığı kaynak grubu. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin.| Hayır |
| OmsWorkspaceName | OMS çalışma alanı adı. Blockchain ağ için OMS Blockchain çalışma'nın kaynak grubu dışında dağıttıysanız, bu parametre yalnızca geçirin | Hayır |

## <a name="what-is-collected"></a>Ne toplanır?

Çıktı ZIP dosyası aşağıdaki klasör yapısını içerir:

| Klasör veya dosya | Açıklama  |
|---------|---------|
| \Summary.txt | Sistem Özeti |
| \Metrics\blockchain | Ölçümleri blockchain hakkında |
| \Metrics\Workbench | Çalışma ekranı hakkında ölçümleri |
| \Details\Blockchain | Blockchain hakkında ayrıntılı Günlükler |
| \Details\Workbench | Çalışma ekranı hakkında ayrıntılı Günlükler |

Özet dosyası, uygulamanın genel durumu ve sistem durumu uygulamanın bir anlık görüntüsünü sunar. Özet üst hataları ve meta verileri, hizmetleri çalıştırma hakkında önemli noktalar, önerilen eylemleri sağlar.

**Ölçümleri** klasörü, zaman içinde çeşitli sistem bileşenleri ölçümlerini içerir. Örneğin, çıktı dosyası `\Details\Workbench\apiMetrics.txt` farklı yanıt kodları ve yanıt süresi toplama süresi boyunca özetini içerir. **Ayrıntıları** klasörü belirli çalışma ekranı veya arka plandaki blockchain ağ sorunlarını gidermek için ayrıntılı günlük içerir. Örneğin, `\Details\Workbench\Exceptions.csv` akıllı Sözleşmelerle hatalar veya blockchain ile etkileşim sorun giderme için yararlı olan sistem içinde gerçekleşen en son özel durum listesini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain çalışma ekranı mimarisi](blockchain-workbench-architecture.md)