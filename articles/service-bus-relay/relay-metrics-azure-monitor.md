---
title: Azure İzleyici (Önizleme) Azure geçişi ölçümlerde | Microsoft Docs
description: Azure geçişi izlemek için Azure izleme kullanın
services: service-bus-relay
documentationcenter: .NET
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: spelluru
ms.openlocfilehash: bd62624406adb006fdcd7d59f72db3fb5e1848a0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798035"
---
# <a name="azure-relay-metrics-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme), Azure geçişi ölçümleri
Azure geçişi ölçümleri, Azure aboneliğinizdeki kaynakları durumunu sağlar. Zengin ölçüm verileri ile genel Relay kaynakları, yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde durumunu değerlendirebilirsiniz. Bu istatistikler Azure geçişi durumunu izlemek için yardımcı önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [almak Azure İzleyici ölçümleri .NET ile](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek.

> [!IMPORTANT]
> Bu makale yalnızca WCF geçişi için Azure geçişi karma bağlantılar özelliği için geçerlidir. 

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ya da erişim ölçümleri ile yapabilecekleriniz [Azure portalında](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve Operations Management Suite ve Event Hubs gibi analiz çözümleri kullanın. Daha fazla bilgi için [İzleme verilerine Azure İzleyicisi tarafından toplanan](../azure-monitor/platform/data-platform.md).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu yapılandırılan [tanılama ayarları](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) Azure İzleyici'de.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

Zaman içinde ölçümleri izleyebilirsiniz [Azure portalında](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanını seçin ve ardından **ölçümler (Önizleme)**. 

Boyutlar destekleyen ölçümler için istenen boyut değeri ile filtrelemesi gerekir.

## <a name="billing"></a>Faturalandırma

Ölçümleri kullanarak Azure İzleyici'de önizleme şu an ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Ölçüm verilerini Gelişmiş analiz için Azure İzleyici günlüklerine akış sahipse Azure İzleyici günlüklerine göre ayrıca faturalandırılır.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Azure geçişi ölçümler için desteklenen zaman aralığı 1 dakikadır.

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Microsoft.Relay başarılı (Önizleme) | Azure geçişi için belirtilen bir süredeki yapılan başarılı bir dinleyici bağlantı sayısı. <br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay-Senderconnections'da (Önizleme)|Belirli bir dönem boyunca dinleyici bağlantılarında istemci hataları sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay-ServerError (Önizleme)|Belirli bir dönem boyunca dinleyici bağlantılarda sunucu hataları sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections başarılı (Önizleme)|Belirtilen bir zaman dilimi içerisinde yapılan başarılı gönderen bağlantılarının sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-Senderconnections'da (Önizleme)|Belirli bir dönem boyunca gönderen bağlantılarında istemci hataları sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-ServerError (Önizleme)|Gönderen bağlantısı belirli bir süre içinde sunucu hataları sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay-TotalRequests (Önizleme)|Dinleyici bağlantı belirli bir dönem boyunca toplam sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-TotalRequests (Önizleme)|Belirtilen bir süredeki Gönderenler tarafından yapılan bağlantı istekleri.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|ActiveConnections (Önizleme)|Belirtilen bir süre boyunca etkin bağlantı sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|ActiveListeners (Önizleme)|Belirtilen bir süre boyunca etkin dinleyiciler sayısı.<br/><br/> Birim: Sayı <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|ListenerDisconnects (Önizleme)|Belirli bir dönem boyunca bağlantısı kesilmiş dinleyicileri sayısı.<br/><br/> Birim: Bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|SenderDisconnects (Önizleme)|Belirli bir dönem boyunca bağlantısı kesilmiş Gönderenler sayısı.<br/><br/> Birim: Bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="memory-usage-metrics"></a>Bellek kullanım ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|BytesTransferred (Önizleme)|Belirtilen bir süredeki aktarılan bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure geçişi, Azure İzleyicisi'nde ölçümler için aşağıdaki boyutlarını destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Azure geçişi, Mesajlaşma varlıkları ad alanı altında destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




