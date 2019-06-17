---
title: include dosyası
description: include dosyası
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 2614c9290bf31813d59ee753a31622bccf0682b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114483"
---
| Resource | Hedef | Sabit sınırı |
|----------|--------------|------------|
| Bölge başına depolama Eşitleme Hizmetleri | 20 depolama Eşitleme Hizmetleri | Evet |
| Depolama eşitleme hizmeti başına eşitleme grupları | 100 eşitleme grupları | Evet |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucuları | Evet |
| Bulut uç noktaları her eşitleme grubu | 1 bulut uç noktası | Evet |
| Sunucu uç noktaları her eşitleme grubu | 50 sunucu uç noktaları | Hayır |
| Sunucu başına sunucu uç noktaları | 30 sunucu uç noktaları | Evet |
| Dosya sistemi nesneleri (dizinler ve dosyalar) her bir eşitleme grubu | 25 milyon nesneleri | Hayır |
| Dosya sistemi nesneleri (dizinleri ve dosyaları) bir dizinde en fazla sayısı | 1 milyon nesneleri | Evet |
| En büyük nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 64 KiB | Evet |
| Dosya boyutu | 100 GiB | Hayır |
| Katmanlanmış bir dosyanın en küçük dosya boyutu | 64 KiB | Evet |
| Eşzamanlı bir eşitleme oturumları | V4 aracı ve daha sonra: Kullanılabilir sistem kaynaklarına göre değişiklik gösterir. <BR> V3 aracı: İşlemci veya en fazla sunucu başına sekiz etkin eşitleme oturumu başına iki active sync oturum. | Evet

> [!Note]  
> Bir Azure dosya eşitleme uç noktası için bir Azure dosya paylaşımı boyutunu ölçeklendirebilirsiniz. Azure dosya paylaşımı boyutu sınırına ulaşıldığında, eşitleme çalışılacak mümkün olmayacaktır.