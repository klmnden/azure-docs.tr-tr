---
title: "Özel raporlar verizon'dan | Microsoft Docs"
description: "Aşağıdaki raporlar kullanarak, CDN için kullanım desenlerini görüntüleyebilirsiniz: bant genişliği, aktarılan verileri, isabet, önbellek durumları, önbellek isabet oranı, IPv4/IPv6 aktarılan veriler."
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: v-deasim
ms.openlocfilehash: f09195dc07a96ebcca7f7a9e4bcf521fae13630c
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="custom-reports-from-verizon"></a>Verizon'dan özel raporlar

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Verizon profillerini Verizon özel raporları Yönet portalı yoluyla kullanarak kenar CNAME'ler raporlar için toplanacak veri türünü tanımlayabilirsiniz.


## <a name="accessing-verizon-custom-reports"></a>Verizon özel raporlar erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili Yönet düğmesi](./media/cdn-reports/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **özel raporlar** çıkma. Tıklatın **kenar CNAME'ler**.
   
    ![CDN Yönetim Portalı - menü özel raporlar](./media/cdn-reports/cdn-custom-reports.png)

## <a name="edge-cnames-custom-report"></a>Edge CNAME'ler özel raporu
Edge CNAME'ler özel rapor için özel rapor günlük etkinleştirildiği kenar CNAME'ler isabetler ve aktarılan veri istatistikleri sağlar. Edge CNAME'ler Azure CDN uç noktası ana bilgisayar adları ve tüm ilişkili özel etki alanı ana bilgisayar adları oluşur. 

Özel rapor veri günlük kaydı bir bir sınır CNAME'ın özel raporlama özelliği etkinleştirdikten sonra saat başlar. Tüm platformlar için veya belirli bir platform için bir sınır CNAME'ler raporu oluşturarak, rapor verilerini görüntüleyebilirsiniz. Bu rapor için kapsamı özel rapor verilerini belirtilen süre içinde toplanan kenar CNAME'ler sınırlıdır. Kenarın CNAME'ler rapor üst 10 kenarı CNAMEs için bir grafik ve veri tablosu ölçümleri seçeneğinde tanımlanan ölçüm göre oluşur. 

Aşağıdaki rapor seçeneklerini tanımlayarak özel bir rapor oluşturmak:

- Ölçümler: Aşağıdaki seçenekleri desteklenir:

   - İsabet: özel raporlama özelliği etkin olduğu bir kenar CNAME yönlendirilmiş isteklerinin toplam sayısını gösterir. Bu ölçüm, istemciye döndürülen durum kodu içermez.

   - Veri transfer: özel raporlama özelliği etkin olduğu bir kenar CNAME yönlendirilmiş istekleri için toplam uç sunuculardan (örneğin, web tarayıcıları) HTTP istemcilere aktarılan veri miktarını gösterir. Aktarılan veri miktarını yanıt gövdesi için HTTP yanıt üstbilgilerini ekleyerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarını, gerçek dosya boyutundan daha büyük.

- Gruplandırmalar: çubuk grafik gösterilen istatistikleri türünü belirler. Aşağıdaki seçenekleri desteklenir:

   - HTTP yanıt kodları: istatistikleri HTTP yanıt koduna göre düzenler (örneğin, 200, 403, istemciye döndürülen vb.). 

   - Önbellek durumu: istatistikleri önbellek durumuna göre düzenler.


Rapor için tarih aralığını ayarlamak için ya da önceden tanımlanmış tarih aralığı gibi seçebileceğiniz **Bugün** veya **bu hafta**, aşağı açılan listeden ya da, seçebilirsiniz **özel** ve Takvim simgelere tıklayarak el ile bir tarih aralığı girin. 

Tarih aralığı seçtikten sonra tıklatın **Git** raporu oluşturmak için.

Sağındaki Excel simgesini tıklatarak verileri Excel biçiminde dışarı aktarabilirsiniz **Git** düğmesi.

![CNAMEs raporu](./media/cdn-reports/cdn-cnames-report.png)

## <a name="edge-cnames-custom-report-fields"></a>Edge CNAME'ler özel rapor alanları

| Alan                     | Açıklama   |
|---------------------------|---------------|
| 2xx                       | İstekleri ya da aktarılan veriler (MB) bir 2xx HTTP durum kodunu sonuçları CNAME kenar için toplam sayısını gösterir (örneğin, 200 Tamam). |
| 3xx                       | İstekleri ya da aktarılan veriler (MB) bir 3xx HTTP durum kodunu sonuçları CNAME kenar için toplam sayısını gösterir (örneğin, 302 bulundu veya 304 değiştirilemez. |
| 4xx                       | İstekleri ya da aktarılan veriler (MB) bir 4xx HTTP durum kodunu sonuçları CNAME kenar için toplam sayısını gösterir (örneğin, 400 Hatalı istek, 403 Yasak veya 404 bulunamadı). |
| 5XX                       | Kenarın bir 5xx HTTP durum kodunu (örneğin, 500 İç sunucu hatası veya hatalı 502 ağ geçidi) sonuçları CNAME için istekleri ya da aktarılan veriler (MB) toplam sayısını gösterir. |
| İsabetli Önbellek Okuma Yüzdesi               | Doğrudan önbelleğinden istemciye sunulduğunu alınabilir isteklerinin yüzdesini gösterir. |
| İsabetli Önbellek Okuma Sayısı                | İstekleri ya da aktarılan veriler (MB) bir önbellek isabet (örneğin, TCP_EXPIRED_HIT, TCP_HIT veya TCP_PARTIAL_HIT) sonuçları CNAME kenar için toplam sayısını gösterir. İstenen içerik önbelleğe alınmış bir sürümü bulunan bir önbellek isabet oluşur. |
| Aktarılan veriler (MB)     | Toplam aktarılan veri miktarını (MB) uç sunuculardan için CNAME kenar HTTP istemcileri (web tarayıcıları) gösterir. Aktarılan veri miktarını yanıt gövdesi için HTTP yanıtı üstbilgileri ekleyerek hesaplanır. Sonuç olarak, her varlık için aktarılan veri miktarını, gerçek dosya boyutundan daha büyük. |
| Açıklama               | Kendi ana bilgisayar adı bir çizgiyle CNAME tanımlar |
| İsabetli                      | Kenarın CNAME isteklerine toplam sayısını gösterir. |
| İsabetsiz                    | İstekleri ya da aktarılan veriler (MB) kenarın önbellek isabetsizliği (örneğin, TCP_CLIENT_REFRESH_MISS, TCP_EXPIRED_MISS veya TCP_MISS) içinde sonuçları CNAME için toplam sayısını gösterir. Önbellek isabetsizliği, istenen içerik isteği dikkate alınır uç sunucusunda önbelleğe alınmamış oluşur. | 
| Önbellek yok                  | İstekleri ya da aktarılan veriler (MB) CONFIG_NOCACHE önbellek durum koduna sonuçları CNAME kenar için toplam sayısını gösterir.  |
| Diğer                     | İsteklerini veya 2xx - 5xx aralık dışında kalan bir HTTP durum kodu sonuçlanır CNAME kenar için aktarılan (MB) belirtilen veri toplam sayısını gösterir. |
| Platform                  | Edge CNAME'ın trafiğini işler platform gösterir. |
| Atanmamış               | İstekleri ya da aktarılan veriler (MB) toplam sayısı hangi önbellek durum kodunu veya HTTP durum kodu için bilgi değil kaydedilen kenar CNAME için gösterir.  |
| Uncacheable               | İstekleri ya da aktarılan veriler (MB) bir UNCACHEABLE önbellek durum kodunu sonuçları CNAME kenar için toplam sayısını gösterir.  |


## <a name="considerations"></a>Dikkat edilmesi gerekenler
Raporları yalnızca son 18 ay içinde oluşturulabilir.

