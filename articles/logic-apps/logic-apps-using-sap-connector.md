---
title: SAP sistemlerini - Azure Logic Apps bağlanma
description: Erişim ve Azure Logic Apps ile iş akışlarını otomatik hale getirerek SAP kaynaklarını yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 05/09/2019
tags: connectors
ms.openlocfilehash: bccefec80ef3afd6d312bb1048d3be5d8e708728
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258159"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure Logic Apps'ten SAP sistemlerini bağlanma

Bu makalede, SAP bağlayıcısını kullanarak şirket içi SAP kaynaklarınızdan bir mantıksal uygulama içinde nasıl erişeceği gösterilmektedir. Bağlayıcısı, R/3 ve şirket içi ECC sistemleri gibi SAP’nin klasik yayınlarıyla çalışır. Bunlar - şirket içinde veya bulutta barındırılan her yerde bağlayıcı SAP'nin s/4 HANA gibi yeni SAP HANA tabanlı sistemleriyle tümleşme de sağlar. SAP bağlayıcısını SAP NetWeaver tabanlı sistemlerde Ara belge (IDoc) veya iş uygulaması programlama arabirimi (BAPI) veya uzak işlev çağrısı (RFC) aracılığıyla gelen ve giden ileti veya veri tümleştirmeyi destekler.

SAP bağlayıcısını kullanan [SAP .NET Bağlayıcısı (NCo) kitaplığı](https://support.sap.com/en/product/connectors/msnet.html) ve bu işlemleri veya eylemleri sağlar:

* **SAP için gönderme**: IDoc tRFC göndermek, BAPI işlevlerini RFC veya RFC/tRFC SAP sistemlerinde arayın.
* **SAP'den alma**: IDoc tRFC almak, tRFC BAPI işlevlerini veya RFC/tRFC SAP sistemlerinde çağırın.
* **Şemalar oluşturabilirsiniz**: IDoc, BAPI veya RFC SAP yapıtlar için şemalar oluşturur.

SAP bağlayıcısı, yukarıdaki tüm işlemler için kullanıcı adı ve parola aracılığıyla temel kimlik doğrulamasını destekler. Bağlayıcı ayrıca destekler [güvenli ağ iletişimi (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true), kullanılabileceği için SAP NetWeaver çoklu oturum açma veya bir dış güvenlik ürün tarafından sağlanan ek güvenlik özellikleri.

SAP bağlayıcısı şirket içi SAP sistemlerini tümleşir [şirket içi veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md). Gönderme senaryolarda, örneğin, SAP sistemine logic apps'ten ileti gönderilirken veri ağ geçidi bir RFC istemcisi olarak görev yapar ve SAP için logic apps'ten alınan isteklerden iletir.
Benzer şekilde, alma senaryolarda veri ağ geçidi SAP'den isteklerini alır ve mantıksal uygulamaya ileten bir RFC sunucu görevi görür.

Bu makalede örnek daha önce açıklanan tümleştirme senaryolarını kapsayan sırasında SAP ile tümleştiren mantıksal uygulamalar oluşturma işlemini gösterir.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi izlemek için bu öğeler gerekir:

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* SAP sisteminiz ve mantıksal uygulamanızın iş akışı başlatan tetikleyici erişmek istediğiniz mantıksal uygulaması. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* [SAP uygulama sunucusu](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) veya [SAP ileti sunucusu](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* En son yükleyip [şirket içi veri ağ geçidi](https://www.microsoft.com/download/details.aspx?id=53127) herhangi bir şirket içi bilgisayarda. Devam etmeden önce Azure portalında, ağ geçidi ayarlama emin olun. Ağ geçidi, güvenli bir şekilde erişim şirket verilerine ve kaynaklarına yardımcı olur. Daha fazla bilgi için [yükleme şirket içi veri ağ geçidi için Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* SNC çoklu oturum açma (SSO ile) kullanıyorsanız, ağ geçidi karşı SAP kullanıcı eşlenmiş bir kullanıcı olarak çalıştığından emin olun. Varsayılan hesabını değiştirmek için seçin **hesabını değiştir**, kullanıcı kimlik bilgilerini girin.

  ![Ağ geçidi hesabını değiştirme](./media/logic-apps-using-sap-connector/gateway-account.png)

* Bir dış güvenlik ürünle SNC etkinleştiriyorsanız SNC kitaplığı veya aynı makinede ağ geçidinin yüklendiği dosyaları kopyalayın. Bazı örnekler SNC ürünlerin [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, ve benzeri.

* Şu anda en son SAP istemci kitaplığı yükleyip [SAP Bağlayıcısı (NCo) 3.0.21.0 Microsoft .NET Framework 4.0 ve Windows 64 bit (x64)](https://softwaredownloads.sap.com/file/0020000001865512018), şirket içi veri ağ geçidi ile aynı bilgisayarda. Bu sürümü yükleyin veya sonraki bir sürümü bu nedenlerle:

  * SAP NCo sürümlerde aynı anda birden fazla IDoc ileti gönderildiğinde kilitli. Bu durum iletileri zaman aşımına neden SAP hedefe gönderilen tüm sonraki iletiler engeller.

  * Şirket içi veri ağ geçidi yalnızca 64-bit sistemlerde çalışır. Aksi takdirde, 32 bit derlemeleri veri ağ geçidi ana bilgisayar hizmeti desteklemediğinden bir "bozuk görüntü" hatası alıyorum.

  * .NET Framework 4.5 hem veri ağ geçidi ana bilgisayar hizmeti hem de Microsoft SAP bağdaştırıcısı kullanın. .NET Framework 4.0 için SAP NCo .NET çalışma zamanı için 4.0 4.7.1 kullanan işlemleri ile çalışır. .NET Framework 2.0 için SAP NCo .NET çalışma zamanı 2.0 için 3.5 kullanan işlemleri ile çalışır ve artık en yeni şirket içi veri ağ geçidi ile çalışır.

* İleti içeriği SAP sunucunuza bir örnek IDoc dosya gibi gönderebilirsiniz. Bu içerik, XML biçiminde olması ve kullanmak istediğiniz SAP eylemi için ad alanı içerir.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>SAP için Gönder

Bu örnek, bir HTTP isteği ile tetikleyebileceğiniz bir mantıksal uygulama kullanır. Mantıksal uygulama bir ara belgesi (IDoc) bir SAP sunucusuna gönderir ve mantıksal uygulama adlı istek sahibi bir yanıt döndürür. 

### <a name="add-http-request-trigger"></a>HTTP isteği tetikleyicisi Ekle

Azure Logic Apps'te, her mantıksal uygulama ile başlamalıdır bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts), belirli bir olay harekete geçirilir gerçekleşen veya belirli bir koşul karşılanıyorsa zaman. Her zaman tetikleyici Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışı çalışmaya başlar.

Gönderebilirsiniz, böylece bu örnekte, bir mantıksal uygulama ile bir uç nokta azure'da oluşturduğunuz *HTTP POST istekleri* mantıksal uygulamanız için. Mantıksal uygulamanız bu HTTP isteği aldığında, tetiklenir ve sonraki adıma akışınızda çalışır.

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı açılır bir boş mantıksal uygulama oluşturun.

1. Arama kutusuna filtreniz olarak "http isteği" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında**

   ![HTTP isteği tetikleyicisi ekleyin](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Şimdi mantıksal uygulamanız için bir uç nokta URL'si oluşturabilmek mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   Uç nokta URL'si artık Tetikleyiciniz, örneğin görünür:

   ![Uç noktası için URL oluşturun](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>SAP Eylem Ekle

Azure Logic apps'te bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) akışınıza bir tetikleyici veya başka bir eylem izleyen bir adımdır. Mantıksal uygulamanıza bir tetikleyici henüz eklemediniz ve bu örneği takip etmek istiyorsanız [Bu bölümde açıklanan tetikleyici ekleme](#add-trigger).

1. Mantıksal Uygulama Tasarımcısı tetikleyicinin altında seçin **yeni adım**.

   !["Yeni adım" seçin](./media/logic-apps-using-sap-connector/add-action.png)

1. Arama kutusuna filtreniz olarak "sap" girin. Eylem listesinden şu eylemi seçin: **SAP için ileti gönder**
  
   ![SAP Gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Alternatif olarak, aramak yerine seçin **Kurumsal** sekmesini ve SAP eylemi seçin.

   ![Kurumsal sekmesinden SAP Gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Bağlantı ayrıntıları için istenirse, artık SAP bağlantı oluşturun. Bağlantınız zaten varsa, böylece SAP eyleminizi ayarlayabilirsiniz. Aksi halde, sonraki adıma geçin.

   **Şirket içi SAP bağlantı oluşturma**

   1. SAP sunucunuzun bağlantı bilgilerini verin. İçin **veri ağ geçidi** özelliği, ağ geçidi yüklemeniz için Azure portalında oluşturulan veri ağ geçidi seçin.

      Varsa **oturum açma türü** özelliği **uygulama sunucusu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Varsa **oturum açma türü** özelliği **grubu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP ileti sunucusu bağlantısını oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Varsayılan olarak, güçlü, geçersiz değerler için XML doğrulama şemaya gerçekleştirerek denetlemek için kullanılır. Bu davranış, daha önce sorunları belirlemenize yardımcı olabilir. **Güvenli yazarak** seçenek geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Daha fazla bilgi edinin [ **güvenli yazarak** seçeneği](#safe-typing).

   1. İşiniz bittiğinde **Oluştur**’u seçin.

      Mantıksal uygulamalar, ayarlar ve bağlantının düzgün çalıştığından emin olun, bağlantınızı test eder.

1. Şimdi Bul ve SAP sunucunuzdan bir eylem seçin.

   1. İçinde **SAP eylem** kutusunda, klasör simgesini seçin. Dosya listede bulun ve kullanmak istediğiniz SAP iletiyi seçin. Listede gezinmek için okları kullanın.

      Bu örnek ile IDoc seçer **siparişler** türü.

      ![Bulma ve IDoc eylemini seçin](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Gerçekleştirmek istediğiniz eylemi bulamazsanız, örneğin bir yolu el ile girebilirsiniz:

      ![IDoc eylem yol el ile sağlayın](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Değer, SAP eylemi için ifade Düzenleyicisi'ni kullanarak sağlayın. Bu şekilde, aynı eylem için farklı bir ileti türlerini kullanabilirsiniz.

      IDoc işlemleri hakkında daha fazla bilgi için bkz. [ileti şemaları IDOC işlemleri için](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   1. İçine tıklayın **giriş iletisi** dinamik içerik listesinde görünmesi kutusu. Bu listeden altında **olduğunda bir HTTP isteği alındığında**seçin **gövdesi** alan.

      Bu adım, HTTP istek Tetikleyiciniz gövdesi içeriğini içerir ve SAP sunucunuza çıktı gönderen.

      !["Gövde" alanı seçin](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      İşiniz bittiğinde, SAP eyleminizi şu örnekteki gibi görünür:

      ![Tam SAP eylemi](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>HTTP yanıt eylemi ekleme

Şimdi mantıksal uygulamanızın iş akışı için bir yanıt eylemi ekleyin ve SAP eylem çıktısı içerir. Bu şekilde mantıksal uygulamanız sonuçları özgün istek sahibine SAP sunucunuzdan döndürür.

1. SAP eylem altında mantıksal Uygulama Tasarımcısı seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "yanıt" girin. Eylem listesinden şu eylemi seçin: **Yanıt**

1. İçine tıklayın **gövdesi** dinamik içerik listesinde görünmesi kutusu. Bu listeden altında **SAP ileti gönderir**seçin **gövdesi** alan.

   ![Tam SAP eylemi](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Mantıksal uygulamanızı kaydedin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Mantıksal uygulamanızı önceden, mantıksal uygulama menüsünde etkinleştirilmemişse seçin **genel bakış**. Araç çubuğunda **etkinleştirme**.

1. Mantıksal Uygulama Tasarımcısı araç çubuğunda **çalıştırma**. Bu adım, mantıksal uygulamanızı el ile başlatır.

1. HTTP istek Tetikleyiciniz URL'yi bir HTTP POST isteği göndererek mantıksal uygulamanızı tetikleyecek ve isteğinizi içerik, ileti içerir. Gönderme isteği, bir aracı gibi kullanabileceğiniz [Postman](https://www.getpostman.com/apps).

   Bu makalede, XML biçiminde olmalı ve ad alanında, örneğin kullanmakta olduğunuz SAP eylemi için bir IDoc dosyası isteği gönderir:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. HTTP isteğinizi gönderdikten sonra mantıksal uygulamanızı yanıttan bekleyin.

   > [!NOTE]
   > Yanıt için gerekli tüm adımları içinde tamamlanmıyor, mantıksal uygulamanızın zaman aşımına uğrayabilir [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md). Bu durum bir durumda, istekleri engellenmiş. Sorunları tanılamanıza yardımcı olmak için öğrenin [denetleyin ve logic apps uygulamalarınızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Tebrikler, artık SAP sunucunuz ile iletişim kurabilen bir mantıksal uygulama oluşturdunuz. Mantıksal uygulamanız için SAP bağlantınız ayarladıysanız, BAPI ve RFC gibi diğer kullanılabilir SAP Eylemler keşfedebilirsiniz.

## <a name="receive-from-sap"></a>SAP'den alma

Bu örnek, bir SAP sistemden bir ileti alındığında Tetikleyiciler bir mantıksal uygulama kullanır.

### <a name="add-sap-trigger"></a>SAP tetikleyici ekleme

1. Azure portalında mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun.

1. Arama kutusuna filtreniz olarak "sap" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **SAP'den bir ileti alındığında**

   ![SAP tetikleyici ekleme](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Alternatif olarak, gidebilirsiniz **Kurumsal** sekmesini ve tetikleyiciyi seçin:

   ![Kurumsal sekmesinden SAP tetikleyici ekleyin](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Bağlantı ayrıntıları için istenirse, artık SAP bağlantı oluşturun. Bağlantınız zaten varsa, böylece SAP eyleminizi ayarlayabilirsiniz. Aksi halde, sonraki adıma geçin.

   **Şirket içi SAP bağlantı oluşturma**

   1. SAP sunucunuzun bağlantı bilgilerini verin. İçin **veri ağ geçidi** özelliği, ağ geçidi yüklemeniz için Azure portalında oluşturulan veri ağ geçidi seçin.

      Varsa **oturum açma türü** özelliği **uygulama sunucusu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Varsa **oturum açma türü** özelliği **grubu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP ileti sunucusu bağlantısını oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Varsayılan olarak, güçlü, geçersiz değerler için XML doğrulama şemaya gerçekleştirerek denetlemek için kullanılır. Bu davranış, daha önce sorunları belirlemenize yardımcı olabilir. **Güvenli yazarak** seçenek geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Daha fazla bilgi edinin [ **güvenli yazarak** seçeneği](#safe-typing).

1. SAP sistem yapılandırmanıza göre gerekli parametreleri belirtin.

   İsteğe bağlı olarak, bir veya daha fazla SAP eylemleri sağlayabilir. Bu eylemlerin listesi, SAP sunucunuzdan veri ağ geçidi üzerinden tetikleyici aldığı iletileri belirtir. Boş bir liste, tetikleyici tüm mesajlarının iletildiğini belirtir. Liste birden fazla ileti sahipse, tetikleyici yalnızca listesinde belirtilen iletileri alır. SAP sunucunuzdan gönderilen iletiler, ağ geçidi tarafından reddedilir.

   Dosya Seçici'den bir SAP eylemini seçebilirsiniz:

   ![SAP eylemini seçin](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Veya bir eylem el ile belirtebilirsiniz:

   ![SAP eylemi el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Eylem tetikleyici birden fazla ileti almak için ayarladığınızda, nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir.

   ![Tetikleyici örneği](media/logic-apps-using-sap-connector/example-trigger.png)  

   SAP eylem hakkında daha fazla bilgi için bkz. [ileti IDOC işlemleri için şemalar](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

1. SAP sisteminizden iletiler almaya başlamak için şimdi mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Mantıksal uygulamanız artık SAP sisteminizden iletileri almaya hazırdır.

> [!NOTE]
> SAP tetikleyici bir yoklama tetikleyici, ancak bir Web kancası tabanlı tetikleyici yerine değil. Yalnızca bir ileti bulunduğunda tetikleyici yok yoklama gerekli olmayacak biçimde geçidinden çağrılır.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Mantıksal uygulamanızı tetikleyecek SAP sisteminizden bir ileti gönderin.

1. Mantıksal uygulama menüsünde, **genel bakış**ve gözden geçirme **çalıştırma geçmişi** mantıksal uygulamanız için yeni her çalıştırma için.

1. Tetikleyici çıktılar bölümünü SAP sisteminizden gönderilen ileti gösterilir en son çalıştırma açın.

## <a name="generate-schemas-for-artifacts-in-sap"></a>SAP içinde yapıtları için şemalar oluşturur

Bu örnek, bir HTTP isteği ile tetikleyebileceğiniz bir mantıksal uygulama kullanır. SAP eylemi bir SAP sistemiyle şemaları BAPI ve belirtilen ara belge (IDoc) üretmek için bir istek gönderir. Yanıtta şemaları tümleştirme hesabı için Azure Resource Manager Bağlayıcısı kullanılarak yüklenir.

### <a name="add-http-request-trigger"></a>HTTP isteği tetikleyicisi Ekle

1. Azure portalında mantıksal Uygulama Tasarımcısı açılır bir boş mantıksal uygulama oluşturun.

1. Arama kutusuna filtreniz olarak "http isteği" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında**

   ![HTTP isteği tetikleyicisi ekleyin](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Şimdi mantıksal uygulamanız için bir uç nokta URL'si oluşturabilmek mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**'i seçin.

   Uç nokta URL'si artık Tetikleyiciniz, örneğin görünür:

   ![Uç noktası için URL oluşturun](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Şemaları oluşturmak için SAP eylem ekleme

1. Mantıksal Uygulama Tasarımcısı tetikleyicinin altında seçin **yeni adım**.

   !["Yeni adım" seçin](./media/logic-apps-using-sap-connector/add-action.png)

1. Arama kutusuna filtreniz olarak "sap" girin. Eylem listesinden şu eylemi seçin: **Şemalar oluşturabilirsiniz**
  
   ![SAP Gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Alternatif olarak, seçebilirsiniz **Kurumsal** sekmesini ve SAP eylemi seçin.

   ![Kurumsal sekmesinden SAP Gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Bağlantı ayrıntıları için istenirse, artık SAP bağlantı oluşturun. Bağlantınız zaten varsa, böylece SAP eyleminizi ayarlayabilirsiniz. Aksi halde, sonraki adıma geçin.

   **Şirket içi SAP bağlantı oluşturma**

   1. SAP sunucunuzun bağlantı bilgilerini verin. İçin **veri ağ geçidi** özelliği, ağ geçidi yüklemeniz için Azure portalında oluşturulan veri ağ geçidi seçin.

      Varsa **oturum açma türü** özelliği **uygulama sunucusu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      Varsa **oturum açma türü** özelliği **grubu**, genellikle isteğe bağlı görünür, bu özellikler gereklidir:

      ![SAP ileti sunucusu bağlantısını oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Varsayılan olarak, güçlü, geçersiz değerler için XML doğrulama şemaya gerçekleştirerek denetlemek için kullanılır. Bu davranış, daha önce sorunları belirlemenize yardımcı olabilir. **Güvenli yazarak** seçenek geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Daha fazla bilgi edinin [ **güvenli yazarak** seçeneği](#safe-typing).

   1. İşiniz bittiğinde **Oluştur**’u seçin. 
   
      Mantıksal uygulamalar, ayarlar ve bağlantının düzgün çalıştığından emin olun, bağlantınızı test eder.

1. Şema oluşturmak istediğiniz yapıt yolunu belirtin.

   Dosya Seçici'den SAP eylemini seçebilirsiniz:

   ![SAP eylemini seçin](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Ya da eylem el ile girebilirsiniz:

   ![SAP eylemi el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Birden fazla yapıt şemaları oluşturmak için her bir yapıt için SAP eylem ayrıntıları gibi sağlayın:

   ![Yeni Öğe Ekle seçin](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![İki öğeleri göster](media/logic-apps-using-sap-connector/schema-generator-example.png)

   SAP eylem hakkında daha fazla bilgi için bkz. [ileti şemaları IDOC işlemleri için](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Tasarımcı araç çubuğunda **çalıştırma** için mantıksal uygulamanız için bir çalıştırma tetikleyin.

1. Çalıştır'ı açın ve çıkışları denetleyin **şemalar oluşturabilirsiniz** eylem.

   Belirtilen ileti listesi için oluşturulan şemalar çıktıları gösterir.

### <a name="upload-schemas-to-integration-account"></a>Şemaları tümleştirme hesabına yükleyin.

İsteğe bağlı olarak yükleyebilir veya oluşturulan şemalar gibi bir blob, depolama veya tümleştirme hesabı depolarından depolar. Bu örnek, şemaları tümleştirme hesabı aynı mantıksal uygulama için Azure Resource Manager Bağlayıcısı kullanarak nasıl yükleneceğini gösterir. Bu nedenle tümleştirme hesapları diğer XML eylemleri ile birinci sınıf bir deneyim sağlar.

1. Logic Apps Tasarımcısı'nda tetikleyicinin altında seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "Resource Manager" girin. Şu eylemi seçin: **Bir kaynak güncelle**

   ![Azure Resource Manager eylemi seçin](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Azure aboneliğiniz, Azure kaynak grubu ve tümleştirme hesabı dahil olmak üzere eylemi için ayrıntıları girin. SAP belirteçleri alanları eklemek için alanlar kutuları içine tıklayın ve görüntülenen dinamik içerik listesinden seçin.

   1. Açık **yeni parametre Ekle** listesinde ve seçin **konumu** ve **özellikleri** alanları.

   1. Bu örnekte gösterildiği gibi ayrıntıları bu yeni alanlar için sağlar.

      ![Azure Resource Manager eylemi için ayrıntıları girin](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   SAP **şemalar oluşturabilirsiniz** eylem Tasarımcı otomatik olarak ekler, böylece bu şemalar koleksiyon olarak oluşturur bir **her** döngüye eylem. Bu eylem nasıl görüneceğini gösteren bir örnek aşağıda verilmiştir:

   ![Azure Resource Manager eylemi "for each" döngüsü](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  

   > [!NOTE]
   > Şemaları base64 ile kodlanmış bir biçim kullanın. Şemaları tümleştirme hesabı için karşıya yüklemek için bunlar kullanarak çözülmüş gereken `base64ToString()` işlevi. Kodunu gösteren bir örnek aşağıdadır `"properties"` öğesi:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Tasarımcı araç çubuğunda **çalıştırma** mantıksal uygulamanızı el ile tetiklemek için.

1. Başarılı bir sonra çalıştırın, tümleştirme hesabı'na gidin ve oluşturulan şemaları mevcut olduğunu denetleyin.

## <a name="enable-secure-network-communications-snc"></a>Güvenli ağ iletişimi (SNC) etkinleştir

Başlamadan önce yukarıda listelenen sağlandığından emin olun [önkoşulları](#pre-reqs):

* Şirket içi veri ağ geçidi, SAP sistemine aynı ağda bulunan bir makinede yüklü.

* Çoklu oturum açma için ağ geçidi için SAP kullanıcı eşlenmiş bir kullanıcı olarak çalışıyor.

* Ek güvenlik işlevleri sağlayan SNC kitaplığı, veri ağ geçidi ile aynı makinede yüklü. Örnekler bunlardan bazıları [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos, NTLM, ve benzeri.

SNC SAP sistemine gelen veya isteklerinizi etkinleştirmek için seçin **kullanım SNC** SAP bağlantı onay kutusunu işaretleyin ve bu özellikleri sağlar:

   ![SAP SNC bağlantı yapılandırma](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Özellik | Açıklama |
   |----------| ------------|
   | **SNC kitaplığı** | SNC kitaplığı adı veya NCo yükleme konumu göreli yol veya mutlak yol. Örneğin, `sapsnc.dll` veya `.\security\sapsnc.dll` veya `c:\security\sapsnc.dll` |
   | **SNC SSO** | SNC bağlanırken SNC kimlik genellikle arayan kimliğini doğrulamak için kullanılır. Böylece kullanıcı adı ve parola bilgiler arayan kimliğini doğrulamak için kullanılabilir, ancak satır yine de şifrelenir geçersiz kılma başka bir seçenektir. |
   | **SNC adımı** | Çoğu durumda, bu özelliği atlanmış olabilir. Yüklü SNC çözümü, genellikle kendi SNC adı bilir. "Birden çok kimlik" destekleyen çözümler için bu belirli hedef veya sunucu için kullanılacak kimlik belirtmeniz gerekebilir. |
   | **SNC iş ortağı adı** | Arka uç SNC adı |
   | **SNC kaliteyi koruma** | SNC bu belirli hedef/sunucu iletişimi için kullanılacak hizmet kalitesi. Varsayılan değer, arka uç sistem tarafından tanımlanır. En yüksek değer SNC için kullanılan güvenlik ürüne göre tanımlanır. |
   |||

   > [!NOTE]
   > Snc_lıb ve snc_lıb_64 ortam değişkenleri, veri ağ geçidi ve SNC kitaplığı sahip olduğu bir makinede ayarlanmamalıdır. Ayarlanırsa, bunlar bağlayıcı aracılığıyla geçirilen SNC kitaplığı değer öncelikli.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Güvenli yazma

SAP bağlantınızı oluşturduğunuzda varsayılan olarak, güçlü yazım, yazım geçersiz değerler için XML doğrulama şemaya gerçekleştirerek denetlemek için kullanılır. Bu davranış, daha önce sorunları belirlemenize yardımcı olabilir. **Güvenli yazarak** seçenek geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Seçerseniz **güvenli yazarak**, SAP TIMS türü ve DATS türünü dizeleri yerine XML eşdeğerlerine olarak kabul edilir `xs:date` ve `xs:time` nerede `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Güvenli yazarak tüm şema oluşturma, hem "gönderilen" Yük ve yanıt "alındı" ve tetikleyici için gönderilen iletiyi davranışını etkiler. 

Güçlü yazım, yazım kullanıldığında (**güvenli yazarak** etkin değil), şema DATS ve TIMS türleri için daha basit XML türleri eşler:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Güçlü yazım, yazım kullanarak iletileri gönderirken, eşleşen bir XML türü biçimine DATS ve TIMS yanıt uyumludur:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Zaman **güvenli yazarak** olan etkin şema DATS eşler ve XML TIMS türlerine dize uzunluğu kısıtlamaları yalnızca alanlarla örneğin:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

İletileri gönderirken **güvenli yazarak** etkin DATS ve TIMS yanıt şu örnekteki gibi görünür:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```


## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Şu anda bilinen sorunlar ve sınırlamalar için SAP bağlayıcısını şunlardır:

* Yalnızca tek bir gönderme için SAP çağrısı veya iletinin tRFC ile çalışır. Aynı oturumda birden çok tRFC çağrıları yapma gibi iş uygulaması programlama arabirimi (BAPI) işleme düzeni desteklenmiyor.

* SAP tetikleyici SAP'den batch IDoc'ları alma desteklemez. Bu eylem, SAP sistemine ve veri ağ geçidi arasında bağlantı hatası RFC neden olabilir.

* SAP tetikleyici veri ağ geçidi kümelerini desteklemez. Yük devretme bazı durumlarda, SAP sistemiyle iletişim kuran veri ağ geçidi düğümü etkin düğüm, beklenmeyen davranışla farklılık gösterebilir. Veri ağ geçidi kümeleri gönderme senaryoları için desteklenir.

* SAP bağlayıcısı şu anda SAP yönlendirici dizeleri desteklememektedir. Şirket içi veri ağ geçidi, bağlanmak istediğiniz SAP sistemiyle aynı LAN üzerinde olmalıdır.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, gözden geçirme [bağlayıcının başvuru sayfası](/connectors/sap/).

## <a name="next-steps"></a>Sonraki adımlar

* [Şirket içi sistemlere bağlanın](../logic-apps/logic-apps-gateway-connection.md) mantıksal uygulamalardan
* Doğrulama, Dönüşüm ve diğer ileti işlemleri ile öğrenin [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
