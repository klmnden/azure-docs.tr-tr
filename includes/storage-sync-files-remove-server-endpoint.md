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
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738692"
---
Hayır: sunucusu uç noktası kaldırma bir sunucunun yeniden başlatılmasını gibi değil! Neredeyse hiçbir zaman kaldırarak ve sunucu uç noktasını yeniden eşitleme, bulut katmanlandırma veya diğer yönlerini Azure dosya eşitleme sorunlarını düzeltmek için uygun bir çözüm olan. Sunucusu uç noktası kaldırma olduğu zararlı ve sunucu uç nokta ad alanı dışında dosyaları katmanlı durumunda veri kaybına neden olabilir (bakın [neden katmanlı dosyaların mevcut sunucu uç nokta ad alanı dışında](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) için Daha fazla bilgi) veya sunucu uç nokta ad alanı içinde var olan katmanlı dosyaları için erişilemeyen dosyalar. Bu sorunları sunucusu uç noktası ne zaman yeniden çözümlemez. Bulut etkin katmanlama yükletmemiştiniz olsa bile katmanlı dosyalar, sunucu uç nokta ad alanı içinde bulunabilir. Bu nedenle, bu belirli klasör ile Azure dosya eşitleme kullanmayı veya açıkça Bunu yapmak için bir Microsoft mühendisi tarafından istenen istediğiniz sürece sunucusu uç noktası kaldırmayın öneririz. Kaldır sunucu uç noktaları hakkında daha fazla bilgi için bkz: [sunucusu uç noktası kaldırmak](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    