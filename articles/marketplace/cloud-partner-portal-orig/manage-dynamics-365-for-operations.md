---
title: Dynamics 365 bulut iş ortağı portalı Operations teklifi için oluşturma
description: Dynamics 365 bulut iş ortağı portalı Operations teklifi için oluşturma
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: a9ada25641e2a56beb9083b145a507c8fd41a46f
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935086"
---
# <a name="how-to-create-dynamics-365-for-operations-offer-via-cloud-partner-portal"></a>Dynamics 365 bulut iş ortağı portalı Operations teklifi için oluşturma

Yayımlama Portalı izin vererek bir teklifi yayımlama doğru işbirliği yapmak birden çok kişiye portalında rol tabanlı erişim sağlar. Bkz: [bulut portalı kullanıcıları Yönet](./cloud-partner-portal-manage-users.md) daha fazla bilgi için.

Teklif, yayımcı adına yayımlanmadan önce hesabı, bir kişi ile \"sahibi\" rol uyacağınızı kabul edersiniz gerek [kullanım](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft gizlilik bildirimi](https://www.microsoft.com/privacystatement/default.aspx), ve [Microsoft Azure sertifikası Program sözleşmesi](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="how-to-create-a-new-dynamics-365-for-operations-offer"></a>İşlem teklifi için yeni bir Dynamics 365 oluşturma

Tüm önkoşulların karşılandığını sonra yazma işlemleri teklif için Dynamics 365 başlamaya hazır olursunuz.

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Sol gezinti çubuğundan tıklayarak \"+ yeni teklif\" seçip \"operasyonlar için Dynamics 365\".
3. Yeni bir teklif \"Düzenleyicisi\" görünümü artık açılır, için ve biz yazma başlamaya hazırsınız.
4. \"Forms\" doldurulması gereken içinde soldaki görünür \"Düzenleyicisi\" görünümü. Her \"form\" doldurulması için alan kümesi oluşur. Gerekli alanlar, kırmızı bir yıldızla işaretlenir (\*).

Yazma işlemleri teklif için bir Dynamics 365 için dört ana form vardır:

- Teklif ayarları
- Teknik bilgileri
- StoreFront ayrıntıları
- Kişiler

## <a name="how-to-fill-out-the-offer-settings-form"></a>Teklif ayarları formu doldurun nasıl

Teklif ayarları formu teklif ayarlarını belirtmek için temel bir biçimidir. Farklı alanlar, aşağıda açıklanmıştır.

### <a name="offer-id"></a>Teklif Kimliği

Bu teklif içinde bir yayımcı profili için benzersiz bir tanımlayıcıdır. Bu kimliği, ürün URL'lerinde görünür olur. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimliği tire bitemez ve en çok 50 karakter olabilir. Bu alan, teklif etkin hale gelir sonra kilitli.

bir teklifle bir yayımcı contoso yayımcı oluşturursa, örneğin, Teklif kimliği *örnek dynamics365 işlemleri için*, appsource'ta görünür `https://appsource.microsoft.com/marketplace/apps/**contoso**.*sample-dynamics365 for operations*?tab=Overview\`.

### <a name="publisher-id"></a>Yayımcı kimliği

Bu açılan listeyi, bu teklif altında yayımlamak istediğiniz yayımcı profilinizle seçmenize olanak sağlar. Bu alan, teklif etkin hale gelir sonra kilitli.

Ad

Bu, teklifiniz için görünen addır. Bu, görünecek addır [AppSource](https://appsource.microsoft.com). En fazla 50 karakter olabilir.

Tıklayarak \"Kaydet\" ilerlemenizi kaydetmek için. Sonraki adım, teklifiniz için teknik bilgileri doldurmanız olacaktır.

## <a name="how-to-fill-out-the-technical-info-form"></a>Nasıl teknik bilgi formu doldurun

Teknik bilgi form teklif sayfanızın görüntülenen bilgiler içerir. Farklı alanları için yönergeler aşağıda verilmiştir.

![Yeni Teklif ekran](./media/publish_d365_new_offer/Technical_info.png)

### <a name="solution-identifier"></a>Çözüm tanımlayıcısı

Öncelikle, çözüm tanımlayıcısı değil.

1. Bu tanımlayıcı bulmak için yaşam döngüsü Hizmetleri ve yönetim çözümünü seçin.
2. Uygun çözüm seçtikten sonra çözüm tanımlayıcısı paket genel bakış görürsünüz. \*\*Tanımlayıcısı boş ise, Düzenle'yi seçin ve paketinizi görünmesini çözüm tanımlayıcısı için yeniden yayımlayın.

### <a name="validation-assets"></a>Doğrulama varlıklar

OTOMOBİLİNİZİN (özelleştirme analiz raporları) buraya yükleyin.

### <a name="does-solution-enable-translations"></a>Çözüm çeviri(ler) olanak sağlıyor mu?

Seçin \'Evet\' veya \'Hayır\'.

### <a name="does-solution-include-localizations"></a>Çözüm Localization(s) içeriyor mu?

Seçin \'Evet\' veya \'Hayır\'.

### <a name="product-version"></a>Ürün Sürümü

Yeni AX seçin. Son olarak Kaydet'e tıklayın.

## <a name="how-to-fill-out-storefront-details-form"></a>Nasıl mağaza ayrıntı formu doldurun.

İlk teklif ayrıntılarınızı olur.

1. **Teklif özeti**

    \- Çözümünüzü (en fazla 100 karakter) kısa bir özetini girin.

2. **Teklif açıklaması**

    \- Çözümünüzün kısa bir açıklama girin. Açıklamanızı çözümünüzü işlevsel ayak izine sahip olmalıdır ve doğrudan BPM kitaplığınızı ile hizalamanız gerekir. Örneğin, sahip olduğunuz özellikleri diyelim ki x, y, z biz LCS içinde BPM Kitaplığı'nda belgelenir bu olduğundan emin olun son gözden geçirme sırasında pazarlama içeriğinizi.

![Mağaza Ayrıntılar ekranı](./media/publish_d365_new_offer/offer_details.png)

### <a name="listing-details"></a>Liste ayrıntıları

![Mağaza Ayrıntılar ekranı](./media/publish_d365_new_offer/storefront_details.png)

1. **Sektörler** -en fazla iki sektörler verilen seçeneklerden denetleyin.
2. **Kategorileri** -en fazla üç kategorilerden seçeneklerle denetleyin.
3. **Uygulama türü** -belirli seçenekler arasından seçim.
4. **Uygulamanız için bağlantıdan** -çözümünüz için Yardım bağlantısına girin.
5. **Desteklenen ülkeler/bölgeler** -verilen seçeneklerden kontrol edin.
6. **Desteklenen diller** -verilen seçeneklerden kontrol edin.
7. **Uygulama sürümü** -çözümünüzün yayımlanan sürümü girin. (örneğin, 1.0.0.0)
8. **Uygulama yayın tarihi** -sürüm girin, solution(mm/dd/yyyy) tarihi.
9. **Uygulamanızı çalışır ürünleri** -uygulamanızı çalışır listeye özgü ürünleri. En fazla üç ürün listeleyebilirsiniz. Bir ürün listelemek için yanındaki artı işaretini (yeni) tıklayın ve uygulamanızı çalışır bir ürünün adını girmeniz için yeni bir açık metin alanı oluşturulur.
10. **Arama anahtar sözcükleri** -kullanıcılar, bir arama sırasında çözümünüzü bulmak için kullanabilir genel koşulları girin. \*\*Bu anahtar sözcükler, Market'te görüntülenmez.
11. **Anahtar Gizle** -genel görünümden gizlemek için teklif Önizleme URL'si ile birleştirilmesini hangi önemli nokta bu. Bir parola değil. Burada herhangi bir dize girebilirsiniz.

### <a name="marketing-artifacts"></a>Pazarlama Yapıtları

1. Ardından karşıya yükleme, **logoları**, **ekran görüntüleri**. \*\*Her yükleme için boyutları unutmayın ve tüm görüntü PNG biçiminde olmalıdır.
2. **Tanıtım videoları** -tıklayarak \"+ yeni\". Bir tanıtım videosu çözümünüzün (yalnızca YouTube veya Vimeo bağlantılar) karşıya yükleyin. \*\*. Videonuzu hazırlamada görünür hale getirmek için belirtilen boyut, küçük resim karşıya lütfen unutmayın.
3. **Belgeler** - çözümünüze ilgili tüm belgeleri karşıya yüklemesine ve belge için bir ad girmeyi unutmayın.

### <a name="legal"></a>Yasal Bilgiler

Bu bilgiler, gizlilik ilkesi ve kullanım için bağlantı içerir. ' % S'çözüm gizlilik ilkesi URL'si ve kullanım girin. \*\*Müşteri, bu ilkeler portalında görmek mümkün olacaktır.

### <a name="customer-support"></a>Müşteri Desteği

Destek URL'si, kullanıcılarınız tarafından portalda yalnızca görülür.

### <a name="leads-management"></a>Müşteri adayları Yönetimi

Select neden olduğu bir CRM sistemine depolanır. Seçin \"Azure tablo\" aşağıdaki CRM sistemleri birine sahipseniz burada: Salesforce, Marketo, Microsoft Dynamics CRM. Burada seçtiğiniz CRM biz uygulamanızı appsource'ta (müşteri adayları) deneyin son kullanıcıların ayrıntıları burada yazacak sistemidir. Seçtiğiniz CRM sistemine bağlı olarak aşağıdaki karşılık gelen URL'yi alanlar sonraki kümesini tamamlamak hakkında daha fazla bilgi için tıklayın.

![Tedarik Yönetimi ayrıntıları](./media/publish_d365_new_offer/leads.png)

1. Azure tablo için başvuru [burada](https://aka.ms/leadsettingforazuretable)
2. Dynamics CRM online, başvuru [burada](https://aka.ms/leadsettingfordynamicscrm)
3. Marketo için [burada](https://aka.ms/leadsettingformarketo)
4. Başvurmak için Salesforce [burada](https://aka.ms/leadsettingforsalesforce)

## <a name="how-to-fill-out-the-contacts-form"></a>Nasıl kişiler formu doldurun.

Bu bilgiler Microsoft ve müşteri için kullanılacak destekler. Mühendislik birimi ilgili kişisi ve müşteri desteği, şirketiniz ve çözümünüz için destek URL'sini girin. Microsoft, çözümünüzün hakkında ve müşteri desteği için soru varsa bu bilgileri tek bir kişi noktası olarak kullanılır.

![Teklif kişiler ekranı](./media/publish_d365_new_offer/Contacts.png)
