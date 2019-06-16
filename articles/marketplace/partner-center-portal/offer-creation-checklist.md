---
title: Denetim oluşturma - Azure ticari Market sunar
description: Ayrıntıları teklif oluşturma işleminde sağlayabilirsiniz. -Azure Market ticari
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 904058c2c98c8ded2ea9c91e8aa7ec595aa49b05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66481452"
---
# <a name="offer-creation-checklist"></a>Teklif oluşturma denetim listesi

Teklif oluşturma işlemi, birden çok sayfalarla sizi yönlendirir. Her öğe hakkında daha fazla bilgi için bağlantılar içeren her sayfada sağlayabilir ilişkin ayrıntılar aşağıdadır.

Sağlamak veya belirtmek için gerekli olan öğeler aşağıda belirtilmiştir. Bazı alanlar isteğe bağlı veya varsayılan değerleri sağladığınızdan, istediğiniz gibi değiştirebilirsiniz. Bu bölümlerde, burada listelenen sırayla çalışmanız gerekmez.

| **Öğesi**    | **Amacı**  |
| :---------- | :-------------------|
| [**Yeni Teklif kalıcı**](#new-offer-modal) | Toplanan kimlik bilgilerini sunar.  |
| [Teklif kurulum sayfası](#offer-setup-page) | Anahtar özellikleri kullanmak ve teklifinizi Microsoft yoluyla satmak üzere nasıl seçme kabul etmek sağlar.  |
| [Özellikler sayfası](#properties-page) | Kategorileri ve teklifinizi marketleri teklifinizi ve uygulamanızı uygulama sürümünü destekleyen yasal sözleşmeleri gruplandırmak için kullanılan sektörler tanımlayın. |
| [Teklif Listesi sayfa](#offer-listing-page) | Market'te teklifinizi açıklamaları da dahil olmak üzere ve varlıkları pazarlama görüntülenecek teklif ayrıntılarını tanımlayın. |
| [Önizleme sayfası](#preview-page) | Teklifiniz için daha geniş Market kitleyi dinamik teklifinizi yayımlamadan önce serbest için sınırlı bir önizleme kitle tanımlayın. |
| [Teknik yapılandırma sayfası sunar](#technical-configuration-page)  | Yalnızca Microsoft aracılığıyla teklif satmak seçerseniz kullanılabilir. Teklifinizin bağlanmak için kullanılan teknik ayrıntılarını (URL yolu, Web kancası, Kiracı kimliği ve uygulama kimliği) tanımlayın. |
| [**Yeni Plan kalıcı**](#plan-identity-modal) | Kimlik bilgilerini toplar planlayın.  |
| [Liste Sayfası planlama](#plan-listing-page)  | Yalnızca Microsoft aracılığıyla teklif satmak seçerseniz kullanılabilir. Plan Market'te listelemek için kullanılan ayrıntılarını tanımlayın.  |
| [Fiyatlandırma planını & kullanılabilirlik sayfası](#plan-pricing--availability-page)  | Yalnızca Microsoft aracılığıyla teklif satmak seçerseniz kullanılabilir.  İş özellikleri (fiyatlandırma modeli), teklifinizin her plan (sürüm) için hedef kitle ve Pazar kullanılabilirlik toplar.  |
| [Test sürücü listesi sayfası](#test-drive-listing-page)  | Yalnızca bir test sürüşüne teklifiniz için teklif seçerseniz kullanılabilir. Test sürüşü Market'te listesine kullanılan ayrıntılarını tanımlayın.  |
| Test sürücü teknik yapılandırma sayfası  | Yalnızca bir test sürüşüne teklifiniz için teklif seçerseniz kullanılabilir. Müşterilerin deneyebilmesi satın alma için gerçekleştirmeden önce teklifiniz için sağlayan tanıtım (veya "sürücü test") için teknik ayrıntıları tanımlayın.  |
| [Gözden geçirin ve yayımlama sayfası](#review-and-publish-page)  | Yayımlama, her sayfanın durumunu görmek ve sertifika ekibine Notlar sağlayın, istediğiniz değişiklikleri seçin.  |


## <a name="new-offer-modal"></a>Yeni Teklif kalıcı 

İlk bilgileri sağlamanız istenir bir ad ve teklifinizi Kimliğini parçalarıdır. 

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Teklif kimliği  | Gerekli, oluşturulduktan sonra değiştirilemez. En fazla 50 karakter ve yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi oluşmalıdır. |
| Teklif adı  | Gereklidir. |

## <a name="offer-setup-page"></a>Teklif kurulum sayfası

Teklif Kurulum Burada, farklı Kanallar ve satış hareketlerin iyileştirilmiş hem de test sürüşü ve müşteri gibi önemli özelliklerle kullanımını müşteri adayları bildirmek sayfasıdır. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------|  
| Microsoft satış yapmak ister misiniz?  | Gereklidir. Varsayılan: Evet |
| Nasıl potansiyel müşterilerin teklife etkileşime girerek listeleme istiyorsunuz? (Eylem çağrısı)  | Microsoft satış değil gereklidir. Varsayılan: Ücretsiz deneme, seçenekleri: "Şimdi edinin", "Ücretsiz deneme", "benimle iletişim kurun." |
| Deneme URL'si  | Yol müşteriler teklifi dökümüyle etkileşime geçtiğimiz sırada "Ücretsiz deneme" seçilirse, gerekli. |
| Teklif URL'si  | Teklif listesi ile etkileşimde şekilde müşteriler "Alma şimdi" seçilirse, gerekli |
| Kanallar  | İsteğe bağlı. Varsayılan: CSP (satıcı) kanala abone değil.  |
| Test Sürüşü | İsteğe bağlı. Varsayılan: Etkin test sürüşü yok.  |
| Test Sürüşü türü | Bir test sürüşüne etkinleştirilirse gereklidir. Varsayılan: Hiçbiri seçilmedi. Seçenekler: Azure Resource Manager, Dynamics 365 Business Central için Dynamics 365 müşteri katılımı, işlemleri, mantıksal uygulama için Dynamics 365 için BI güç.  |
| Müşteri adayı yönetimi – bir CRM sistemine bağlanın | Gerekli Microsoft satış veya listeleme "benimle iletişime geçin."olarak sunar. Varsayılan: CRM sistemine bağlı. CRM seçenekleri: Azure tablo, Azure blob, Dynamics CRM online, HTTPs uç noktası, Marketo, Salesforce  |

## <a name="properties-page"></a>Özellikler sayfası

Kategorileri ve teklifinizi marketleri teklifinizi ve uygulamanızı uygulama sürümünü destekleyen yasal sözleşmeleri gruplandırmak için kullanılan sektörler tanımladığınız yerlerde özellikleri sayfasıdır. Böylece uygun şekilde görüntülenir ve doğru ortaklık kümesi müşterileri için sunulan teklifinizle ilgili tam ve doğru ayrıntıları bu sayfada verdiğinizden emin olun. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------|  
| Kategori ve alt kategorisi | 1 ve en fazla 3 gereklidir. Varsayılan: Hiçbiri seçilmedi. |
| Sektör ve subindustries | İsteğe bağlı. 2 L1 sektörler maks. ve 2 subindustries varsayılan her L1 sektör içinde en fazla: Hiçbiri seçilmedi |
| Uygulama sürümü  | İsteğe bağlı. Varsayılan: Yok. |
| Standart sözleşme'ı kullanın  | İsteğe bağlı. Varsayılan: seçili değil.  | |
| Kullanım koşulları  | Standart sözleşme seçili değilse gereklidir.  |

## <a name="offer-listing-page"></a>Teklif Listesi sayfa

Müşteriler teklife ilişkin liste Market'te görüntülerken bkz görüntüler ve metin sağlarsınız listeleme sayfasıdır. 

| **Alan adı**    | **Notlar**   |
| :---------------- | :-----------| 
| Ad  | Gerekli, en fazla 50 karakter. |
| Özet  | Gerekli, en fazla 100 karakter. | 
| Açıklama  | Gerekli, en fazla 3000 karakter. |
| Başlarken yönergeleri  | Gerekli, en fazla 3000 karakter. |
| Başlarken yönergeleri  | Gerekli, en fazla 3000 karakter. |
| Arama anahtar sözcükleri  | İsteğe bağlı, önerilen, en fazla 3 anahtar sözcükleri. |
| Gizlilik İlkesi URL'si  | Gereklidir. |
| CSP programı pazarlama malzemeleri URL'si  | İsteğe bağlı. |
| Başlık ve URL faydalı bağlantılar  | İsteğe bağlı. |
| Destekleyici belgeler başlık + dosyası  | Gerekli, en az 1 ve en fazla 3. PDF dosyası biçiminde olmalıdır. |
| Ekran görüntüleri  | Gerekli, en az 1 ekran görüntüsü ve en fazla 5; dört veya üzeri önerilir. 1280 X 720 biçimde PNG olmalıdır. |
| Store logolar (küçük, Orta, büyük, geniş, Hero)  | (48 X 48) küçük ve büyük (216 X 216) gerekli; diğer boyutları isteğe bağlı ancak önerilen: Orta (90 x 90) geniş (255 x 115), Hero (815 x 290). PNG biçiminde olması gerekir. |
| Video adı, URL + küçük resmi  | İsteğe bağlı, önerilen, en fazla 4 videolar. Küçük Resim 1280 x 720 PNG biçiminde olması gerekir. YouTube veya Vimeo video barındırılması gerekir. |
| Kişiler (mühendislik, CSP programı, destek)  | Mühendislik ve destek ilgili kişisi (adı, e-posta ve telefon numarası) gerekli; İsteğe bağlı ancak önerilen CSP programı başvurun. |
| Destek URL'si  | Gereklidir. |

## <a name="preview-page"></a>Önizleme sayfası

Teklif Canlı geçmeden önce tüm gereksinimleri karşıladığından emin doğrulamak için teklif Önizleme erişimi için hedef kitle belirttiğiniz Önizleme sayfasıdır. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| AAD/MSA'e-posta + açıklaması | En az 1 ve bir CSV dosyasını karşıya el ile veya 20 girilen en fazla 10 gereklidir. |

## <a name="technical-configuration-page"></a>Teknik yapılandırma sayfası 

Teklifinizin bağlanmak için Microsoft tarafından kullanılan bir teknik ayrıntılar belirttiğiniz teknik yapılandırma sayfasıdır. Microsoft satış değil karar verdiyseniz, bu sayfa için görünür değil.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Giriş sayfası URL'si | Microsoft satış gereklidir. |
| Bağlantı Web kancası | Microsoft satış gereklidir. |
| Azure AD Kiracı kimliği | Microsoft satış gereklidir. |
| Azure AD uygulama kimliği | Microsoft satış gereklidir. |

## <a name="plan-identity-modal"></a>Kimlik kalıcı planlama

İlk bilgileri sağlamanız istenir. bir ad ve planınız için bir kimlik parçalarıdır. Microsoft satış değil karar verdiyseniz, bu sayfa için görünür değil.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Plan kimliği  | Microsoft satış gereklidir. Oluşturulduktan sonra değiştirilemez. En fazla 50 karakter ve yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi oluşmalıdır. |
| Plan adı  | Microsoft satış gereklidir. Teklifteki tüm planlar arasında benzersiz olması gerekir. En çok 50 karakter. |

## <a name="plan-listing-page"></a>Plan listesi sayfası

Sayfa listesi müşterilerin planı Market'te görüntülerken görmek metin sağlarsınız plandır. Microsoft satış değil karar verdiyseniz, bu sayfa için görünür değil.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Plan açıklaması   | Microsoft satış gereklidir. En fazla 500 karakter. | |

## <a name="plan-pricing--availability-page"></a>Plan fiyatlandırması ve kullanılabilirlik sayfası

Plan fiyatlandırması ve kullanılabilirlik iş özelliklerini, hedef kitle ve Pazar kullanılabilirlik teklifinizin her plan (sürüm) için tanımladığınız sayfasıdır. Microsoft satış değil karar verdiyseniz, bu sayfa için görünür değil.

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Pazar kullanılabilirlik  | Gerekli, en az 1 ve 141 max. |
| Fiyatlandırma Modeli  | Gereklidir. Varsayılan: Sabit fiyat. Seçenekler: Kullanıcı başına sabit fiyat. |
| Minimum ve maksimum bilgisayar lisansı  | Seçili isteğe bağlı, bilgisayar başına yalnızca kullanılabilir fiyatlandırma modeli. |
| Faturalama dönemi  | Gereklidir. Varsayılan: Aylık. Seçenekler: Aylık, yıllık. |
| Fiyat  | Gerekli ABD Doları seçilen Terime aylık faturalandırma, ay başına; veya yıllık, yıl başına ABD Doları seçilen Terime faturalama. |
| Hedef kitle planlama  | İsteğe bağlı. Varsayılan: Genel-planı. Seçenekler: Genel, özel Kiracı Kimliğine göre |
| Kısıtlı planlama İzleyici (Kiracı kimliği + açıklaması)  | Özel plan seçtiyseniz gereklidir. En az 1 ve en fazla 10 kimlikleri el ile girdiyseniz Kiracı. En fazla CSV içeri aktarma dosyasının 20000. |

## <a name="test-drive-listing-page"></a>Test sürücü listesi sayfası

Yalnızca bir test sürüşüne teklifiniz için teklif seçerseniz kullanılabilir. Test sürüşü Market'te listesine kullanılan ayrıntılarını tanımlayın.

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Açıklama  | Gereklidir. |
| El ile kullanıcı adı + dosyası  | Gerekli, en fazla 1 belge. PDF biçiminde olmalıdır. |
| Video adı, URL + küçük resmi  | İsteğe bağlı, önerilen. Küçük resim 533 x 324 JPGP veya PNG biçiminde olması gerekir. YouTube veya Vimeo video barındırılması gerekir. |

## <a name="review-and-publish-page"></a>Gözden geçirin ve yayımlama sayfası

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Sertifika için Notlar  | İsteğe bağlı. |

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir SaaS teklifi oluşturma](./create-new-saas-offer.md)
