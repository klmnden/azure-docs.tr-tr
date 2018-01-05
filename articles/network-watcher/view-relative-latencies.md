---
title: "Görüntüleme göreceli gecikmeleri Azure bölgeleri belirli konumlardan | Microsoft Docs"
description: "Göreceli gecikmeleri Internet sağlayıcıları Azure bölgeleri belirli konumlardan nasıl görüntüleyeceğinizi öğrenin."
services: network-watcher
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: a6c2ffa619eeff8b455df8a8b2157525af12c640
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="view-relative-latency-to-azure-regions-from-specific-locations"></a>Görünüm belirli konumlardan Azure bölgeleri için göreli gecikme

Bu öğreticide, Azure kullanmayı öğrenin [Ağ İzleyicisi](network-watcher-monitoring-overview.md) uygulamanızı dağıtmak veya hizmeti için hangi Azure bölgesi karar vermenize yardımcı olacak hizmet, kullanıcıya demografik bağlı. Ayrıca, Azure hizmet sağlayıcıları bağlantıları değerlendirmek için kullanabilirsiniz.  
        
## <a name="create-a-network-watcher"></a>Ağ İzleyicisi oluşturma

Zaten bir Ağ İzleyicisi en az bir Azure'da varsa [bölge](https://azure.microsoft.com/regions), bu bölümdeki görevler atlayabilirsiniz. Ağ İzleyici için bir kaynak grubu oluşturun. Bu örnekte, kaynak grubu Doğu ABD bölgesinde oluşturuldu, ancak herhangi bir Azure bölgede kaynak grubu oluşturabilirsiniz.

```powershell
New-AzureRmResourceGroup -Name NetworkWatcherRG -Location eastus
```

Ağ İzleyicisi oluşturun. En az bir Azure bölgede oluşturulan bir Ağ İzleyicisi olması gerekir. Bu örnekte, bir Ağ İzleyicisi'nin BİZE Doğu Azure bölgesinde oluşturulur.

```powershell
New-AzureRmNetworkWatcher -Name NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG -Location eastus
```

## <a name="compare-relative-network-latencies-to-a-single-azure-region-from-a-specific-location"></a>Göreli ağ gecikmeleri tek bir Azure bölgesine belirli bir konumdan karşılaştırın.

Hizmet sağlayıcıları değerlendirmek veya bir kullanıcı "site yavaş"gibi bir sorun raporlama sorun giderme hizmet dağıtıldığı azure bölgesine belirli bir konumdan. Örneğin, aşağıdaki komut, Washington eyaleti yasalarına Amerika Birleşik Devletleri'nde ve Batı ABD 2 Azure bölgesini aralık 13-15 2017 arasında arasındaki ortalama göreli Internet servis sağlayıcısı gecikmeleri döndürür:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Location "West US 2" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

> [!NOTE]
> Önceki komutta bölge Ağ İzleyicisi'ni alınırken belirttiğiniz bölge ile aynı olması gerekmez. Önceki komutu, yalnızca var olan bir Ağ İzleyicisi belirtmenizi gerektirir. Ağ İzleyicisi'ni herhangi bir bölgede olabilir. İçin değerler belirlerseniz `-Country` ve `-State`, geçerli olması gerekir. Değerleri büyük/küçük harfe duyarlıdır. Veriler, sınırlı sayıda ülkeler, durumları ve şehir için kullanılabilir. Komutları çalıştırmak [görünümü kullanılabilir ülke, durumları, şehirler ve sağlayıcıları](#view-available) kullanılabilir ülke, şehirler ve önceki komut ile kullanmak için durumlarını listesini görüntülemek için. 

> [!WARNING]
> 14 Kasım 2017 için sonra bir tarihi belirtmelidir `-StartTime` ve `-EndTime`. 14 Kasım 2017 önce bir tarihi belirterek hiçbir veri döndürür. 

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

Döndürülen çıkışında, değeri **puan** göreli gecikme bölgeler ve sağlayıcılar arasında değil. 100 en düşük gecikme iken bir puan 1 en kötü (yüksek) gecikme olur. Göreceli gecikmeleri güne ait ortalaması alınır. Önceki örnekte, sahip temizleyin sırada gecikmeleri aynı iki gün olan ve küçük bir fark iki sağlayıcı gecikme süresi arasında sahip de temizleyin, her iki sağlayıcıları için gecikme 1-100 ölçeğini düşük. Bazen Amerika Birleşik Devletleri'nde Washington eyaleti fiziksel olarak yakın Batı ABD 2 Azure bölgesi olduğundan bu, beklenen bir durumdur, ancak sonuçları beklendiği gibi değil. Büyük tarih aralığını belirtin, zaman içinde gecikme daha ortalama.

## <a name="compare-relative-network-latencies-across-azure-regions-from-a-specific-location"></a>Belirli bir konumdan Azure bölgeler arasında göreceli ağ gecikmeleri Karşılaştır

Belirli bir konuma ve belirli bir Azure bölgesine kullanarak arasında göreceli gecikmeleri belirtme yerine IF `-Location`göreceli gecikmeleri tüm Azure bölgeleri için belirli bir fiziksel konumundan belirlemek istediğinizi, bunu çok yapabilirsiniz. Örneğin, aşağıdaki komutu birincil kullanıcılarınızın Washington durumda bulunan Comcast kullanıcılar ise bir hizmeti dağıtmak için hangi azure bölgesi değerlendirmenize yardımcı olur:

```powershell
Get-AzureRmNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -Provider "Comcast Cable Communications, LLC - ASN 7922" `
  -Country "United States" `
  -State "washington" `
  -StartTime "2017-12-13" `
  -EndTime "2017-12-15"
```

>[!NOTE]
Farklı olarak tek bir konumu belirttiğinizde, bir konum belirtin veya "Batı US2", "Batı ABD", gibi birden çok konumda belirtirseniz yok, Internet servis sağlayıcısı komutu çalıştırırken belirtmeniz gerekir. 

## <a name="view-available"></a>Görünüm kullanılabilir ülke, durumları, şehirler ve sağlayıcıları

Veriler belirli Internet hizmet sağlayıcıları, Ülkeler, durumları ve şehir için kullanılabilir. Tüm kullanılabilir Internet servis sağlayıcılarının bir listesini görüntülemek için söz konusu ülke, durumları ve, verileri görüntüleyebilirsiniz şehirler, aşağıdaki komutu girin:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList -NetworkWatcherName NetworkWatcher_eastus -ResourceGroupName NetworkWatcherRG
```

Verileri yalnızca Ülkeler, durumları ve önceki komutu tarafından döndürülen şehir için kullanılabilir. Önceki komutu, var olan bir Ağ İzleyicisi belirtmenizi gerektirir. Belirtilen örnek *NetworkWatcher_eastus* adlı bir kaynak grubunda Ağ İzleyicisi *NetworkWatcherRG*, ancak tüm mevcut Ağ İzleyicisi'ni belirtebilirsiniz. Varolan bir Ağ İzleyicisi yoksa, görevleri tamamlayarak oluşturmak [bir Ağ İzleyicisi oluşturma](#create-a-network-watcher). 

Önceki komutu çalıştırdıktan sonra geçerli değerleri belirten tarafından döndürülen çıktının filtreleyebilirsiniz **Ülke**, **durumu**, ve **Şehir**istenirse.  Örneğin, Amerika Birleşik Devletleri'nde Seattle, Washington, kullanılabilir Internet hizmet sağlayıcıları listesini görüntülemek için aşağıdaki komutu girin:

```powershell
Get-AzureRmNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG `
  -City seattle `
  -Country "United States" `
  -State washington
```

> [!WARNING]
> Belirtilen değer **Ülke** büyük ve küçük olmalıdır. İçin belirtilen değerlere **durumu** ve **Şehir** küçük olmalıdır. Değerleri için hiçbir değerlerle komutu çalıştırdıktan sonra döndürülen çıktının listelenmiş olmalıdır **Ülke**, **durumu**, ve **Şehir**. Yanlış durum belirttiğinizde, veya için bir değer belirtin, **Ülke**, **durumu**, veya **Şehir** olmayan komutu ile hiçbir değer bu çalıştırdıktan sonra döndürülen çıkışı Özellikler, döndürülen çıkış boştur.
