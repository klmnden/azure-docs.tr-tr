---
title: Azure Blockchain Workbench ile ilgili sorunları giderme
description: Azure Blockchain Workbench uygulama sorunlarını giderme.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: b0263761a4aaf663b16584fbf9caa11bb124d5c4
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510083"
---
# <a name="azure-blockchain-workbench-troubleshooting"></a>Azure Blockchain Workbench ile ilgili sorunları giderme

Bir PowerShell Betiği ile hata ayıklama Geliştirici Yardımcısı veya desteklemek kullanılabilir. Betik özetini oluşturur ve sorun giderme için ayrıntılı günlükleri toplar. Toplanan günlükleri şunlardır:

* Ethereum gibi blok zinciri ağ
* Blockchain Workbench'i mikro hizmetler
* Application Insights
* Azure izleme (Azure İzleyici günlüklerine)

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
| OmsSubscriptionId | Azure İzleyicisi'ni nereden oturum abonelik kimliği dağıtılır. Blok zinciri ağ için Azure İzleyici günlüklerine Blockchain Workbench'i'nın kaynak grubu dışında dağıtılırsa, bu parametre yalnızca geçirin.| Hayır |
| OmsResourceGroup |Azure İzleyicisi'ni nereden oturum kaynak grubu dağıtılır. Blok zinciri ağ için Azure İzleyici günlüklerine Blockchain Workbench'i'nın kaynak grubu dışında dağıtılırsa, bu parametre yalnızca geçirin.| Hayır |
| OmsWorkspaceName | Log Analytics çalışma alanı adı. Blok zinciri ağ için Azure İzleyici günlüklerine Blockchain Workbench'i'nın kaynak grubu dışında dağıtılırsa Bu parametre yalnızca başarılı | Hayır |

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
> [Azure Blockchain Workbench Application Insights sorun giderme kılavuzu](https://aka.ms/workbenchtroubleshooting)
