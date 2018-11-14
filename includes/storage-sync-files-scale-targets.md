---
title: include dosyası
description: include dosyası
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 03fe587ede297ac7dea90b7a5fb2d5323f60659e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628224"
---
| Kaynak | Hedef | Sabit sınırı |
|----------|--------------|------------|
| Abonelik başına depolama Eşitleme Hizmetleri | Bölge başına 15 depolama Eşitleme Hizmetleri | Hayır |
| Depolama eşitleme hizmeti başına eşitleme grupları | 100 eşitleme grupları | Evet |
| Depolama eşitleme hizmeti başına kayıtlı sunucular | 99 sunucuları | Evet |
| Bulut uç noktaları her eşitleme grubu | 1 bulut uç noktası | Evet |
| Sunucu uç noktaları her eşitleme grubu | 50 sunucu uç noktaları | Hayır |
| Sunucu başına sunucu uç noktaları | 33-99 sunucu uç noktaları | Evet, ancak (CPU, bellek, birimler, Dosya Değişim sıklığı, dosya sayısı, vs.) yapılandırmasına göre değişir |
| Uç nokta boyutu | 4 TiB | Hayır |
| Dosya sistemi nesneleri (dizinler ve dosyalar) her bir eşitleme grubu | 25 milyon nesneleri | Hayır |
| Dosya sistemi nesneleri (dizinleri ve dosyaları) bir dizinde en fazla sayısı | 200.000 nesneleri | Evet |
| En büyük nesne (dizinler ve dosyalar) adı uzunluğu | 255 karakter | Evet |
| En büyük nesne (dizinler ve dosyalar) güvenlik tanımlayıcısı boyutu | 4 KiB | Evet |
| Dosya boyutu | 100 giB | Hayır |
| Katmanlanmış bir dosyanın en küçük dosya boyutu | 64 KiB | Evet |
| Eşzamanlı bir eşitleme oturumları | V4 aracı: sınırı kullanılabilir sistem kaynaklarına göre değişir. <BR> V3 aracı: sunucu başına oturum sayısı en fazla 8 etkin veya işlemci başına 2 active sync oturum eşitleme | Evet
