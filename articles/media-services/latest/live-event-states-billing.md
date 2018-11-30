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
ms.openlocfilehash: 1a49f62d7b5e21fe9d6483f71b729a9100aff1a3
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585582"
---
# <a name="liveevent-states-and-billing"></a>Livestream durumları ve faturalandırma

Azure Media Services'de, durumu hemen sonra Fatura bir Livestream başlar **çalıştıran**. Faturalandırma gelen Livestream durdurmak için Livestream durdurmak zorunda.

Zaman **LiveEventEncodingType** üzerinde [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) standart (Temel), Media Services'ın otomatik kapatır hala herhangi Livestream devre dışı olarak ayarlanmış **çalıştıran** 12 saat belirtin. sonra giriş akışı kaybolur ve başka hiçbir **LiveOutput**çalıştıran s. Ancak, yine de Livestream içinde saat için faturalandırılırsınız **çalıştıran** durumu.

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
