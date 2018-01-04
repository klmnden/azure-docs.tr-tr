---
title: "Öğretici: Azure BizTalk Services kullanarak EDIFACT faturaları işlem | Microsoft Docs"
description: "Oluşturma ve kutusunu Bağlayıcısı veya API uygulamasını yapılandırma ve Azure App Service'te bir mantıksal uygulama içinde kullanma"
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 2ebd6a8cb70f218c3b56bc78c9b853dbf51ab468
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Öğretici: Azure BizTalk Services kullanarak işlem EDIFACT faturalar

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk Services Portalı'nı yapılandırmak ve X12 ve EDIFACT sözleşmelerini dağıtmak için kullanabilirsiniz. Bu öğreticide, faturalar ticaret iş ortakları arasında değiş tokuşu için EDIFACT sözleşmesi oluşturma ele. Bu öğretici EDIFACT ileti değiş tokuşu Northwind ve Contoso olmak üzere iki ticari ortaklar içeren bir uçtan uca iş çözüm yazılır.  

## <a name="sample-based-on-this-tutorial"></a>Bu öğretici temel örnek
Bu öğretici bir örnek yazılır **EDIFACT faturaları kullanarak BizTalk Services gönderme**, indirin kullanılabilen [MSDN kod Galerisi'nden](http://go.microsoft.com/fwlink/?LinkId=401005). Örnek kullanın ve örnek nasıl oluşturulmuş anlamak için bu öğreticiyi. Veya, kendi çözümü başından başlayarak yukarı oluşturmak için bu öğreticiyi kullanabilirsiniz. Bu çözümün nasıl oluşturulmuş anlamanız Bu öğreticinin ikinci bir yaklaşım yöneliktir. Ayrıca, mümkün olduğunca öğretici örneği ile tutarlıdır ve yapılar (örneğin, şemalar, Dönüşümlerin) için aynı adları olarak örnekteki kullanılan kullanır.  

> [!NOTE]
> Bu çözüm EDI köprüsüne EAI köprü ileti gönderme içerdiğinden yeniden kullanır [BizTalk Services örnek zincirleme köprüsü](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) örnek.  
> 
> 

## <a name="what-does-the-solution-do"></a>Çözüm ne yapar?
Bu çözümde, Northwind EDIFACT faturalar Contoso alır. Bu faturalar standart EDI biçimde değil. Bu nedenle, Northwind için fatura göndermeden önce bir EDIFACT (INVOIC olarak da bilinir) fatura belgeye dönüştürülmesi gerekir. Alındığında, Northwind EDIFACT fatura işlemek ve Contoso için (CONTRL olarak da bilinir) bir denetim iletisi döndürmek gerekir.

![][1]  

Bu iş senaryosu elde etmek için Microsoft Azure BizTalk Services ile sağlanan özellikler Contoso kullanır.

* Contoso EAI köprüleri orijinal faturayı EDIFACT INVOIC dönüştürmek için kullanır.
* EAI köprü bu durumda BizTalk Services Portalı'nda yapılandırılmış anlaşmanın bir parçası olarak dağıtılan bir EDI gönderme köprüsü iletiyi gönderir.
* EDI gönderme köprüsü EDIFACT INVOIC işler ve Northwind olarak yönlendirir.
* Fatura aldıktan sonra Northwind döner bir CONTRL iletisi EDI anlaşmanın bir parçası dağıtılan köprüsü alırsınız.  

> [!NOTE]
> İsteğe bağlı olarak, bu çözüm Ayrıca toplu işleme yığınlardaki ayrı ayrı her bir fatura göndermek yerine faturaları göndermek için nasıl kullanılacağını gösterir.  
> 
> 

Senaryonun tamamlanması için Service Bus kuyruklarını fatura Contoso Northwind olarak göndermek veya Northwind onay almak için kullanırız. Bu kuyruklar, bir yükleme olarak kullanılabilir ve kullanılabilir olan örnek paketine dahil olan bir istemci uygulaması kullanılarak oluşturulabilir Bu öğreticinin bir parçası olarak.  

## <a name="prerequisites"></a>Ön koşullar
* Hizmet veri yolu ad alanı olması gerekir. Bir ad alanı oluşturma ile ilgili yönergeler için bkz: [nasıl yapılır: oluşturmak veya bir hizmet veri yolu hizmet Namespace değiştirmek](https://msdn.microsoft.com/library/azure/hh674478.aspx). Bize sağlanan, bir hizmet veri yolu ad alanı zaten sahip olduğunuzu varsaymaktadır adlı **edifactbts**.
* BizTalk Services aboneliğinizin olması gerekir. Bu öğretici için bize adlı bir BizTalk Services abonelik olduğunu varsayın **contosowabs**.
* BizTalk Hizmetleri aboneliğinize BizTalk Hizmetleri portalındaki kaydedin. Yönergeler için bkz: [bir BizTalk hizmeti dağıtımı BizTalk Hizmetleri portalındaki kaydetme](https://msdn.microsoft.com/library/hh689837.aspx)
* Visual Studio yüklü olmalıdır.
* BizTalk Services SDK'sı olması gerekir. SDK'dan gelen indirebilirsiniz [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>1. adım: hizmet veri yolu Kuyrukları Oluşturma
Bu çözüm, ileti ticaret iş ortakları arasında değiş tokuşu için Service Bus kuyruklarını kullanır. Contoso ve Northwind iletileri EAI ve/veya EDI köprüleri bunları burada tüketen gelen sıralara gönderir. Bu çözüm için üç hizmet veri yolu kuyrukları gerekir:

* **northwindreceive** – Northwind Contoso fatura bu sırayı alır.
* **contosoreceive** – Contoso Bu kuyruk Northwind bildirim alır.
* **askıya** – tüm askıya alınan iletileri bu kuyruğa yönlendirilir. İşleme sırasında başarısız olursa iletileri askıya alınır.

Bu hizmet veri yolu kuyrukları örnek paketinde bulunan bir istemci uygulaması kullanarak oluşturabilirsiniz.  

1. Örnek indirdiğiniz konumdan açmak **öğretici gönderme faturaları kullanarak BizTalk Services EDI Bridges.sln**.
2. Tuşuna **F5** oluşturun ve başlatın **öğretici istemci** uygulama.
3. Ekranında, hizmet veri yolu ACS ad alanı, verenin adı ve verenin anahtarı girin.
   
   ![][2]  
4. Bir ileti kutusu üç sıraları hizmet veri yolu ad alanınız içinde oluşturulacak ister. **Tamam** düğmesine tıklayın.
5. Öğretici çalıştıran istemci bırakın. Açık öğesini **Service Bus** > ***hizmet veri yolu ad alanınız*** > **sıraları**ve üç sıraları oluşturulduğunu doğrulayın.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>2. adım: Oluşturma ve ticari ortak sözleşmesi dağıtma
Contoso Northwind arasındaki ticari ortak sözleşmesi oluşturun. Ticari ortak sözleşmesi hangi ileti şeması kullanmak için hangi Mesajlaşma protokolünü kullanmak için vb. gibi iki iş ortakları arasında bir ticari sözleşme tanımlar. Ticari ortak sözleşmesi, bir ticaret ortakları iletileri göndermek için iki EDI köprüleri içerir (adlı **EDI gönderme köprüsü**) ve bir ticaret ortaklarından gelen iletileri almak için (adlı **EDI alma köprüsü**).

Bu çözüm bağlamında EDI gönderme köprüsü gönderme tarafı Sözleşmesi'nin karşılık gelir ve EDIFACT fatura Contoso Northwind olarak göndermek için kullanılır. Benzer şekilde, EDI alma köprüsü alış tarafı Sözleşmesi'nin karşılık gelen ve Northwind alma bildirimleri için kullanılır.  

### <a name="create-the-trading-partners"></a>Ticari ortaklar oluşturma
İle başlamak ticari ortaklar Contoso Northwind oluşturun.  

1. BizTalk Services Portalı'nda üzerinde **ortakları** sekmesini tıklatın, **Ekle**.
2. Yeni iş ortağı sayfasındaki girin **Contoso** bir ortak adı ve ardından olarak **kaydetmek**.
3. İkinci iş ortağı oluşturmak için adımı tekrarlayın **Northwind**.  

### <a name="create-the-agreement"></a>Sözleşmesi oluşturma
Ticari ortak sözleşmeleri ortakları ticari iş profilleri arasında oluşturulur. Bu çözüm ortakları oluşturduğumuz olduğunda otomatik olarak oluşturulan varsayılan iş ortağı profilleri kullanır.  

1. BizTalk Services Portalı'nda tıklatın **anlaşmaları** > **Ekle**.
2. Yeni anlaşmanın içinde **genel ayarları** sayfasında, aşağıdaki resimde gösterildiği gibi değerleri belirtin ve ardından **devam**.
   
   ![][3]  
   
   Tıklattıktan sonra **devam**, iki sekme **alma ayarı** ve **gönderme ayarları** kullanılabilir hale gelir.
3. Contoso Northwind arasındaki gönderme sözleşmesi oluşturun. Bu anlaşma, Contoso EDIFACT fatura Northwind olarak nasıl göndereceğini yönetir.
   
   1. Tıklatın **gönderme ayarları**.
   2. Varsayılan değerleri üzerinde korumak **gelen URL**, **dönüştürme**, ve **Batching** sekmeleri.
   3. Üzerinde **Protokolü** sekmesinde, altında **şemaları** bölümünde, karşıya yükleme **EFACT_D93A_INVOIC.xsd** şema. Bu şema örnek paketi ile birlikte kullanılabilir.
      
      ![][4]  
   4. Üzerinde **aktarım** sekmesinde, Service Bus kuyruklarını ayrıntılarını belirtin. Gönderme tarafı anlaşması için kullandığımız **northwindreceive** EDIFACT fatura Northwind olarak göndermek için sıra ve **askıya** işleme sırasında başarısız ve askıya alınan iletileri yönlendirmek için sıra. Bu kuyruklar oluşturulan **1. adım: hizmet veri yolu Kuyrukları Oluşturma** (Bu konudaki).
      
      ![][5]  
      
      Altında **Aktarım Ayarları > taşıma türü** ve **ileti askıya Ayarları > taşıma türü**, Azure hizmet veri yolu seçin ve değerleri görüntüde gösterildiği gibi girin.
4. Alma sözleşmesinde Contoso Northwind arasındaki oluşturun. Bu anlaşma nasıl Contoso Northwind alındısı yönetir.
   
   1. Tıklatın **alma ayarlarını**.
   2. Varsayılan değerleri üzerinde korumak **aktarım** ve **dönüştürme** sekmeleri.
   3. Üzerinde **Protokolü** sekmesinde, altında **şemaları** bölümünde, karşıya yükleme **EFACT_4.1_CONTRL.xsd** şema. Bu şema örnek paketi ile birlikte kullanılabilir.
   4. Üzerinde **rota** sekmesinde, yalnızca onayları Northwind gelen Contoso için yönlendirildiğinden emin olmak için bir filtre oluşturun. Altında **yönlendirme ayarlarını**, tıklatın **Ekle** yönlendirme filtre oluşturmak için.
      
      ![][6]  
      
      1. İçin değerler sağlayın **kural adı**, **yol kuralı**, ve **rota hedef** görüntüde gösterildiği gibi.
      2. **Kaydet** düğmesine tıklayın.
   5. Üzerinde **rota** yeniden sekmesinde, askıya alınmış onayları (işleme sırasında başarısız onayları) burada yönlendirilir belirtin. Azure Service Bus aktarım türü ayarlayın, rota hedef türüne **sıra**, kimlik doğrulama türü için **paylaşılan erişim imzası** (SAS), hizmet veri yolu ad alanı için SAS bağlantı dizesi girin ve sıra adı enter **askıya**.
5. Son olarak, tıklatın **dağıtma** sözleşmesi dağıtmak için. Uç noktaları unutmayın burada gönderme ve alma anlaşmaları dağıtılan.
   
   * Üzerinde **gönderme ayarları** sekmesinde, altında **gelen URL**, uç nokta unutmayın. EDI gönderme köprüsü kullanarak Northwind Contoso ileti göndermek için bu uç noktaya bir ileti göndermeniz gerekir.
   * Üzerinde **alma ayarı** sekmesinde, altında **aktarım**, uç nokta unutmayın. Contoso için Northwind ileti göndermek için EDI almak kullanarak köprüsü, bu uç noktaya bir ileti göndermesi gerekir.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>3. adım: Oluşturma ve BizTalk Services projesi dağıtma
Önceki adımda EDI gönderme dağıtılan ve EDIFACT faturaları ve onayları işleme anlaşmaları alabilirsiniz. Bu sözleşmeler, yalnızca standart EDIFACT ileti şemaya uygun işlem iletileri görüntüleyebilirsiniz. Bununla birlikte, senaryo için bu çözüm, Contoso fatura Northwind için şirket içi bir özel şemada gönderir. EDI gönderme köprüsü için ileti gönderilmeden önce bu nedenle, bu şirket içi şemadan standart EDIFACT fatura şemaya dönüştürülmesi gerekir. BizTalk Services EAI proje gerçekleştirir.

BizTalk Services projesi **InvoiceProcessingBridge**, Dönüşümlerin ileti Ayrıca, indirdiğiniz örnek bir parçası olarak yer almaktadır. Proje aşağıdaki yapılar içerir:

* **INHOUSEINVOICE. XSD** – Northwind olarak gönderilen şirket içi faturayı şeması.
* **EFACT_D93A_INVOIC. XSD** – standart EDIFACT fatura şeması.
* **EFACT_4.1_CONTRL. XSD** – Contoso için Northwind gönderir EDIFACT bildirim şeması.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – şirket içi faturayı şema standart EDIFACT fatura şemaya eşlemeleri Dönüştür.  

### <a name="create-the-biztalk-services-project"></a>BizTalk Services projesi oluşturma
1. Visual Studio çözümünde InvoiceProcessingBridge projenizi genişletin ve ardından açın **MessageFlowItinerary.bcs** dosya.
2. Tuvalde herhangi bir yere tıklayın ve ayarlayın **BizTalk hizmeti URL'si** özelliği kutusunda BizTalk Services abonelik adını belirtin. Örneğin, `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Araç Kutusu'ndan sürükleyin bir **Xml One-Way köprüsü** tuvale. Ayarlama **varlık adı** ve **göreli adres** köprüsüne özelliklerini **ProcessInvoiceBridge**. Çift **ProcessInvoiceBridge** köprüsü yapılandırması yüzeyini açın.
4. İçinde **ileti türleri** kutusunda, artı (**+**) düğmesini gelen ileti şemasını belirtin. Gelen ileti EAI köprü için her zaman şirket içi faturayı olduğundan, bu ayar **INHOUSEINVOICE**.
   
   ![][8]  
5. Tıklatın **Xml dönüştürme** şekli ve özellik kutusuna için **eşlemeleri** özelliği, üç nokta düğmesine (**...** ) düğmesi. İçinde **eşlemeleri seçimi** iletişim kutusunda **INHOUSEINVOICE_to_D93AINVOIC** dönüşüm dosyasında ve ardından **Tamam**.
   
   ![][9]  
6. Geri dönerek **MessageFlowItinerary.bcs**, araç kutusu'ndan sürükleyin bir **iki yönlü dış hizmet uç noktası** sağındaki **ProcessInvoiceBridge**. Ayarlama, **varlık adı** özelliğine **EDIBridge**.
7. Çözüm Gezgini'nde genişletin **MessageFlowItinerary.bcs** çift tıklayın ve **EDIBridge.config** dosya. İçeriğinin yerine geçecek **EDIBridge.config** aşağıdaki.
   
   > [!NOTE]
   > .Config dosyasını düzenlemek neden gerekiyor mu? Köprü Tasarımcı tuvaline eklediğimiz dış hizmet uç noktası, daha önce dağıttığınız EDI köprüleri temsil eder. EDI köprüleri gönderme ile iki yönlü köprüleri olan ve alma tarafı. Ancak, tek yönlü bir köprü köprüsü Designer'a eklediğimiz EAI köprü olur. Bu nedenle, iki köprüleri farklı ileti exchange desenlerini işlemek için bir özel köprüsü davranış .config dosyasına yapılandırmasına dahil olmak üzere kullanırız. Ayrıca, özel davranış da EDI gönderme köprüsü uç kimlik doğrulaması gerçekleştirir. Bu özel davranış ayrı bir örnek olarak kullanılabilir [BizTalk Services örnek - EDI EAI zincirleme köprüsü](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Bu çözüm örneği yeniden kullanır.  
   > 
   > 
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.serviceModel>
       <extensions>
         <behaviorExtensions>
           <add name="BridgeAuthentication" 
                 type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
         </behaviorExtensions>
       </extensions>
       <behaviors>
         <endpointBehaviors>
           <behavior name="BridgeAuthenticationConfiguration">
             <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
             <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                   issuername="owner" 
                                   issuersecret="[YOUR ACS SECRET]" />
             <webHttp />
           </behavior>
         </endpointBehaviors>
       </behaviors>
       <bindings>
         <webHttpBinding>
           <binding name="BridgeBindingConfiguration">
             <security mode="Transport" />
           </binding>
         </webHttpBinding>
       </bindings>
       <client>
         <clear />
         <!--
           Go BizTalk Portal > Agreement > Send Settings > Inbound URL
           Copy the Endpoint URL and paste it in the below address field
         -->
         <endpoint name="TwoWayExternalServiceEndpointReference1" 
                   address="[YOUR EDI BRIDGE SEND URI]" 
                   behaviorConfiguration="BridgeAuthenticationConfiguration" 
                   binding="webHttpBinding" 
                   bindingConfiguration="BridgeBindingConfiguration" 
                   contract="System.ServiceModel.Routing.IRequestReplyRouter" />
       </client>
     </system.serviceModel>
   </configuration>
   
   ```
8. Yapılandırma ayrıntıları dahil etmek için EDIBridge.config dosyasını güncelleştirme
   
   * Altında  *<behaviors>* , ACS ad alanı ve BizTalk Services aboneliğiyle ilişkili anahtarı sağlayın.
   * Altında  *<client>* , EDI Gönderme Sözleşmesi dağıtıldığı uç noktası sağlar.
   
   Değişiklikleri kaydetmek ve yapılandırma dosyasını kapatın.
9. Araç Kutusu'ndan tıklatın **bağlayıcı** ve katılma **ProcessInvoiceBridge** ve **EDIBridge** bileşenleri. Bağlayıcıyı seçin ve Özellikler kutusunda ayarlanan **filtre koşulu** için **eşleşen tüm**. Bu EAI köprü tarafından işlenen tüm iletileri EDI köprüsüne yönlendirilir sağlar.
   
   ![][10]  
10. Çözüme değişiklikleri kaydedin.  

### <a name="deploy-the-project"></a>Projeyi dağıtın
1. BizTalk Services projesi oluşturulduğu bilgisayarda indirin ve BizTalk Services aboneliğiniz için SSL sertifikası yükleyin. BizTalk Services'ın altında tıklatın **Pano**ve ardından **SSL sertifikası indir**. Sertifikaya çift tıklayın ve yüklemeyi tamamlamak için istemi izleyin. Altına sertifikayı yüklediğinizden emin olun **güvenilen kök sertifika yetkilileri** sertifika deposu.
2. Visual Studio Çözüm Gezgini'nde sağ **InvoiceProcessingBridge** proje ve ardından **dağıtma**.
3. Görüntüde gösterildiği gibi değerler sağlayın ve ardından **dağıtma**. Tıklayarak BizTalk Services için ACS kimlik bilgilerini alabilirsiniz **bağlantı bilgilerini** BizTalk Services panosundan.
   
   ![][11]  
   
   Çıkış Bölmesi'nden, EAI köprü dağıtıldığı, örneğin, uç noktasını kopyalayın `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Bu uç nokta URL'si daha sonra ihtiyacınız olacak.  

## <a name="step-4-test-the-solution"></a>4. adım: Test çözümü
Bu konuda, çözümü kullanarak test etmek nasıl ele **öğretici istemci** örnek bir parçası olarak sağlanan uygulama.  

1. Visual Studio'da başlatmak için F5 tuşuna basarak **öğretici istemci**.
2. Ekran, Service Bus kuyruklarını burada oluşturduğumuz adımından önceden doldurulmaz değerlere sahip olmalıdır. **İleri**’ye tıklayın.
3. BizTalk Services abonelik için ACS kimlik bilgilerini ve uç noktaları sonraki penceresinde sağlamak burada EAI ve EDI (Al) köprüleri dağıtılır.
   
   EAI köprü uç noktası önceki adımda kopyaladığınız. EDI köprüsü uç noktası, BizTalk Services Portalı'nda alırsınız, anlaşmayı Git > alma ayarı > aktarım > uç nokta.
   
   ![][12]  
4. Contoso altında sonraki penceresinde tıklatın **şirket içi faturayı gönderin** düğmesi. Dosya Aç iletişim kutusu, INHOUSEINVOICE.txt dosyasını açın. Dosya içeriğini inceleyin ve ardından **Tamam** fatura göndermek için.
   
   ![][13]  
5. Birkaç saniye içinde Northwind fatura alınır. Tıklatın **iletiyi görüntüle** Northwind tarafından alınan fatura görmek için bağlantı. Contoso tarafından gönderilen bir şirket içi bir şema ederken Northwind tarafından alınan fatura standart EDIFACT şemada nasıl olduğuna dikkat edin.
   
   ![][14]  
6. Fatura'yı seçin ve ardından **gönderin alındısı**. Açılan iletişim kutusunda, değişim kimliği alınan fatura ve gönderilen bildirim aynı olduğuna dikkat edin. Tamam'ı **gönderin alındısı** iletişim kutusu.
   
   ![][15]  
7. Birkaç saniye içinde bildirim başarıyla Contoso alındı.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>(İsteğe bağlı) 5. adım: gönderme EDIFACT fatura gruplar halinde
BizTalk Services EDI köprüleri de destekler giden iletiler toplu işleme. Bu özellik, toplu iletiler (belirli ölçütlerine uyan) yerine tek bir ileti almak için tercih ettiğiniz iş ortakları almak için kullanışlıdır.

Toplu işlemler ile çalışırken en önemli en boy toplu sürüm ölçütlerini olarak da bilinir gerçek sürümüdür. Sürüm ölçütlerini nasıl alıcı ortağın iletileri almak istediği üzerinde bağlı olabilir. Toplu işleme etkinleştirilirse sürüm ölçütlerini verildikten kadar EDI köprüsü giden ileti alıcı ortağın göndermez. Örneğin, bir toplu işleme ölçütlerini toplu ileti boyutu gönderileri üzerinde temel yalnızca 'n' iletileri toplu. Sağlayacak şekilde her gün sabit bir saatte bir toplu gönderilen bir toplu iş ölçütlerini de zamana dayalı olabilir. Bu çözümde, ileti boyutu tabanlı ölçütleri deneyin.

1. BizTalk Services Portalı'nda daha önce oluşturduğunuz Sözleşmesi'ı tıklatın. Gönderme Ayarları > Toplu işleme > Toplu ekleyin.
2. Batch, adı **InvoiceBatch**, bir açıklama girin ve ardından **sonraki**.
3. Hangi iletilerin toplu tanımlayan bir toplu ölçütü belirtin. Bu çözümde, tüm iletileri toplu. Bu nedenle, Gelişmiş tanımları seçeneği kullan'ı seçin ve girin **1 = 1**. Bu her zaman true olacak bir durumdur ve bu nedenle tüm iletileri toplu. **İleri**’ye tıklayın.
   
   ![][17]  
4. Toplu sürüm ölçütlerini belirtin. Açılan kutusundan seçin **MessageCountBased**ve **sayısı**, belirtin **3**. Başka bir deyişle, toplu üç iletiler Northwind olarak gönderilir. **İleri**’ye tıklayın.
   
   ![][18]  
5. Özeti gözden geçirin ve ardından **kaydetmek**. Tıklatın **dağıtma** sözleşmesi yeniden dağıtılamadı.
6. Geri dönerek **öğretici istemci**, tıklatın **şirket içi faturayı gönderin**ve fatura göndermek için istemleri izleyin. Toplu iş boyutu karşılanmadı çünkü hiçbir fatura Northwind aldığınızı fark edeceksiniz. Böylece Northwind olarak gönderilen üç fatura iletileri sahip iki kez daha, bu adımı yineleyin. Bu, 3 mesaj toplu sürüm ölçütlerini karşılayan ve northwind'de faturayı görmelisiniz.

<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

