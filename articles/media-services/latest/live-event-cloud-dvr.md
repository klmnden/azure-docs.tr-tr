---
title: Azure Media Services Livestream ve bulut DVR | Microsoft Docs
description: Bu makalede LiveOutput nedir ve bulut DVR kullanmayı açıklar.
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
ms.date: 11/28/2018
ms.author: juliako
ms.openlocfilehash: 5de7496d73ebe1c89ce27ef27df73b197f34e7c7
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642294"
---
# <a name="using-a-cloud-dvr"></a>Bir bulut DVR kullanma

A [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) giden canlı akış akışın ne kadar (örneğin, bulut DVR kapasitesini) kaydedilir ve canlı akış izleme görüntüleyiciler olup olmadığını başlayabilirsiniz gibi özellikleri denetlemenize olanak verir. Arasındaki ilişkiyi bir **Livestream** ve kendi **LiveOutput**s ilişki benzer geleneksel televizyon yayına verebileceğiniz bir kanal (**Livestream**) video ve bir kayıt sabit akışını temsil eder (**LiveOutput**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: LiveEvents eşdeğer özelliği ArchiveWindowLength özelliği aracılığıyla yönetilir. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

## <a name="liveoutput"></a>LiveOutput

**ArchiveWindowLength** değeri belirleyen ne kadar arka planda bir Görüntüleyici geçerli Canlı konumdan arama.  **ArchiveWindowLength** ne kadar istemci uzayabileceğini de belirler.

Bir futbol oyun akış ve atanmış varsayalım bir **ArchiveWindowLength** yalnızca 30 dakika. Oyun başlatıldıktan sonra 45 dakika etkinliğinizi izlemeyi başlatan bir Görüntüleyici geri en fazla 15 dakikalık işareti arama. **LiveOutput**s oyun kadar devam edecek **Livestream** durdurulur, ancak içerik, dışında kalan **ArchiveWindowLength** alanından sürekli olarak iptal edilir depolama ve kurtarılamaz. Bu örnekte, video olayının başlangıç ve 15 dakikalık işareti arasında DVR ve blob depolama kapsayıcısında temizlenmiş [varlık](https://docs.microsoft.com/rest/api/media/assets). Arşiv geri alınamaz ve Azure blob depolama kapsayıcısında kaldırılır.

Her **LiveOutput** ile ilişkili bir **varlık**, ilişkili Azure blob depolama kapsayıcısına video kaydetmek için kullanır. LiveOutput yayımlamak için oluşturmanız gereken bir **StreamingLocator** söz konusu **varlık**. Oluşturduktan sonra bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), izleyicilerinizin için sağlayan bir akış URL'si oluşturabilirsiniz.

A **Livestream** en fazla üç eşzamanlı olarak çalışan destekler **LiveOutput**bir canlı akış alan en fazla 3 kayıtları/arşivleri oluşturabilmesi için s. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Yayın için ihtiyacınız olduğunu varsayın 24 x 7 Canlı doğrusal akışı ve müşterilere oldukça görüntülemek için isteğe bağlı içerik olarak sunmak için gün boyunca farklı programların "kayıt" oluşturun. Bu senaryo için bir kısa arşiv penceresi 1 saat önce birincil bir LiveOutput oluşturun veya bu izleyicilerinize içine ayarlamak birincil canlı akış, daha az –. Oluşturmak bir **StreamingLocator** bu **LiveOutput** ve uygulama veya web sitesi "Live" akış olarak yayımlayın. Sırada **Livestream** olan çalıştıran, ikinci bir eş zamanlı programlama yoluyla oluşturabilirsiniz **LiveOutput** bir program (veya bazı işler sağlamak için 5 dakika erken) içerecek şekilde kırpmanıza daha sonra başında. Bu ikinci **LiveOutput** program bittikten sonra 5 dakika silinebilir. Bu ikinci ile **varlık**, yeni bir oluşturabilirsiniz **StreamingLocator** uygulama Kataloğu'nda bir isteğe bağlı varlık olarak bu programı yayımlamak için. Bu işlemi yineleyebilirsiniz birden çok kez diğer program sınırları veya isteğe bağlı videolar paylaşmak istediğiniz öne çıkan özellikleri için tüm bunları yaparken "Live" akış ilk **LiveOutput** doğrusal akış yayını devam eder. 

> [!NOTE]
> **LiveOutput**s oluşturma başlatıp silindiğinde durdurabilir. Sildiğinizde **LiveOutput**, arka plandaki silmezsiniz **varlık** ve varlık içeriği.  

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)

