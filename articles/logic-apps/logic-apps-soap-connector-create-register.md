---
title: "Oluşturma ve SOAP bağlayıcılar - Azure Logic Apps kaydetme | Microsoft Docs"
description: "Azure Logic Apps’te kullanılmak üzere SOAP bağlayıcıları ayarlama"
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
ms.date: 10/24/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 031762e5639fc52e0b0a6a5bf8d12db25da25e12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Oluşturma ve Azure Logic Apps içinde SOAP bağlayıcılar kaydetme

SOAP hizmetlerini mantıksal uygulama iş akışlarınızla tümleştirmek için Basit Nesne Erişim Protokolü (SOAP) hizmetinizi açıklayan Web Hizmetleri Açıklama Dili’ni (WSDL) kullanarak özel bir SOAP bağlayıcısı oluşturup kaydedebilirsiniz. SOAP bağlayıcıları önceden oluşturulmuş bağlayıcılar gibi çalıştığından, bunları mantıksal uygulamalarınızdaki diğer bağlayıcılarla aynı şekilde kullanabilirsiniz.


## <a name="prerequisites"></a>Önkoşullar

SOAP bağlayıcınızı kaydetmek için aşağıdaki öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) ile başlayabilirsiniz. Aksi takdirde, bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/) kaydolun.

* Buradaki herhangi bir öğe:
  * SOAP hizmetinizi ve API'leri tanımlayan bir WSDL’nin URL’si
  * SOAP hizmetinizi ve API'leri tanımlayan bir WSDL dosyası

  Bu öğretici için [Siparişler SOAP Hizmeti](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl) örneğimizi kullanabilirsiniz.

* İsteğe bağlı: Özel bağlayıcınız için simge olarak kullanılacak bir görüntü


## <a name="1-create-your-connector"></a>1. Bağlayıcınızı oluşturma

1. Azure portalında, ana Azure menüsünden **Yeni**’yi seçin. Arama kutusuna filtreniz olarak “logic apps bağlayıcısı” yazın ve Enter tuşuna basın.

   ![Yeni, “logic apps bağlayıcısı” ifadesini aratın](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Sonuç listesinden **Logic Apps Bağlayıcısı** > **Oluştur**’u seçin.

   ![Logic Apps bağlayıcısı oluşturma](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Bağlayıcınızı kaydetmek için tabloda açıklandığı gibi ayrıntıları sağlayın. İşiniz bittiğinde **Panoya sabitle** > **Oluştur**’u seçin.

   ![Mantıksal Uygulama özel bağlayıcı ayrıntıları](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Özellik | Önerilen değer | Açıklama | 
   | -------- | --------------- | ----------- | 
   | **Ad** | *soap-connector-name* | Bağlayıcınıza bir ad verin. | 
   | **Abonelik** | *Azure-subscription-name* | Azure aboneliğinizi seçin. | 
   | **Kaynak grubu** | *Azure-resource-group-name* | Azure kaynaklarınızı düzenlemek için bir Azure grubu oluşturun veya seçin. | 
   | **Konum** | *deployment-region* | Bağlayıcınız için bir dağıtım bölgesi seçin. | 
   |||| 

   Azure bağlayıcınızı dağıttıktan sonra Logic Apps bağlayıcı menüsü otomatik olarak açılır. 
   Aksi takdirde, Azure panosundan bağlayıcınızı seçin.

## <a name="2-define-your-connector"></a>2. Bağlayıcınızı tanımlama

Şimdi, bağlayıcınızı oluşturmak için WSDL dosyasını veya URL’sini, bağlayıcınızın kullandığı kimlik doğrulamasını ve soap bağlayıcınızın sağladığı eylem ve tetikleyicileri belirtin


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Bağlayıcınızın WSDL dosyasını veya URL'sini belirtin

1. Bağlayıcınızın menüsünde **Logic Apps Bağlayıcısı** zaten seçili değilse bunu seçin. Araç çubuğunda **Düzenle**'yi seçin.

   ![Özel bağlayıcısını Düzenle](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. SOAP bağlayıcınızın eylem ve tetikleyicilerini oluşturmak, korumak ve tanımlamak üzere bu tablolarda ayrıntıları sağlayabilmeniz için **Genel**’i seçin.

   1. **Özel bağlayıcılar** için, API’nizi açıklayan WSDL dosyasını sağlayabilmeniz için **API Uç Noktanıza** yönelik **SOAP**’yi seçin.

      ![API’nizin WSDL dosyasını sağlama](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Seçenek | Biçim |Açıklama | 
      | ------ | ------ | ----------- | 
      | **Dosyadan karşıya WSDL yükleme** | *WSDL dosyası* | WSDL dosyanızın konumuna göz atın ve bu dosyayı seçin. | 
      | **URL'den karşıya WSDL yükleme** | http://*path-to-wsdl-file* | Hizmetinizin WSDL dosyasının URL’sini sağlayın. | 
      | **SOAP'den REST'e** |   | SOAP hizmetindeki API’leri REST API’lere dönüştürme. | 
      |||| 

   2. **Genel bilgiler** için bağlayıcınıza ait bir simgeyi karşıya yükleyin. 
   Genellikle **Açıklama**, **Ana Bilgisayar** ve **Temel URL** alanları WSDL dosyanızdan otomatik olarak doldurulur. 
   Aksi takdirde, bu bilgileri tabloda açıklandığı gibi ekleyin ve **Devam**’ı seçin. 

      ![Bağlayıcı ayrıntıları](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Seçenek veya ayar | Biçim | Açıklama | 
      | ----------------- | ------ | ----------- | 
      | **Karşıya Simge Yükleme** | *png-or-jpg-file-under-1-MB* | Bağlayıcınızı temsil eden bir simge <p>Renk: Tercihen renkli bir arka plan üzerine beyaz logo. <p>Boyutlar: 230 piksel bir kare içinde yaklaşık 160 piksel boyutunda logo | 
      | **Simge arka plan rengi** | *icon-brand-color-hexadecimal-code* | <p>Simgenizin arkasındaki, simge dosyanızın arka plan rengiyle eşleşen renk. <p>Biçim: Onaltılık. Örneğin, #007ee5 mavi rengi temsil eder. | 
      | **Açıklama** | *connector-description* | Bağlayıcınız için kısa bir açıklama sağlayın. | 
      | **Ana Bilgisayar** | *connector-host* | SOAP hizmetiniz için ana bilgisayar etki alanını sağlayın. | 
      | **Temel URL** | *connector-base-URL* | SOAP hizmetiniz için temel URL’yi sağlayın. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Bağlayıcınızın kullandığı kimlik doğrulamasını açıklayın

1. Şimdi bağlayıcınızın kullandığı kimlik doğrulamasını gözden geçirmek veya açıklamak için **Güvenlik**’i seçin. Kimlik doğrulaması, hizmetiniz ile herhangi bir istemci arasında kullanıcılarınızın kimliklerinin uygun şekilde akışının gerçekleştirilmesini sağlar.

   Bağlayıcınızın **Kimlik doğrulama türü** varsayılan olarak **Kimlik doğrulaması yok** olarak ayarlanır.
   
   ![Kimlik doğrulaması türü](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Kimlik doğrulama türünü değiştirmek için **Düzenle**’yi seçin. **Temel kimlik doğrulaması**’nı seçebilirsiniz. Varsayılan değerler dışındaki parametre etiketlerini kullanmak için **Parametre etiketi** altında bunları güncelleştirin.

   ![Temel kimlik doğrulaması](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Güvenlik bilgilerini girdikten sonra bağlayıcınızı kaydetmek için sayfanın üst kısmından **Bağlayıcıyı güncelleştir**’i ve sonra **Devam**’ı seçin. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Bağlayıcınızın eylem ve tetikleyicilerini gözden geçirme, güncelleştirme veya tanımlama

1. Şimdi kullanıcılarınızın iş akışlarına ekleyebileceği eylem ve tetikleyicileri gözden geçirmek, düzenlemek veya yenilerini tanımlamak için **Tanım**’ı seçin.

   Eylem ve tetikleyiciler, **Tanım** sayfasını otomatik olarak dolduran WSDL dosyanızda tanımlanan işlemleri temel alır ve hem istek hem de yanıt değerlerini içerir. Bu nedenle, bu sayfada gerekli işlemler zaten görünüyorsa herhangi bir değişiklik yapmadan kayıt işleminin bir sonraki adımına geçebilirsiniz.

   ![Bağlayıcı tanımı](./media/logic-apps-soap-connector-create-register/definition.png)

2. İsteğe bağlı olarak, var olan eylemler ve Tetikleyicileri düzenleme veya yeni bir tane eklemek istiyorsanız, [bu adımlarla devam](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Bağlayıcınızı oluşturmayı tamamlama

Hazır olduğunuzda bağlayıcınızı dağıtmak için **Bağlayıcıyı Güncelleştir**’i seçin. 

Tebrikler! Artık bir mantıksal uygulama oluşturduğunuzda bağlayıcınızı Logic Apps Tasarımcısı’nda bulabilir ve mantıksal uygulamanıza ekleyebilirsiniz.

![Logic Apps Tasarımcısı’nda bağlayıcınızı bulma](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Bağlayıcınızı diğer Logic Apps kullanıcılarıyla paylaşma

Kayıtlı ve sertifikasız özel bağlayıcılar, Microsoft tarafından yönetilen bağlayıcılar gibi çalışır, ancak *yalnızca* bağlayıcının yazarı ve mantıksal uygulamalar için bu uygulamaların dağıtıldığı bölgede aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip olanlar tarafından görüntülenip kullanılabilir. Paylaşım isteğe bağlı olsa da, bağlayıcılarınızı başka kullanıcılarla paylaşmak istediğiniz senaryolarınız olabilir. 

> [!IMPORTANT]
> Bir bağlayıcı paylaştığınızda, başkaları da bu bağlayıcıya bağımlı olmaya başlayabilir. 
> ***Bağlayıcınız silindiğinde, bu bağlayıcıya olan tüm bağlantılar silinir.***
 
Bağlayıcınızı harici kullanıcılar bu bu gibi bir durumda sınırları dışında tüm Logic Apps kullanıcılarla paylaşmak için [Bağlayıcınızı Microsoft sertifika için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>SSS

**S:** SOAP bağlayıcısı genel kullanıma sunuldu mu? </br>
**C:** SOAP bağlayıcısı **Önizleme** aşamasındadır ve henüz genel kullanıma sunulmamıştır.

**S:** SOAP bağlayıcısı için herhangi bir kısıtlama ve bilinen sorun var mı? </br>
**C:** Evet, bkz. [SOAP bağlayıcısı ile ilgili kısıtlamalar ve bilinen sorunlar](../api-management/api-management-api-import-restrictions.md#wsdl).

**S:** Özel bağlayıcılar için herhangi bir sınırlama var mı? </br>
**C:** Evet, bkz. [özel bağlayıcı sınırlamaları](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Destek alın

* Geliştirme ve ekleme konusunda destek almak ya da kayıt sihirbazında bulunmayan özellikleri istemek için [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com) adresine başvurun. Microsoft, bu hesaba gönderilen geliştirici sorularını ve sorunlarını izleyip bunları uygun ekibe yönlendirir.

* Soru sormak veya soruları yanıtlamak ya da diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Logic Apps’in geliştirilmesine yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın. 

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı: Bağlayıcınızı Onayla](../logic-apps/custom-connector-submit-certification.md)
* [Özel bağlayıcı hakkında SSS](../logic-apps/custom-connector-faq.md)