---
title: "Azure Logic Apps için Bağlayıcılar | Microsoft Docs"
description: "Mantıksal uygulamalar derlemek ve oluşturmak için Microsoft tarafından yönetilen tüm kullanılabilir bağlayıcılar arasından seçim yapın"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: c14ac7592efabfec8668d7437463e2d8771ee072
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connectors-list"></a>Bağlayıcıların listesi
> [!TIP]
> [A’dan Z’ye tam liste](#az) (bu konuda), Logic Apps ile kullanabileceğiniz tüm bağlayıcıları listeler. [Bağlayıcı ayrıntıları](/connectors/)'nda Swagger'da tanımlanan tetikleyiciler ve eylemlerin yanı sıra her bir bağlayıcıya yönelik sınırlar listelenir.

Bağlayıcılar, mantıksal uygulama oluşturma işleminin ayrılmaz bir parçasıdır. Bu bağlayıcıları kullanarak, oluşturduğunuz veriler ve zaten sahip olduğunuz veriler ile farklı işlemler yapmak üzere şirket içi ve bulut uygulamalarınızı gerçek anlamda genişletebilirsiniz. Bağlayıcılar aşağıdaki kategorilerde kullanılabilir: 

* **Standart bağlayıcılar**: Mantıksal uygulamaları kullandığınızda otomatik olarak kullanılabilir durumdadır ve eklenir. Service Bus, Power BI, Oracle Database, OneDrive ve daha birçok örnek mevcuttur.

* **Tümleştirme hesabı bağlayıcıları**: Bir tümleştirme hesabı satın aldığınızda kullanılabilir. Bu bağlayıcıları kullanarak, XML dönüştürüp doğrulayabilir, AS2 / X12 / EDIFACT ile işletmeden işletmeye iletileri işleyebilir ve düz dosyaları kodlayıp kod çözebilirsiniz. BizTalk Server ile çalışıyorsanız, bu bağlayıcılar BizTalk iş akışlarınızı Azure'a genişletmek için uygundur.  

    BizTalk Server ayrıca bir mantıksal uygulamadan alma ve mantıksal uygulama gönderme işlemini içeren bir [Logic Apps bağdaştırıcısına](https://msdn.microsoft.com/library/mt787163.aspx) sahiptir.

* **Enterprise bağlayıcıları**: MQ ve SAP içerir. Ek bir maliyet karşılığında sunulur. 

[Logic Apps Fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/) ve [Fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md) bölümlerinde maliyetlerle ilgili daha fazla bilgi verilmektedir. 

## <a name="popular-connectors"></a>Popüler bağlayıcılar
Bu bağlayıcıları kullanarak veri ve bilgileri başarılı bir şekilde işleyen binlerce uygulama ve milyonlarca yürütme vardır. Aşağıdaki tabloda en popüler olanları ve kullanıcılarımızın en sık kullandığı bazı örnekler listelenmektedir:

| |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][AzureBlobStorageicon]<br/>**Azure Blob<br/>Depolama**][AzureBlobStoragedoc] | Depolama hesabınızda herhangi bir görevi otomatik hale getirmek isterseniz bu bağlayıcıya bakmanız gerekir. CRUD (create, read, update, delete) işlemlerini destekler. | [![API Simgesi][Azure-Functionsicon]<br/>**Azure İşlevleri**][azure-functionsdoc] | Özel C# veya node.js kod parçacıkları çalıştıran işlevler oluşturun ve sonra bu işlevleri mantıksal uygulamalarınızda kullanın.  |
| [![API Simgesi][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | En çok talep gören bağlayıcılardan biridir. Müşteri adayları ile iş akışlarını otomatikleştirmeye ve daha birçok işleme yardımcı olan tetikleyici ve eylemler içerir. | [![API Simgesi][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Bir Olay Hub’ındaki olayları tüketin ve yayımlayın. Örneğin, Event Hubs'ı kullanarak mantıksal uygulamanızdan çıkış alabilir ve ardından söz konusu çıkışı gerçek zamanlı bir analiz sağlayıcısına gönderebilirsiniz. |
| [![API Simgesi][FTPicon]<br/>**FTP**][FTPdoc] | FTP sunucunuza İnternet'ten erişilebiliyorsa, iş akışlarını dosya ve klasörlerle çalışacak şekilde otomatikleştirebilirsiniz. <br/><br/>SFTP ayrıca SFTP bağlayıcısı ile kullanılabilir. | [![API Simgesi][HTTPicon]<br/>**HTTP**][httpdoc] | HTTP üzerinden herhangi bir uç nokta ile iletişim kurmak için mantıksal uygulamaları kullanın. |
| [![API Simgesi][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | İş akışlarınızda Office 365 e-posta ve olaylarını kullanmaya yönelik çok sayıda tetikleyici ve çok daha fazla sayıda eylem. <br/><br/>Bu bağlayıcı, tatil isteklerini, gider raporlarını, vb. onaylamaya yönelik bir *onay e-postası* eylemi içerir. <br/><br/>Office 365 kullanıcıları ayrıca Office 365 Kullanıcıları bağlayıcısı ile kullanılabilir.| [![API Simgesi][HTTP-Requesticon]<br/>**İstek/Yanıt**][HTTP-Requestdoc] | Bu bağlayıcı bir HTTPS URL'si sağlar. Mantıksal uygulama bu URL’ye yönelik bir istek aldığında mantıksal uygulama başlatılır. |
| [![API Simgesi][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Müşteri Adayları gibi nesnelere ve daha fazlasına erişmek için Salesforce hesabınızla kolayca oturum açın. |  [![API Simgesi][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Mantıksal uygulamalardaki en popüler bağlayıcı olmasının yanı sıra, zaman uyumsuz mesajlaşma ve kuyruklar, abonelikler ve konular ile yayımlama/abone olma işlemlerine yönelik tetikleyiciler ve eylemler içerir. |
|  [![API Simgesi][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | SharePoint ile bir işlem yapıyor ve otomasyondan yararlanabiliyorsanız, bu bağlayıcıya bakmanız önerilir. Şirket içi SharePoint ve SharePoint Online ile birlikte kullanılabilir. | [![API Simgesi][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | En çok kullanılan bağlayıcılardan biri olmasının yanı sıra, şirket içi SQL Server ve bir Azure SQL Veritabanına bağlanabilir. | 
| [![API Simgesi][Twittericon]<br/>**Twitter**][Twitterdoc] | Bir Twitter hesabıyla kolayca oturum açın ve yeni tweet gönderildiğinde bir iş akışı başlatın. Daha sonra bu tweetleri bir SQL veritabanı veya SharePoint listesine kaydedin. | | | 

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları 

Enterprise Integration Pack (EIP), BizTalk Server topluluğu tarafından iyi bilinen bağlayıcılar içerir. Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) satın aldığınızda aşağıdaki bağlayıcıları da elde edersiniz: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API Simgesi][as2icon]<br/>**AS2</br> kodunu çözme**][as2decode] | [![API Simgesi][as2icon]<br/>**AS2</br> kodlama**][as2encode] | [![API Simgesi][x12icon]<br/>**EDIFACT</br> kodunu çözme**][EDIFACTdecode] | [![API Simgesi][x12icon]<br/>**EDIFACT</br> kodlama**][EDIFACTencode] |
[![API Simgesi][flatfileicon]<br/>**Düz dosya</br> kodlama**][flatfiledoc] | [![API Simgesi][flatfiledecodeicon]<br/>**Düz dosya</br> kodunu çözme**][flatfiledecodedoc] | [![API Simgesi][integrationaccounticon]<br/>**Tümleştirme<br/>hesabı**][integrationaccountdoc] | [![API Simgesi][xmltransformicon]<br/>**Dönüştürme<br/>XML**][xmltransformdoc] |
| [![API Simgesi][x12icon]<br/>**X12</br> kodunu çözme**][x12decode] | [![API Simgesi][x12icon]<br/>**X12</br> kodlama**][x12encode] | [![API Simgesi][xmlvalidateicon]<br/>**XML <br/>doğrulama**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Mantıksal uygulamalarınızın içinden kurumsal uygulamalarınıza bağlanın.

|  |  |
| --- | --- |
|[![API Simgesi][MQicon]<br/>**MQ**][mqdoc]|[![API Simgesi][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>A’dan Z’ye tam liste

[Bağlayıcı ayrıntıları](/connectors/)'nda Swagger'da tanımlanan tetikleyiciler ve eylemlerin yanı sıra her bir bağlayıcıya yönelik sınırlar listelenir.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>10-8 Randevu Planlama<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Azure API Management<br/>Azure Uygulama Hizmetleri<br/>Azure Uygulaması<br/>Azure Otomasyonu<br/>[Azure Blob Depolama][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure İşlevleri][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Azure Kuyrukları<br/>Azure Resource Manager<br/>[Azure SQL Veritabanı][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Kıyaslama E-postası<br/>Bing Arama<br/>Bitbucket<br/>Bitly<br/>BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito Forms<br/>Bilişsel Hizmetler Görüntü İşleme API’si<br/>Bilişsel Hizmetler Yüz Tanıma API’si<br/>Bilişsel Hizmetler LUIS<br/>Bilişsel Hizmetler Metin Analizi<br/>Common Data Service<br/>İçerik Dönüştürme<br/>Denetleme-Sonlandırma<br/>[Özel API’ler / web uygulamaları][api/web-appdoc]<br/><br/><a name="d"></a>Veri İşlemleri<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[Dosya Sistemi][filesystemdoc]<br/>[Düz Dosya][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Google Takvim<br/>Google Kişiler<br/>Google Drive<br/>Google E-Tablolar<br/>Google Görevler<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP Web Kancası][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Tümleştirme Hesabı<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Orta<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN Hava Durumu<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Office 365 Kullanıcıları<br/>Office 365 Video<br/>OneDrive<br/>OneDrive İş<br/>OneNote (İş)<br/>[Oracle Veritabanı][oracle-db-doc]<br/>Outlook Customer Manager<br/>Outlook Görevleri<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[İstek / Yanıt][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[SAP Uygulama Sunucusu][sapconnector]<br/>[SAP İleti Sunucusu][sapconnector]<br/>[Zamanlama][recurrencedoc]<br/>Kapsam<br/>SendGrid<br/>Batch'e ileti gönderme<br/>[Service Bus][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[XML Dönüştürme][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Değişkenler<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[XML Doğrulaması][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> Azure hesabına kaydolmadan Azure Logic Apps’i kullanmaya başlamak için [Logic Apps’i Deneyin](https://tryappservice.azure.com/?appservice=logic) sayfasına gidin. Başlangıç düzeyinde kısa süreli mantıksal uygulamayı hemen oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.

## <a name="connectors-as-triggers-and-actions"></a>Tetikleyici ve eylem olarak bağlayıcılar

**Tetikleyici**, mantıksal uygulamanızın bir örneğini başlatır veya çalıştırır. Bazı bağlayıcılar, belirli olaylar meydana geldiğinde uygulamanızı bilgilendiren tetikleyiciler sağlar. Örneğin, FTP bağlayıcısı bir dosya güncelleştirildiğinde mantıksal uygulamanızı başlatan `OnUpdatedFile` tetikleyicisine sahiptir. 

Mantıksal uygulamalar aşağıdaki tetikleyici türlerini içerir:  

* *Yoklama tetikleyicileri*: Bu tetikleyiciler yeni verileri denetlemek için hizmetinizi belirtilen aralıkta yoklar. 

    Yeni veriler kullanılabilir olduğunda, mantıksal uygulamanızın yeni bir örneği girdi olarak verilerle çalışır. 

* *Anında iletme tetikleyicileri*: Bu tetikleyiciler, uç noktada bir olayın meydana gelmesine ilişkin verileri dinler ve ardından mantıksal uygulamanızın yeni bir örneğini tetikler.

* *Yineleme tetikleyicisi*: Bu tetikleyici, önceden belirlenmiş bir zamanlamaya göre mantıksal uygulamanızın bir örneğini başlatır.

Bağlayıcılar ayrıca iş akışınızda kullanabileceğiniz **eylemler** sağlar. Örneğin, mantıksal uygulamanız verileri arayabilir ve bu verileri daha sonra mantıksal uygulamanızda kullanabilir. Daha ayrıntılı olarak, bir SQL veritabanından müşteri verilerini arayabilir ve sonra iş akışınızı oluşturmak için bu müşteri verilerinizi kullanabilirsiniz. 

> [!TIP]
> [Bağlayıcılara genel bakış](connectors-overview.md) bölümünde tetikleyici ve eylemler hakkında daha fazla bilgi verilmektedir. 


## <a name="message-manipulation-actions"></a>İleti işleme eylemleri

Mantıksal uygulamalar, yük verilerinizi değiştirebilen veya işleyebilen yerleşik eylemler içerir. Yerleşik **Veri İşlemleri** bağlayıcısı aşağıdaki eylemleri içerir: 

| | |
|---|---|
| **Oluştur** | Daha sonra veya iş akışınızı derlerken kullanabileceğiniz değerler ya da nesneler derleyin veya oluşturun. Örneğin, birden çok adımın değerleriyle bir JSON nesnesi yazabilir veya daha sonra bir mantıksal uygulama çalıştırmasında başvurmak üzere sabit değer hesaplayabilirsiniz. |
| **CSV tablosu oluşturma**<br/>**HTML tablosu oluşturma** | Dizi sonuç kümesini bir CSV veya HTML tablosuna dönüştürün. Örneğin, CRM "Liste kayıtları" eylemini ekleyin ve bugün eklenen kayıtlar için bir filtre ekleyin. Sonra sonuçları bir e-posta ile HTML tablosu halinde gönderin. |
| **Filtre dizisi** (sorgu) | Bir sonuç kümesini, sizi ilgilendiren girişlerle filtreleyin. Örneğin, `#Azure` içeren tüm tweetleri arayın ve sonra yalnızca `Tweeted_by_followers > 50` olan sonuçları döndürmek için döndürülen tweetleri “filtreleyin”. |
| **Birleştir** | Bir diziyi bazı sınırlayıcılarla birleştirin. Örneğin, Anahtar Tümcecikleri Algıla işlemi, bir anahtar tümcecik dizisi döndürür. Bu tümcecikleri bir `,` veya benzer bir şey ile “birleştirebilirsiniz”. Bu nedenle, `["Some", "Phrase"]` yerine `"Some, Phrase"` elde edersiniz. |
| **JSON Ayrıştırma** | Tasarımcıda bir JSON nesnesinden değerleri ayrıştırabilir ve değerlere erişebilirsiniz. Örneğin, Azure İşleviniz bir JSON yükü döndürürse, bu yükü başka bir adımda JSON özelliklerine erişmek için ayrıştırabilirsiniz. Eylem ayrıca JSON yükünün çalışma zamanında belirtilen şema ile eşleştiğini doğrular. | 
| **Seç** | Daha fazla işleme için bir dizinin belirli özelliklerini seçin. SQL'de "Kayıtları listeleme" seçeneğini belirlerseniz ve 15 sütun döndürülürse daha fazla işleme için bu sütunların yalnızca birkaçını seçin. Çıkış, yalnızca seçtiğiniz özellikleri içeren bir dizidir. |

## <a name="custom-connectors-and-azure-certification"></a>Özel bağlayıcılar ve Azure sertifikaları 

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API’leri çağırmak için, özel bağlayıcı olarak REST tabanlı API Apps oluşturarak [Logic Apps platformunu genişletebilirsiniz](../logic-apps/logic-apps-create-api-app.md). 

Özel API Apps’i genel kullanıma sunmak ve Azure’da kullanılabilir hale getirmek isterseniz, adaylarınızı [Microsoft Azure Sertifika programına](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/) gönderin.

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını görmek için [Azure Logic Apps forumuna](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) gidin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](http://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

Bağlayıcılarla ilgili olarak değinmediğimiz bir konu başlığı veya önemli olduğunu düşündüğünüz herhangi bir ayrıntı var mı? Yanıtınız evet ise mevcut konu başlıklarımıza ekleme yaparak veya kendi konu başlığınızı oluşturarak bize yardımcı olabilirsiniz. Belgelerimiz açık kaynak olup GitHub'da barındırılır. Başlamak için [GitHub depomuza](https://github.com/Microsoft/azure-docs) gidin. 

## <a name="next-steps"></a>Sonraki adımlar
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/logic-apps-create-a-logic-app.md)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)
* [Mantıksal uygulamalarınızı izleyin](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure İşlevleri ile tümleştirin"
[db2doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2’ye bağlanın. Bir satırı güncelleştirin, bir tabloyu alın ve diğer işlemleri yapın"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "CRM Online verileriyle çalışabilmek için Dynamics CRM Online’a bağlanın"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Azure Event Hubs’a bağlanma. Logic Apps ile Event Hubs arasında olay gönderip alma"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftpdoc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[httpdoc]: ./connectors-native-http.md "HTTP bağlayıcısı ile HTTP aramaları yapın"
[http-requestdoc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "HTTP + Swagger bağlayıcısı ile HTTP çağrıları yapın"
[informixdoc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix’e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Office 365 hesabınıza bağlanın. E-posta gönderip alın, takvim ve kişilerinizi yönetin ve daha fazlasını yapın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Bir Oracle veritabanına bağlanarak satır ekleme, silme ve daha fazlası"
[mqdoc]: ./connectors-create-api-mq.md "Şirket içi MQ veya Azure bağlantısı gerçekleştirin ve ileti gönderip alın"
[recurrencedoc]:  ./connectors-native-recurrence.md "Mantıksal uygulamalar için yinelenen eylemleri tetikleyin"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetin"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[service-busdoc]: ./connectors-create-api-servicebus.md "Service Bus Kuyruklarından ve Konu Başlıklarından iletiler gönderin ve Service Bus Kuyrukları ile Aboneliklerinden iletiler alın"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "SharePoint Online’a bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "SharePoint Şirket içi sunucusuna bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Azure SQL Veritabanı veya SQL sunucusuna bağlanın. SQL veritabanı tablosunda girdiler oluşturun, güncelleştirin, alın ve silin."
[twitterdoc]: ./connectors-create-api-twitter.md "Twitter’a bağlanın. Zaman çizelgelerini alın, tweetler gönderin ve daha fazlasını yapın"
[webhookdoc]: ./connectors-native-webhook.md "Mantıksal uygulamalarınıza Web kancası eylemleri ve tetikleyiciler ekleyin"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Enterprise integration AS2 hakkında bilgi edinin."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Enterprise integration X12 hakkında bilgi edinin."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Enterprise integration XML doğrulaması hakkında bilgi edinin."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Enterprise integration dönüştürmeleri hakkında bilgi edinin."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Enterprise integration AS2 kod çözme hakkında bilgi edinin"
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Enterprise integration AS2 kodlama hakkında bilgi edinin"
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Enterprise integration X12 kod çözme hakkında bilgi edinin"
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Enterprise integration X12 kodlama hakkında bilgi edinin"
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Enterprise integration EDIFACT kod çözme hakkında bilgi edinin"
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Enterprise integration EDIFACT kodlama hakkında bilgi edinin"
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Tümleştirme hesabınızda şemalar, haritalar, iş ortakları ve daha fazlasını arayın"


[boxDoc]: ./connectors-create-api-box.md "Box’a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[delaydoc]: ./connectors-native-delay.md "Gecikmeli eylemleri gerçekleştirin"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Dropbox'a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[facebookdoc]: ./connectors-create-api-facebook.md "Facebook’a bağlanın. Zaman tünelinde gönderi yapın, sayfa akışı alın ve daha fazlasını yapın"
[githubdoc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Verilerinizle çalışabilmek için GoogleDrive’a bağlanın"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Elektronik tablolarınızı değiştirebilmek için Google E-Tablolar’a bağlanın"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmek için Google Görevler’e bağlanın"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Google Takvime bağlanır ve takvimi yönetebilir."
[http-responsedoc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[instagramdoc]: ./connectors-create-api-instagram.md "Instagram’a bağlanın. Olayları tetikleyin veya üzerinde işlem yapın"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "MailChimp hesabınıza bağlanın. Postaları yönetin ve otomatikleştirin"
[mandrilldoc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Microsoft Translator’a bağlanın. Metinleri çevirin, dilleri algılayın ve daha fazlasını yapın" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Office 365 için video bilgileri, video listeleri ile kanallar ve kayıttan yürütme URL'lerini alın"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Kişisel Microsoft OneDrive’ınıza bağlanın. Dosyaları karşıya yükleyin, silin, listeleyin ve daha fazlasını yapın"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "İş için Microsoft OneDrive’ınıza bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Outlook posta kutunuza bağlanın. E-posta, takvim, kişi ve diğerlerini yönetin"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Microsoft Project Online’a bağlanın. Proje, görev, kaynaklar ve daha fazlasını yönetin"
[querydoc]: ./connectors-native-query.md "Sorgu eylemi ile dizileri seçip filtreleyin"
[rssdoc]: ./connectors-create-api-rss.md "Akış öğeleri yayımlayıp alın, RSS akışında yeni bir öğe yayımlandığında işlemleri tetikleyin."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "SendGrid’e bağlanın. E-posta gönderin ve alıcı listelerini yönetin"
[sftpdoc]: ./connectors-create-api-sftp.md "SFTP hesabınıza bağlanın. Dosyalarınızı karşıya yükleyin, alın, silin ve diğer işlemleri yapın"
[slackdoc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtpdoc]: ./connectors-create-api-smtp.md "SMTP sunucusuna bağlanın ve ekler içeren e-postalar gönderin"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[trellodoc]: ./connectors-create-api-trello.md "Trello’ya bağlanın. Projelerinizi yönetin ve herhangi bir şeyi herhangi bir kimseyle düzenleyin"
[twiliodoc]: ./connectors-create-api-twilio.md "Twilio’ya bağlanın. İletiler gönderip alın, mevcut numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Wunderlist’e bağlanın. Görevlerinizi ve yapılacaklar listenizi yönetin, tüm işlerinizi eşitleyin ve daha fazlasını yapın"
[yammerdoc]: ./connectors-create-api-yammer.md "Yammer’a bağlanın. İleti gönderin, yeni iletiler alın ve daha fazlasını yapın"
[youtubedoc]: ./connectors-create-api-youtube.md "YouTube’a bağlanın. Videolarınızı ve kanallarınızı yönetin"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
