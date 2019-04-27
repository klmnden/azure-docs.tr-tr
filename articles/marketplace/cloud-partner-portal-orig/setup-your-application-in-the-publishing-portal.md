---
title: Yayımlama Portalı'nda uygulamanızı kurma | Microsoft Docs
description: Uygulamanızı buluta Yayımlama Portalı'nda ayarlama yönergeleri.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 8ac0fbb1c62e4162e1c4ad040365a16d055e4552
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60483239"
---
# <a name="setup-your-application-in-the-publishing-portal"></a>Yayımlama Portalı'nda uygulamanızı kurma

Artık uygulamanızı Yayımlama Portalı'nda ayarlamaya hazır olursunuz.

## <a name="login-and-create-a-new-offer"></a>Oturum açma ve yeni bir teklif oluşturun

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Sol gezinti çubuğundan tıklayın "+ Yeni Teklif" ve "Dynamics 365 müşteri katılımı için."

![Yeni bir teklif seçme](./media/CRMScreenShot14.png)

1. Yeni bir teklif "Düzenleyicisi" görünümü artık açılır ve biz yazma başlamaya hazırsınız.

![Yeni Teklif ekran](./media/CRMScreenShot15.png)

1. "Doldurulması gerekir formlar" sol "Düzenleyicisi" görünümü içinde görülebilir. Her "form" doldurulması için alan kümesi oluşur. Gerekli alanlar, kırmızı bir yıldızla işaretlenir (\*).

Dynamics 365 müşteri katılımı teklif için yazma için dört ana formu vardır.

* Teklif ayarları
* Teknik bilgileri
* StoreFront ayrıntıları
* Kişiler

## <a name="fill-out-the-offer-settings-form"></a>Teklif ayarları formu doldurun

Teklif ayarları formu teklif ayarlarını belirtmek için temel bir biçimidir. Farklı alanlar, aşağıda açıklanmıştır.

### <a name="offer-id"></a>Teklif Kimliği

Bu teklif içinde bir yayımcı profili için benzersiz bir tanımlayıcıdır. Bu kimliği, ürün URL'lerinde görünür olur. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimliği tire bitemez ve en çok 50 karakter olabilir. Bu alan, teklif etkin hale gelir sonra kilitli.

Örneğin, bir yayımcı varsa **"contoso"** yayımcılar, Teklif kimliği ile bir teklif oluşturur **"örnek-WebApp"**, appsource'ta görünür "https:\//appsource.microsoft.com/marketplace/apps/contoso.sample-WebApp?tab=Overview"

### <a name="publisher-id"></a>Yayımcı kimliği

Bu açılan listeyi, bu teklif altında yayımlamak istediğiniz yayımcı profilinizle seçmenize olanak sağlar. Bu alan, teklif etkin hale gelir sonra kilitli.

### <a name="name"></a>Ad

Bu, teklifiniz için görünen addır. Bu, görünecek addır [AppSource](https://appsource.microsoft.com/). En fazla 50 karakter olabilir.

"İlerlemenizi kaydetmek için Kaydet"'a tıklayın. Teklifiniz için teknik bilgi eklemek için sonraki adım olacaktır.

## <a name="fill-out-the-technical-info-form"></a>Teknik bilgi formu doldurun


Burada, Dynamics 365 müşteri katılımı çözüm için özel bilgiler doldurur teknik bilgi biçimidir. Geldiğinizde daha fazla bilgi sunar. Aşağıdaki örneğe bakın.

![Teknik bilgiler ekranı](./media/CRMScreenShot16.png)

### <a name="application-info"></a>Uygulama bilgileri

Bunların çoğu yayımcılar bırakır varsayılan değerleri, kullanıcı, Hayır, Hayır, alanları ve boş bir uygulama yapılandırma URL'ye göre yukarıdaki ekran görüntüsü.

### <a name="crm-package"></a>CRM paket

![CRM paket bilgileri](./media/CRMScreenShot17.png)

Bu alanlar için bir açıklama aşağıda verilmiştir:

* Paketinin dosya adı: Dosya adı ZIP dosyası oluşturma CRM AppSource paketiniz olduğunda ve Yukarıdaki adımda oluşturduğunuz. Yukarıdaki örnekte olduğu "Microsoft\_SamplePackage.zip".
* Paket konumunuzun URL'si: Bu, yukarıda belirtilen paket dosya adını içeren Azure depolama hesabına URL'dir. Yukarıdaki bölümde adım 9'da oluşturulan URL'dir.
* Paket dosyanızda birden fazla crm paket vardır: Evet'i seçerek **yalnızca** crm farklı paketleri ile birden çok sürümünü destekleniyorsa. Çoğu iş ortakları için bu "Hayır" olacaktır. Evet'i seçerseniz, çözümünüzün her sürüm için AppSource paketleri oluşturmanız gerekir. _Not: Bu, birden fazla varsa istemesi olmadığını **zip** dosyaları. Yalnızca bir sürümü ancak birden çok solution.zip dosyanız varsa, yine de "Hayır" seçmeniz gerekir Paketleme aracı bunlar birlikte otomatik olarak çıkarır._

### <a name="crm-package-availability"></a>CRM paket kullanılabilirlik

Bu bölümde, CRM paketiniz için kullanılabilir hale getirilir, hangi bölgeleri seçin. Hangi bölgede hangi ülkelerde hizmet daha fazla bilgi için lütfen bağlantıya bakın: [https://o365datacentermap.azurewebsites.net/](https://o365datacentermap.azurewebsites.net/)

Not: Almanya dağıtma "Bağımsız ve ABD Devleti bulut" özel izinler bağımsız gerektirir ve doğrulama sırasında sertifika

## <a name="storefront-details"></a>StoreFront ayrıntıları

### <a name="offer-summary"></a>Teklif özeti

Bu teklife ilişkin değer önerisi bir özetidir. Bu teklife ilişkin arama sayfasında görünür. En fazla 100 karakter olmalıdır.

### <a name="offer-description"></a>Teklif açıklaması

Bu, uygulama Ayrıntıları sayfasında görünecek açıklamasıdır. İzin verilen en fazla 1300 karakterdir

### <a name="industries"></a>Sektörler

Uygulamanızı en iyi hizalanır sektör seçin. Uygulamanız varsa bu boş bırakabilirsiniz birden çok sektörler için ilişkilendirir.

### <a name="categories"></a>Categories

Uygulamanıza uygun olan kategorilerini seçin. En fazla 3 seçin.

### <a name="app-type"></a>Uygulama türü

Uygulamanızı Appsource'ta etkinleştirecek deneme türünü seçin. 'Ücretsiz' uygulamanızı ücretsiz olduğu anlamına gelir. 'Deneme' müşterilerin uygulamanızı appsource'ta kısa bir süre için deneyebilirsiniz anlamına gelir. 'İstek deneme sürümü için' için Dynamics 365 müşteri katılımı uygulamaları için desteklenmez. Bu seçeneği belirlemeyin.

### <a name="help-link-for-your-app"></a>Uygulamanız için Yardım bağlantısına

Uygulamanız için ilgili bilgiler yardımcı olan bir sayfaya URL'sini girin.

### <a name="supported-countriesregions"></a>Desteklenen ülkeler/bölgeler

Bu alan, teklifinizi deneme sürümünde olacaktır ülkeler/bölgeler belirler.

### <a name="supported-languages"></a>Desteklenen diller

Uygulamanızın desteklediği dilleri seçin. Bu listede yer almayan diğer dilleri uygulamanız destekliyorsa, teklifinizi yayımlayın ve adresinden bize e-posta devam: [ appsource@microsoft.com ](mailto:appsource@microsoft.com) bize bildirin.

### <a name="app-version"></a>Uygulama sürümü

Uygulamanız için sürüm numarasını girin

### <a name="app-release-date"></a>Uygulama yayın tarihi

Uygulamanız için yayın tarihi girin

### <a name="products-your-app-works-with-max-3"></a>Ürünleri uygulamanızı (en fazla 3) ile çalışır.

Uygulamanızı çalışır listeye özgü ürünleri. En fazla üç ürün listeleyebilirsiniz. Bir ürün listelemek için yanındaki artı işaretini (yeni) tıklayın ve uygulamanızı çalışır bir ürünün adını girmeniz için yeni bir açık metin alanı oluşturulur.

### <a name="search-keywords-max-3"></a>Arama anahtar sözcükleri (en fazla 3)

AppSource bağlı olarak anahtar sözcükler aramak müşteri sağlar. Müşterilere, uygulamanızın gösterilecek anahtar sözcükler kümesini girebilirsiniz.

Örneğin uygulama "My e-posta hizmeti" e-posta, posta, posta hizmeti bazı anahtar sözcükler olabilir. Kullanıcılar büyük olasılıkla aramak için uygulamanızı Appsource'ta arama kutusuna için kullanacağı sözcükler'i seçin.

### <a name="hide-key"></a>Anahtar Gizle

Bu genel görünümden gizlemek için teklif Önizleme URL'si ile birleştirilmiş bir anahtardır. Bir parola değil. Burada herhangi bir dize girebilirsiniz.

### <a name="offer-logo-png-format-48x48"></a>Teklif logo (png biçiminde, 48 x 48)

Bu, uygulamanızın arama sayfasında görünür. **Png biçiminde izin verilir.** 48PX bir çözümleme bir png görüntüsünü karşıya yükleme\*48PX

### <a name="offer-logo-png-format-216x216"></a>Teklif logo (png biçiminde, 216 x 216)

Bu, uygulamanızın ayrıntı sayfasında görünür. **Png biçiminde izin verilir.** 216PX bir çözümleme bir png görüntüsünü karşıya yükleme\*216PX

### <a name="videos"></a>Videolar

En fazla dört videoları karşıya yükleyebilirsiniz. Karşıya yüklemek istediğiniz her video için ekran adı, URL'si (YouTube veya Vimeo yalnızca) ve küçük resim video ile ilişkilendirilecek doldurmanız gerekir. Küçük resim png biçiminde olması gerekir ve 1280PX olmalıdır\*720PX. Yeni video eklemek için artı işareti tıklayın. Videoları thumbnail(s) uygulamanızın ayrıntı sayfasında görünür.

### <a name="documents"></a>Belgeler

En fazla üç PDF biçimli belgeler karşıya yükleyebilirsiniz. Karşıya yüklemek istediğiniz her belge için belge adını girin ve belge yüklemeniz gerekir. Belge pdf biçiminde olması gerekir.

Yeni belgeler eklemek için artı işareti'a tıklayın.

### <a name="screenshots"></a>Ekran görüntüleri

Bu, uygulamanızın AppSource ayrıntı sayfasında görünecek ekran görüntüleri vardır.

### <a name="privacy-policy"></a>Gizlilik İlkesi

Uygulamanızın gizlilik ilkesi için URL girin

### <a name="terms-of-use"></a>Kullanım koşulları

Uygulamanızın kullanım koşulları girin. Appsource'ta müşterilerin uygulamanızı deneyebilmeniz için önce bu koşulları kabul etmesi gerekir

### <a name="support-url"></a>Destek URL'si

Uygulamanız için destek URL'sini girin.

### <a name="lead-destination"></a>Hedef yol

Select neden olduğu bir CRM sistemine depolanır. "Azure tablo", aşağıdaki CRM sistemleri birine sahipseniz burada seçin: Salesforce, Marketo, Microsoft Dynamics CRM. Burada seçtiğiniz CRM biz uygulamanızı appsource'ta (müşteri adayları) deneyin son kullanıcıların ayrıntıları burada yazacak sistemidir. CRM sistemine bağlı olarak seçin, karşılık gelen URL'yi aşağıya alanlar sonraki kümesini tamamlamak hakkında daha fazla bilgi için tıklayın

* [Azure tablosu](./cloud-partner-portal-lead-management-instructions-azure-table.md)
* [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
* [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
* [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)

## <a name="storefront-details"></a>StoreFront ayrıntıları

İlgili kişi ayrıntılarını arasındaki dahili iletişimi için Microsoft ve iş ortağı yalnızca kullanılır. Not: İzlenen bir e-posta adresi bu alanları kullanmak önemlidir. Appsource'ta yayımlama ilerlemenizi üzerinde sizinle iletişim kurmak için bu e-posta kullanacağız. Destek URL'si müşterilere görünür olacaktır.
