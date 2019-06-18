---
title: Azure Media Services canlı olay ve bulut DVR | Microsoft Docs
description: Bu makalede, Canlı çıkış nedir ve bulut DVR kullanmayı açıklar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/14/2019
ms.author: juliako
ms.openlocfilehash: 4dd14587ec7e1473953981c1ef8c32c59eb9a1d6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60322344"
---
# <a name="using-a-cloud-dvr"></a>Bulut DVR kullanma

A [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) giden canlı akış akışın ne kadar (örneğin, bulut DVR kapasitesini) kaydedilir ve canlı akış izleme görüntüleyiciler olup olmadığını başlayabilirsiniz gibi özellikleri denetlemenize olanak verir. Arasındaki ilişkiyi bir **canlı olay** ve kendi **Canlı çıkış**s benzer geleneksel televizyon yayına verebileceğiniz bir kanal (**canlı olay**) bir sabiti temsil eder Stream, video ve bir kayıt (**Canlı çıkış**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: Canlı olayları eşdeğer özelliği ArchiveWindowLength özelliği aracılığıyla yönetilir. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

## <a name="live-output"></a>Canlı çıkış

**ArchiveWindowLength** değeri belirleyen ne kadar arka planda bir Görüntüleyici geçerli Canlı konumdan arama.  **ArchiveWindowLength** ne kadar istemci uzayabileceğini de belirler.

Bir futbol oyun akış ve atanmış varsayalım bir **ArchiveWindowLength** yalnızca 30 dakika. Oyun başlatıldıktan sonra 45 dakika etkinliğinizi izlemeyi başlatan bir Görüntüleyici geri en fazla 15 dakikalık işareti arama. **Canlı çıkış**s oyun kadar devam edecek **canlı olay** durdurulmuş, ancak içerik, dışında kalan **ArchiveWindowLength** alanından sürekli olarak iptal edilir depolama ve kurtarılamaz. Bu örnekte, video olayının başlangıç ve 15 dakikalık işareti arasında DVR ve blob depolama kapsayıcısında temizlenmiş [varlık](https://docs.microsoft.com/rest/api/media/assets). Arşiv geri alınamaz ve Azure blob depolama kapsayıcısında kaldırılır.

Her **Canlı çıkış** ile ilişkili bir **varlık**, ilişkili Azure blob depolama kapsayıcısına video kaydetmek için kullanır. Canlı çıkış yayımlamak için oluşturmanız gereken bir **akış Bulucu** söz konusu **varlık**. Oluşturduktan sonra bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators), izleyicilerinizin için sağlayan bir akış URL'si oluşturabilirsiniz.

A **canlı olay** en fazla üç eşzamanlı olarak çalışan destekler **Canlı çıkış**bir canlı akış alan en fazla 3 kayıtları/arşivleri oluşturabilmesi için s. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Yayın için ihtiyacınız olduğunu varsayın 24 x 7 Canlı doğrusal akışı ve müşterilere oldukça görüntülemek için isteğe bağlı içerik olarak sunmak için gün boyunca farklı programların "kayıt" oluşturun. Bu senaryo için bir birincil Canlı çıkış, bir kısa arşiv penceresi 1 saat ile oluşturabilir veya daha az – bu izleyicilerinize içine ayarlamak birincil canlı akış. Oluşturmak bir **akış Bulucu** bu **Canlı çıkış** ve uygulama veya web sitesi "Live" akış olarak yayımlayın. Sırada **canlı olay** olan çalıştıran, ikinci bir eş zamanlı programlama yoluyla oluşturabilirsiniz **Canlı çıkış** bir program (veya bazı işler sağlamak için 5 dakika erken) içerecek şekilde kırpmanıza daha sonra başında. Bu ikinci **Canlı çıkış** program bittikten sonra 5 dakika silinebilir. Bu ikinci ile **varlık**, yeni bir oluşturabilirsiniz **akış Bulucu** uygulama Kataloğu'nda bir isteğe bağlı varlık olarak bu programı yayımlamak için. Bu işlemi yineleyebilirsiniz birden çok kez diğer program sınırları veya isteğe bağlı videolar paylaşmak istediğiniz öne çıkan özellikleri için tüm bunları yaparken "Live" akış ilk **Canlı çıkış** doğrusal akış yayını devam eder. 

> [!NOTE]
> **Çıkış canlı**s oluşturma başlatıp silindiğinde durdurabilir. Sildiğinizde **Canlı çıkış**, arka plandaki silmezsiniz **varlık** ve varlık içeriği. 
>
> Yayımladıysanız **Canlı çıkış** varlık kullanan bir **akış Bulucu**, **canlı olay** (DVR pencere uzunluğunun en fazla) kadar görüntülenebilirolmayadevamedecek**Akış Bulucu**'s süre sonu veya silme işlemi, hangisi gelir önce.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)

