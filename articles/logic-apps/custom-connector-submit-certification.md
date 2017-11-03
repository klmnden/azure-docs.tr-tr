---
title: "Özel bağlayıcılar - Azure Logic Apps onaylamak | Microsoft Docs"
description: "Bağlayıcılar Azure mantıksal uygulamaları, Microsoft Flow ve Microsoft PowerApps tüm kullanıcılar için kullanılabilir hale"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9bcb138c181df0c12548cc5dc721542fd9c9ba68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="submit-your-connectors-for-microsoft-certification"></a>Microsoft Sertifika, bağlayıcıları gönderme

Özel bağlayıcılar Azure mantıksal uygulamaları, Microsoft Flow ve Microsoft PowerApps tüm kullanıcılar için genel olarak kullanılabilir hale getirmek Microsoft'a Bağlayıcınızı gözden geçirme, doğrulama ve yayımlama için onay için gönderin. 

## <a name="certification-criteria"></a>Sertifika ölçütleri

| Özellik | Ayrıntılar | Gerekli ya da önerilen |
|------------|---------|-------------------------|
| Yazılım olarak-hizmet (SaaS) uygulaması | İyi Logic Apps, akış ve PowerApps uygun kullanıcı senaryosu karşılar. | Gerekli |
| Kimlik doğrulama türü | API'nizi, OAuth2, API anahtarı veya temel kimlik doğrulaması desteklemesi gerekir. | Gerekli | 
| Destek | Böylece müşteriler Yardım alabilir destek bağlantı sağlamanız gerekir. | Gerekli | 
| Kullanılabilirlik ve çalışır durumda kalma süresi | Uygulamanızı sahip en az % 99,9 çalışma süresi. | Önerilen | 
|||| 

Ayrıca, bir Microsoft iş ortağı veya bağımsız yazılım satıcısı (ISV) olmadığınız ve onaylamak ve genel olarak bir bağlayıcı yayın istiyorsanız, ya da temel alınan hizmet veya API kullanmak için mevcut açık hakları sahip olmanız gerekir.

## <a name="validation-phases"></a>Doğrulama aşamaları

Microsoft, Bağlayıcınızı değerlendirmek için bu doğrulama aşamalar kullanır:

| Doğrulama aşaması | Açıklama | 
| ----- | ----------- |
| İşlev | Microsoft, Logic Apps, akış ve PowerApps Bağlayıcınızı bu hataları için test eder: <p>-Geçersiz OpenAPI (Swagger) veya JSON özel Bağlayıcı Sihirbazı'ndan tanımı bölümde görünür hataları <p>-Özel Bağlayıcı Sihirbazı'ndan Test bölümde görünür çalışma zamanı ve şema doğrulama hataları | 
| İçerik | Microsoft Bağlayıcınızı her bir varlık için kolay adlar ve açıklamalar kullandığını denetler. Her işlem, giriş parametresi ve bağlantı ucunun Swagger yanıt özniteliği bu öğeleri sahip olduğundan emin olun: <p>- [Özet](../logic-apps/custom-connector-openapi-extensions.md#summary) <br>- [Açıklama](../logic-apps/custom-connector-openapi-extensions.md#description) </br>- [Görünürlük bilgileri](../logic-apps/custom-connector-openapi-extensions.md#visibility) | 
||| 

## <a name="nominate-and-submit-your-connector-to-microsoft-for-certification"></a>Belirler ve sertifika için Microsoft'a Bağlayıcınızı gönderin

Sertifika için uygulamak için aşağıdaki adımları izleyin:

1. **Belirler**

   1. [Adaylığı gönderme](https://go.microsoft.com/fwlink/?linkid=848754).

      1. Sayfanın alt kısmındaki seçin **Bize Ulaşın**.
      2. Formu doldurun ve seçin **ISV bağlayıcı program ve ortak pazarlama hakkında sorular**.
      3. Seçin **gönderme**.

   2. Karşılıklı olmayan Gizlilik Sözleşmesi ve aldığınız iş ortağı sözleşmesi oturum açın. 

      Microsoft, devam etmeden önce bu imzalı sözleşmeleri gerektirir. 
      Biz sonra Bağlayıcınızı sertifika ölçütleri karşılayıp karşılamadığını kontrol edebilirsiniz. 
   
   3. Bağlayıcınızı onaylanırsa Microsoft eklenmesi için yönergeler bildirir.
    
2. **Gözden geçirme**

   1. Bu bilgiler Adaylığı kişiniz gözden geçirme için gönder:

      * API'nizi tanımlayan OpenAPI dosyasını
      * Bağlayıcınızı temsil eden simge dosyası (.png veya .jpg) 
      
        Simge ~ 160 piksel logosu 230 piksel kare içinde olması gerekir. 
        Renkli arka plan üzerinde beyaz logo tercih edilir.
      
      * Simge 's marka renk renkli arka planda simge dosyası eşleşmelidir onaltılık biçimde

      * Doğrulama için bir test hesabı
      * Destek ile iletişime geçin

   2. Size daha fazla bilgiye ihtiyacınız varsa, sizinle iletişime geçer.

3. **Yayımlama**

    Biz, bağlayıcı işlevselliği ve içerik doğrulama sonra tüm ürünleri ve bölgeler arasında biz bağlayıcı dağıtım için hazırlama.
    
    Varsayılan olarak, tüm bağlayıcılar "premium" bağlayıcılar yayımlanır. 
    Uygulamanızı Azure ile oluşturulduysa, Office 365 Kurumsal planına sahip tüm kullanıcılar için kullanılabilir "standart" bir bağlayıcı olarak Bağlayıcınızı listelemek için uygulayabilirsiniz. 
    Daha fazla ayrıntı için Adaylığı kişiniz isteyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)
* [Özel bağlayıcı genel bakış](../logic-apps/custom-connector-overview.md)