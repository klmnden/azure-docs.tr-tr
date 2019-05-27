---
title: include dosyası
description: include dosyası
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 33dff710d83bd12a8db343a89c6e4576d1397ba7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114559"
---
Hayır: bir sunucu uç noktasını kaldırarak bir sunucunun yeniden başlatılması gibi değil! Neredeyse hiçbir zaman kaldırarak ve sunucu uç noktasını yeniden eşitleme, bulut katmanlandırma veya diğer yönleri Azure dosya eşitleme ile sorunları çözme için uygun bir çözüm olan. Sunucu uç noktası kaldırma zararlı olduğunu ve sunucu uç noktası ad dışında dosyaları katmanlı durumunda veri kaybına neden bulunabilir (bkz [neden katmanlı dosyalar mevcut sunucu uç noktası ad dışında](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) için Daha fazla bilgi için) veya sunucu uç noktası ad içinde var olan katmanlı dosyalar için erişilemeyen dosyalar. Bu sorunlar, ne zaman sunucu uç noktasını yeniden çözümlemez. Hiçbir zaman bulut katmanlaması etkin olsa bile, katmanlı dosya sunucusu uç nokta ad alanı içinde bulunabilir. Bu nedenle, Azure dosya eşitleme ile bu belirli klasör kullanmayı veya açıkça Bunu yapmak için bir Microsoft mühendisi tarafından söylenen istediğiniz sürece sunucu uç noktasını kaldırmayın öneririz. Remove sunucu uç noktaları hakkında daha fazla bilgi için bkz. [sunucu uç noktası Kaldır](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    