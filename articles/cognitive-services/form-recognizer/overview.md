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
ms.openlocfilehash: 82ee2aa5627ac5fa4584f5af6b6b80cc2813c667
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441828"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

Azure Form Tanıma, form belgelerinden anahtar-değer çiftlerini ve tablo verilerini tanımlamak ve ayıklamak için makine öğrenimi teknolojisini kullanan bir bilişsel hizmettir. Ardından, asıl dosyadaki ilişkileri de içeren yapılandırılmış verinin çıktısını verir. Basit bir REST API kullanarak kolayca, iş akışı veya uygulama tümleştirmenize karmaşıklığını azaltmak için Özel Form tanıyıcı modelinizi çağırabilirsiniz. Başlamak için beş doldurulmuş form belgeler veya doldurulmuş iki forms artı, giriş malzeme olarak aynı türde boş bir form yeterlidir. Uyarlandığından doğru sonuçlar, belirli bir içeriğe ağır el ile müdahale veya kapsamlı veri bilimi uzmanlığına gerek kalmadan hızla alın.

## <a name="request-access"></a>Erişim izni iste
Form tanıyıcı erişimi sınırlı önizlemede kullanıma sunulmuştur. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu. Form, şirketiniz ve Form tanıyıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler ekibi tarafından isteğiniz onaylanırsa, hizmete erişmek için yönergeleri içeren bir e-posta alacaksınız.

## <a name="what-it-does"></a>Ne yapar?

Girişinizi göndermek, algoritma, küme türleri, formlar hangi anahtarları ve tablolar varsa bulur ve değerleri anahtarları ve girişleri tablolara ilişkilendirilecek öğrenir eğitir. Denetimsiz öğrenme, el ile veri etiketleme veya yoğun kodlama ve bakım olmadan, modelin alanlar ve girdiler arasındaki düzen ve ilişkileri anlamasına izin verir. Bunun aksine, önceden eğitilen makine öğrenimi modelleri, standartlaştırılmış veri gerektirir ve sektöre özel formlar gibi geleneksel biçimlerinden farklılık göstermesi giriş malzemesi ile kullanıldığında azdır.

Modeli eğitme sonra test ve onu yeniden eğitme ve sonunda güvenilir bir şekilde gereksinimlerinize göre daha fazla formlardaki verileri ayıklamak için kullanın.

## <a name="what-it-includes"></a>Neleri içerir

Form tanıyıcı, REST API olarak kullanılabilir. Oluşturmak, eğitmek ve API çağırarak bir modeli Puanlama. İsterseniz, yerel bir Docker kapsayıcısı içinde modelini çalıştırabilirsiniz.

## <a name="input-requirements"></a>Giriş gereksinimleri

[!INCLUDE [input requirements](./includes/input-requirements.md)]

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
| Eğitim | Aynı türde beş formları kullanarak formlarınızı analiz etmek için yeni bir modeli eğitin. Ya da boş bir form ile doldurulmuş iki biçimde eğitin.  |
| Çözümle  |Formun modeliniz özel anahtar-değer çiftleri ve tabloları ayıklamak için bir akış olarak geçirilen bir tek belge analiz edin.  |

Keşfedin [REST API başvuru belgesini](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Bu hizmet olarak sunulan bir [Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bir Azure hizmetinin altında [çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Tüm bilişsel hizmetler ile gibi Form tanıyıcı hizmeti kullanan geliştiriciler Microsoft müşteri verilerini ilkeleri haberdar olmanız gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Tamamlanması bir [hızlı](quickstarts/curl-train-extract.md) kullanmaya başlamak için [tanıyıcı API'leri](https://aka.ms/form-recognizer/api).
