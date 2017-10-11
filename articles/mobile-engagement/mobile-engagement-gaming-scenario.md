---
title: "Oyun uygulaması için Azure Mobile Engagement uygulaması"
description: "Azure Mobile Engagement uygulamak için oyun uygulaması senaryosu"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0ca35a3d634db8eb5c63afacba046a35b8a3e7ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a>Oyun uygulaması ile Mobile Engagement uygulama
## <a name="overview"></a>Genel Bakış
Oyun başlatma yeni bir temel Balıkçılık role play/stratejisi oyun uygulaması sunmuştur. Oyun 6 ay için hazır ve çalışır olmuştur. Bu oyun büyük bir başarı ve yüklemeleri milyonlarca sahiptir ve bekletme diğer başlatma oyun uygulamalara kıyasla çok yüksek. Üç aylık gözden geçirme toplantı adresindeki Paydaşlar (ARPU) kullanıcı başına ortalama gelir artırmak ihtiyaç duydukları kabul etmiş olursunuz. Premium oyun paketlerin özel teklifler kullanılabilir. Oyun bu paketleri, kullanıcıların kendi Balıkçılık satırları ve lures veya tackles oyundaki performansını ve görünümünü yükseltme izin verin. Bununla birlikte, paket satış çok düşük. Bu nedenle bunlar ilk olarak Müşteri Deneyimi analiz aracı ile analiz etmek karar verin ve ardından bir katılım geliştirmek için kesimleme satış kullanarak artırmak için program Gelişmiş.

Temel [Azure Mobile Engagement - en iyi yöntemlerle Başlangıç Kılavuzu](mobile-engagement-getting-started-best-practices.md) bir katılım stratejisi oluşturmaya.

## <a name="objectives-and-kpis"></a>Hedefleri ve KPI'leri
Önemli katılımcılara oyun karşılamak için. Tüm % 15 premium paket satışları artırmak için bir ana hedef üzerinde-kabul ediyorum. Bunlar iş anahtar performans göstergelerini (KPI'lar) ölçü ve sürücü bu amaç oluşturun

* Oyun hangi düzeyde, bu paketleri satın alınan?
* Kullanıcı başına, oturumu başına, haftalık ve aylık gelir nedir?
* Sık kullanılan satın alma türleri nelerdir?

Bölüm 1 / [Başlarken Kılavuzu](mobile-engagement-getting-started-best-practices.md) hedefleri ve KPI'leri nasıl tanımlanacağını açıklar. 

Şimdi tanımlanan iş KPI'leri ile yeni kullanıcı eğilimlerini ve saklama belirlemek için katılım KPI'leri mobil ürün Yöneticisi oluşturur.

* İzleme bekletme ve şu aralıklar kullanın: her gün, 2 günde, haftalık, aylık ve 3 ayda
* Etkin kullanıcı sayısı
* Uygulama derecelendirme deposundaki

BT Ekibi önerilerine dayalı olarak, aşağıdaki teknik KPI'ları, aşağıdaki soruları yanıtlamak için eklendi:

* My kullanıcı yolu nedir (hangi sayfasını ziyaret, ne kadar zaman kullanıcılar harcamanız üzerinde)
* Kilitlenme veya oturum başına karşılaşılan hataların sayısı
* İşletim sistemi sürümleri çalıştıran Kullanıcılarım nelerdir?
* Kullanıcılarım için ekran ortalama boyutu nedir?
* Ne tür bir internet bağlantısı Kullanıcılarım var mı?

Her KPI için ihtiyaç duyacağı ve kendi playbook nerede veri mobil ürün Yöneticisi belirtir.

## <a name="engagement-program-and-integration"></a>Katılım programı ve tümleştirme
Gelişmiş katılım programı oluşturulmadan önce mobil proje Director projeden derin bir anlayış nasıl ve ürünleri kullanıcılar tarafından tüketilen olması gerekir.

3 ay sonra kendi uygulama anında iletme bildirimi satış artırmak için yeterli veri mobil proje Director topladı. Hüseyin, öğrenir:

* İlk satın alma genellikle 14 düzeyinde gerçekleşir. Bu gibi durumlarda için % 90'ını, satın alma yeni efsanevi Silahlar için 3 ' dir.
* Bu gibi durumlarda % 80'i, ürünle birlikte devam etmek ve daha fazla olun bir satın alma, yaptığınız kullanıcılar satın alır.
* 20 düzeyi geçtiğinde kullanıcılar birden fazla 10/hafta harcamanız başlatın.
* Kullanıcılar, 16, 24 ve 32 düzeyinde premium paketleri satın eğilimindedir.

Bu çözümleme sayesinde mobil proje Director belirli itme uygulama satış artırmak için bildirim dizileri oluşturmaya karar verir. Halil, kendisine çağıran üç anında iletme sıralarını oluşturur: program, satış programı ve etkin olmayan programı'na Hoş Geldiniz. Daha fazla bilgi için bkz [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
