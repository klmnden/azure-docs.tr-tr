---
title: Dağıtım sütunları – hiper ölçekli (Citus) (Önizleme) PostgreSQL için Azure veritabanı'nda seçin
description: Dağıtım sütunları hiper ölçekli senaryoları için iyi seçimler
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e9fba14b8979f739fd29bc277e32fb544221d08a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65078993"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Dağıtım sütunları – hiper ölçekli (Citus) (Önizleme) PostgreSQL için Azure veritabanı'nda seçin

Her tablonun dağıtım sütunu seçilmesi olan **en önemli birini** kararları modelleme. Hiper ölçekli satırları parçalardaki satırları dağıtım sütununun değerine göre depolar.

Doğru seçim grupları verileri yapmadan aynı fiziksel düğümlerde birlikte tüm SQL özellikleri için hızlı ve ekleme desteği sorgular ilgili. Sistem yavaş çalışır ve tüm SQL özellikleri düğümlere desteklemeyeceği bir yanlış seçim yapar.

Bu bölümde dağıtım iki en yaygın hiper ölçekli senaryolar için sütun ipuçları sağlar.

### <a name="multi-tenant-apps"></a>Çok Kiracılı uygulamalar

Çok kiracılı mimari hiyerarşik veritabanı sorguları sunucu grubundaki düğümleri dağıtmak üzere modelleme biçimi kullanır.  Verileri hiyerarşisinin üstünde olarak da bilinen *Kiracı kimliği*ve her bir tabloda bir sütundaki depolanmış olması gerekir.

Hiper ölçekli sorgular içeren ve eşleşen tablo parça bulur hangi Kiracı kimliği olup olmadığını denetler. Bu sorgu parça içeren bir tek çalışan düğümüne yönlendirir. Çalışan bir sorgu ile aynı düğümde yerleştirilen ilgili tüm verileri birlikte bulundurma çağrılır.

Aşağıdaki diyagram, birlikte bulundurma çok kiracılı veri modelindeki gösterir. İki tablo, hesapları ve kampanyalar içerir ve her tarafından dağıtılmış `account_id`. Gölgeli kutular, parçaların hangi çalışan düğümü içerdiği her rengi temsil eder, temsil eder. Yeşil parçalar, bir çalışan düğümü ve diğerinde mavi birlikte depolanır. Nasıl bir birleştirme sorgusu Kampanyalar hesapları arasındaki gerekli tüm verilerin birlikte bir düğümde her iki tablonun hesabın aynısını kısıtlarken gerekir fark\_kimliği.

![çok kiracılı birlikte bulundurma](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Bu tasarımda kendi şemanızı uygulamak için uygulamanızdaki bir kiracı nelerden belirleyin. Şirket, hesap, kuruluş veya müşteri ortak örnekler içerir. Sütun adı gibi bir şey olacaktır `company_id` veya `customer_id`. Her sorgularınızın inceleyin ve kendinize sorun: satırları aynı Kiracı kimliği ile ilgili tüm tabloları kısıtlamak için ek WHERE yan tümcelerini olsaydı işe yarar?
Bir kiracı için çok kiracılı model sorguları kapsayan, satış veya envanter sorguları içinde belirli bir depolama örneği için kapsamlı.

#### <a name="best-practices"></a>En İyi Uygulamalar

-   **Bölüm dağıtılmış tablolar genel Kiracı tarafından\_kimlik sütunu.** Örneğin, kiracılar, şirketler, Kiracı olduğu bir SaaS uygulamasında\_kimliği büyük olasılıkla şirket olması\_kimliği.
-   **Küçük kiracılar arası tablolar için başvuru tabloları dönüştürün.** Birden fazla Kiracı bilgileri küçük bir tabloya paylaştığınızda, bir başvuru tablosu dağıtın.
-   **Tüm uygulama sorgular filtresi Kiracı tarafından kısıtlamak\_kimliği.** Her sorgu, aynı anda tek bir kiracı için bilgi istemeniz gerekir.

Okuma [çok kiracılı öğretici](./tutorial-design-database-hyperscale-multi-tenant.md) örneği, bu tür bir uygulama oluşturma.

### <a name="real-time-apps"></a>Gerçek zamanlı uygulamalar

Çok kiracılı mimari, hiyerarşik bir yapısı tanıtır ve verileri birlikte bulundurma Kiracı başına sorgularını yönlendirmek üzere kullanır. Bunun aksine, gerçek zamanlı mimarileri, verilerin yüksek düzeyde paralel işleme elde etmek için belirli dağıtım özelliklerine bağlıdır.

Gerçek zamanlı modelinde dağıtım sütunları için bir terim olarak "varlık kimliği" kullanın. Kullanıcılar, konak veya cihaz bunun normal varlıklardır.

Tarih veya kategoriye göre gruplandırılmış sayısal Toplamalar için gerçek zamanlı sorgular genellikle isteyin. Hiper ölçekli, bu sorgular kısmi sonuçlar için her parça gönderir ve son yanıt Düzenleyici düğümde birleştirir. Sorguların hızlı mümkün olduğu kadar düğümü katkıda bulunuyorsanız ve hiçbir tek düğüm orantısız miktarda iş yapmalısınız çalıştırdığınızda.

#### <a name="best-practices"></a>En İyi Uygulamalar

-   **Dağıtım sütunu olarak yüksek ayarına sahip bir sütun seçin.** Karşılaştırma için bir \"durumu\" alan değerleri "Yeni" olan bir sipariş tabloda "Ücretli" ve "sevk" olduğunda dağıtım sütununun zayıf bir seçimdir. Bu, yalnızca birkaç bu değerler, veri tutabilen bir parça sayısı ve bu işlem düğüm sayısını sınırlar varsayar. Yüksek bir kardinalite ile sütunlar ek olarak, sık kullanılan group by yan tümcesi veya join anahtarlar olarak seçmek uygundur.
-   **Eşit dağıtım ile bir sütun seçin.** Tablo bazı yaygın değerlere dengesiz bir sütun üzerinde dağıtırsanız, veri tablosundaki belirli parçalarda accumulate eğilimindedir. Bu parçalardaki bulunduran düğümlerin diğerlerine göre daha fazla çalışmayı yapan sona erecek.
-   **Ortak sütunlarından olgu ve boyut tabloları dağıtın.**
    Yalnızca bir dağıtım anahtarı olgu tablonuz olabilir. Başka bir anahtar birleştirme tabloları olgu tablosu ile birlikte bulunması zorunlu değildir. Ne sıklıkla birleştirilir ve katılma satırların boyutuna bağlı olarak birlikte bulundurmanıza olanak bir boyut seçin.
-   **Bazı boyut tabloları başvuru tablolara değiştirin.** Bir boyut tablosunu olgu tablosu ile birlikte olamaz, Boyut tablosuna tüm düğümlerin bir başvuru tablosu biçiminde kopyalarını dağıtarak sorgu performansını iyileştirebilir.

Okuma [gerçek zamanlı Pano öğretici](./tutorial-design-database-hyperscale-realtime.md) örneği, bu tür bir uygulama oluşturma.

### <a name="timeseries-data"></a>Zaman serisi verileri

Zaman serisi iş yükünü, uygulamalar eski bilgileri arşivleme sırasında son bilgilerini sorgulayın.

Zaman serisi hiper ölçekli bilgilerinde modelleme en yaygın hata timestamp dağıtım sütunu kullanıyor. Saatini temel alan bir karma dağıtım zamanları görünüşte rastgele zaman aralıklarını parçalarda birlikte tutmak yerine farklı parçalar halinde dağıtacak. Sorgular genellikle zaman ilgili başvuru zaman aralıkları (örneğin en son veriler), bu nedenle böyle bir karma dağıtım Ağ Yükü sunulmasını sağlar.

#### <a name="best-practices"></a>En İyi Uygulamalar

-   **Bir zaman damgası dağıtım sütunu olarak seçmeyin.** Farklı dağıtım sütunu seçin. Çok kiracılı uygulama, Kiracı kimliği kullanın veya varlık kimliği gerçek zamanlı bir uygulama kullanma
-   **Bunun yerine zaman PostgreSQL tablo bölümleme kullanın.** Tablo bölümleme, büyük bir zaman sıralamalı veri tablosu her farklı zaman aralıkları içeren birden fazla devralınan tablolar halinde bölmek için kullanın.  Hiper ölçekli Postgres bölümlenmiş bir tablodaki dağıtma parçalar devralınan tablolar oluşturur.

Okuma [zaman serisi öğretici](https://aka.ms/hyperscale-tutorial-timeseries) örneği, bu tür bir uygulama oluşturma.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi nasıl [birlikte bulundurma](concepts-hyperscale-colocation.md) arasında sorguları çalıştırma hızlı Dağıtılmış veri yardımcı olur
