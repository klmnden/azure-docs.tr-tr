---
title: Form tanıyıcı nedir?
titleSuffix: Azure Cognitive Services
description: Form ve tablo verilerini ayrıştırmak için Form tanıyıcı kullanmayı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: df3db534550e709e40cc94d5f951056d93a1003e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027078"
---
# <a name="what-is-form-recognizer"></a>Form tanıyıcı nedir?

Azure Form tanıyıcı belirleyin ve formu belgelerden anahtar-değer çiftleri ve tablo verilerini ayıklamak için makine öğrenimi teknolojisi kullanan bir bilişsel bir hizmettir. Ardından, özgün dosyada ilişkileri içeren yapılandırılmış verileri çıkarır. Özel formu tanıyıcı modelinizi, iş akışı veya uygulama kolayca tümleştirmenize ve karmaşıklığını azaltmak için basit bir REST API kullanarak çağırabilirsiniz. Yalnızca beş form belgelerine ya da boş bir formu kullanmaya başlamak için giriş malzeme olarak aynı tür gerekir. Özel içeriğinizi ağır el ile müdahale veya kapsamlı veri bilimi uzmanlığına gerek kalmadan uyarlanmış ve sonuçları hızlı bir şekilde, doğru bir şekilde kazanabilir.

## <a name="request-access"></a>Erişim izni iste
Form tanıyıcı sınırlı erişim önizleme olarak kullanılabilir. Önizleme erişim elde etmek için lütfen doldurun ve gönderme [Bilişsel Hizmetleri Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu. Form, şirketiniz ve Form tanıyıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler ekibi tarafından isteğiniz onaylanırsa, hizmete erişmek yönergeler içeren bir e-posta alacaksınız.

## <a name="what-it-does"></a>Ne yapar?

Girişinizi göndermek, algoritma, küme türleri, formlar hangi anahtarları ve tablolar varsa bulur ve değerleri anahtarları ve girişleri tablolara ilişkilendirilecek öğrenir eğitir. Denetimsiz öğrenme el ile veri etiketleme veya yoğun kodlama ve Bakım düzeni ve alanları ve girişleri arasındaki ilişkileri anlamak model sağlar. Aksine, modelleri standart hale getirilmiş verilerin taşınmasını ve daha az doğru geleneksel biçimlerinden farklılık göstermesi giriş malzemesi ile önceden eğitilen makine öğrenimi, sektöre özgü forms ister.

Modeli eğitilir sonra test etmek, yeniden eğitme ve sonunda güvenilir bir şekilde gereksinimlerinize göre daha fazla formlardaki verileri ayıklamak için kullanın.

## <a name="what-it-includes"></a>Neleri içerir

Form tanıyıcı, REST API olarak kullanılabilir. Oluşturabilir, eğitmek ve API çağırarak bir modeli Puanlama ve isteğe bağlı olarak yerel bir Docker kapsayıcısı içinde modelini çalıştırabilirsiniz.

## <a name="input-requirements"></a>Giriş gereksinimleri

Form tanıyıcı aşağıdaki gereksinimleri karşılaması giriş belgeler üzerinde çalışır:

* JPG, PNG veya PDF biçiminde (metin veya taranan). Katıştırılmış PDF metin karakter ayıklama ve konumda hata kaybetme riski olduğu tercih edilir.
* Dosya boyutu küçüktür 4 megabayt (MB) olmalıdır
* Görüntüler için boyutları 4200 x 4200 piksel ve 50 x 50 arasında olmalıdır
* Kağıt belgelerden taranan, yüksek kaliteli taramalar forms olmalıdır
* Latin alfabesi (İngilizce karakterler) kullanmanız gerekir
* Yazdırılan veriler (değil resimlerdeki el yazısı)
* Anahtarlar ve değerler içermelidir
* Anahtarları yukarıda veya değerlerin ancak değil aşağıda sola veya sağa görünür.

Ayrıca, Form tanıyıcı henüz aşağıdaki giriş veri türlerini desteklemez:

* Karmaşık tablolar (iç içe yerleştirilmiş tablolar, birleştirilen üstbilgi veya hücre vb.) 
* Onay kutusu veya radyo düğmeleri
* PDF belgeleri 50 sayfaları uzun

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım:** Azure portalında bir Form tanıyıcı kaynağı oluşturun.

**2. adım:** Hızlı Başlangıç için uygulamalı deneyim deneyin:
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve REST API ile cURL kullanarak form verilerini ayıklama](quickstarts/curl-train-extract.md)
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve Python ile REST API kullanarak form verilerini ayıklama](quickstarts/python-train-extract.md)

Öğrenme amacıyla ücretsiz hizmeti öneririz, ancak bu, ücretsiz sayfaların sayısını ayda 500 sayfalara sınırlı olduğunu unutmayın.

**3. adım:** REST API kullanımı eğitmek ve forms yapılandırılmış verileri ayıklamak için aşağıdaki API'leri inceleyin.

| REST API | Açıklama |
|-----|-------------|
| Eğitim | Aynı türden 5 forms veya boş bir form kullanılarak formlarınızı analiz etmek için yeni bir modeli eğitin.  |
| Çözümleme  |Formun modeliniz özel anahtar-değer çiftleri ve tabloları ayıklamak için bir akış olarak geçirilen bir tek belge analiz edin.  |

Keşfedin [REST API başvuru belgesini](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Hizmet olarak sunulan bir [Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bir Azure hizmetinin altında [çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Verilerinizin sahipliği korunur ve yalnızca, sözleşmenizde açıklandığı gibi çevrimiçi hizmetleri sağlamak için kullanırız:

### <a name="processing-of-customer-data-ownership"></a>Müşteri verilerinin işlenmesini; sahipliği

Müşteri verilerini kullanılan veya aksi durumda yalnızca bu hizmetleri sağlamayla uyumlu amaçlar da dahil olmak üzere çevrimiçi hizmetler müşteri için işlenir. Microsoft'un kullanın veya aksi halde müşteri verilerini işlemek veya reklam ya da benzeri ticari amaçlarla bundan bilgi. Taraflar arasında müşteri başlık tüm hak, korur ve giriş ve müşteri verilerini İlginiz dahilinde. Microsoft, müşteri verilerini, müşteriye çevrimiçi hizmetleri sağlamak için Microsoft Müşteri verir hakları dışında hiçbir hak devralır.

Olarak tüm Bilişsel hizmetler ile Form tanıyıcı hizmeti kullanan geliştiricilerin Microsoft'un müşteri verilerini ilkelerinin bilmeniz gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

İzleyin bir [hızlı](quickstarts/curl-train-extract.md) kullanmaya başlamak için [tanıyıcı API'leri](https://aka.ms/form-recognizer/api).
