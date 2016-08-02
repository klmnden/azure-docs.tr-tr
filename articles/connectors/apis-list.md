<properties
    pageTitle="Microsoft Azure Mantıksal Uygulamaları’nda kullanılan Microsoft tarafından yönetilen bağlayıcıların listesi | Microsoft Azure App Service"
    description="Azure App Service’de Logic Apps oluşturmak için kullanabileceğiniz Microsoft tarafından yönetilen bağlayıcıların tam listesini alın."
    services="app-service\logic"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/31/2016"
    ms.author="deonhe"/>

# Yönetilen bağlayıcıların listesi

>[AZURE.NOTE] Makalenin bu sürümü Logic Apps 2015-08-01-önizleme şema sürümü için geçerlidir. 2014-12-01-önizleme şema sürümü için, [Bağlayıcılar Listesi](../app-service-logic/app-service-logic-connectors-list.md)’ne tıklayın. 

Fiyatlandırma bilgileri ve her bir Hizmet Katmanına dahil olanların listesi için bkz. [Azure App Service Fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Bir Azure hesabı için kaydolmadan önce Azure Logic Apps’i kullanmaya başlamak istiyorsanız [Logic Apps’i Deneyin](https://tryappservice.azure.com/?appservice=logic)’e gidin. App Service’de başlangıç düzeyinde kısa süreli mantıksal uygulamayı hemen oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.

Bu hizmetleri çağıran uygulamaları oluşturmak üzere bu bağlayıcıları hızlı şekilde kullanmayı öğrenmek için simgeyi seçin. Bu bağlayıcılar Mantıksal uygulamalar, PowerApps ve Akışlar oluşturmak için kullanılabilir.

|Bağlayıcılar||||
|-----------|-----------|-----------|-----------|
|[![API Icon][blobicon]<br/>**Azure Blob**][azureblobdoc]|[![API Icon][boxicon]<br/>**Box**][boxDoc]|[![API Icon][crmonlineicon]<br/>**CRM Online**][crmonlinedoc]|[![API Icon][dropboxicon]<br/>**Dropbox**][dropboxdoc]|
|[![API Icon][facebookicon]<br/>**Facebook**][facebookdoc]|[![API Icon][ftpicon]<br/>**FTP**][ftpdoc]|[![API Icon][githubicon]<br/>**GitHub**][githubdoc]|[![API Icon][googledriveicon]<br/>**Google Drive**][googledrivedoc]|
|[![API Icon][mailchimpicon]<br/>**MailChimp**][mailchimpdoc]|[![API Icon][microsofttranslatoricon]<br/>**Translator**][microsofttranslatordoc]|[![API Icon][office365icon]<br/>**Office 365**<br/>**Outlook**][office365outlookdoc]|[![API Icon][office365icon]<br/>**Office 365**<br/>**Users**][office365usersdoc]|
|[![API Icon][office365icon]<br/>**Office 365**<br/>**Video**][office365videodoc]|[![API Icon][onedriveicon]<br/>**OneDrive**][onedrivedoc]|[![API Icon][onedriveicon]<br/>**OneDrive<br/>for Business**][onedriveforbusinessdoc]|[![API Icon][outlookicon]<br/>**Outlook**][outlookdoc]|
|[![API Icon][projectonlineicon]<br/>**Project<br/>Online**][projectonlinedoc]|[![API Icon][rssicon]<br/>**RSS**][rssdoc]|[![API Icon][salesforceicon]<br/>**Salesforce**][salesforcedoc]|[![API Icon][sendgridicon]<br/>**SendGrid**][sendgriddoc]|
|[![API Icon][servicebusicon]<br/>**Service Bus**][servicebusdoc]|[![API Icon][sftpicon]<br/>**SFTP**][sftpdoc]|[![API Icon][sharepointicon]<br/>**SharePoint**<br/>**Online**][sharepointdoc]|[![API Icon][slackicon]<br/>**Slack**<br/>][slackdoc]|
|[![API Icon][smtpicon]<br/>**SMTP**][smtpdoc]|[![API Icon][sqlicon]<br/>**SQL Azure**][sqldoc]|[![API Icon][trelloicon]<br/>**Trello**][trellodoc]|[![API Icon][twilioicon]<br/>**Twilio**][twiliodoc]|
|[![API Icon][twittericon]<br/>**Twitter**][twitterdoc]|[![API Icon][wunderlisticon]<br/>**Wunderlist**][wunderlistdoc]|[![API Icon][yammericon]<br/>**Yammer**][yammerdoc] | |

> [AZURE.NOTE] 2014-12-01-önizleme şeması kullanarak Logic Apps oluşturuyorsanız, BizTalk için olanlar gibi kurumsal tümleştirme bağlayıcılarının yukarıdaki listede yer almadığını fark edersiniz. Bunların önemli olduğunu biliyor ve kısa sürede hizmetinize sunmak için çalışıyoruz. Paylaşılması için kesin bir tarih verememekle birlikte, bunların kullanımınıza sunulmasının ilk önceliklerimizden biri olduğunu bilin. Bu sırada [v1 API’lerinize ve BizTalk API’lerinize Logic Apps’den](https://blogs.msdn.microsoft.com/logicapps/2016/02/25/accessing-v1-apis-and-biztalk-apis-from-logic-apps/) erişebilirsiniz. Anlayışınız için teşekkür ederiz. Bizi izlemeye devam edin.


### Bağlayıcılar tetikleyiciler olabilir
Bazı bağlayıcılar, belirli olaylar meydana geldiğinde, uygulamanızı bilgilendirebilecek tetikleyiciler sağlar. Örneğin, FTP bağlayıcısı OnUpdatedFile tetikleyicisine sahiptir. Bu tetikleyiciyi dinleyen ve tetikleyici her tetiklendiğinde eyleme geçen bir Mantıksal uygulama, PowerApp ya da Akış oluşturabilirsiniz.

İki tür tetikleyici sunucusu bulunur:  

* Yoklama Tetikleyicileri: Bu tetikleyiciler yeni verileri denetlemek için hizmetinizi belirtilen aralıkta yoklar. Yeni veriler kullanılabilir olduğunda, uygulamanızın yeni bir örneği girdi olan verilerle çalışır. Aynı verinin birden çok kez kullanılmasını önlemek için, tetikleyici okunan ve uygulamanıza iletilen verileri temizleyebilir.
* Anında İletme Tetikleyicileri: Bu tetikleyiciler uç noktada bir olayın meydana gelmesine ilişkin verileri dinler. Ardından, uygulamanızın yeni bir örneğini tetikler. Twitter bağlayıcısı böyle bir örnektir.


### Bağlayıcılar eylemler olabilir
Bağlayıcılar uygulamalarınız içinde eylemler olarak da kullanılabilir. Eylemler, daha sonra uygulamanızın yürütülmesinde kullanılabilecek verilere bakılmasında faydalıdır. Örneğin, sipariş işleme sırasında bir SQL veritabanında müşteri verilerini aramanız gerekebilir. Veya, hedef tablodaki verileri yazmanız, güncelleştirmeniz ya da silmeniz gerekebilir. Bağlayıcılar tarafından sağlanan eylemleri kullanarak bunu yapabilirsiniz. Eylemler Swagger meta verilerde tanımlanan işlemlere eşlenir.


[Yenilikler](../app-service-logic/app-service-logic-schema-2015-08-01.md)  
[Şimdi Mantıksal uygulama oluşturun](../app-service-logic/app-service-logic-create-a-logic-app.md)  
[PowerApps kullanmaya hemen başlayın](../power-apps/powerapps-get-started-azure-portal.md)  
[Mevcut Mantıksal uygulamaları en son şema sürümüne geçirin](connectors-schema-migration.md) 

<!--Connectors Documentation-->
[azureblobdoc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları yönetmek için Azure blob’a bağlanın."
[bingsearchDoc]: ./connectors-create-api-bingsearch.md "Bing’de web, resimleri, haberler ve video arayın."
[boxDoc]: ./connectors-create-api-box.md "Box’a bağlanır ve dosya görevleri karşıya yükleyebilir, alabilir, silebilir, listeleyebilir ve daha fazlasını yapabilir."
[crmonlinedoc]: ./connectors-create-api-crmonline.md "Dynamics CRM Online’a bağlanın ve Dynamics CRM Online verilerinizle daha fazlasını yapın."
[dropboxdoc]: ./connectors-create-api-dropbox.md "Dropbox’a bağlanın ve dosya görevlerini alın, silin, listeleyin ve daha fazlasını yapın."
[exceldoc]: ./connectors-create-api-excel.md "Excel’e bağlanın."
[facebookdoc]: ./connectors-create-api-facebook.md "Facebook’a bağlanarak zaman akışında gönderi yapın, sayfa akışı alın ve daha fazlasını yapın."
[ftpdoc]: ./connectors-create-api-ftp.md "FTP / FTPS sunucusuna bağlanır ve dosyaları yükleme, alma, silme daha fazlası dahil farklı FTP görevlerini yapar."
[googledrivedoc]: ./connectors-create-api-googledrive.md "GoogleDrive’a bağlanın ve verilerinizle etkileşim kurun."
[microsofttranslatordoc]: ./connectors-create-api-microsofttranslator.md
[office365outlookdoc]: ./connectors-create-api-office365-outlook.md " Office 365 Bağlayıcısı, Office 365 hesabınızı kullanarak e-posta gönderebilir ve alabilir, takviminizi yönetebilir ve kişilerinizi yönetebilir."
[officeunifieddoc]: ./connectors-create-api-bingsearch.md
[office365usersdoc]: ./connectors-create-api-office365-users.md
[office365videodoc]: ./connectors-create-api-office365-video.md
[onedrivedoc]: ./connectors-create-api-onedrive.md "Kişisel Microsoft OneDrive’ınıza bağlanır ve dosyaları karşıya yükler, siler, listeler ve daha fazlasını yapar."
[onedriveforbusinessdoc]: ./connectors-create-api-onedriveforbusiness.md "İş Microsoft OneDrive’ınıza bağlanır ve dosyalarınızı karşıya yükler, siler, listeler ve daha fazlasını yapar."
[outlookdoc]: ./connectors-create-api-outlook.md "Outlook posta kutunuza bağlanın ve e-posta adresinize erişin ve daha fazlasını yapın."
[projectonlinedoc]: ./connectors-create-api-projectonline.md "Microsoft Project Online’a bağlanır."
[rssdoc]: ./connectors-create-api-rss.md "RSS bağlayıcısı kullanıcıların akış öğelerini yayımlamasını ve almasını sağlar. Ayrıca, akışta yeni bir öğe yayımlandığında, kullanıcıların işlemleri tetiklemesini sağlar."
[salesforcedoc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın ve hesapları, müşteri adaylarını, fırsatları yönetin ve daha fazlasını yapın."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Microsoft Project Online’a bağlanır."
[servicebusdoc]: ./connectors-create-api-servicebus.md "Service Bus Sıralarından ve Konulardan iletiler gönderebilir ve Service Bus Sıralarından ve Aboneliklerden iletiler alabilir."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Belgeleri yönetmek ve öğeleri listelemek için SharePoint Online’a bağlanır."
[slackdoc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın."
[sftpdoc]: ./connectors-create-api-sftp.md "SFTP’ye bağlanır ve dosyaları karşıya yükleyebilir, alabilir, silebilir ve daha fazlasını yapabilir."
[githubdoc]: ./connectors-create-api-github.md "GitHub’a bağlanır ve sorunları izleyebilir."
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Daha iyi E-posta Gönderin."
[smtpdoc]: ./connectors-create-api-smtp.md "SMTP sunucusuna bağlanır ve ekler içeren e-postalar gönderebilir."
[sqldoc]: ./connectors-create-api-sqlazure.md "SQL Azure veritabanına bağlanır. SQL veritabanı tablosunda girdiler oluşturabilir, güncelleştirebilir, alabilir ve silebilirsiniz."
[trellodoc]: ./connectors-create-api-trello.md "Trello, herkesle birlikte her şeyi düzenlemenin ücretsiz, esnek ve görsel yoludur."
[twiliodoc]: ./connectors-create-api-twilio.md "Twilio’ya bağlanır ve iletiler alabilir ve gönderebilir, mevcut numaraları alabilir, gelen telefon numaralarını yönetebilir ve daha fazlasını yapabilir."
[twitterdoc]: ./connectors-create-api-twitter.md "Twitter’a bağlanır ve akışları alır, tweetler gönderir ve daha fazlasını yapar."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Yaşamınızı eşitlenmiş olarak tutun."
[yammerdoc]: ./connectors-create-api-yammer.md "İletiler göndermek ve yeni iletileri almak için Yammer’a bağlanır."

<!--Icon references-->
[blobicon]: ./media/apis-list/blobicon.png
[bingsearchicon]: ./media/apis-list/bingsearchicon.png
[boxicon]: ./media/apis-list/boxicon.png
[ftpicon]: ./media/apis-list/ftpicon.png
[githubicon]: ./media/apis-list/githubicon.png
[crmonlineicon]: ./media/apis-list/dynamicscrmicon.png
[dropboxicon]: ./media/apis-list/dropboxicon.png
[excelicon]: ./media/apis-list/excelicon.png
[facebookicon]: ./media/apis-list/facebookicon.png
[googledriveicon]: ./media/apis-list/googledriveicon.png
[mailchimpicon]: ./media/apis-list/mailchimpicon.png
[microsofttranslatoricon]: ./media/apis-list/translatoricon.png
[office365icon]: ./media/apis-list/office365icon.png
[onedriveicon]: ./media/apis-list/onedriveicon.png
[onedriveforbusinessicon]: ./media/apis-list/onedriveforbusinessicon.png
[outlookicon]: ./media/apis-list/outlookicon.png
[projectonlineicon]: ./media/apis-list/projectonlineicon.png
[rssicon]: ./media/apis-list/rssicon.png
[salesforceicon]: ./media/apis-list/salesforceicon.png
[sendgridicon]: ./media/apis-list/sendgridicon.png
[servicebusicon]: ./media/apis-list/servicebusicon.png
[sftpicon]: ./media/apis-list/sftpicon.png
[sharepointicon]: ./media/apis-list/sharepointicon.png
[slackicon]: ./media/apis-list/slackicon.png
[smtpicon]: ./media/apis-list/smtpicon.png
[sqlicon]: ./media/apis-list/sqlicon.png
[trelloicon]: ./media/apis-list/trelloicon.png
[twilioicon]: ./media/apis-list/twilioicon.png
[twittericon]: ./media/apis-list/twittericon.png
[wunderlisticon]: ./media/apis-list/wunderlisticon.png
[yammericon]: ./media/apis-list/yammericon.png



<!---HONumber=Jun16_HO2-->


