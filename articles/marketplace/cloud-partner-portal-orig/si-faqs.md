---
title: Satıcı Insights SSS
description: Hakkında sık sorulan sorular bulut iş ortağı portalı satıcı Insights özelliğidir.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: 2719b6b47225576f2eadeb5e5b40b3aa7b39444d
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943080"
---
<a name="seller-insights-faq"></a>Satıcı Insights SSS
===================

Bu makalede, yaygın kullanıcı yordamlar ve satıcı Insights hakkında sorular için yönergeler sağlar.


<a name="find-definitions-for-the-values-in-the-downloaded-transaction-file"></a>Değerler için tanımları indirilen işlem dosyasında Bul
------------------------------------------------------------------

İşlem dosyasına ölçüm değerleri tanımlarını makalesinde bulunan [satıcı Insights tanımları](./si-insights-definitions-v4.md).


<a name="see-customer-details-of-transactions-for-which-ive-been-paid"></a>İşlem için ödeme yapmıştım Müşteri ayrıntıları bakın
-------------------------------------------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **ödeme durumu**ve yalnızca "Ücretli aşımı" değeri görüntülemek için filtre uygulayın Müşteri ayrıntıları içeren aşağıdaki sütunlar görüntülenir: **Şirket adı**, **müşteri e-posta**, **Müşteri Ülke**, **Müşteri durumu**, ve **müşterinin posta kodunu**.


<a name="calculate-my-open-accounts-receivable"></a>My Aç hesapları Hesapla
-------------------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **ödeme durumu**ve yalnızca değeri "Gelecek ödeme" ve "İçin hazır değil ödeme." görüntülemek için filtre uygulayın Ardından etiketli sütunu Topla **ödeme tutarı (PC)**.


<a name="calculate-revenue-by-customer-usage-period"></a>Müşteri kullanım döneme göre gelir hesaplayın
------------------------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **işlem durumu**ve filtre "Ücretli" değeri.   Listelenen her işlem için etiketli sütun **ödeme tutarı (PC)** ödediğiniz tutar temsil eder.  Sütun işlemle ilişkili kullanım dönemi tahmin etmek için kullanmak **ödeme tarihi**, kullanım işlem geçerli olduğu dönemin son günü Kapat bir yaklaşığını olduğu.


<a name="calculate-your-bad-debt"></a>Hatalı borç hesaplayın
---------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **son koleksiyon durumu**ve yalnızca görüntüleme değeri "yazma devre dışı." filtresini uygulayın. Ardından etiketli sütunu Topla **ödeme tutarı (PC)**.


<a name="view-payout-or-customer-contact-information"></a>Ödeme veya müşteri irtibat bilgilerini görüntüleyin
-------------------------------------------

"Sahip" rolü ve "katılımcı" rolü olan bir kullanıcı olarak oturum açın. Sahip rolü, ödeme ve müşteri bilgileri görürsünüz. Bu makalede kullanıcı rolleri hakkında daha fazla bilgi bulabilirsiniz [kullanıcıları yönetme](./cloud-partner-portal-manage-users.md).


<a name="calculate-my-advance-payouts"></a>My öncelikli ödeme hesaplayın
----------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **işlem türü**ve yalnızca "Ücretsiz" değeri görüntülemek için filtre uygulayın Ardından, etiketli sütununu bulun **son koleksiyon durumu**ve yalnızca "Sürüyor" değeri görüntülemek için filtre uygulayın. Son olarak, sum **ödeme tutarı (PC)** tüm geliştirmeleri hesaplamak için sütun Ücretli için önce koleksiyonu müşteriden.


<a name="calculate-customer-refunds"></a>Müşteri para iadesi hesaplayın
--------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra etiketli sütununu bulun **son koleksiyon durumu**ve yalnızca "Para iadesi" değeri görüntülemek için filtre uygulayın Sum **ücret tutarı (PC)** tüm para iadesi hesaplamak için sütun müşterileriniz için işlenen.


<a name="identify-which-transactions-involved-a-microsoft-channel-partner"></a>Bir Microsoft iş ortağı kanalı hangi işlemleri söz konusu tanımlama
----------------------------------------------------------------

Sütundaki tüm işlemleri **Azure lisans türü** içeren bir Microsoft iş ortağı kanalı "Bulut çözümü sağlayıcısı" ve "Kurumsal aracılığıyla satıcısı" değerleri görüntülemek için filtre. İş ortağı ile ilgili daha fazla ayrıntı için bulabilirsiniz, **satıcı adı** ve **satıcı e-posta** ödeme Modül yükleme ve müşteri modül indirme.


<a name="identify-trial-usage-and-trial-conversions"></a>Deneme kullanımı ve deneme dönüştürmeleri tanımlar
------------------------------------------

Sipariş, kullanım ve ödeme modülü yüklemeleri artık içeren **deneme bitiş tarihi** deneme süresi belirli Bu sipariş için uygunsa bittiğinde anlamanıza yardımcı olacak. Deneme kullanımı ve siparişler görmek için bulun **SKU türü faturalama** sütun indirmeler ve yalnızca "Deneme" değeri görüntülemek için filtre uygulayın Deneme dönüştürmeler görmek için bulun **deneme bitiş tarihi** sütun indirmeler ve uygulama yalnızca görüntülemek için filtre siparişleri **deneme bitiş tarihi** bugünün tarihi ve **İptal tarihi** sütun boş veya daha sonraki **deneme bitiş tarihi**.


<a name="when-is-my-monthly-payout-calculated"></a>My aylık ödeme yaptığınızda hesaplanır
------------------------------------

Ödemeler, her ay 15 tarafından doğrulanmak için hazır olan tüm tutarlar için önceki ayın son takvim günü tarafından verilir. Ayın üçüncü günü Microsoft önceki ayın ödeme tutarı hesaplamak ve "Gelecek ödeme" ile indirme işleminiz tüm geçerli ücret işlemleri güncelleştirmek **ödeme durumu** sütun. Ödeme istek banka hesabı için aynı zamanda gönderilene kadar bu işlem bu durumda kalır, **ödeme durumu** "Ücretli çıkış" güncelleştirilir ve "Ödeme tarihi" tarihi size gönderilen gösterecek şekilde güncelleştirilir Ödeme bankanız isteğine.


<a name="calculate-customer-acquisition-and-loss"></a>Müşteri edinme ve zarar hesaplayın
---------------------------------------

Müşteri ilk satın tekliflerinizi birini bularak tarih gördüğünüz **tarih alınan** müşteri indirme sütunu. Benzer şekilde, daha sonra artık sahip oldukları herhangi bir teklifin sizin tarafınızdan bularak yayımlanan tarih gördüğünüz **tarih kayıp** müşteri indirme sütunu.


<a name="finding-more-help"></a>Daha fazla yardım bulma
-----------------

- [Satıcı Insights tanımları](./si-insights-definitions-v4.md) -Ölçümler ve verileri için tanımları Bul

- [Satıcı Insights'ı kullanmaya başlama](./si-getting-started.md) -satıcı öngörü özelliği giriş.

