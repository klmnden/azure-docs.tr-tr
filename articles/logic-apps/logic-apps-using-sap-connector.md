---
title: SAP sistemleri - Azure Logic Apps bağlanma | Microsoft Docs
description: Erişim ve Azure Logic Apps ile iş akışları otomatik hale getirerek SAP kaynakları yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: a9346092e0a24709a9888937effdf802bf1b09fb
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300224"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure mantıksal uygulamaları SAP sistemlerine bağlanmak

Bu makalede, SAP uygulama sunucusu ve SAP ileti sunucusu bağlayıcıları kullanarak SAP kaynaklarınızdan bir mantıksal uygulama içinde nasıl erişebileceğiniz gösterilmektedir. Böylece, görevler, işlemler ve logic apps oluşturarak SAP veri ve kaynaklarınız yönetme iş akışlarını otomatikleştirebilirsiniz.

Bu örnek, bir HTTP isteğiyle tetiklemek bir mantıksal uygulama kullanır. Mantıksal uygulama ara bir belge (IDoc) bir SAP sunucusuna gönderir ve bir mantıksal uygulama adlı istek yanıtı döndürür.
Bu örnek kullanan geçerli SAP bağlayıcılar Eylemler ancak değil tetikleyiciler, olan [HTTP isteği tetikleyici](../connectors/connectors-native-reqres.md) mantığı uygulamanın iş akışındaki ilk adım olarak. SAP bağlayıcısı özgü teknik bilgi için bu başvuru makalelere bakın: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">SAP uygulama sunucusu Bağlayıcısı</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">SAP ileti sunucusu Bağlayıcısı</a>

Henüz bir Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi izlemek için bu öğeler gerekir:

* SAP sisteminizi ve mantığı uygulamanızın akışı başlar bir tetikleyici erişmek istediğiniz mantıksal uygulama. SAP bağlayıcıları şu anda yalnızca eylemleri sağlar. Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP uygulama sunucusu</a> veya <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP ileti sunucusu</a>

* En son yükleyip [şirket içi veri ağ geçidi](https://www.microsoft.com/download/details.aspx?id=53127) herhangi bir şirket içi bilgisayarda. Devam etmeden önce Azure Portalı'ndaki Ağ geçidiniz ayarlamanız emin olun. Ağ geçidi, güvenli bir şekilde erişim verilerini yardımcı olur ve şirket içinde kaynaklardır. Daha fazla bilgi için bkz: [yükleme şirket içi veri ağ geçidi için Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Şu anda son SAP istemci kitaplığı yükleyip <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">Microsoft .NET Framework 4.0 ve Windows 64-bit (x64) için SAP Bağlayıcısı (NCo) 3.0.20.0</a>, şirket içi veri ağ geçidi ile aynı bilgisayarda. Bu sürümü yükleyin ya da daha sonra bu nedenlerle:

  * Aynı anda birden fazla IDoc ileti gönderildiğinde SAP NCo sürümlerde karşılıklı haline gelir. 
  Bu durum iletileri zaman aşımına neden SAP hedefi gönderilen tüm sonraki iletiler engeller.

  * Şirket içi veri ağ geçidi yalnızca 64-bit sistemlerde çalışır. 
  Aksi takdirde, veri ağ geçidi ana bilgisayar hizmeti 32-bit derlemeleri desteklemediği için "bozuk görüntü" hata iletisi.

  * .NET Framework 4.5 hem veri ağ geçidi ana bilgisayar hizmeti hem de Microsoft SAP bağdaştırıcısı kullanın. .NET Framework 4.0 SAP NCo .NET çalışma zamanı 4.0 için 4.7.1 kullanan işlemler ile çalışır. 
  .NET Framework 2.0 için SAP NCo .NET çalışma zamanı 2.0 için 3.5 kullanmak işlemlerle çalışır ve artık son şirket içi veri ağ geçidi ile çalışır.

* İleti içeriği örnek IDoc dosyası gibi SAP sunucusuna gönderebilir. Bu içerik, XML biçiminde olması ve kullanmak istediğiniz SAP eylemi için ad alanı içerir.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>HTTP isteği tetikleyicisi ekleyin

Azure Logic Apps içinde her mantıksal uygulama başlamalı ve bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay olduğunda etkinleşir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her tetikleyici ateşlenir Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Böylece, gönderebilir Bu örnekte, bir mantıksal uygulama azure'da bir uç nokta ile oluşturduğunuz *HTTP POST istekleri* mantığı uygulamanıza. Mantıksal uygulamanızı HTTP istekleri aldığında, tetikleyici harekete ve sonraki adım, iş akışınızı çalıştırır.

1. Azure portalında mantığı Uygulama Tasarımcısı'nı açar boş mantıksal uygulama oluşturun. 

2. Arama kutusuna "http isteği", filtre olarak girin. Tetikleyiciler listesinden Bu tetikleyici: **isteği - durumlarda bir HTTP isteği aldı**

   ![HTTP isteği tetikleyicisi ekleyin](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Mantıksal uygulamanız için bir uç nokta URL'si oluşturmak için şimdi mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

   Uç nokta URL'si şimdi de, tetikleyici, örneğin görünür:

   ![Uç noktası için URL oluştur](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>SAP Eylem Ekle

Azure mantıksal uygulamaları içinde bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) tetikleyicinin veya başka bir eylem izler, iş akışınızı bir adımdır. Bir tetikleyici mantıksal uygulamanızı henüz eklemediniz ve bu örnek, izlemek istediğiniz [Bu bölümde açıklanan tetikleyici eklemek](#add-trigger).

1. Tetikleyici altında mantığı Uygulama Tasarımcısı'nda seçin **yeni adım** > **Eylem Ekle**.

   ![Eylem ekleme](./media/logic-apps-using-sap-connector/add-action.png) 

2. Arama kutusuna "sap server", filtre olarak girin. Eylemler listesinden SAP sunucunuz için bir eylem seçin: 

   * **SAP uygulama sunucusu - SAP Gönder**
   * **SAP ileti Server - SAP Gönder**

   Bu eylem Bu örnek kullanır: **SAP uygulama sunucusu - SAP Gönder**

   !["SAP uygulama sunucusu" veya "SAP ileti Server" seçin](media/logic-apps-using-sap-connector/select-sap-action.png)

3. Bağlantı ayrıntılarını istenirse, SAP bağlantınızı şimdi oluşturun. Bağlantı zaten varsa, SAP eyleminizi ayarlayabilirsiniz Aksi halde, sonraki adımla devam edin. 

   **Şirket içi SAP bağlantısı oluşturma**

   1. İçin **ağ geçitleri**seçin **Connect şirket içi veri ağ geçidi üzerinden** böylece şirket içi bağlantı özellikleri görünür.

   2. SAP sunucunuz için bağlantı bilgilerini sağlayın. 
   İçin **ağ geçidi** özelliği, oluşturduğunuz Azure portalında, ağ geçidi yükleme için örneğin veri ağ geçidi seçin:

      **SAP uygulama sunucusu**

      ![SAP uygulama sunucusuna bağlantı oluşturma](./media/logic-apps-using-sap-connector/create-SAP-app-server-connection.png)  

      **SAP ileti sunucusu**

      ![SAP ileti sunucu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. İşiniz bittiğinde **Oluştur**’u seçin.

      Logic Apps kurar ve bağlantı düzgün çalıştığından emin olmayı bağlantınızı test eder.

4. Şimdi Bul ve SAP sunucunuzdan bir eylem seçin. 

   1. İçinde **SAP eylem** kutusunda, klasör simgesini seçin. 
   Klasör listesinden bulun ve kullanmak istediğiniz eylemi seçin. 

      Bu örnek seçer **IDOC** IDoc eylem kategorisi. 

      ![Bulma ve IDoc eylemi seçin](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Gerçekleştirmek istediğiniz eylemi bulamazsanız, örneğin bir yolu el ile girebilirsiniz:

      ![El ile IDoc eylem yolunu belirtin](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      IDoc işlemleri hakkında daha fazla bilgi için bkz: [ileti IDOC işlemleri için şemalar](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. İçini tıklatın **giriş iletisi** dinamik içerik listesi görünmesi kutusu. 
   Altında liste **zaman bir HTTP isteği alındığında**seçin **gövde** alan. 

      Bu adım, HTTP isteği tetikleyici gövde içerikten içerir ve SAP sunucunuza çıkışı gönderir.

      !["Body" alanını seçin](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      İşiniz bittiğinde, SAP eyleminizi aşağıdaki gibi görünür:

      ![Tam SAP eylemi](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>HTTP yanıt Eylem Ekle

Şimdi bir yanıt eylemi mantığı uygulamanızın iş akışına eklemek ve SAP eylem çıktısını içerir. Bu şekilde, mantıksal uygulamanızı özgün istek sahibine SAP sunucunuzdan sonuçları döndürür. 

1. SAP eylem altında mantığı Uygulama Tasarımcısı'nda seçin **yeni adım** > **Eylem Ekle**.

2. Arama kutusuna "yanıt", filtre olarak girin. Bu eylem Eylemler listeden seçin: **istek - yanıt**

3. İçini tıklatın **gövde** dinamik içerik listesi görünmesi kutusu. Bu listeden altında **SAP için Gönder**seçin **gövde** alan. 

   ![Tam SAP eylemi](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Mantıksal uygulamanızı kaydedin. 

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulamanızı zaten mantığı uygulama menünüzde değil etkinleştirilirse, seçin **genel bakış**. Araç çubuğunda seçin **etkinleştirmek**. 

2. Mantıksal Uygulama Tasarımcısı araç çubuğundaki seçin **çalıştırmak**. Bu adım, mantıksal uygulamanızı el ile başlar.

3. HTTP isteği tetikleyici URL'yi bir HTTP POST isteği göndererek mantıksal uygulamanızı tetikler ve iletinizi isteğinizle birlikte içerik içerir. Gönderme isteği için bir aracı gibi kullanabileceğiniz [Postman](https://www.getpostman.com/apps). 

   Bu makalede, XML biçiminde ve gerekir, örneğin kullanmakta olduğunuz SAP eylemi için ad alanı dahil bir IDoc dosyası isteği gönderir: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. HTTP isteğinizi gönderdikten sonra mantıksal uygulamanızı yanıttan bekleyin.

> [!NOTE]
> Yanıt için gereken tüm adımları içinde son yok, mantıksal uygulamanızı zaman aşımı olabilir [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md). Bu durum oluşursa, isteği engellenen. Sorunları tanılamanıza yardımcı olmak için öğrenin [denetleyin ve mantıksal uygulamalarınızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Tebrikler, SAP sunucunuzla iletişim kurabilen bir mantıksal uygulama artık oluşturduğunuz. SAP bağlantınız mantığı uygulamanız için ayarladığınız, BAPI ve RFC gibi diğer kullanılabilir SAP eylemleri keşfedebilirsiniz.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcılar Swagger dosyaları tarafından açıklanan Bağlayıcısı hakkında teknik ayrıntılar için bu başvuru makalelere bakın: 

* [SAP uygulama sunucusu](/connectors/sapapplicationserver/)
* [SAP ileti sunucusu](/connectors/sapmessageserver/)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi sistemlere bağlanmak](../logic-apps/logic-apps-gateway-connection.md) mantığı uygulamalardan
* Dönüştürme nasıl doğrulamak ve diğer ileti işlemleriyle öğrenin [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Diğer hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)
