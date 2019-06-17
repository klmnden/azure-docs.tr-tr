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
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 2907b5be7f8d5fda3d510484179e80b065ab64b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074886"
---
# <a name="live-event-states-and-billing"></a>Canlı olay durumları ve faturalandırma

Azure Media Services canlı bir olay durumuna geçer hemen sonra Fatura başlar **çalıştıran**. Faturalandırma canlı olay durdurmak için Canlı etkinliği durdurmak zorunda.

Zaman **LiveEventEncodingType** üzerinde [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) standart veya Premium1080p, Media Services'ın otomatik kapatıldığında tüm Canlı hala olay devre dışı olarak ayarlanmış **çalıştıran** durum 12 saat sonra giriş akışı kaybolur ve başka hiçbir **Canlı çıkış**çalıştıran s. Ancak, yine de canlı olay, saat için faturalandırılırsınız **çalıştıran** durumu.

## <a name="states"></a>Durumlar

Canlı olay şu durumlardan birinde olabilir.

|Eyalet|Açıklama|
|---|---|
|**Durduruldu**| Bu ilk canlı olay oluşturulduktan sonra durumudur (autostart sürece true.) Bu durumda hiçbir faturalandırma gerçekleşir. Bu durumda, canlı olay özellikleri güncelleştirilebilir ama akışa izin verilmiyor.|
|**Başlatma**| Canlı olay başlatılır ve kaynakları ayrılır. Bu durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez. Canlı olay, bir hata oluşursa, durduruldu durumuna döndürülür.|
|**Çalıştıran**| Canlı kaynakları tahsis edilmiş, alma ve önizleme URL'leri olayı oluşturan ve canlı akışlarınızdan alabildiğini. Bu noktada, faturalandırma etkin değil. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir.|
|**Durduruluyor**| Canlı olay durduruldu ve kaynaklar serbest sağlandı. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez.|
|**Silme**| Canlı olay siliniyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Güncelleştirmeleri veya akış, bu durum süresince verilmez.|

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
