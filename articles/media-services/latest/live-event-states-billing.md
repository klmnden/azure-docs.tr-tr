---
title: Livestream durumları ve Azure Media Services faturalandırma | Microsoft Docs
description: Bu konu Azure Media Services Livestream durumları ve faturalandırma hakkında genel bir bakış sağlar.
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
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: 588aeede123848900fac6fab663dd1f6c6c169b6
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53719430"
---
# <a name="liveevent-states-and-billing"></a>Livestream durumları ve faturalandırma

Azure Media Services'de, durumu hemen sonra Fatura bir Livestream başlar **çalıştıran**. Faturalandırma gelen Livestream durdurmak için Livestream durdurmak zorunda.

Zaman **LiveEventEncodingType** üzerinde [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) standart, Media Services'ın otomatik kapatır hala herhangi Livestream devre dışı olarak ayarlanmış **çalıştıran** 12 saat sonra belirtin. Giriş akışı kaybolur ve başka hiçbir **LiveOutput**çalıştıran s. Ancak, yine de Livestream içinde saat için faturalandırılırsınız **çalıştıran** durumu.

## <a name="states"></a>Durumlar

Livestream şu durumlardan birinde olabilir.

|Durum|Açıklama|
|---|---|
|**Durduruldu**| Bu ilk Livestream oluşturulduktan sonra durumudur (autostart sürece true.) Bu durumda hiçbir faturalandırma gerçekleşir. Bu durumda Livestream özellikleri güncelleştirilebilir ama akışa izin verilmiyor.|
|**Başlatma**| Livestream başlatılır ve kaynakları ayrılır. Bu durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez. Bir hata oluşursa Livestream durduruldu durumuna döndürülür.|
|**Çalıştıran**| Kaynakları tahsis edilmiş, alma ve önizleme URL'leri Livestream oluşturulan ve canlı akışlarınızdan alabildiğini. Bu noktada, faturalandırma etkin değil. Livestream kaynakta daha fazla faturalama durdurmak için Stop açıkça çağırmanız gerekir.|
|**Durduruluyor**| Livestream durduruldu ve kaynaklar serbest sağlandı. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez.|
|**Silme**| Livestream siliniyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez.|

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
