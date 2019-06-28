---
title: B2B Kurumsal tümleştirme - Azure Logic Apps için RosettaNet iletileri
description: Azure Logic Apps Enterprise Integration Pack ile Exchange RosettaNet iletileri
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 88e02f3fbbca8007fdf479bb973f50c42a878d6e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332829"
---
# <a name="exchange-rosettanet-messages-for-b2b-enterprise-integration-in-azure-logic-apps"></a>Azure Logic apps'teki B2B Kurumsal tümleştirme için RosettaNet mesaj alışverişi 

[RosettaNet](https://resources.gs1us.org) iş bilgilerini paylaşmak için standart süreçler kurmuştur kar amacı gütmeyen bir Konsorsiyumu. Bu standartlar, tedarik zinciri işlemleri için yaygın olarak kullanılan ve semiconductor, elektronik ve lojistik sektörlerde yaygın. RosettaNet consortium oluşturur ve ortak iş işlem tanımları için tüm RosettaNet ileti alışverişlerinde sağlayan ortak arabirimi işlemleri (PIP) korur. RosettaNet XML tabanlı ve iş süreçlerini arabirimleri ve uygulama çerçeveleri şirketler arasındaki iletişim için ileti yönergeleri tanımlar.

İçinde [Azure Logic Apps](../logic-apps/logic-apps-overview.md), RosettaNet bağlayıcı RosettaNet standartlarını destekleyen tümleştirme çözümleri oluşturmanıza yardımcı olur. Bağlayıcı RosettaNet uygulaması çerçevesi (RNIF) sürüm 2.0.01 üzerinde temel alır. RNIF RosettaNet PIP işbirliğine dayalı bir şekilde çalıştırmak iş ortakları sağlayan bir açık ağ uygulama çerçevesidir. Bu çerçeve, ileti yapısı, bildirimler, çok amaçlı Internet Posta Uzantıları (MIME) kodlama ve dijital imza gereksinimini tanımlar.

Özellikle, bağlayıcı bu yetenekleri sağlar:

* Kodlama veya RosettaNet iletileri alabilirsiniz.
* Kod çözme veya RosettaNet ileti gönderin.
* Yanıt ve bildirimi başarısız oluşturulmasını bekleyin.

Bu özellikler için 2.0.01 RNIF tarafından tanımlanan tüm PIP bir bağlayıcıyı destekler. İş ortağıyla iletişim zaman uyumlu veya zaman uyumsuz olabilir.

## <a name="rosettanet-concepts"></a>RosettaNet kavramları

Bazı kavramları ve RosettaNet belirtimi için benzersizdir ve RosettaNet tabanlı tümleştirmeler oluştururken önemli olan şartları şunlardır:

* **PIP**

  RosettaNet kuruluş oluşturur ve ortak iş işlem tanımları için tüm RosettaNet ileti alışverişlerinde sağlayan ortak arabirimi işlemleri (PIP) korur. Her PIP belirtimi, bir belge türü tanımı (DTD'nin) dosyası ve bir ileti kılavuz belge sağlar. DTD'nin dosya hizmeti içeriğinin ileti yapısını tanımlar. Kullanıcı tarafından okunabilen bir HTML dosyası olan ileti kılavuz belge öğesi düzeyi kısıtlamaları belirtir. Birlikte, bu dosyalar, iş süreci tam tanımı sağlar.

   PIP, bir üst düzey iş işlevi veya küme ve bir yürütülemiyor veya segment tarafından ayrılır. Örneğin, satın alma siparişi için PIP "3A4" olan "3" sırasında sipariş yönetimi işlevdir ve "3A" olan teklif & sipariş girişi yürütülemiyor. Daha fazla bilgi için [RosettaNet site](https://resources.gs1us.org).

* **Eylem**

  Bölümü bir PIP iş ortakları arasında değiş tokuş edileceğini işletmeye iletileri yapılacak olan.

* **Sinyal**

   Bölümü bir PIP eylem iletilere yanıt olarak gönderilen bildirimler sinyal iletileri şunlardır.

* **Tek bir eylem ve çift eylemi**

  Bir tek eylem PIP için yalnızca bir bildirim sinyal iletisi yanıttır. Bir çift eylem PIP Başlatıcı bir yanıt iletisi alır ve tek eylem ileti akışı yanı sıra bir bildirim ile yanıtlar.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) sözleşmenizi ve diğer B2B yapıtlarını depolamak için. Bu tümleştirme hesabı, Azure aboneliğinizle ilişkili olmalıdır.

* En az iki [iş ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) , tümleştirme hesabınızdaki tanımlanır ve altında "DUNS" niteleyicisi ile yapılandırılmış **iş kimlikleri**

* Göndermek veya tümleştirme hesabınızdaki RosettaNet iletileri almak için gerekli olan bir PIP işlem yapılandırması. İşlem yapılandırmasını tüm PIP yapılandırma özellikleri depolar. Bu yapılandırma, iş ortağı ile bir sözleşme oluşturduğunuzda ardından başvurabilirsiniz. PIP işlem yapılandırması, tümleştirme hesabı oluşturmak için bkz [ekleme PIP işlem yapılandırmasını](#add-pip).

* İsteğe bağlı [sertifikaları](../logic-apps/logic-apps-enterprise-integration-certificates.md) şifreleme, şifre çözme veya tümleştirme hesabına yükleme iletileri imzalamak için. Yalnızca kullanım imzalama veya şifreleme, sertifikalar gereklidir.

<a name="add-pip"></a>

## <a name="add-pip-process-configuration"></a>PIP işlem Yapılandırması Ekle

PIP işlem yapılandırması, tümleştirme hesabınıza eklemek için bu adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), bulmak ve tümleştirme hesabınızı açın.

1. Üzerinde **genel bakış** bölmesinde **RosettaNet PIP** Döşe.

   ![RosettaNet kutucuk seçin](media/logic-apps-enterprise-integration-rosettanet/select-rosettanet-tile.png)

1. Altında **RosettaNet PIP**, seçin **Ekle**. PIP bilgilerinizi sağlayın.

   ![RosettaNet PIP ayrıntıları ekleyin](media/logic-apps-enterprise-integration-rosettanet/add-rosettanet-pip.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Ad** | Evet | PIP adınız |
   | **PIP kod** | Evet | PIP üç basamaklı kodu. Daha fazla bilgi için [RosettaNet PIP](https://docs.microsoft.com/biztalk/adapters-and-accelerators/accelerator-rosettanet/rosettanet-pips). |
   | **PIP sürümünü** | Evet | Seçili PIP koduna göre kullanılabilir PIP sürüm numarası |
   ||||

   Bu PIP özellikleri hakkında daha fazla bilgi için ziyaret [RosettaNet Web sitesi](https://resources.gs1us.org/RosettaNet-Standards/Standards-Library/PIP-Directory#1043208-pipsreg).

1. İşiniz bittiğinde seçin **Tamam**, PIP yapılandırması oluşturur.

1. İşlem yapılandırmasını düzenleyin veya görüntülemek için PIP seçip **JSON olarak Düzenle**.

   Tüm işlem yapılandırması ayarları PIP'ın teknik özelliklerinin dışında gelir. Mantıksal uygulamalar, bu özellikler en yaygın kullanılan değerleri varsayılan değerlerle ayarların çoğu doldurur.

   ![RosettaNet PIP yapılandırmasını düzenle](media/logic-apps-enterprise-integration-rosettanet/edit-rosettanet-pip.png)

1. Ayarları uygun PIP belirtiminde değerlere karşılık gelir ve iş gereksinimlerinizi karşılayacak onaylayın. Gerekirse, json'da değerlerini güncelleştirin ve değişiklikleri kaydedin.

## <a name="create-rosettanet-agreement"></a>RosettaNet sözleşmesi oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), bulun ve açın, tümleştirme hesabı, açık değilse.

1. Üzerinde **genel bakış** bölmesinde **sözleşmeleri** Döşe.

   ![Anlaşmaları kutucuğunu seçin](media/logic-apps-enterprise-integration-rosettanet/select-agreement-tile.png)

1. Altında **sözleşmeleri**, seçin **Ekle**. Anlaşma ayrıntılarını sağlayın.

   ![Anlaşma Ayrıntıları Ekle](media/logic-apps-enterprise-integration-rosettanet/add-agreement-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Ad** | Evet | Anlaşma adı |
   | **Sözleşme türü** | Evet | Seçin **RosettaNet**. |
   | **Konak iş ortağı** | Evet | Bir anlaşma hem konak hem de Konuk iş ortağı gerektirir. Konak iş ortağı sözleşmesi yapılandıran kuruluşu temsil eder. |
   | **Konak kimliği** | Evet | Konak iş ortağı için bir tanımlayıcı |
   | **Konuk iş ortağı** | Evet | Bir anlaşma hem konak hem de Konuk iş ortağı gerektirir. Konuk iş ortağı, konak iş ortağı iş yapmakta kuruluşu temsil eder. |
   | **Konuk kimliği** | Evet | Konuk iş ortağı için bir tanımlayıcı |
   | **Ayarları Al** | Varies | Bu özellikler konak iş ortağı tarafından alınan tüm iletileri için geçerlidir |
   | **Gönderme ayarları** | Varies | Bu özellikler konak iş ortağı tarafından gönderilen tüm iletiler için geçerlidir |  
   | **RosettaNet PIP başvuruları** | Evet | Anlaşma için PIP başvuruları. Tüm RosettaNet iletileri PIP yapılandırmaları gerektirir. |
   ||||

1. Konuk iş ortağı gelen iletileri almak için sözleşmenize ayarlamak için seçin **alma ayarı**.

   ![Ayarları Al](media/logic-apps-enterprise-integration-rosettanet/add-agreement-receive-details.png)

   1. İmzalama veya şifreleme gelen iletiler için altında etkinleştirmek için **iletileri**seçin **ileti imzalanmasını** veya **ileti şifrelenir** sırasıyla.

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **İletinin imzalanmış olması gerekir** | Hayır | Gelen iletiler seçilen sertifika ile oturum açın. |
      | **Sertifika** | Evet, imzalama etkinse | Sertifika imzalama için kullanılacak |
      | **İleti şifrelemeyi etkinleştir** | Hayır | Seçilen sertifika ile gelen iletileri şifreler. |
      | **Sertifika** | Şifreleme etkinse, Evet | Sertifika şifreleme için kullanılacak |
      ||||

   1. Her seçim altında ilgili seçin [sertifika](./logic-apps-enterprise-integration-certificates.md), imzalama veya şifreleme için kullanılacak tümleştirme hesabı'na daha önce eklenen.

1. Konuk iş ortağı ileti göndermek için sözleşmenize ayarlamak için seçin **gönderme ayarları**.

   ![Gönderme ayarları](media/logic-apps-enterprise-integration-rosettanet/add-agreement-send-details.png)

   1. İmzalama veya şifreleme giden iletiler için altında etkinleştirmek için **iletileri**seçin **ileti imzalamayı etkinleştir** veya **ileti şifrelemeyi etkinleştir** sırasıyla. Her seçim altında karşılık gelen algoritmayı seçin ve [sertifika](./logic-apps-enterprise-integration-certificates.md), imzalama veya şifreleme için kullanılacak tümleştirme hesabı'na daha önce eklenen.

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **İleti imzalamayı etkinleştir** | Hayır | Giden iletiler seçili imzalama algoritması ve sertifika ile oturum açın. |
      | **İmza algoritması** | Evet, imzalama etkinse | Seçilen sertifika kullanmak için imza algoritmasını tabanlı |
      | **Sertifika** | Evet, imzalama etkinse | Sertifika imzalama için kullanılacak |
      | **İleti şifrelemeyi etkinleştir** | Hayır | Seçili şifreleme algoritması ve sertifika ile giden şifreleyin. |
      | **Şifreleme algoritması** | Şifreleme etkinse, Evet | Seçili sertifikayı kullanmak için şifreleme algoritması tabanlı |
      | **Sertifika** | Şifreleme etkinse, Evet | Sertifika şifreleme için kullanılacak |
      ||||

   1. Altında **uç noktaları**, eylem iletileri ve bildirimleri göndermek için kullanılacak gerekli URL'leri belirtin.

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **Eylem URL'si** |  Evet | Eylem iletileri göndermek için kullanılacak URL. URL, zaman uyumlu ve zaman uyumsuz iletiler için gerekli bir alandır. |
      | **Bildirim URL'si** | Evet | Bildirim iletileri göndermek için kullanılacak URL. URL, zaman uyumsuz iletiler için gerekli bir alandır. |
      ||||

1. İş ortakları için RosettaNet PIP başvurularıyla sözleşmenize ayarlamak için seçin **RosettaNet PIP başvuran**. Altında **PIP adı**, önceden oluşturulmuş, PIP adını seçin.

   ![PIP başvuruları](media/logic-apps-enterprise-integration-rosettanet/add-agreement-pip-details.png)

   Seçiminiz, tümleştirme hesabında, ayarladığınız PIP temel alan kalan özellikleri doldurur. Gerekirse, değiştirebileceğiniz, **PIP rol**.

   ![Seçili PIP](media/logic-apps-enterprise-integration-rosettanet/add-agreement-selected-pip.png)

Bu adımları tamamladıktan sonra RosettaNet ileti göndermek veya almak hazırsınız.

## <a name="rosettanet-templates"></a>RosettaNet şablonları

Geliştirme sürecinizi hızlandırın ve tümleştirme desenleri önermek için RosettaNet ileti kodlama ve kodunu çözme için mantıksal uygulama şablonları kullanabilirsiniz. Mantıksal uygulama oluşturduğunuzda, mantıksal Uygulama Tasarımcısı şablon galerisinde arasından seçim yapabilirsiniz. Ayrıca bu şablonları bulabilirsiniz [Azure Logic Apps için GitHub deposu](https://github.com/Azure/logicapps).

![RosettaNet şablonları](media/logic-apps-enterprise-integration-rosettanet/decode-encode-rosettanet-templates.png)

## <a name="receive-or-decode-rosettanet-messages"></a>Alma veya RosettaNet iletileri kodunu çözme

1. [Boş mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

1. [Tümleştirme hesabınızı bağlayın](logic-apps-enterprise-integration-create-integration-account.md#link-account) mantıksal uygulamanız için.

1. Bir eylem RosettaNet iletisinin kodunu çözün ekleyebilmeniz için önce istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir.

1. Tetikleyici ekledikten sonra seçin **yeni adım**.

   ![İstek tetikleyicisi Ekle](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Arama kutusuna "rosettanet" girin ve şu eylemi seçin: **RosettaNet kodunu çözme**

   ![Bulun ve "Kod çözme RosettaNet" eylemini seçin](media/logic-apps-enterprise-integration-rosettanet/select-decode-rosettanet-action.png)

1. Eylemin özelliklerini için bilgileri sağlayın:

   ![Eylem ayrıntıları sağlayın](media/logic-apps-enterprise-integration-rosettanet/decode-action-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İleti** | Evet | Kodu çözülecek RosettaNet iletisi  |
   | **Üst Bilgiler** | Evet | RNIF sürümüdür, sürüm ve iş ortakları arasında iletişim türünü belirtir ve zaman uyumlu veya zaman uyumsuz yanıt türü için değerler sağlayan HTTP üstbilgileri |
   | **Rol** | Evet | PIP konak iş ortağı rolü |
   ||||

   RosettaNet kod çözme eylemi, diğer özellikleri ile birlikte çıktıyı içerir **giden sinyal**, hangi kodlayın ve iş ortağı geri dönüş veya diğer herhangi bir işlem bu Çıktıyı yapmanız seçebilirsiniz.

## <a name="send-or-encode-rosettanet-messages"></a>Gönderme veya RosettaNet ileti kodlama

1. [Boş mantıksal uygulama oluşturma](quickstart-create-first-logic-app-workflow.md).

1. [Tümleştirme hesabınızı bağlayın](logic-apps-enterprise-integration-create-integration-account.md#link-account) mantıksal uygulamanız için.

1. RosettaNet iletisini kodlamak için eylem ekleyebilmeniz için önce istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir.

1. Tetikleyici ekledikten sonra seçin **yeni adım**.

   ![İstek tetikleyicisi Ekle](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Arama kutusuna "rosettanet" girin ve şu eylemi seçin: **RosettaNet kodlayın**

   ![Bulun ve "RosettaNet kodlama" eylemini seçin](media/logic-apps-enterprise-integration-rosettanet/select-encode-rosettanet-action.png)

1. Eylemin özelliklerini için bilgileri sağlayın:

   ![Eylem ayrıntıları sağlayın](media/logic-apps-enterprise-integration-rosettanet/encode-action-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İleti** | Evet | Kodlanacak RosettaNet iletisi  |
   | **Konak iş ortağı** | Evet | Konak iş ortağı adı |
   | **Konuk iş ortağı** | Evet | Konuk iş ortağı adı |
   | **PIP kod** | Evet | PIP kod |
   | **PIP sürümünü** | Evet | PIP sürümünü |  
   | **PIP örneği kimliği** | Evet | Bu PIP ileti için benzersiz tanımlayıcı |  
   | **İleti türü** | Evet | Kodlanacak iletisinin türü |  
   | **Rol** | Evet | Konak iş ortağı rolü |
   ||||

   Kodlanan ileti artık iş ortağı göndermek hazırdır.

1. Kodlanan ileti göndermek için bu örnekte **HTTP** olan eylemi, "HTTP - kodlanmış Gönder ileti iş ortağı" olarak yeniden adlandırıldı.

   ![RosettaNet ileti göndermek için HTTP eylemi](media/logic-apps-enterprise-integration-rosettanet/send-rosettanet-message-to-partner.png)

   PIP tarafından tanımlanan tüm adımları tamamlandığında RosettaNet standartlarına göre iş işlemleri tam olarak kabul edilir.

1. Konak iş ortağı için kodlanan ileti gönderdiğinde ana sinyali ve bildirim için bekler. Bu görevi gerçekleştirmek için ekleme **RosettaNet yanıtı bekle** eylem.

   !["Yanıtı bekle RosettaNet" Eylem Ekle](media/logic-apps-enterprise-integration-rosettanet/rosettanet-wait-for-response-action.png)

   Bekleme ve yeniden deneme sayısı için kullanılacak süre PIP yapılandırma, tümleştirme hesabındaki dayanır. Yanıt alınmazsa, bu eylem, bir bildirim hatası oluşturur. Yeniden deneme işlemlerini için her zaman koyun **kodla** ve **yanıtı bekle** eylemleri bir **kadar** döngü.

   ![RosettaNet eylemlerle kadar döngü](media/logic-apps-enterprise-integration-rosettanet/rosettanet-loop.png)

## <a name="next-steps"></a>Sonraki adımlar

* Doğrulama, Dönüşüm ve diğer ileti işlemleri ile öğrenin [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
