---
title: "Azure portalında bir depolama hesabı için üretilen iş ve gecikmeyi ölçümleri doğrulayın | Microsoft Docs"
description: "Bir depolama hesabı portalında üretilen iş ve gecikmeyi ölçümlerini doğrulayın öğrenin."
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: b3102bd4e40e10fe88c12295794da37e359c56f1
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="verify-throughput-and-latency-metrics-for-a-storage-account"></a>Bir depolama hesabı için üretilen iş ve gecikmeyi ölçümleri doğrulayın

Bu öğretici dört ve bir dizi son bölümü parçasıdır. Önceki eğitimlerine karşıya yükleme ve bir Azure depolama hesabı larges miktarda rastgele veri indirme öğrendiniz. Bu öğretici Azure portalında üretilen iş ve gecikmeyi görüntülemek için ölçümleri nasıl kullanabileceğinizi gösterir.

Bölümünde dizisinin dört öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Azure portalında grafikleri yapılandırın
> * Aktarım hızı ve gecikme süresi ölçümlerini doğrulama

[Azure storage ölçümleri](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Azure İzleyicisi, performans ve kullanılabilirlik depolama hesabınızın birleştirilmiş görünüme sağlamak için kullanır.

## <a name="configure-metrics"></a>Ölçümlerini yapılandırın

Gidin **ölçümleri (Önizleme)** altında **ayarları** depolama hesabınızdaki.

BLOB üzerinden seçin **alt hizmet** açılır.

Altında **ÖLÇÜM**, aşağıdaki tabloda bulunan ölçümleri birini seçin:

Aşağıdaki ölçümleri gecikme süresi ve verimlilik uygulamanın hakkında bir fikir verir. Portalda yapılandırma 1 dakikalık ortalamalar ölçümleridir. Dakika verileri halfed ortalama için ise bir dakika ortasında bir işlem tamamlandı. Uygulama yükleme ve indirme işlemleri zaman aşımına ve gerçek süre miktarı çıktı sağlanan dosyaları yükleme ve indirme sürdü. Bu bilgiler, portal ölçümleri birlikte işleme tam olarak anlamak için kullanılabilir.

|Ölçüm|Tanım|
|---|---|
|**Başarı E2E gecikme süresi**|Bir depolama birimi hizmeti veya belirtilen API işlemi yapılan başarılı istekleri ortalama uçtan uca gecikme. Bu değer, istek okuma, bir yanıt gönderir ve yanıtın bildirim almak için Azure Storage içinde gerekli işleme süresini içerir.|
|**Başarı sunucu gecikme süresi**|Başarılı bir isteği işlemek için Azure Storage tarafından kullanılan ortalama süre. Bu değer SuccessE2ELatency içinde belirtilen ağ gecikme süresi dahil değildir. |
|**İşlemler**|Depolama hizmet ya da belirtilen API işlemi için yapılan isteklerin sayısı. Bu sayı, hataları üretilen istekleri yanı sıra başarılı ve başarısız istekleri içerir. Örnekte, blok boyutu 100 MB olarak ayarlandı. Bu durumda, her 100 MB bloğu bir işlem olarak kabul edilir.|
|**Giriş**|Giriş verileri miktarını. Bu sayı, Azure içinde giriş yanı sıra Azure Storage içine bir dış istemcinin giriş içerir. |
|**Çıkış**|Çıkış veri miktarı. Bu sayı, çıkış Azure içinde yanı sıra Azure Storage içine dış bir istemciden çıkış içerir. Sonuç olarak, bu sayı Faturalanabilir çıkış yansıtmaz. |

Seçin **son 24 saat (otomatik)** yanına **zaman**. Seçin **son bir saat** ve **Minute** için **zaman ayrıntı düzeyi**, ardından **Uygula**.

![Depolama hesabı ölçümleri](./media/storage-blob-scalable-app-verify-metrics/figure1.png)

Grafikler atanmış birden fazla ölçüm olabilir, ancak Grup yeteneği boyutlara göre bir doğrudan birden fazla ölçüm atama devre dışı bırakır.

## <a name="dimensions"></a>Boyutlar

[Boyutlar](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#metrics-dimensions) grafikler daha derin arayın ve daha ayrıntılı bilgi almak için kullanılır. Farklı ölçümleri farklı boyutlarda. Kullanılabilir bir boyuttur **API adı** boyut. Bu boyut her ayrı API çağrısı şemasına çıkışı keser. Aşağıdaki ilk görüntü toplam işlemleri bir depolama hesabı için bir örnek grafiğini gösterir. İkinci görüntü aynı grafik ancak API ile seçilen boyutu adı gösterir. Gördüğünüz gibi her bir işlem daha fazla ayrıntı kaç çağrıları API adı tarafından yapılan içine vermiş listelenir.

![Depolama hesabı ölçümleri - işlemler boyut olmadan](./media/storage-blob-scalable-app-verify-metrics/transactionsnodimensions.png)

![Depolama hesabı ölçümleri - işlemleri](./media/storage-blob-scalable-app-verify-metrics/transactions.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silin. Bunu yapmak için VM için kaynak grubu seçin ve Sil'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde dizisinin dört nasıl gibi örnek çözümü için ölçümleri görüntüleme hakkında öğrenilen:

> [!div class="checklist"]
> * Azure portalında grafikleri yapılandırın
> * Aktarım hızı ve gecikme süresi ölçümlerini doğrulama

Önceden oluşturulmuş depolama örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure depolama kod örnekleri](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-blob-scalable-app-download-files.md