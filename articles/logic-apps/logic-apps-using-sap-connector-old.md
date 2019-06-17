---
title: SAP sistemlerini - Azure Logic Apps bağlayın | Microsoft Docs
description: Erişim ve Azure Logic Apps ile iş akışlarını otomatik hale getirerek SAP kaynakları yönetme
author: ecfan
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: d677c0eae9c92f90783ed4ebd95a528b34c872ec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60847479"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure Logic Apps'ten SAP sistemlerini bağlanma

> [!NOTE]
> Bu SAP bağlayıcısı kullanımdan kaldırma için zamanlandı. Lütfen kullanın ya da geçiş [daha yeni ve daha gelişmiş SAP Bağlayıcısı](./logic-apps-using-sap-connector.md). 

Bu makalede, SAP uygulama sunucusu ve SAP ileti sunucusu bağlayıcıları kullanarak, SAP kaynaklarınızı mantıksal uygulama içinde nasıl erişeceği gösterilmektedir. Böylece, görevler, süreçleri ve mantıksal uygulamalar oluşturarak SAP veri ve kaynaklarınız yönetme iş akışlarını otomatik hale getirebilirsiniz.

Bu örnek, bir HTTP isteği ile tetikleyebileceğiniz bir mantıksal uygulama kullanır. Mantıksal uygulama bir ara belgesi (IDoc) bir SAP sunucusuna gönderir ve mantıksal uygulama adlı istek sahibi bir yanıt döndürür.
Bu örnekte geçerli SAP bağlayıcıları Eylemler, ancak değil tetikleyicileri, sahip, bu nedenle [HTTP isteği tetikleyicisi](../connectors/connectors-native-reqres.md) mantıksal uygulama iş akışındaki ilk adım olarak. SAP bağlayıcısı özel teknik bilgiler için bu başvuru makalelere bakın: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">SAP uygulama sunucusu Bağlayıcısı</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">SAP ileti sunucusu Bağlayıcısı</a>

Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi izlemek için bu öğeler gerekir:

* SAP sisteminiz ve mantıksal uygulamanızın iş akışı başlatan tetikleyici erişmek istediğiniz mantıksal uygulaması. SAP bağlayıcılar şu anda yalnızca eylemleri belirtin. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP uygulama sunucusu</a> veya <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP ileti sunucusu</a>

* En son yükleyip [şirket içi veri ağ geçidi](https://www.microsoft.com/download/details.aspx?id=53127) herhangi bir şirket içi bilgisayarda. Devam etmeden önce Azure portalında, ağ geçidi ayarlama emin olun. Ağ geçidi, güvenli bir şekilde erişim veri yardımcı olur ve şirket içi kaynaklardır. Daha fazla bilgi için [yükleme şirket içi veri ağ geçidi için Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Şu anda en son SAP istemci kitaplığı yükleyip <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">Microsoft .NET Framework 4.0 ve Windows 64 bit (x64) SAP Bağlayıcısı (NCo) 3.0.20.0</a>, şirket içi veri ağ geçidi ile aynı bilgisayarda. Bu sürümü yükleyin veya sonraki bir sürümü bu nedenlerle:

  * Aynı anda birden fazla IDoc ileti gönderildiğinde SAP NCo sürümlerde kilitlendiğini haline gelir. 
  Bu durum iletileri zaman aşımına neden SAP hedefe gönderilen tüm sonraki iletiler engeller.

  * Şirket içi veri ağ geçidi yalnızca 64-bit sistemlerde çalışır. 
  Aksi takdirde, 32 bit derlemeleri veri ağ geçidi ana bilgisayar hizmeti desteklemediğinden bir "bozuk görüntü" hatası alıyorum.

  * .NET Framework 4.5 hem veri ağ geçidi ana bilgisayar hizmeti hem de Microsoft SAP bağdaştırıcısı kullanın. .NET Framework 4.0 için SAP NCo .NET çalışma zamanı için 4.0 4.7.1 kullanan işlemleri ile çalışır. 
  .NET Framework 2.0 için SAP NCo .NET çalışma zamanı 2.0 için 3.5 kullanan işlemleri ile çalışır ve artık en yeni şirket içi veri ağ geçidi ile çalışır.

* İleti içeriği SAP sunucunuza bir örnek IDoc dosya gibi gönderebilirsiniz. Bu içerik, XML biçiminde olması ve kullanmak istediğiniz SAP eylemi için ad alanı içerir.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>HTTP isteği tetikleyicisi Ekle

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Gönderebilirsiniz, böylece bu örnekte, bir mantıksal uygulama ile bir uç nokta azure'da oluşturduğunuz *HTTP POST istekleri* mantıksal uygulamanız için. Mantıksal uygulamanız bu HTTP isteği aldığında, tetiklenir ve sonraki adıma akışınızda çalışır.

1. Azure portalında mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun. 

2. Arama kutusuna filtreniz olarak "http isteği" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **İstek - bir HTTP isteği alındığında**

   ![HTTP isteği tetikleyicisi ekleyin](./media/logic-apps-using-sap-connector-old/add-trigger.png)

3. Şimdi mantıksal uygulamanız için bir uç nokta URL'si oluşturabilmek mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

   Uç nokta URL'si artık Tetikleyiciniz, örneğin görünür:

   ![Uç noktası için URL oluşturun](./media/logic-apps-using-sap-connector-old/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>SAP Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Mantıksal uygulamanıza bir tetikleyici henüz eklemediniz ve bu örneği takip etmek istiyorsanız [Bu bölümde açıklanan tetikleyici ekleme](#add-trigger).

1. Mantıksal Uygulama Tasarımcısı tetikleyicinin altında seçin **yeni adım** > **Eylem Ekle**.

   ![Eylem ekleme](./media/logic-apps-using-sap-connector-old/add-action.png) 

2. Arama kutusuna filtreniz olarak "sap sunucusu" girin. Eylem listesinden SAP sunucunuz için bir eylem seçin: 

   * **SAP uygulama sunucusu - SAP Gönder**
   * **SAP ileti sunucusu - SAP Gönder**

   Bu örnek, bu eylem kullanır: **SAP uygulama sunucusu - SAP Gönder**

   !["SAP uygulama sunucusu" veya "SAP ileti sunucusu" seçin](media/logic-apps-using-sap-connector-old/select-sap-action.png)

3. Bağlantı ayrıntıları için istenirse, artık SAP bağlantı oluşturun. Bağlantınız zaten varsa, böylece SAP eyleminizi ayarlayabilirsiniz. Aksi halde, sonraki adıma geçin. 

   **Şirket içi SAP bağlantı oluşturma**

   1. İçin **ağ geçitleri**seçin **şirket içi veri ağ geçidi üzerinden Bağlan** böylece şirket içi bağlantı özellikleri görünür.

   2. SAP sunucunuzun bağlantı bilgilerini verin. 
   İçin **ağ geçidi** özelliği, oluşturduğunuz Azure Portalı'nda, ağ geçidi yükleme için örneğin veri ağ geçidi seçin:

      **SAP uygulama sunucusu**

      ![SAP uygulama sunucusu bağlantısı oluşturma](./media/logic-apps-using-sap-connector-old/create-SAP-app-server-connection.png)  

      **SAP ileti sunucusu**

      ![SAP ileti sunucusu bağlantısını oluşturma](media/logic-apps-using-sap-connector-old/create-SAP-message-server-connection.png) 

   2. İşiniz bittiğinde **Oluştur**’u seçin.

      Mantıksal uygulamalar, ayarlar ve bağlantının düzgün çalıştığından emin olun, bağlantınızı test eder.

4. Şimdi Bul ve SAP sunucunuzdan bir eylem seçin. 

   1. İçinde **SAP eylem** kutusunda, klasör simgesini seçin. 
   Klasör listesinde, bulmak ve kullanmak istediğiniz eylemi seçin. 

      Bu örnekte seçer **IDOC** IDoc eylem kategorisi. 

      ![Bulma ve IDoc eylemini seçin](./media/logic-apps-using-sap-connector-old/SAP-app-server-find-action.png)

      Gerçekleştirmek istediğiniz eylemi bulamazsanız, örneğin bir yolu el ile girebilirsiniz:

      ![IDoc eylem yol el ile sağlayın](./media/logic-apps-using-sap-connector-old/SAP-app-server-manually-enter-action.png)

      IDoc işlemleri hakkında daha fazla bilgi için bkz. [ileti IDOC işlemleri için şemalar](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. İçine tıklayın **giriş iletisi** dinamik içerik listesinde görünmesi kutusu. 
   Altında liste **olduğunda bir HTTP isteği alındığında**seçin **gövdesi** alan. 

      Bu adım, HTTP istek Tetikleyiciniz gövdesi içeriğini içerir ve SAP sunucunuza çıktı gönderen.

      !["Gövde" alanı seçin](./media/logic-apps-using-sap-connector-old/SAP-app-server-action-select-body.png)

      İşiniz bittiğinde, SAP eyleminizi şu örnekteki gibi görünür:

      ![Tam SAP eylemi](./media/logic-apps-using-sap-connector-old/SAP-app-server-complete-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>HTTP yanıt eylemi ekleme

Şimdi mantıksal uygulamanızın iş akışı için bir yanıt eylemi ekleyin ve SAP eylem çıktısı içerir. Bu şekilde mantıksal uygulamanız sonuçları özgün istek sahibine SAP sunucunuzdan döndürür. 

1. SAP eylem altında mantıksal Uygulama Tasarımcısı seçin **yeni adım** > **Eylem Ekle**.

2. Arama kutusuna filtreniz olarak "yanıt" girin. Eylem listesinden şu eylemi seçin: **İstek - yanıt**

3. İçine tıklayın **gövdesi** dinamik içerik listesinde görünmesi kutusu. Bu listeden altında **göndermek için SAP**seçin **gövdesi** alan. 

   ![Tam SAP eylemi](./media/logic-apps-using-sap-connector-old/select-sap-body-for-response-action.png)

4. Mantıksal uygulamanızı kaydedin. 

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Mantıksal uygulamanızı önceden, mantıksal uygulama menüsünde etkinleştirilmemişse seçin **genel bakış**. Araç çubuğunda **etkinleştirme**. 

2. Mantıksal Uygulama Tasarımcısı araç çubuğunda **çalıştırma**. Bu adım, mantıksal uygulamanızı el ile başlatır.

3. HTTP istek Tetikleyiciniz URL'yi bir HTTP POST isteği göndererek mantıksal uygulamanızı tetikleyecek ve isteğinizi içerik, ileti içerir. Gönderme isteği, bir aracı gibi kullanabileceğiniz [Postman](https://www.getpostman.com/apps). 

   Bu makalede, XML biçiminde olmalı ve ad alanında, örneğin kullanmakta olduğunuz SAP eylemi için bir IDoc dosyası isteği gönderir: 

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
> Yanıt için gerekli tüm adımları içinde tamamlanmıyor, mantıksal uygulamanızın zaman aşımına uğrayabilir [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md). Bu durum bir durumda, istekleri engellenmiş. Sorunları tanılamanıza yardımcı olmak için öğrenin [denetleyin ve logic apps uygulamalarınızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Tebrikler, artık SAP sunucunuz ile iletişim kurabilen bir mantıksal uygulama oluşturdunuz. Mantıksal uygulamanız için SAP bağlantınız ayarladıysanız, BAPI ve RFC gibi diğer kullanılabilir SAP Eylemler keşfedebilirsiniz.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcılar Swagger dosyaları tarafından açıklandığı Bağlayıcısı hakkında teknik ayrıntılar için bu başvuru makalelere bakın: 

* [SAP uygulama sunucusu](/connectors/sapapplicationserver/)
* [SAP ileti sunucusu](/connectors/sapmessageserver/)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi sistemlere bağlanın](../logic-apps/logic-apps-gateway-connection.md) mantıksal uygulamalardan
* Doğrulama, Dönüşüm ve diğer ileti işlemleri ile öğrenin [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
