---
title: Form Tanıma nedir?
titleSuffix: Azure Cognitive Services
description: Form ve tablo verilerini ayrıştırmak için Form tanıyıcı kullanmayı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: 8fb382227c71fce7ebe062057adf5edfb90a1a92
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601633"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

Azure Form Tanıma, form belgelerinden anahtar-değer çiftlerini ve tablo verilerini tanımlamak ve ayıklamak için makine öğrenimi teknolojisini kullanan bir bilişsel hizmettir. Ardından, asıl dosyadaki ilişkileri de içeren yapılandırılmış verinin çıktısını verir. Basit bir REST API kullanarak kolayca, iş akışı veya uygulama tümleştirmenize karmaşıklığını azaltmak için Özel Form tanıyıcı modelinizi çağırabilirsiniz. Başlamak için beş form belgelerine ya da boş bir form, giriş malzeme olarak aynı türde yeterlidir. Uyarlandığından doğru sonuçlar, belirli bir içeriğe ağır el ile müdahale veya kapsamlı veri bilimi uzmanlığına gerek kalmadan hızla alın.

## <a name="request-access"></a>Erişim izni iste
Form tanıyıcı erişimi sınırlı önizlemede kullanıma sunulmuştur. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu. Form, şirketiniz ve Form tanıyıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler ekibi tarafından isteğiniz onaylanırsa, hizmete erişmek için yönergeleri içeren bir e-posta alacaksınız.

## <a name="what-it-does"></a>Ne yapar?

Girişinizi göndermek, algoritma, küme türleri, formlar hangi anahtarları ve tablolar varsa bulur ve değerleri anahtarları ve girişleri tablolara ilişkilendirilecek öğrenir eğitir. Denetimsiz öğrenme, el ile veri etiketleme veya yoğun kodlama ve bakım olmadan, modelin alanlar ve girdiler arasındaki düzen ve ilişkileri anlamasına izin verir. Bunun aksine, önceden eğitilen makine öğrenimi modelleri, standartlaştırılmış veri gerektirir ve sektöre özel formlar gibi geleneksel biçimlerinden farklılık göstermesi giriş malzemesi ile kullanıldığında azdır.

Modeli eğitme sonra test ve onu yeniden eğitme ve sonunda güvenilir bir şekilde gereksinimlerinize göre daha fazla formlardaki verileri ayıklamak için kullanın.

## <a name="what-it-includes"></a>Neleri içerir

Form tanıyıcı, REST API olarak kullanılabilir. Oluşturmak, eğitmek ve API çağırarak bir modeli Puanlama. İsterseniz, yerel bir Docker kapsayıcısı içinde modelini çalıştırabilirsiniz.

## <a name="input-requirements"></a>Giriş gereksinimleri

Form tanıyıcı bu gereksinimleri karşılayan giriş belgeler üzerinde çalışır:

* JPG, PNG veya PDF biçiminde olmalıdır (metin veya taranan). Metin katıştırılmış PDF karakter ayıklama ve konumda hata kaybetme riski olduğu için idealdir.
* Dosya boyutu küçüktür 4 megabayt (MB) olmalıdır.
* Görüntüler için boyutları 4200 x 4200 piksel ve 50 x 50 piksel arasında olmalıdır.
* Kağıt belgelerden taranan forms yüksek kaliteli taramalar olması gerekir.
* Metin Latin alfabesi (İngilizce karakterler) kullanmanız gerekir.
* Veri (değil resimlerdeki el yazısı) yazdırılan gerekir.
* Veri anahtarları ve değerleri içermesi gerekir.
* Anahtarları yukarıda veya değerlerin ancak değil aşağıda sola veya sağa görünür.

Form tanıyıcı şu anda bu giriş veri türlerini desteklemez:

* Karmaşık tablolar (iç içe yerleştirilmiş tablolar, birleştirilen üstbilgi veya hücre vb.).
* Onay kutusu veya radyo düğmeleri.
* PDF belgeleri 50 sayfaları uzun.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım:** Azure portalında bir Form tanıyıcı kaynağı oluşturun.

**2. adım:** Hızlı Başlangıç için uygulamalı deneyim deneyin:
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve REST API ile cURL kullanarak form verileri ayıklayın](quickstarts/curl-train-extract.md)
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve Python ile REST API kullanarak form verileri ayıklayın](quickstarts/python-train-extract.md)

Ücretsiz hizmet teknoloji öğrenirken kullanır, ancak ücretsiz sayfa sayısı başına aylık 500 sayfalara sınırlı olduğunu aklınızda bulundurun öneririz.

**3. adım:** REST API'leri İnceleme

Eğitim ve forms yapılandırılmış verileri ayıklamak için aşağıdaki API'leri kullanın.

| REST API | Açıklama |
|-----|-------------|
| Eğit | Aynı türden beş forms veya boş bir form kullanılarak formlarınızı analiz etmek için yeni bir modeli eğitin.  |
| Çözümle  |Formun modeliniz özel anahtar-değer çiftleri ve tabloları ayıklamak için bir akış olarak geçirilen bir tek belge analiz edin.  |

Keşfedin [REST API başvuru belgesini](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Bu hizmet olarak sunulan bir [Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bir Azure hizmetinin altında [çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Tüm bilişsel hizmetler ile gibi Form tanıyıcı hizmeti kullanan geliştiriciler Microsoft müşteri verilerini ilkeleri haberdar olmanız gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Tamamlanması bir [hızlı](quickstarts/curl-train-extract.md) kullanmaya başlamak için [tanıyıcı API'leri](https://aka.ms/form-recognizer/api).
