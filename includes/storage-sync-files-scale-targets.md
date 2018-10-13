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
ms.openlocfilehash: a29f1c4a625552dd958884c6a172bee470e61ca6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49312490"
---
| Kaynak | Hedef | Sabit sınırı |
|----------|--------------|------------|
| Abonelik başına depolama Eşitleme Hizmetleri | 15 depolama Eşitleme Hizmetleri | Hayır |
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
| Eşzamanlı bir eşitleme oturumları | her işlemci veya en fazla 8 active eşitleme oturumu sunucu başına 2 active sync oturumları | Evet |
