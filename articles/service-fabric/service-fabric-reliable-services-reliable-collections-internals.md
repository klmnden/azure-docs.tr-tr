---
title: "Azure Service Fabric güvenilir durumu Yöneticisi ve güvenilir koleksiyonu iç | Microsoft Docs"
description: "Derin Dalış güvenilir koleksiyonu kavramları ve Azure Service Fabric tasarımında."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Azure Service Fabric güvenilir durumu Yöneticisi ve güvenilir koleksiyonu dahili bileşenleri
Bu belge, güvenilir durum Yöneticisi ve güvenilir koleksiyonları çekirdek bileşenleri arka planda nasıl çalıştığını görmek için içinde ilgili alır.

> [!NOTE]
> Bu belge iş sürüyor değil. Daha fazla bilgi edinmek istediğiniz hangi konu bize iletmek için bu makalede yorumlar ekleyin.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Yerel Kalıcılık modeli: günlük ve denetim noktası
Durum Yöneticisi'ni güvenilir ve güvenilir koleksiyonları günlük ve denetim noktası adlı Kalıcılık modeli izleyin.
Bu modelde, her bir durum değişikliği diskte önce oturum açmış ve bellekte uygulanır.
Tam durum nadiren (paketini kalıcı Denetim noktası).
Farkları sıralı yalnızca Ekle yazma diskteki Gelişmiş performans için içine bırakılacağı avantajdır.

Günlük ve denetim noktası modeli daha iyi anlamak için ilk sonsuz disk senaryosu bakalım.
Çoğaltılana önce güvenilir durum Yöneticisi her işlemi günlüğe kaydeder.
Günlüğe kaydetme güvenilir yalnızca bellekte işlemi uygulamak koleksiyonları sağlar.
Çoğaltma başarısız olur ve yeniden başlatılması gerekiyor bile günlükleri, kalıcı olduğundan, güvenilir durum Yöneticisi çoğaltma kaybetti tüm işlemleri yürütme kendi günlüğünde yeterli bilgiye sahip.
Disk sonsuz olduğu gibi günlük kayıtlarını hiçbir zaman kaldırılmaları gerekir ve yalnızca bellek içi durumunu yönetmek güvenilir koleksiyonu gerekir.

Şimdi sınırlı disk senaryosu bakalım.
Günlük kayıtlarını birleştirdiğinizde güvenilir durum yöneticisinin disk alanı yetersiz çalıştırın.
Bu durum önce güvenilir durum Yöneticisi'ni yeni kayıtlar için yer açmak için baytta bir kesecek gerekiyor.
Güvenilir durum Yöneticisi'ni kontrol noktası güvenilir koleksiyonlarına bellek içi durumlarına disk ister.
Bu noktada, güvenilir koleksiyonları bellek içi durumuna kalıcı.
Güvenilir koleksiyonları bunların denetim noktaları tamamlandığında, güvenilir durum Yöneticisi disk alanını boşaltmak için günlük kısaltabilir.
Çoğaltma yeniden başlatılması gerektiğinde, güvenilir koleksiyonları belirttiğinizde durumlarına geri yükleyin ve güvenilir durum Yöneticisi kurtarır ve en son kontrol bu yana gerçekleşen tüm durum değişikliklerini geri kazanır.

Başka bir değer denetim noktası oluşturma ekleme olan yaygın senaryolar kurtarma sürelerini artırır. Günlüğü, son denetim noktasının bu yana gerçekleşen tüm işlemleri içerir.
Bu nedenle, güvenilir sözlükte belirli bir satır için birden çok değer gibi bir öğede birden fazla sürümünü içerebilir.
Buna karşılık, güvenilir koleksiyonu kontrol noktaları her bir anahtar değeri yalnızca en son sürümü.

## <a name="next-steps"></a>Sonraki adımlar
* [İşlemler ve kilitleri](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

