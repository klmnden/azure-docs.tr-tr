---
title: "Öğretici: Azure BizTalk Services'ı kullanarak EDIFACT faturalarını işleme | Microsoft Docs"
description: Nasıl oluşturup kutusunda bağlayıcı veya API uygulamasını yapılandırma ve Azure App Service'te bir mantıksal uygulamada kullanma
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: ''
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 7093be36e34785d4153c257d411128efe463dee1
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918969"
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Öğretici: Azure BizTalk Services’ı kullanarak EDIFACT faturalarını işleme

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk Hizmetleri portalını yapılandırmak ve X12 ve EDIFACT sözleşmelerini dağıtmak için kullanabilirsiniz. Bu öğreticide, biz nasıl faturalar ticaret iş ortakları arasında değiş tokuşu için bir EDIFACT sözleşmesi oluşturun bakın. Bu öğreticide iki ticari ortaklar EDIFACT mesaj alışverişi Northwind ve Contoso ile ilgili bir uçtan uca iş çözüm yazılır.  

## <a name="sample-based-on-this-tutorial"></a>Bu öğretici temel örnek
Bir örnek, Bu öğretici yazıldığı sırada **EDIFACT faturalarını kullanarak BizTalk Services gönderme**, indirmesine kullanılabilen [MSDN Kod Galerisi](https://go.microsoft.com/fwlink/?LinkId=401005). Örneği kullanın ve örnek nasıl oluşturulmuş anlamak için bu öğreticiyi. Veya Bu öğretici, kendi çözüm en başından başlayarak oluşturmak için kullanabilirsiniz. Bu öğretici, bu çözümün nasıl oluşturulmuş anlamanız ikinci yaklaşım yöneliktir. Ayrıca, mümkün olduğunca öğretici örnek ile tutarlıdır ve örnek kullanılan yapılar (örneğin, şemalar, dönüşümler) için aynı adları kullanır.  

> [!NOTE]
> Bu çözüm, bir EDI köprüsüne bir EAI Köprüsü'nden ileti gönderme gerektirdiğinden, yeniden kullanır [BizTalk Hizmetleri zincirleme örneği köprüsü](https://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) örnek.  
> 
> 

## <a name="what-does-the-solution-do"></a>Çözüm ne yapar?
Bu çözümde, Northwind EDIFACT faturalarını Contoso alır. Bu fatura standart bir EDI biçimde değil. Bu nedenle, fatura için Northwind göndermeden önce bir EDIFACT fatura (INVOIC olarak da bilinir) belgesine dönüştürülmesi gerekir. Alındığında, Northwind EDIFACT fatura işleyebilir ve (kontrol olarak da bilinir) bir denetim iletisi döndürmek için Contoso gerekir.

![][1]  

Bu iş senaryosu için Microsoft Azure BizTalk Hizmetleri ile sağlanan özelliklerden Contoso kullanır.

* Contoso EAI köprüsü orijinal faturayı EDIFACT INVOIC dönüştürmek için kullanır.
* EAI köprüsü BizTalk Services Portalı'nda yapılandırılmış anlaşmanın bir parçası olarak dağıtılan bir EDI gönderme köprüsü ardından iletiyi gönderir.
* EDI gönderme köprüsü EDIFACT INVOIC işler ve Northwind için yönlendirir.
* Fatura aldıktan sonra anlaşmanın bir parçası dağıtılan köprüsü EDI Northwind döndürür bir kontrol iletisi alırsınız.  

> [!NOTE]
> İsteğe bağlı olarak, bu çözüm, ayrıca toplu işleme yerine, her bir fatura ayrı ayrı toplu işler halinde, faturaları göndermek için nasıl kullanılacağını gösterir.  
> 
> 

Bu senaryoyu tamamlamak için Service Bus kuyruklarını fatura Contoso için Northwind gönderip bildirim Northwind kullanırız. Bu kuyruklar bir indirme olarak kullanılabilir ve kullanılabilir örnek paket içerisine dâhil istemci uygulaması kullanılarak oluşturulabilir Bu öğreticinin bir parçası olarak.  

## <a name="prerequisites"></a>Önkoşullar
* Service Bus ad alanı olmalıdır. Bir ad alanı oluşturma ile ilgili yönergeler için bkz: [nasıl yapılır: Oluşturma veya değiştirme bir Service Bus hizmeti Namespace](/previous-versions/azure/azure-services/hh674478(v=azure.100)). Bize sağlanan, bir Service Bus ad alanı zaten sahip olduğunuzu varsaymaktadır adlı **edifactbts**.
* Bir BizTalk Hizmetleri aboneliğinizin olması gerekir. Bu öğreticide, bize adlı bir BizTalk Services aboneliği sahip olduğunuz varsayılır **contosowabs**.
* BizTalk Hizmetleri portalında BizTalk Hizmetleri aboneliğinizi kaydedin. Yönergeler için [BizTalk Hizmetleri portalında BizTalk hizmet dağıtımı kaydediliyor](/previous-versions/azure/hh689837(v=azure.100))
* Visual Studio yüklü olmalıdır.
* BizTalk Services SDK'sı yüklü olması gerekir. SDK'sından indirebilirsiniz [https://go.microsoft.com/fwlink/?LinkId=235057](https://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>1. Adım: Hizmet veri yolu kuyrukları oluştur
Bu çözüm, hizmet veri yolu kuyrukları ticaret iş ortakları arasında mesaj alışverişi kullanır. Contoso ve Northwind EDI ve/veya EAI köprüleri bunları burada tüketme gelen iletileri kuyruklara gönderir. Bu çözüm için üç hizmet veri yolu kuyrukları gerekir:

* **northwindreceive** – Northwind Contoso fatura bu sıraya alır.
* **contosoreceive** – Contoso Bu kuyruk Northwind bildirim alır.
* **askıya** – tüm askıya alınan iletileri bu kuyruğa yönlendirilir. İşleme sırasında başarısız olursa, iletileri askıya alınır.

Bu hizmet veri yolu kuyrukları örnek paket içerisine dâhil bir istemci uygulaması kullanarak oluşturabilirsiniz.  

1. Örnek indirdiğiniz konumdan açmak **öğretici gönderme faturalar kullanarak BizTalk Services EDI Bridges.sln**.
2. Tuşuna **F5** oluşturmak ve başlatmak için **öğretici istemci** uygulama.
3. Ekranda, hizmet veri yolu ACS ad alanı, verenin adı ve verenin anahtarı girin.
   
   ![][2]  
4. Üç kuyrukları, Service Bus ad alanında oluşturulur bir ileti kutusu ister. **Tamam** düğmesine tıklayın.
5. Öğretici çalıştıran istemci bırakın. Açık öğesini **Service Bus** > ***Service Bus ad alanınızı*** > **kuyrukları**ve üç kuyrukları oluşturulduğunu doğrulayın.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>2. Adım: Oluşturma ve ticari ortak sözleşmesi dağıtma
Contoso Northwind arasındaki ticari ortak sözleşmesi oluşturma. Ticari ortak sözleşmesi hangi ileti şeması kullanmak için hangi Mesajlaşma protokolü kullanmak için vb. gibi iki iş ortakları arasında ticari sözleşme tanımlar. Ticari ortak sözleşmesi ortaklar için ileti göndermek için iki EDI köprüleri içerir (adlı **EDI gönderme köprüsü**) ve iş ortakları alım-satım iletileri almak için (adlı **EDI alma köprüsü**).

Bu çözüm bağlamında EDI gönderme köprüsü anlaşma Gönder-tarafı için karşılık gelen ve EDIFACT fatura Contoso için Northwind göndermek için kullanılır. Benzer şekilde, EDI alma köprüsü alış tarafı için kabul ettiğiniz sözleşmenin karşılık gelen ve Northwind alma bildirimleri için kullanılır.  

### <a name="create-the-trading-partners"></a>Ticari ortaklar oluşturma
İle başlamak için Contoso ve Northwind ticaret iş ortakları oluşturun.  

1. BizTalk Hizmetleri portalında, üzerinde **iş ortakları** sekmesinde **Ekle**.
2. Yeni iş ortağı sayfaya girin **Contoso** bir iş ortağı adı ve ardından olarak **Kaydet**.
3. İkinci iş ortağı oluşturmak için yineleyin **Northwind**.  

### <a name="create-the-agreement"></a>Sözleşmesi oluşturma
Ticari ortak sözleşmeleri, ticari iş ortakları, iş profilleri arasında oluşturulur. Bu çözüm, iş ortakları oluşturduğumuz olduğunda otomatik olarak oluşturulan varsayılan iş ortağı profilleri kullanır.  

1. BizTalk Hizmetleri portalında **sözleşmeleri** > **Ekle**.
2. İçinde yeni anlaşmanın **genel ayarlar** sayfasında, aşağıdaki resimde gösterildiği gibi değerleri belirtin ve ardından **devam**.
   
   ![][3]  
   
   Tıkladıktan sonra **devam**, iki sekme **alma ayarı** ve **gönderme ayarları** kullanılabilir hale gelir.
3. Contoso Northwind arasındaki gönderme sözleşmesi oluşturun. İşbu sözleşme, Contoso EDIFACT fatura için Northwind nasıl göndereceğini yönetir.
   
   1. Tıklayın **gönderme ayarları**.
   2. Varsayılan değerleri üzerinde korumak **gelen URL**, **dönüştürme**, ve **toplu işleme** sekmeler.
   3. Üzerinde **Protokolü** sekmesindeki **şemaları** bölümünde, karşıya yükleme **EFACT_D93A_INVOIC.xsd** şema. Bu şema örnek paketi ile kullanılabilir.
      
      ![][4]  
   4. Üzerinde **aktarım** sekmesinde, Service Bus Kuyrukları için ayrıntıları belirtin. Gönderme tarafı sözleşmesi için kullandığımız **northwindreceive** Northwind için EDIFACT fatura göndermek için sıra ve **askıya** işleme sırasında başarısız ve askıya alınan iletileri yönlendirmek için kuyruk. Bu kuyruklardaki oluşturduğunuz **1. adım: Hizmet veri yolu Kuyrukları Oluşturma** (Bu konudaki).
      
      ![][5]  
      
      Altında **Aktarım Ayarları > taşıma türü** ve **ileti askıya alma ayarları > taşıma türü**, Azure Service Bus'ı seçin ve görüntüde gösterilen değerleri sağlayın.
4. Alma sözleşmesinde Contoso Northwind arasındaki oluşturun. İşbu sözleşme, nasıl Contoso Northwind alındısı yönetir.
   
   1. Tıklayın **alma ayarlarını**.
   2. Varsayılan değerleri üzerinde korumak **aktarım** ve **dönüştürme** sekmeler.
   3. Üzerinde **Protokolü** sekmesindeki **şemaları** bölümünde, karşıya yükleme **EFACT_4.1_CONTRL.xsd** şema. Bu şema örnek paketi ile kullanılabilir.
   4. Üzerinde **rota** sekmesinde, Northwind gelen yalnızca belirli bir onayları için Contoso yönlendirildiğinden emin olmak için bir filtre oluşturun. Altında **yönlendirme ayarlarını**, tıklayın **Ekle** yönlendirme filtresi oluşturmak için.
      
      ![][6]  
      
      1. İçin değerler sağlayın **kural adı**, **yol kuralı**, ve **yol hedef** görüntüde gösterildiği gibi.
      2. **Kaydet**’e tıklayın.
   5. Üzerinde **rota** yeniden sekmesinde, askıya alınmış bir onayları (işleme sırasında başarısız bildirimleri) burada yönlendirilir belirtin. Azure Service Bus için aktarım türü ayarlayın, yol, hedef türe **kuyruk**, kimlik doğrulama türü **paylaşılan erişim imzası** (SAS), Service Bus ad alanı için SAS bağlantı dizesi girin ve kuyruk adı olarak enter **askıya**.
5. Son olarak, tıklayın **Dağıt** sözleşmesi dağıtılacak. Uç noktalara dikkat edin burada gönderme ve alma sözleşmeleri dağıtılan.
   
   * Üzerinde **gönderme ayarları** sekmesindeki **gelen URL**, uç nokta unutmayın. EDI gönderme Köprüsü'nü kullanarak Northwind için Contoso ileti göndermek için bu uç noktaya bir ileti göndermeniz gerekir.
   * Üzerinde **alma ayarı** sekmesindeki **aktarım**, uç nokta unutmayın. Contoso için Northwind ileti göndermek için EDI almak kullanarak köprüsü, bu uç noktaya bir ileti göndermeniz gerekir.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>3. Adım: BizTalk Services projesi oluşturma ve dağıtma
Önceki adımda EDI Gönder dağıtılan ve anlaşmalar EDIFACT faturalarını ve onayları işlemek için alıyorsunuz. Bu sözleşmeler, standart EDIFACT iletisi şemaya uygun işlem iletileri olabilir. Ancak, senaryo için bu çözüm, Contoso fatura Northwind için bir şirket içi özel şemasında gönderir. EDI gönderme köprüsü için ileti gönderilmeden önce bu nedenle, şirket içi şemadan standart EDIFACT fatura şemaya dönüştürülmesi gerekir. BizTalk Hizmetleri EAI proje yapar.

BizTalk Services projesi **InvoiceProcessingBridge**, dönüşümler iletisi Ayrıca, indirdiğiniz örnek bir parçası olarak yer almaktadır. Proje şu yapıtları içerir:

* **INHOUSEINVOICE. XSD** – Northwind için gönderilen şirket içi faturayı şeması.
* **EFACT_D93A_INVOIC. XSD** – standart EDIFACT fatura şema.
* **EFACT_4.1_CONTRL. XSD** – Contoso için Northwind gönderir EDIFACT bildirim şeması.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – şirket içi faturayı şema standart EDIFACT fatura şemaya eşler Dönüştür.  

### <a name="create-the-biztalk-services-project"></a>BizTalk Services projesi oluşturma
1. Visual Studio çözümünde InvoiceProcessingBridge projesini genişletin ve ardından açın **MessageFlowItinerary.bcs** dosya.
2. Tuvalde herhangi bir yere tıklayın ve ayarlama **BizTalk hizmeti URL'SİNİN** özellik kutusuna, BizTalk Services abonelik adını belirtin. Örneğin, `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Araç kutusundan sürükleyin bir **Xml One-Way köprüsü** tuvaline. Ayarlama **varlık adı** ve **göreli adres** köprüsüne özelliklerini **ProcessInvoiceBridge**. Çift **ProcessInvoiceBridge** köprüsü yapılandırması surface'ı açın.
4. İçinde **ileti türlerini** kutusunda, artı işaretine tıklayın (**+**) düğmesini gelen ileti şemasını belirtin. Gelen iletinin EAI köprüsü için her zaman şirket içi faturayı olduğundan, bu ayar **INHOUSEINVOICE**.
   
   ![][8]  
5. Tıklayın **Xml dönüşümü** şekli ve özellik kutusuna için **haritalar** özelliği, üç noktaya tıklayın (**...** ) düğmesi. İçinde **haritalar seçimi** iletişim kutusunda **INHOUSEINVOICE_to_D93AINVOIC** dönüşüm dosyasında ve ardından **Tamam**.
   
   ![][9]  
6. Geri Git **MessageFlowItinerary.bcs**ve araç kutusundan sürükleyin bir **iki yönlü dış hizmet uç noktası** sağındaki **ProcessInvoiceBridge**. Ayarlama, **varlık adı** özelliğini **EDIBridge**.
7. Çözüm Gezgini'nde **MessageFlowItinerary.bcs** ve çift **EDIBridge.config** dosya. İçeriğinin yerine geçecek **EDIBridge.config** aşağıdaki.
   
   > [!NOTE]
   > .Config dosyasını düzenlemek neden ihtiyacım var? Köprü Tasarımcı tuvaline ekledik dış hizmet uç noktası, daha önce dağıttığınız EDI köprüleri temsil eder. EDI köprüsü olan gönderme ile iki yönlü Köprüsü ve alma tarafı. Ancak, köprü tasarımcıya ekledik EAI köprüsü, tek yönlü bir köprü olur. Bu nedenle, iki köprüleri farklı ileti exchange örneklerini işlemek için bir özel köprüsü davranışı yapılandırmasına .config dosyasında dahil ederek kullanırız. Ayrıca, özel durum, ayrıca EDI gönderme köprüsü uç kimlik doğrulaması gerçekleştirir. Bu özel davranış olarak ayrı bir örneğini şurada kullanılabilir [BizTalk Hizmetleri zincirleme örneği - EDI için EAI köprüsü](https://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Bu çözüm, örnek kullanır.  
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
8. Yapılandırma ayrıntılarını içermesi için EDIBridge.config dosyasını güncelleştirin
   
   * Altında *<behaviors>*, ACS ad alanı ve BizTalk Hizmetleri aboneliğinizle ilişkili bir anahtar sağlar.
   * Altında *<client>*, EDI Gönderme Sözleşmesi dağıtıldığı uç noktası sağlar.
   
   Değişiklikleri kaydedin ve yapılandırma dosyasını kapatın.
9. Araç kutusundan tıklayın **bağlayıcı** ve katılma **ProcessInvoiceBridge** ve **EDIBridge** bileşenleri. Bağlayıcıyı seçin ve Özellikler kutusunda ayarlanan **filtre koşulu** için **eşleşen tüm**. Bu, EAI köprüsü tarafından işlenen tüm iletileri EDI köprüsüne yönlendirilmesini sağlar.
   
   ![][10]  
10. Çözüm için değişiklikleri kaydedin.  

### <a name="deploy-the-project"></a>Projeyi dağıtın
1. BizTalk Services projesi oluşturulduğu bilgisayara indirip BizTalk Hizmetleri aboneliğiniz için SSL sertifikası yükleyin. BizTalk Services'ın altında tıklatın **Pano**ve ardından **SSL sertifikası indir**. Sertifikaya çift tıklayın ve yüklemeyi tamamlamak için istemi izleyin. Altına sertifikayı yüklediğinizden emin olun **güvenilen kök sertifika yetkilileri** sertifika deposu.
2. Visual Studio Çözüm Gezgini'nde sağ **InvoiceProcessingBridge** proje ve ardından **Dağıt**.
3. Görüntüde gösterilen değerleri sağlayın ve ardından **Dağıt**. Tıklayarak, BizTalk Services için ACS kimlik bilgilerini alabilirsiniz **bağlantı bilgilerini** BizTalk Hizmetleri panosundan.
   
   ![][11]  
   
   EAI köprüsü dağıtıldığı, örneğin, uç nokta çıkış bölmesinde Kopyala `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Daha sonra Bu uç nokta URL'si gerekir.  

## <a name="step-4-test-the-solution"></a>4. Adım: Çözüm test
Bu konu başlığında, bir çözüm kullanarak test etme atacağız **öğretici istemci** örnek bir parçası olarak sağlanan uygulama.  

1. Visual Studio'da başlatmak için F5 tuşuna basarak **öğretici istemci**.
2. Ekran, Service Bus kuyruklarını burada oluşturduk. adımdan önceden doldurulmuş değerlere sahip olmalıdır. **İleri**’ye tıklayın.
3. BizTalk Services aboneliği için ACS kimlik bilgilerini ve uç noktaları sonraki penceresinde sağlamak burada EAI ve EDI (alma) köprüleri dağıtılır.
   
   EAI köprüsü uç noktası önceki adımda kopyaladığınız. EDI için BizTalk Hizmetleri portalında köprüsü uç noktası, alma, anlaşmayı gidin > alma ayarları > aktarım > bitiş noktası.
   
   ![][12]  
4. Contoso altında bir sonraki pencereye tıklayın **şirket içi faturayı gönderin** düğmesi. Dosya iletişim kutusunu açın, INHOUSEINVOICE.txt dosyasını açın. Dosyanın içeriği inceleyin ve ardından **Tamam** fatura göndermek için.
   
   ![][13]  
5. Birkaç saniye içinde fatura Northwind alınır. Tıklayın **iletiyi görüntüle** Northwind tarafından alınan fatura görmek için bağlantı. Contoso tarafından gönderilen bir şirket içi bir şema ederken Northwind tarafından alınan fatura standart EDIFACT şemasında nasıl olduğuna dikkat edin.
   
   ![][14]  
6. Fatura seçin ve ardından **bildirim gönderme**. Açılan iletişim kutusunda, değişim Kimliğine alınan fatura ve gönderilen bildirim aynı olduğuna dikkat edin. Tamam'ı **bildirim gönderme** iletişim kutusu.
   
   ![][15]  
7. Birkaç saniye içinde bildirim başarıyla Contoso'da alındı.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>(İsteğe bağlı) 5. adım: Toplu olarak EDIFACT faturayı gönderin
BizTalk Hizmetleri EDI köprüleri de destekler giden iletiler toplu işleme. Bu özellik, toplu iletiler (belirli bir ölçütü karşılayan) yerine tek bir ileti almak için tercih ettiğiniz iş ortakları almak için kullanışlıdır.

Toplu işlemler ile çalışırken en önemli boyut, toplu işlem yayın ölçütü olarak da bilinir, gerçek sürümüdür. Yayın ölçütü nasıl alıcı iş ortağına iletileri almak istediği üzerinde temel alabilir. Toplu işleme etkinse, yayın ölçütü verildikten kadar EDI köprüsü giden ileti alıcı iş ortağına göndermez. Örneğin, bir toplu işleme ölçütlerini ileti boyutu dağıtımları üzerinde bir batch temel yalnızca 'n' iletileri toplu. Bir batch sabit bir zaman her gün gönderilir, batch ölçütleri zamana bağlı da olabilir. Bu çözümde, biz ileti boyutu tabanlı ölçütü deneyin.

1. BizTalk Hizmetleri portalında, daha önce oluşturduğunuz sözleşmesi tıklayın. Gönderme Ayarları > Toplu işleme > Toplu ekleyin.
2. Batch adı olarak **InvoiceBatch**bir açıklama belirtin ve ardından **sonraki**.
3. İletileri toplu olarak tanımlayan bir batch ölçütlerini belirtin. Bu çözümde, biz tüm iletileri toplu. Bu nedenle, Gelişmiş tanımları seçeneği kullan'ı seçin ve girin **1 = 1**. Bu, her zaman true olur bir durumdur ve tüm iletileri bu nedenle toplu olarak. **İleri**’ye tıklayın.
   
   ![][17]  
4. Bir toplu sürüm ölçütlerini belirtin. Açılan kutusundan seçin **MessageCountBased**ve **sayısı**, belirtin **3**. Bu, toplu üç iletiler için Northwind gönderileceği anlamına gelir. **İleri**’ye tıklayın.
   
   ![][18]  
5. Özeti gözden geçirin ve ardından **Kaydet**. Tıklayın **Dağıt** sözleşmesi yeniden dağıtmak için.
6. Geri Git **öğretici istemci**, tıklayın **şirket içi faturayı gönderin**ve fatura göndermek için yönergeleri izleyin. Toplu iş boyutu karşılanmadı çünkü hiçbir fatura Northwind aldığını göreceksiniz. İçin Northwind gönderilen üç fatura iletileri sahip iki kez daha, bu adımı yineleyin. Bu, 3 mesaj toplu sürüm ölçütlerini karşılayan ve northwind'de faturayı görmelisiniz.

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

