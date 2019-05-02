---
title: Azure medya ayrılmış birimi - ölçeklendirmek için CLI kullanma | Microsoft Docs
description: Bu konuda, ölçeklendirme medya Azure Media Services ile işleme için CLI kullanma gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6e7b3b316a8a6dcde95bdf872dbda4cd1372f072
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721816"
---
# <a name="scaling-media-processing"></a>Medya işlemeyi ölçeklendirme

Azure Media Services ile sunulan Medya Ayrılmış Birimlerini (MRU) yöneterek hesabınızdaki medya işleme kapasitesini ölçeklendirebilirsiniz. MRU ile medya işleme görevlerinizin işlenme hızını belirler. Şu ayrılmış birim türlerinden seçebilirsiniz: **S1**, **S2**, veya **S3**. Örneğin, aynı kodlama işi **S2** ayrılmış birim türünü kullandığınızda **S1** türüne göre daha hızlı çalışır. 

Ayrılmış birim türünü belirtmenin yanı sıra, ayrılmış birim ile hesabınızı sağlamak için belirtebilirsiniz. Sağlanan ayrılmış birim sayısı, verili bir hesapta eşzamanlı olarak işlenebilecek medya görevlerinin sayısını belirler. Örneğin, beş medya görevi aynı anda uzun çalışacağı beş ayrılmış birim, hesabınız varsa, olarak işlenmek üzere görevleri vardır. Kalan görevlerin kuyrukta bekler ve sıralı olarak çalışan bir görev tamamlandığında işlemek için toplanmış. Sağlanan herhangi bir ayrılmış birim hesabınız yoksa, ardından görevleri sıralı olarak seçilir. Bu durumda, bir görev tamamlama ve ileri bir başlangıç arasındaki bekleme süresini sistemde kaynaklarının kullanılabilirliğine bağlıdır.

## <a name="choosing-between-different-reserved-unit-types"></a>Farklı ayrılmış birim türlerinden seçme

Aşağıdaki tabloda farklı kodlama hızlarını arasında seçim yaparken bir karar vermenize yardımcı olur. Şirket birkaç Kıyaslama durum ayrıca sağlar [indirebileceğiniz bir video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) kendi testleri gerçekleştirmek için:

|RU türü|Senaryo|Örnek sonuçlarını [video 7 dk 1080 p](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)|
|---|---|---|
| **S1**|Tek bit hızlı kodlama. <br/>SD veya çözümleri altındaki dosyaları, duyarlı, düşük maliyetli değildir zaman.|Tekli bit hızı SD çözümleme MP4 dosyasını "H264 Çoklu bit hızı SD 16 x 9" kullanarak kodlama yaklaşık 7 dakika sürer.|
| **S2**|Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>SD hem HD kodlaması için normal kullanım.|"H264 tekli bit hızı ile 720 p" kodlama yaklaşık 6 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 720p" yaklaşık 12 dakika sürer hazır.|
| **S3**|Tekli bit hızı ve Çoklu bit hızlı kodlama.<br/>Tam HD ve 4K çözünürlüklü videolar. Kodlama duyarlı, daha hızlı bir döngü süresi.|"H264 tekli bit hızı ile 1080 p" kodlama yaklaşık 3 dakika sürer hazır.<br/><br/>Kodlama ile "H264 Çoklu bit hızı 1080p" yaklaşık 8 dakika sürer hazır.|

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Media Services v3 veya Video Indexer tarafından tetiklenen ses analizi ve Video analizi işleri, S3 birimi türü önemle tavsiye edilir.
* Paylaşılan havuzu kullanıyorsanız, diğer bir deyişle, tüm ayrılmış birim daha sonra kodla görevlerinizi aynı performans adet S1 RU olarak sahip. Ancak, kuyruğa alınmış durumda görevlerinizi ayırmasına zamana üst sınır yoktur ve herhangi bir zamanda yalnızca en fazla bir görev çalışacağı.

Bu makalenin geri kalanında nasıl kullanılacağını gösterir [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) MRU ölçeklendirebilirsiniz.

> [!NOTE]
> Media Services v3 veya Video Indexer ile tetiklenen Ses Analizi ve Video Analizi İşleri için hesabınıza 10 S3 MRU sağlamanız önerilir. 10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).
>
> Şu anda diğer v3 kaynakları yönetmek için Azure portalını kullanamazsınız. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](developers-guide.md) birini kullanın.

## <a name="prerequisites"></a>Önkoşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Ölçek medya ayrılmış birimleri CLI ile

`mru` komutunu çalıştırın.

Aşağıdaki [az ams hesabınızı mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) komut, medya ayrılmış birimi "amsaccount" hesabını kullanarak kümeleri **sayısı** ve **türü** parametreleri.

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Faturalandırma

Medya ayrılmış birimi sağlanan dakika sayısına, hesabınızdaki üzerinden ücretlendirilirsiniz. Bu bağımsız olarak mı ortaya çıkar hesabınızda çalışan herhangi bir işi vardır. Ayrıntılı bir açıklaması için SSS bölümüne bakın. [Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) sayfası.   

## <a name="next-step"></a>Sonraki adım

[Videoları analiz etme](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Ayrıca bkz.

* [Kotalar ve sınırlamalar](limits-quotas-constraints.md)
* [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
