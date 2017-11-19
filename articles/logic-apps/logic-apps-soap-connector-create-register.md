---
title: "Oluşturma ve SOAP bağlayıcılar - Azure Logic Apps kaydetme | Microsoft Docs"
description: "SOAP bağlayıcılar Azure mantıksal uygulamaları kullanmak için ayarlama"
author: divyaswarnkar
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
ms.author: LADocs; divswa
ms.openlocfilehash: 0323b0f7ee03dce209d5a71c6711988a34ba7633
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Oluşturma ve Azure Logic Apps içinde SOAP bağlayıcılar kaydetme

SOAP Hizmetleri mantığı uygulama akışlarında tümleştirmek için oluşturabilir ve Web Hizmetleri Açıklama Dili (SOAP hizmetinizi tanımlayan WSDL) kullanarak özel bir Basit Nesne Erişim Protokolü (SOAP) bağlayıcı kaydedin. SOAP bağlayıcılar, diğer bağlayıcıları aynı şekilde logic apps içinde kullanabilmeniz için önceden oluşturulmuş bağlayıcılar gibi çalışır.


## <a name="prerequisites"></a>Ön koşullar

SOAP Bağlayıcınızı kaydetmek için bu öğeler gerekir:

* Azure aboneliği. Bir aboneliğiniz yoksa ile başlayabilirsiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Aksi takdirde kaydolun bir [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/pricing/purchase-options/).

* Herhangi bir öğeyi burada:
  * SOAP hizmetinizi tanımlayan bir WSDL ve API'ler için bir URL
  * SOAP hizmetiniz ve API'leri tanımlayan bir WSDL dosyası

  Bu öğretici için Bizim örneğimizde kullanabilirsiniz [siparişleri SOAP servis](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* İsteğe bağlı: özel bağlayıcı için bir simgesi olarak kullanılacak bir görüntü


## <a name="1-create-your-connector"></a>1. Bağlayıcınızı oluşturun

1. Azure portalında, ana Azure menüsünden seçin **yeni**. Arama kutusuna "logic apps bağlayıcı", filtre olarak girin ve Enter tuşuna basın.

   !["Logic apps bağlayıcı" Yeni arama](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Sonuçları listesinden seçin **Logic Apps bağlayıcı** > **oluşturma**.

   ![Logic Apps bağlayıcı oluşturun](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Tabloda açıklandığı gibi Bağlayıcınızı kayıt için ayrıntılı bilgiler sağlar. İşiniz bittiğinde seçin **panoya Sabitle** > **oluşturma**.

   ![Mantıksal uygulama özel bağlayıcı ayrıntıları](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Özellik | Önerilen değer | Açıklama | 
   | -------- | --------------- | ----------- | 
   | **Ad** | *SOAP bağlayıcı adı* | Bağlayıcı için bir ad sağlayın. | 
   | **Abonelik** | *Azure abonelik adı* | Azure aboneliğinizi seçin. | 
   | **Kaynak grubu** | *Azure kaynak grubu adı* | Azure kaynaklarınızı düzenlemek için bir Azure grubu seçin veya oluşturun. | 
   | **Konum** | *Dağıtım bölgesi* | Bağlayıcınızı için dağıtım bölgeyi seçin. | 
   |||| 

   Logic apps bağlayıcı menüsü Azure Bağlayıcınızı dağıtıldıktan sonra otomatik olarak açılır. 
   Aksi durumda, Azure panodan soap Bağlayıcınızı seçin.

## <a name="2-define-your-connector"></a>2. Bağlayıcınızı tanımlayın

WSDL dosyaya veya URL'ye Bağlayıcınızı Bağlayıcınızı kullanır, kimlik doğrulama ve eylemleri ve soap Bağlayıcınızı sağlar Tetikleyiciler oluşturmak için şimdi belirtin


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Bağlayıcı için WSDL dosyası veya URL'yi belirtin

1. Seçili değilse, bağlayıcı'nın menüde seçin **Logic Apps bağlayıcı**. Araç çubuğunda seçin **Düzenle**.

   ![Özel bağlayıcısını Düzenle](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Seçin **genel** oluşturmak için bu tablolar Ayrıntılar sağlayabilir böylece güvenli hale getirme ve SOAP Bağlayıcınızı tetikleyiciler ve Eylemler tanımlama.

   1. İçin **özel Bağlayıcılar**seçin **SOAP** için **API uç noktası** API'nizi tanımlayan WSDL dosyasını sağlayabilir.

      ![API için WSDL dosyası sağlayın](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Seçenek | Biçim |Açıklama | 
      | ------ | ------ | ----------- | 
      | **WSDL dosya karşıya yükleme** | *WSDL dosyası* | WSDL dosya konumuna göz atın ve bu dosyayı seçin. | 
      | **URL'den WSDL karşıya yükle** | http://*yol için wsdl dosyası* | URL için hizmetinizin WSDL dosyası sağlar. | 
      | **KALANLAR SOAP** |   | API SOAP hizmetinde REST API dönüştürün. | 
      |||| 

   2. İçin **genel bilgiler**, Bağlayıcınızı için bir simgeyi karşıya yükleyin. 
   Genellikle, **açıklama**, **konak**, ve **taban URL** alanları WSDL dosyanızdan otomatik olarak doldurulur. 
   Ancak değilseniz, bu bilgileri tabloda açıklandığı şekilde ekleyin ve seçin **devam**. 

      ![Bağlayıcı ayrıntıları](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Seçenek veya ayarı | Biçim | Açıklama | 
      | ----------------- | ------ | ----------- | 
      | **Simgeyi karşıya yükleyin** | *PNG-or-jpg-File-Under-1-MB* | Bağlayıcınızı temsil eden bir simge <p>Renk: Tercihen beyaz logosu arka plan rengi. <p>Boyutlar: 230 piksel kare içinde ~ 160 piksel logosu bir | 
      | **Simge arka plan rengi** | *simge-brand-renk-onaltılık-code* | <p>Arka plan rengi, simge dosyanızdaki eşleşen simge arkasındaki rengi. <p>Biçim: onaltılık. Örneğin, #007ee5 mavi rengi temsil eder. | 
      | **Açıklama** | *Bağlayıcı açıklaması* | Bağlayıcınızı için kısa bir açıklama sağlayın. | 
      | **Ana bilgisayar** | *Bağlayıcı ana bilgisayar* | Ana bilgisayar etki alanı SOAP hizmetiniz için sağlar. | 
      | **Temel URL** | *Bağlayıcı taban URL'si* | SOAP hizmetiniz için temel URL sağlayın. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2B. Bağlayıcınızı kullandığı kimlik doğrulama açıklayın

1. Artık seçim **güvenlik** gözden geçirmek ya da Bağlayıcınızı kullandığı kimlik doğrulama açıklanmaktadır. Kimlik doğrulaması, kullanıcıların kimliklerini hizmetiniz ve istemciler arasında uygun şekilde akış emin olur.

   Varsayılan olarak, Bağlayıcınızı 's **kimlik doğrulama türü** ayarlanır **kimlik doğrulaması yok**.
   
   ![Kimlik doğrulama türü](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Kimlik doğrulama türünü değiştirmek için tercih **Düzenle**. Seçebileceğiniz **temel kimlik doğrulaması**. Varsayılan değerleri dışında parametre etiketleri kullanmak için bunların altında güncelleştirme **parametre etiket**.

   ![Temel kimlik doğrulaması](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Sayfanın üstündeki güvenlik bilgileri girdikten sonra Bağlayıcınızı kaydetmek üzere seçim yapın **güncelleştirme bağlayıcı**, ardından **devam**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Gözden geçirin, güncelleştirmeniz ya da eylemleri ve Bağlayıcınızı Tetikleyicileri tanımlayın

1. Artık seçim **tanımı** gözden geçirebilmeniz için düzenleyin veya yeni eylemleri ve kullanıcılar kendi iş akışlarına ekleyebilirler tetikleyicilerini tanımlayın.

   Eylemler ve tetikleyicileri, otomatik olarak doldurulması WSDL dosyanızda tanımlanan işlemleri üzerinde dayalı **tanımı** sayfasında ve istek ve yanıt değerlerini içerir. Gerekli işlemlerini zaten burada görünüyorsa, bu nedenle, kayıt işleminin sonraki adımına bu sayfada değişiklik yapmadan gidebilirsiniz.

   ![Bağlayıcı tanımı](./media/logic-apps-soap-connector-create-register/definition.png)

2. İsteğe bağlı olarak, var olan eylemler ve Tetikleyicileri düzenleme veya yeni bir tane eklemek istiyorsanız, [bu adımlarla devam](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Bağlayıcınızı oluşturmayı tamamlayın

Hazır olduğunuzda, seçin **güncelleştirme bağlayıcı** Bağlayıcınızı dağıtabilmek için. 

Tebrikler! Şimdi mantıksal uygulama oluşturduğunuzda, Bağlayıcınızı Logic Apps Tasarımcısı'nda bulmak ve bu bağlayıcı mantıksal uygulamanızı ekleyin.

![Logic Apps Tasarımcısı'nda Bağlayıcınızı Bul](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Bağlayıcınızı diğer Logic Apps kullanıcılarla paylaşma

Kayıtlı ancak onaylı olmayan özel bağlayıcılar Microsoft tarafından yönetilen bağlayıcıların gibi çalışır, ancak görünür ve kullanılabilir *yalnızca* bağlayıcı'nın yazar ve aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip kullanıcılar Bu uygulamaları'nın dağıtıldığı bölgede Logic apps için. Paylaşımı isteğe bağlı olsa da, bağlayıcılar diğer kullanıcılarla paylaşmak istediğiniz senaryolar olabilir. 

> [!IMPORTANT]
> Bir bağlayıcı paylaşıyorsanız, diğerlerinin bu bağlayıcıda bağımlı başlayabilir. 
> ***Bağlayıcınızı silindiğinde, bu bağlayıcı için tüm bağlantılar silinir.***
 
Bağlayıcınızı harici kullanıcılar bu bu gibi bir durumda sınırları dışında tüm Logic Apps kullanıcılarla paylaşmak için [Bağlayıcınızı Microsoft sertifika için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>SSS

**S:** SOAP Bağlayıcısı genel olarak kullanılabilir (GA)? </br>
**Y:** SOAP Bağlayıcıdır içinde **Önizleme**, ve GA hizmeti henüz değil.

**S:** tüm kısıtlamaları ve SOAP Bağlayıcısı için bilinen sorunlar vardır? </br>
**Y:** Evet, bkz: [SOAP bağlayıcı kısıtlamaları ve bilinen sorunlar](../api-management/api-management-api-import-restrictions.md#wsdl).

**S:** özel bağlayıcıların herhangi bir sınır vardır? </br>
**Y:** Evet, bkz: [özel bağlayıcı sınırlar burada](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Destek alın

* Geliştirme ve ekleme veya Kayıt Sihirbazı'nda bulunmayan istek özellikleri için destek için iletişim [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft developer sorular ve sorunlar için bu hesabı izler ve bunları uygun ekibine yönlendirir.

* İsteyin veya soruyu yanıtlayın veya diğer Azure mantıksal uygulamaları kullanıcıların ne yaptıklarını bakın, ziyaret [Azure Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Logic Apps geliştirmeye yardımcı olmak için oy veya fikir gönderme [Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Sonraki adımlar

* [İsteğe bağlı: Bağlayıcınızı Onayla](../logic-apps/custom-connector-submit-certification.md)
* [Özel bağlayıcı SSS](../logic-apps/custom-connector-faq.md)