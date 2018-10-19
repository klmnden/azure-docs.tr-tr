---
title: Azure Blockchain Workbench ile ilgili sorunları giderme
description: Azure Blockchain Workbench uygulama sorunlarını giderme.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e205fce8b718e68200face33447e37cd3317298f
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405493"
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench ile ilgili sorunları giderme

Bir PowerShell Betiği ile hata ayıklama Geliştirici Yardımcısı veya desteklemek kullanılabilir. Betik özetini oluşturur ve sorun giderme için ayrıntılı günlükleri toplar. Toplanan günlükleri şunlardır:

* Ethereum gibi blok zinciri ağ
* Blockchain Workbench'i mikro hizmetler
* Application Insights
* Azure (Log Analytics'e) izleme

Sonraki adımları belirlemek ve sorunların kök nedenini belirlemek için bilgileri kullanabilirsiniz. 

## <a name="troubleshooting-script"></a>Betik sorunlarını giderme

PowerShell Betiği sorunlarını gidermek, Github'da kullanılabilir. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/blockchain/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Betiği çalıştırın
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Çalıştırma `collectBlockchainWorkbenchTroubleshooting.ps1` günlük toplama ve sorun giderme bilgileri, bir klasör içeren bir ZIP dosyası oluşturmak için komut dosyası. Örneğin:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Betiği aşağıdaki parametreleri kabul eder:

| Parametre  | Açıklama | Gerekli |
|---------|---------|----|
| Subscriptionıd | Subscriptionıd, oluşturma veya tüm kaynakları bulun. | Evet |
| ResourceGroupName | Blockchain Workbench'i dağıtıldığı Azure kaynak grubunun adı. | Evet |
| OutputDirectory | Çıkış oluşturma yolu. ZIP dosyası. Belirtilmezse, geçerli dizin için varsayılan olarak. | Hayır |
| LookbackHours | Telemetri çekme sırasında kullanılacak saat sayısı. Varsayılan değer 24 saattir. Maksimum değer 90 saattir | Hayır |
| OmsSubscriptionId | Log Analytics dağıtıldığı abonelik kimliği. Blockchain Workbench'i'nın kaynak grubu dışında blok zinciri ağ için Log Analytics dağıttıysanız Bu parametre yalnızca geçirin.| Hayır |
| OmsResourceGroup |Log Analytics dağıtıldığı kaynak grubu. Blockchain Workbench'i'nın kaynak grubu dışında blok zinciri ağ için Log Analytics dağıttıysanız Bu parametre yalnızca geçirin.| Hayır |
| OmsWorkspaceName | Log Analytics çalışma alanı adı. Blockchain Workbench'i'nın kaynak grubu dışında blok zinciri ağ için Log Analytics dağıttıysanız Bu parametre yalnızca başarılı | Hayır |

## <a name="what-is-collected"></a>Ne toplanır?

Çıkış ZIP dosyası aşağıdaki klasör yapısını içerir:

| Klasör veya dosya | Açıklama  |
|---------|---------|
| \Summary.txt | Sistem Özeti |
| \Metrics\blockchain | Blok zinciri ile ilgili ölçümleri |
| \Metrics\Workbench | Workbench hakkında ölçümler |
| \Details\Blockchain | Blockchain hakkında ayrıntılı Günlükler |
| \Details\Workbench | Workbench hakkında ayrıntılı Günlükler |

Özet dosyası, uygulamanın genel durumunu ve uygulama durumunun anlık görüntüsünü sunar. Özet önerilen eylemleri sunar, en sık karşılaşılan hatalar ve meta verileri, hizmetleri çalıştırma konusunda vurgular.

**Ölçümleri** klasörü zamanla çeşitli sistemi bileşenlerinin ölçümleri içerir. Örneğin, çıktı dosyası `\Details\Workbench\apiMetrics.txt` farklı yanıt kodları ve yanıt sürelerini toplama süresi boyunca bir özetini içerir. **Ayrıntıları** klasörü, Workbench veya arka plandaki blockchain ağ belirli sorunları gidermek için ayrıntılı günlükleri içerir. Örneğin, `\Details\Workbench\Exceptions.csv` nitelikli akıllı anlaşmalar hatalarla veya blok zinciri ile etkileşim sorun giderme için kullanışlı olan sistem, ortaya çıkan en son özel durumlar listesini içerir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench mimarisi](architecture.md)
