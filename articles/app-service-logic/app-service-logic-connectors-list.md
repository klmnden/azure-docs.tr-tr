<properties
    pageTitle="Kullanılabilir Bağlayıcılar ve API Apps listesi | Microsoft Azure App Service"
    description="Azure App Service’deki Bağlayıcılar ve API Apps hakkında bilgi alın"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# Logic Apps içinde kullanılacak Bağlayıcılar ve API Apps listesi
>[AZURE.NOTE] Makalenin bu sürümü Logic Apps 2014-12-01-önizleme şema sürümü için geçerlidir. Logic Apps Genel Kullanılabilirlik (GA) sürümü için bkz. [Yeni Bağlayıcılar Listesi](../connectors/apis-list.md).

Logic Apps’inizde kullanılmak üzere Microsoft tarafından oluşturulan tüm kullanılabilir bağlayıcılar ve API Apps hakkında bilgi edinin.

Fiyatlandırma bilgileri ve her bir Hizmet Katmanına dahil olanların listesi için bkz. [Azure App Service Fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Bir Azure hesabı için kaydolmadan önce Logic Apps’i kullanmak üzere [Logic Apps’i Deneyin](https://tryappservice.azure.com/?appservice=logic)’e gidin. App Service’de başlangıç düzeyinde kısa süreli mantıksal uygulamayı hemen oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.

## Çekirdek Bağlayıcılar
Aşağıdaki tabloda Microsoft tarafından oluşturulan ve Çekirdek Bağlayıcılar olarak kullanılabilen tüm mevcut bağlayıcılar ve API Apps listelenmiştir:

Ad | Açıklama
--- | ---
[Azure Service Bus](app-service-logic-connector-azureservicebus.md) | Service Bus Kuyrukları ve Konularından iletiler gönderebilir ve Service Bus Kuyrukları ve Aboneliklerinden iletiler alabilir.
[Bing Çevirmen](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Metni başka bir dile çevirmek için Bing kullanın.
[HTTP](app-service-logic-connector-http.md) | HTTP Dinleyicisi, HTTP sunucusu gibi davranan ve gelen HTTP ya da HTTPS isteklerini dinleyen bir uç nokta açar. HTTP eylemi bir API Uygulaması gerektirmez ve Logic Apps içinde yerel olarak desteklenir.
[Microsoft Office 365](app-service-logic-connector-office365.md) | Office 365 Bağlayıcısı, Office 365 hesabınızı kullanarak e-posta gönderebilir ve alabilir, takviminizi yönetebilir ve kişilerinizi yönetebilir.
[Slack](app-service-logic-connector-slack.md) | Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın.


## Enterprise Integration Bağlayıcıları
Aşağıdaki tabloda Microsoft tarafından oluşturulan ve Enterprise Integration Bağlayıcıları olarak kullanılabilen tüm mevcut Bağlayıcılar ve API Uygulamaları listelenmiştir:

Ad  | Açıklama
------------- | -------------
[AS2 Bağlayıcısı](app-service-logic-connector-as2.md) | AS2 aktarım protokolünü kullanarak ileti alıp gönderebilir. Veriler dijital sertifikalar ve şifreleme kullanılarak güvenli ve güvenilir bir şekilde taşınır.
[BizTalk EDIFACT](app-service-logic-connector-edifact.md) | İşletmeler arası iletişimlerde EDIFACT protokolünü kullanarak iletiler alıp gönderir.
[BizTalk Düz Dosya Kodlayıcısı](app-service-logic-flatfile-encoder.md) | Düz dosya verileri (excel ve csv gibi) ile XML verileri arasında birlikte çalışabilirlik sağlar. Bu API Uygulaması bir düz dosya örneğini XML’ye ve XML’yi düz dosya örneğine dönüştürebilir.
[BizTalk JSON Kodlayıcısı](app-service-logic-connector-jsonencoder.md) | Uygulamanızın JSON ile XML verileri arasında birlikte çalışmasına yardımcı olan kodlayıcı ve kod çözücü. Belirli bir JSON örneğini XML’e, XML’i JSON örneğine dönüştürebilir.
[BizTalk Kuralları](app-service-logic-use-biztalk-rules.md) | Bir kuruluştaki iş mantığını tanımlamak ve denetlemek için BizTalk Kurallarını kullanın. İş ilkeleri, ilişkili uygulamalar yeniden derlenmeden veya yeniden dağıtılmadan güncelleştirilebilir.
[BizTalk Ticari Ortak Yönetimi](app-service-logic-connector-tpm.md) | İş ortakları, sözleşmeler ve sözleşmelerde kullanılan şemalar ile sertifikaları kullanarak işletmeler arası ilişkileri tanımlar ve sürdürür. Bu ilişkiler AS2, EDIFACT ve X12 API Apps kullanılarak uygulanır.
[BizTalk Transform Service](app-service-logic-transform-xml-documents.md) | Verileri bir biçimden başka bir biçime dönüştürür. Ayrıca mevcut bir haritayı (.trfm dosyası) karşıya yükleyebilir, kaynak ve hedef şemalar arasındaki bağlantıları görüntüleyebilir ve örnek girdi XML içeriği ile ‘Test’ işlevini kullanabilirsiniz. Dize işlemeleri ve koşullu atama gibi farklı yerleşik işlevler de mevcuttur.
[BizTalk X12](app-service-logic-connector-x12.md) | İşletmeler arası iletişimlerde X12 protokolünü kullanarak iletiler alıp gönderir.
[BizTalk XML Doğrulayıcı](app-service-logic-xml-validator.md) | XML verilerini önceden tanımlanmış XML şemalarına göre doğrular. Mevcut şemaları kullanabilir ya da bir düz dosya örneği, JSON örneği veya mevcut bağlayıcıları temel alarak şemalar oluşturabilirsiniz.
[BizTalk XPath Ayıklayıcısı](app-service-logic-xpath-extract.md) | Seçtiğiniz bir XPath’i temel alarak verileri arar ve XML içeriğinden ayıklar.
[DB2 Bağlayıcısı](app-service-logic-connector-db2.md) | Şirket içindeki bir IBM DB2 veritabanına ve Windows işletim sistemi çalıştıran bir Azure sanal makinesine bağlanır. Web API ve OData API işlemlerini Informix Yapılandırılmış Sorgu Dili komutları ile eşleyebilir. <br/><br/>Tetikleyici yoktur. Tablo seçme, ekleme, güncelleştirme, silme ve özel deyim eylemleri mevcuttur<br/><br/>Bu bağlayıcı ayrıca bir TCP/IP ağı üzerinden bir Informix sunucusuna bağlanmak üzere DRDA için Microsoft Client içerir.
[Dosya](app-service-logic-connector-file.md) | Bu bağlayıcıyı kullanarak, şirket içi dosya sistemine veya ağına bağlanabilir ve dosyaları karşıya yükleme, silme, listeleme gibi farklı dosya görevlerini tamamlayabilirsiniz.
[FTP<br/>FTPS](app-service-logic-connector-ftp.md) | FTP / FTPS sunucusuna bağlanır ve dosyaları yükleme, alma, silme daha fazlası dahil farklı FTP görevlerini yapar.
[Informix](app-service-logic-connector-informix.md) | Şirket içindeki bir IBM Informix veritabanına ve Windows işletim sistemi çalıştıran bir Azure sanal makinesine bağlanır. Web API ve OData API işlemlerini Informix Yapılandırılmış Sorgu Dili komutları ile eşleyebilir.<br/><br/>Tetikleyici yoktur. Tablo seçme, ekleme, güncelleştirme, silme ve özel deyim eylemleri mevcuttur.<br/><br/>Şirket içinde kullanılırken VPN veya Azure ExpressRoute kullanılabilir. Bu bağlayıcı ayrıca bir TCP/IP ağı üzerinden bir Informix sunucusuna bağlanmak üzere DRDA için Microsoft Client içerir.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Şirket içi SQL Server veya bir Azure SQL veritabanına bağlanır. SQL veritabanı tablosunda girdiler oluşturabilir, güncelleştirebilir, alabilir ve silebilirsiniz.
MQ | Şirket içinde ve Windows işletim sistemi çalıştıran bir Azure sanal makinesinde IBM WebSphere MQ Server sürüm 8’e bağlanır. Şirket içinde kullanılırken VPN veya Azure ExpressRoute kullanılabilir. Bağlayıcı ayrıca MQ için Microsoft Client içerir.<br/><br/>Tetikleyici yoktur. Eylem yoktur.<br/><br/>**Not** Şu anda Logic Apps ile kullanılamamaktadır.

## Tetikleyici Olarak Bağlayıcılar
Logic Apps için tetikleyiciler birkaç bağlayıcı tarafından sağlanır. Bu tetikleyicilerin iki türü vardır:

1. Yoklama Tetikleyicileri: Bu tetikleyiciler yeni verileri denetlemek için hizmetinizi belirtilen aralıkta yoklar. Yeni veriler kullanılabilir olduğunda, uygulamanızın yeni bir örneği girdi olarak verilerle çalışır. Aynı verinin birden çok kez kullanılmasını önlemek için, tetikleyici okunan ve Mantıksal Uygulama’ya geçirilen verileri temizleyebilir. Dosya, SQL ve Azure Storage bu tür bağlayıcıların örnekleridir.
2. Anında İletme Tetikleyicileri: Bu tetikleyiciler uç noktada bir olayın meydana gelmesine ilişkin verileri dinler. Ardından, yeni bir Mantıksal Uygulama örneğini tetikler. HTTP Dinleyicisi ve Twitter bu tür bağlayıcıların örnekleridir.

## Eylem Olarak Bağlayıcılar
Bağlayıcılar Mantıksal Uygulama içinde eylem olarak da kullanılabilir. Eylemler, daha sonra yürütmede kullanılabilecek Mantıksal Uygulama verilerini ararken kullanışlıdır. Örneğin, bir siparişi işlerken bir müşteri hakkında daha fazla bilgi almak üzere SQL veritabanındaki verileri aramanız gerekebilir. Veya, bir hedefteki verileri yazmanız, güncelleştirmeniz ya da silmeniz gerekebilir. Bağlayıcılar tarafından sağlanan eylemleri kullanarak bunu yapabilirsiniz. Eylemler API Apps’teki işlemlerle eşlenir (Swagger meta verileri ile tanımlanan şekilde).

## Kendi Bağlayıcılarınızı ve API Uygulamalarınızı oluşturma
[Bağlayıcılar ve API Apps Referansı](http://aka.ms/appservicesconnectorreference)  
[Azure App Service API uygulaması tetikleyicileri](../app-service-api/app-service-api-dotnet-triggers.md)  
[Mantıksal Uygulama Referansı](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## Bağlayıcılar ve API Apps hakkında daha fazla bilgi
[Bağlayıcılar ve BizTalk API Apps nedir?](app-service-logic-what-are-biztalk-api-apps.md)  
[Azure App Service içinde Karma Bağlantı Yöneticisi](app-service-logic-hybrid-connection-manager.md)  
[Yerleşik API Uygulamalarınızı ve Bağlayıcılarınızı Yönetme ve İzleme](app-service-logic-monitor-your-connectors.md)



<!--HONumber=sep16_HO1-->


