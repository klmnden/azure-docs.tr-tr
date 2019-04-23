---
title: Görüntüleme göreli gecikme Azure bölgelerine belirli konumlardan | Microsoft Docs
description: Göreli gecikme Internet sağlayıcıları arasında Azure bölgelerine belirli konumlardan görüntülemeyi öğrenin.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 895e29d9855372e418ad5ebf2a3949dc01ddb8de
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59792427"
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Görünüm belirli konumlar üzerinden Azure bölgeleri için göreli gecikme

Bu öğreticide, Azure'ı kullanmayı öğrenin [Ağ İzleyicisi](network-watcher-monitoring-overview.md) uygulamanızı dağıtmak veya hizmetinizi için hangi Azure bölgesi karar vermenize yardımcı olacak hizmet, demografik, kullanıcıya dayanarak. Ayrıca, Azure hizmet sağlayıcılarının bağlantıları değerlendirmek için kullanabilirsiniz.  
        

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-network-watcher"></a>Ağ İzleyicisi oluşturma

En az bir Azure Ağ İzleyicisi zaten olup olmadığını [bölge](https://azure.microsoft.com/regions), bu bölümdeki görevlerde atlayabilirsiniz. Ağ İzleyicisi için bir kaynak grubu oluşturun. Bu örnekte, Doğu ABD bölgesinde bir kaynak grubu oluşturulur, ancak herhangi bir Azure bölgesinde bir kaynak grubu oluşturabilirsiniz.

```powershell
New-AzResourceGroup -Name NetworkWatcherRG -Location eastus
```

Ağ İzleyicisi oluşturun. En az bir Azure bölgesinde oluşturulan bir Ağ İzleyicisi olması gerekir. Bu örnekte, bir Ağ İzleyicisi, Doğu ABD Azure bölgesi oluşturulur.

```powershell
New-AzNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Göreli ağ gecikmeleri tek bir Azure bölgesine belirli bir konumdan karşılaştırın.

Hizmet sağlayıcılarını değerlendirmelerine ve sorun giderme "site yavaş gibi" bir sorun rapor etme bir kullanıcı belirli bir konumdan bir hizmet dağıtıldığı azure bölgesi. Örneğin, aşağıdaki komut, Amerika Birleşik Devletleri'nde bulunan Washington eyaleti aralık 2017 13-15 arasında Batı ABD 2 Azure bölgesi arasındaki ortalama göreli Internet hizmet sağlayıcısı gecikmeleri döndürür:

```powershell
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Location "West US 2" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> Önceki komutta bölgede Ağ İzleyicisi alınırken, belirtilen bölge ile aynı olması gerekmez. Önceki komutun yalnızca, mevcut bir Ağ İzleyicisi belirtmesini gerektirir. Ağ İzleyicisi, herhangi bir bölgede olabilir. İçin değerler belirtirseniz `-Country` ve `-State`, geçerli olması gerekir. Değerleri büyük/küçük harf duyarlıdır. Veriler, sınırlı sayıda ülke, eyalet ve şehir için kullanılabilir. Komutları çalıştırmak [görüntülemek kullanılabilir ülke, eyalet, şehir ve sağlayıcıları](#view-available) kullanılabilir ülke, şehir ve durumları önceki komutuyla birlikte kullanılacak bir listesini görüntülemek için. 

> [!WARNING]
> Bir tarih için son 30 gün içinde belirtmelisiniz `-StartTime` ve `-EndTime`. Önceki bir tarihi belirterek döndürülen içinde hiç veri neden olur.

Önceki komut çıktısı aşağıdaki gibidir:

```powershell
AggregationLevel   : State
ProviderLocation   : {
                       "Country": "United States",
                       "State": "washington"
                     }
ReachabilityReport : [
                       {
                         "Provider": "Qwest Communications Company, LLC - ASN 209",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 92
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 92
                           }
                         ]
                       },
                       {
                         "Provider": "Comcast Cable Communications, LLC - ASN 7922",
                         "AzureLocation": "West US 2",
                         "Latencies": [
                           {
                             "TimeStamp": "2017-12-14T00:00:00Z",
                             "Score": 96
                           },
                           {
                             "TimeStamp": "2017-12-13T00:00:00Z",
                             "Score": 96
                           }
                         ]
                       }
                     ]
```

Döndürülen çıktıda değeri **puanı** bölgeler ve sağlayıcılar arasında göreli gecikme olduğunu. 100 gecikme süresi en düşük iken bir puan 1 kötü (yüksek) gecikme süresi ' dir. Göreli gecikme bir gün için ortalaması alınır. Önceki örnekte, sahip temizleme sırasında aynı gecikme süreleriyle her iki gün olduğu ve gecikme süreleriyle her iki sağlayıcıları için iki sağlayıcı gecikme süresini arasında küçük bir fark, sahip de onay kutusunu temizleyin, 1-100 ölçeğinde düşük. Bazen, Amerika Birleşik Devletleri Washington eyaleti fiziksel olarak yakın Batı ABD 2 Azure bölgesini olduğundan bu, beklenen bir durumdur, ancak sonuçlar beklendiği gibi değildir. Belirttiğiniz tarih aralığı daha büyük, daha fazla, ortalama gecikme süresi zaman içinde.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Belirli bir konumdan Azure bölgeleri arasında göreli ağ gecikmeleri karşılaştırın

Eğer, belirli bir konuma kullanarak belirli bir Azure bölgesi arasındaki göreli gecikme belirtmek yerine `-Location`göreli gecikme süreleri için tüm Azure bölgelerinde belirli bir fiziksel konumdan belirlemek istediğinizi, çok bunu yapabilirsiniz. Örneğin, aşağıdaki komut, kullanıcılarınızın birincil içeren Washington eyaletinin bulunan Comcast kullanıcılar ise bir hizmeti dağıtmak için hangi azure bölgesi değerlendirmenize yardımcı olur:

```powershell
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Provider "Comcast Cable Communications, LLC - ASN 7922" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> Farklı olarak tek bir konum belirttiğinizde, bir konum belirtin veya "Batı ABD 2", "Batı ABD", gibi birden çok konumlarını belirtin yoksa, bir Internet hizmet sağlayıcısı komutu çalıştırırken belirtmeniz gerekir. 

## <a name="view-available"></a>Kullanılabilir ülke, eyalet, şehir ve sağlayıcıları görüntüleyin

Veriler, belirli Internet hizmet sağlayıcıları, ülke, eyalet ve şehir için kullanılabilir. Tüm kullanılabilir Internet hizmet sağlayıcıları, ülke, eyalet ve, verileri görüntüleyebilirsiniz, şehirlere listesini görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Veriler, yalnızca ülke, eyalet ve şehir önceki komutu tarafından döndürülen için kullanılabilir. Önceki komutta var olan bir Ağ İzleyicisi belirtmenizi gerektirir. Belirtilen örnek *NetworkWatcher_eastus* adlı bir kaynak grubundaki Ağ İzleyicisi *NetworkWatcherRG*, ancak herhangi bir mevcut Ağ İzleyicisi belirtebilirsiniz. Var olan bir Ağ İzleyicisi yoksa, oluşturun, görevleri tamamlama tarafından [Ağ İzleyicisi oluşturma](#create-a-network-watcher). 

Önceki komutu çalıştırdıktan sonra için geçerli değerleri belirtilerek döndürülen çıkışı filtreleyebilirsiniz **Ülke**, **durumu**, ve **Şehir**, isterseniz.  Örneğin, Amerika Birleşik Devletleri'nde Seattle, Washington'da kullanılabilir Internet hizmet sağlayıcıları listesini görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City Seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> Belirtilen değer **Ülke** büyük ve küçük olmalıdır. İçin belirtilen değerlere **durumu** ve **Şehir** küçük olmalıdır. Değerler için hiçbir değerlerle komutu çalıştırdıktan sonra döndürülen çıktının yer alması gerekir **Ülke**, **durumu**, ve **Şehir**. Size yanlış durum belirtin veya için bir değer belirtin **Ülke**, **durumu**, veya **Şehir** , değildir komut ile hiçbir değer bu çalıştırdıktan sonra döndürülen çıktının Özellikler, döndürülen çıkış boştur.
