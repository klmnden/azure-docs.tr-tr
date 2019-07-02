---
title: Form Tanıma nedir?
titleSuffix: Azure Cognitive Services
description: Form ve tablo verilerini ayrıştırmak için Form tanıyıcı kullanmayı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: a17e47fb059c23ab2e6eb69f3cfe6f2f8d0e51b2
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502902"
---
# <a name="what-is-form-recognizer"></a>Form Tanıma nedir?

Azure Form tanıyıcı belirleyin ve formu belgelerden anahtar/değer çiftleri ve tablo verilerini ayıklamak için makine öğrenimi teknolojisi kullanan bir bilişsel bir hizmettir. Ardından, asıl dosyadaki ilişkileri de içeren yapılandırılmış verinin çıktısını verir. Basit bir REST API kullanarak kolayca, iş akışı veya uygulama tümleştirmenize karmaşıklığını azaltmak için Özel Form tanıyıcı modelinizi çağırabilirsiniz. Başlamak için beş doldurulmuş form belgeler veya doldurulmuş iki forms artı, giriş malzeme olarak aynı türde boş bir form yeterlidir. Uyarlandığından doğru sonuçlar, belirli bir içeriğe ağır el ile müdahale veya kapsamlı veri bilimi uzmanlığına gerek kalmadan hızla alın.

## <a name="custom-models"></a>Özel modelleri

Kendi veri Özel Form tanıyıcı modeli eğitir ve başlatmak için beş örnek girdi biçimleri yeterlidir. Girişinizi göndermek, algoritma türüne göre formları kümeleri, hangi anahtarları ve tabloları mevcut olduğundan bulur ve değerleri anahtarları ve girişleri tablolara ilişkilendirir. Ardından, asıl dosyadaki ilişkileri de içeren yapılandırılmış verinin çıktısını verir. Modeli eğitme sonra test ve onu yeniden eğitme ve sonunda güvenilir bir şekilde gereksinimlerinize göre daha fazla formlardaki verileri ayıklamak için kullanın.

Denetimsiz öğrenme, el ile veri etiketleme veya yoğun kodlama ve bakım olmadan, modelin alanlar ve girdiler arasındaki düzen ve ilişkileri anlamasına izin verir. Aksine, standartlaştırılmış veri önceden eğitilen makine öğrenimi modelleri gerektirir. Bunlar, sektöre özel formlar gibi geleneksel biçimlerinden farklılık göstermesi giriş malzemesi ile daha az doğru.

## <a name="pre-built-receipt-model"></a>Önceden oluşturulmuş giriş modeli

Form tanıyıcı Ayrıca satış makbuzuna okumak için bir model içerir. Bu model, işlem, ticari bir bilgi, vergiler ve toplamları miktarda ve daha fazlası tarih ve saat gibi anahtar bilgisini ayıklar. Ayrıca, tanı ve tüm metni bir alındığında döndürmek için önceden oluşturulmuş girişler modeli eğitilir.

## <a name="what-it-includes"></a>Neleri içerir

Form tanıyıcı, REST API olarak kullanılabilir. Oluşturma, eğitme ve özel bir modeli Puanlama veya bu API'leri çağırarak önceden oluşturulmuş modele erişim. İsterseniz, eğitmek ve yerel bir Docker kapsayıcısı içinde özel modelleri çalıştırmak.

## <a name="input-requirements-custom-model"></a>Giriş gereksinimlerine (özel model)

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="request-access"></a>Erişim izni iste

Form tanıyıcı erişimi sınırlı önizlemede kullanıma sunulmuştur. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu. Form, şirketiniz ve Form tanıyıcı kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Azure Bilişsel hizmetler ekibi tarafından isteğiniz onaylanırsa, hizmete erişmek için yönergeleri içeren bir e-posta alacaksınız.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

**1. adım:** Azure portalında bir Form tanıyıcı kaynağı oluşturun.

**2. adım:** REST API'yi kullanmak için bir hızlı başlangıcı izleyin:
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve REST API ile cURL kullanarak form verileri ayıklayın](quickstarts/curl-train-extract.md)
* [Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve Python ile REST API kullanarak form verileri ayıklayın](quickstarts/python-train-extract.md)
* [Hızlı Başlangıç: CURL kullanarak giriş verilerini ayıklama](quickstarts/curl-receipts.md)
* [Hızlı Başlangıç: Python kullanarak giriş verilerini ayıklama](quickstarts/python-receipts.md)

Teknoloji öğrenirken ücretsiz hizmet kullanmanızı öneririz. Boş sayfa sayısı başına aylık 500 sınırlı olduğunu aklınızda bulundurun.

**3. adım:** REST API'leri İnceleme

Eğitim ve forms yapılandırılmış verileri ayıklamak için aşağıdaki API'leri kullanın.

|||
|---|---|
| Model Eğitme| Aynı türde beş formları kullanarak formlarınızı analiz etmek için yeni bir modeli eğitin. Ya da boş bir form ile doldurulmuş iki biçimde eğitin.  |
| Form analiz edin |Formun modeliniz özel anahtar/değer çiftleri ve tabloları ayıklamak için bir akış olarak geçirilen bir tek belge analiz edin.  |
| Giriş analiz edin |Temel bilgi ve başka bir giriş metin ayıklamak için bir tek giriş belgesi analiz edin.|

Keşfedin [REST API başvuru belgeleri](https://aka.ms/form-recognizer/api) daha fazla bilgi için. 

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Bu hizmet olarak sunulan bir [Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bir Azure hizmetinin altında [çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Tüm bilişsel hizmetler ile gibi Form tanıyıcı hizmeti kullanan geliştiriciler Microsoft müşteri verilerini ilkeleri haberdar olmanız gerekir. Bkz: [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) daha fazla bilgi için Microsoft Trust Center.

## <a name="next-steps"></a>Sonraki adımlar

Tamamlanması bir [hızlı](quickstarts/curl-train-extract.md) kullanmaya başlamak için [tanıyıcı API'leri](https://aka.ms/form-recognizer/api).
