---
title: Azure Logic Apps için Bağlayıcılar | Microsoft Docs
description: İş akışlarını otomatikleştirmek API, yerleşik ile yönetilen şirket içi, tümleştirme hesabını ve Azure Logic Apps enterprise bağlayıcıları
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 06/29/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2bb3e2ce29037372395aa0b30e9f76f3e712667d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096620"
---
# <a name="connectors-for-azure-logic-apps"></a>Azure mantıksal uygulamaları için bağlayıcılar

Azure Logic Apps ile otomatik iş akışları oluşturduğunuzda bağlayıcılar bütünleyici yürütün. Logic apps içinde bağlayıcıları kullanarak, şirket içi için Özellikleri'ni genişletin ve uygulamalar oluşturmak ve zaten veri görevleri gerçekleştirmek için bulut. While Logic Apps teklifleri ~ 200 + bağlayıcıları, bu makalede başarıyla uygulamaları binlerce ve yürütmeleri milyonlarca tarafından veri ve bilgilerinizi işlemek için kullanılan popüler ve daha sık kullanılan bağlayıcılar.
Bağlayıcılar, öğelerin veya yönetilen bağlayıcıların olarak kullanılabilir. 

* [**Öğelerin**](#built-ins): Bu yerleşik Eylemler ve özel zamanlamalarda çalıştırın logic apps oluşturmak Tetikleyicileri Yardım almak ve isteklere yanıt ve Azure çağrı diğer uç ile iletişim İşlevler, Azure API uygulamaları (Web uygulamaları), kendi API'ları, yönetilen ve Azure API Management ve iç içe geçmiş mantığı ile yayımlanan isteklerini alacak uygulama. Yerleşik de kullanabilirsiniz yardımcı eylemleri düzenlemek ve mantığı uygulamanızın iş akışını kontrol ve de verilerle çalışır.

* **Yönetilen bağlayıcıların**: Bu bağlayıcıları tetikleyiciler ve diğer hizmetler ve sistemler erişmek için Eylemler sağlar. Bazı bağlayıcılar önce Azure Logic Apps tarafından yönetilen bağlantılar oluşturmanızı gerektirir. Yönetilen bağlayıcıların bu gruplar halinde düzenlenmiştir:

  |   |   |
  |---|---|
  | [**Yönetilen API bağlayıcılar**](#managed-api-connectors) | Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online ve diğerleri gibi hizmetleri kullanan mantıksal uygulamalar oluşturun. | 
  | [**Şirket içi bağlayıcılar**](#on-premises-connectors) | Yükleme ve kurma sonra [şirket içi veri ağ geçidi][gateway-doc], logic apps erişiminizi şirket içi SQL Server, SharePoint Server, Oracle DB, dosya paylaşımları ve diğerleri gibi sistemleri bu bağlayıcılar Yardım. | 
  | [**Tümleştirme hesap bağlayıcılar**](#integration-account-connectors) | Kullanılabilir, oluşturma ve tümleştirme için bir hesap, bu bağlayıcıların dönüştürme ödeme ve XML doğrulamak, kodlama ve düz dosyalar kod çözme ve işletmeler için işlem (B2B) AS2, EDIFACT ve X12 protokolleri iletileri. | 
  | [**Enterprise bağlayıcıları**](#enterprise-connectors) | Ek bir maliyet SAP ve IBM MQ gibi Kurumsal sistemler için erişim sağlayın. |
  ||| 

  Örneğin, Microsoft BizTalk Server kullanıyorsanız, logic apps için bağlanabilir ve kullanarak, BizTalk Server ile iletişim [BizTalk Server Bağlayıcısı](#on-premises-connectors). 
  Genişletme veya BizTalk benzeri logic apps içinde kullanarak işlemleri [tümleştirme hesap Bağlayıcılar](#integration-account-connectors). 

Her bağlayıcı'nın tetikleyiciler ve Swagger açıklamasını artı herhangi bir sınır tarafından tanımlanan, eylemler hakkında teknik bilgi için bkz: [Bağlayıcısı ayrıntıları](/connectors/). Maliyet bilgileri için bkz: [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/) ve [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Yerleşik öğeler

Logic Apps yerleşik Tetikleyiciler sağlar ve böylece zamanlama tabanlı iş akışları oluşturabilirsiniz eylemler diğer uygulamalar ve hizmetler mantıksal uygulamalarınızı akışı denetimi ile iletişim kurmak ve yönetmek veya verileri işlemek logic apps yardımcı olur. 

|   |   |   |   | 
|---|---|---|---| 
| [![API Icon][schedule-icon]<br/>**zamanlama**][recurrence-doc] | -İle temel karmaşık tekrarları için arasında belirtilen bir zamanlamaya göre mantıksal uygulamanızı çalıştırın **yineleme** tetikleyici. <p>-Belirtilen bir süre ile mantıksal uygulamanızı duraklatmak **gecikme** eylem. <p>-Belirtilen tarihe kadar mantıksal uygulamanızı Duraklat ve ile saat **kadar gecikme** eylem. | [![API Icon][http-icon]<br/>**HTTP**][http-doc] | Herhangi bir uç nokta ile tetikleyiciler ve Eylemler HTTP, HTTP + Swagger, ile HTTP ve HTTP + Web kancası iletişim kurar. | 
| [![API Icon][http-request-icon]<br/>**isteği**][http-request-doc] | -Mantıksal uygulamanızı diğer uygulamalar veya hizmetler, olay kılavuz kaynak olayları Tetikle ya da ile Azure Güvenlik Merkezi uyarılarını yanıtlarını Tetikle aranabilir hale **isteği** tetikleyici. <p>-Bir uygulama için yanıtlar göndermek veya hizmete **yanıt** eylem. | [![API Icon][batch-icon]<br/>**toplu işlem**][batch-doc] | -İşlem iletileri ile yığınlardaki **toplu iletileri** tetikleyici. <p>-Var olan çağrı mantıksal uygulamalar ile Tetikleyicileri toplu **toplu ileti gönderme** eylem. | 
| [![API Icon][azure-functions-icon]<br/>**Azure işlevleri**][azure-functions-doc] | Mantıksal uygulamalarınızı özel kod parçacıkları (C# veya Node.js) çalışan Azure işlevlerini çağırın. | [![API Icon][azure-api-management-icon]</br>**Azure API Yönetimi**][azure-api-management-doc] | Tetikleyiciler ve Eylemler yönetmek ve yayımlamak kendi API Azure API Management ile tanımlanan çağırın. | 
| [![API Icon][azure-app-services-icon]<br/>**Azure uygulama hizmetleri**][azure-app-services-doc] | Azure API uygulamaları veya Web uygulamaları, Azure App Service üzerinde barındırılan çağırın. Swagger dahil edildiğinde tetikleyiciler ve Eylemler bu uygulamaları tarafından tanımlanan diğer birinci sınıf tetikleyiciler ve Eylemler gibi görünür. | [![API Icon][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | İstek tetikleyici ile başlayan diğer mantıksal uygulamalar çağırın. | 
||||| 

### <a name="control-workflow"></a>Denetim akışı

Yapılandırma ve mantığı uygulamanızın iş akışı eylemleri denetlemek için yerleşik eylemler şunlardır:

|   |   |   |   | 
|---|---|---|---| 
| [![Yerleşik simgesi][condition-icon]<br/>**koşulu**][condition-doc] | Bir koşulu değerlendirir ve olup olmadığına göre dayanan çalışma farklı eylemler koşul true veya false. | [![Yerleşik simgesi][for-each-icon]</br>**her**][for-each-doc] | Dizideki her öğe aynı eylemleri gerçekleştirin. | 
| [![Yerleşik simgesi][scope-icon]<br/>**kapsamı**][scope-doc] | Grup eylemlere *kapsamları*, kapsam eylemlerin çalışan işlemini tamamladıktan sonra kendi durumu Al. | [![Yerleşik simgesi][switch-icon]</br>**anahtarı**][switch-doc] | Grup eylemlere *durumlarda*, varsayılan durumu dışında benzersiz değerler atanır. Yalnızca bir ifade, nesne ya da belirteci sonucu, atanan değerinin eşleşen durumda çalıştırın. Herhangi bir eşleşme varsa, varsayılan durumda çalıştırın. | 
| [![Yerleşik simgesi][terminate-icon]<br/>**Sonlandır**][terminate-doc] | Etkin olarak çalışan bir mantıksal uygulama iş akışı durdurun. | [![Yerleşik simgesi][until-icon]<br/>**kadar**][until-doc] | Eylemler belirtilen koşulun doğru olması veya bazı durumu değişti kadar yineleyin. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Yönetmek veya veri işleme

Veri çıktıları ve bunların biçimlerini ile çalışmak için yerleşik eylemler şunlardır:  

|   |   | 
|---|---| 
| ![Yerleşik simgesi][data-operations-icon]<br/>**Veri işlemleri** | Verilerle işlemleri: <p>- **Oluşturan**: tek bir çıktı çeşitli türleri ile birden çok girişi oluşturun. <br>- **CSV tablosu oluşturma**: bir dizi JSON nesnelerini içeren bir virgülle ayrılmış değer (CSV) tablosu oluşturun. <br>- **HTML tablosu oluşturmak**: bir dizi JSON nesnelerini içeren bir HTML tablosu oluşturun. <br>- **Filtre dizisi**: başka bir dizide ölçütlerinize uyan öğelerinden bir dizi oluşturun. <br>- **Katılma**: bir dizeyi bir dizinin tüm öğeleri oluşturmak ve öğelerin belirtilen sınırlayıcı ile ayırın. <br>- **JSON ayrıştırma**: Bu özellikleri, iş akışınızı kullanabilmeniz için kullanıcı dostu belirteçleri özellikler ve değerlerinin JSON içinde içerik oluşturun. <br>- **Seçin**: öğeler veya başka bir dizideki dönüştürme ve bu öğe belirtilen özelliklerine eşleyerek JSON nesneleri içeren bir dizi oluşturmak. | 
| ![Yerleşik simgesi][date-time-icon]<br/>**Tarih saat** | Zaman damgalı işlemleri: <p>- **Zamana Ekle**: zaman damgası için belirtilen birim sayısı ekleyin. <br>- **Saat dilimi Dönüştür**: zaman damgası kaynak saat dilimine hedef zaman dilimine Dönüştür. <br>- **Geçerli saat**: geçerli zaman damgası dize olarak döndürür. <br>- **İleride almak**: geçerli zaman damgası artı belirtilen zaman birimleri döndürür. <br>- **Zamanı alma**: belirtilen zaman birimlerini eksi geçerli zaman damgası döndürür. <br>- **Zamandan çıkarma**: zaman damgası zaman birimlerinden sayısı çıkarma. |
| [![Yerleşik simgesi][variables-icon]<br/>**değişkenleri**][variables-doc] | Değişkenleri ile işlemleri: <p>- **Dizi değişkeni ekleme**: bir son öğesi bir değişkeni tarafından depolanan bir dizi olarak değer Ekle. <br>- **Dize değişkenine append**: bir değişkeni tarafından depolanan bir dizedeki son karakter olarak değer Ekle. <br>- **Azaltma değişkeni**: bir değişkeni sabit bir değer tarafından azaltın. <br>- **Artırma değişkeni**: bir değişkeni bir sabit değer artırın. <br>- **Değişkeni başlatmak**: bir değişken oluşturun ve kendi veri türü ve başlangıç değeri bildirin. <br>- **Değişken Ayarla**: farklı bir değer var olan bir değişkene atayın. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Yönetilen API bağlayıcılar

Görevler, işlemler ve bu hizmetler veya sistemlerde iş akışlarıyla otomatikleştirme daha popüler bağlayıcılar şunlardır:

|   |   |   |   | 
|---|---|---|---| 
| [![API Icon][azure-service-bus-icon]<br/>**Azure hizmet veri yolu**][azure-service-bus-doc] | Zaman uyumsuz ileti, oturumlar ve Logic Apps en yaygın kullanılan bağlayıcısıyla konu aboneliklerini yönetin. | [![API Icon][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Kayıtlarını yönetme, saklı yordamları çalıştırmak veya sorguları gerçekleştirmek için SQL Server şirket içi veya buluttaki bir Azure SQL veritabanı bağlayın. | 
| [![API Icon][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Office 365 e-posta hesabınızı oluşturun ve e-postalar, görevler, takvim olayları ve toplantılar, kişiler, istekleri ve diğer özellikleri yönetmenizi bağlanın. | [![API Icon][azure-blob-storage-icon]<br/>**Azure Blob<br/>depolama**][azure-blob-storage-doc] | Oluşturma ve blob içeriği yönetmek için depolama hesabınıza bağlanın. | 
| [![API Icon][sftp-icon]<br/>**SFTP**][sftp-doc] | Dosya ve klasörlerinizi ile çalışabilmek için internet'ten erişebilirsiniz SFTP sunucularına bağlayın. | [![API Icon][sharepoint-online-icon]<br/>**SharePoint<br/>çevrimiçi**][sharepoint-online-doc] | Dosyaları, ekleri, klasörler ve daha fazlasını yönetmek için SharePoint Online'a bağlanın. | 
| [![API Icon][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Oluşturup kayıtları, öğeleri ve daha fazlasını yönetmek için Dynamics 365 hesabınıza bağlanın. | [![API Icon][ftp-icon]<br/>**FTP**][ftp-doc] | Dosya ve klasörlerinizi ile çalışabilmek için internet'ten erişebilirsiniz FTP sunucularına bağlayın. | 
| [![API Icon][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Oluşturma ve öğeleri kaydeder, işleri, nesneleri ve daha fazlasını gibi yönetme Salesforce hesabınıza bağlanın. | [![API Icon][twitter-icon]<br/>**Twitter**][twitter-doc] | Tweet'leri, followers, zaman çizelgeniz ve daha fazlasını yönetebilirsiniz Twitter hesabınıza bağlanın. Tweet'leri SQL, Excel veya SharePoint kaydedin. | 
| [![API Icon][azure-event-hubs-icon]<br/>**Azure olay hub'ları**][azure-event-hubs-doc] | Kullanabilir ve olayları bir Event Hub ile yayımlayın. Örneğin, mantıksal uygulamanızı Event Hubs ile çıkış almanız ve ardından bir gerçek zamanlı analiz sağlayıcısı çıkışı gönderin. | [![API Icon][azure-event-grid-icon]<br/>**Azure olay**</br>**kılavuz**][azure-event-grid-doc] | Azure kaynakları veya üçüncü taraf kaynakları değiştirdiğinizde, bir olay kılavuz, örneğin, yayımlanan olaylarını izleme. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar 

Burada, şirket içi sistemlerde verilerine ve kaynaklarına erişmesini sağlayan bazı yaygın olarak kullanılan bağlayıcılar bulunmaktadır. Bir şirket içi sistem bağlantı oluşturmadan önce öncelikle [indirme, yükleme ve bir şirket içi veri ağ geçidi kurun][gateway-doc]. Bu ağ geçidi, gerekli ağ altyapısını ayarlamak zorunda kalmadan güvenli bir iletişim kanalı sağlar. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![API simgesi][biztalk-server-icon]<br/>**BizTalk**</br> **Sunucu** | [![API Icon][file-system-icon]<br/>**dosya</br> sistem**][file-system-doc] | [![API Icon][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![API Icon][ibm-informix-icon]<br/>**IBM** </br> **Informix**][ibm-informix-doc] | ![API simgesi][mysql-icon]<br/>**MySQL** | 
| [![API Icon][oracle-db-icon]<br/>**Oracle DB**][oracle-db-doc] | ![API simgesi][postgre-sql-icon]<br/>**PostgreSQL** | [![API Icon][sharepoint-server-icon]<br/>**SharePoint</br> sunucu**][sharepoint-server-doc] | [![API Icon][sql-server-icon]<br/>**SQL</br> sunucu**][sql-server-doc] | ![API simgesi][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları 

Bağlayıcılar oluştururken ve ödeme logic apps ile işletmeden işletmeye (B2B) çözümler oluşturmak için İşte bir [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), Azure Enterprise Integration Pack (EIP) aracılığıyla kullanılabilen. Bu Hesapla oluşturun ve iş ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve benzeri ticari gibi B2B yapılarını depolayın. Bu yapıtların kullanmak için mantıksal uygulamalarınızı tümleştirme hesabınızla ilişkilendirin. BizTalk Server şu anda kullanıyorsanız, bu bağlayıcıların zaten tanıdık görünebilir.

|   |   |   |   | 
|---|---|---|---| 
| [![API Icon][as2-icon]<br/>**AS2</br> kod çözme**][as2-decode-doc] | [![API Icon][as2-icon]<br/>**AS2</br> kodlama**][as2-encode-doc] | [![API Icon][edifact-icon]<br/>**EDIFACT</br> kod çözme**][edifact-decode-doc] | [![API Icon][edifact-icon]<br/>**EDIFACT</br> kodlama**][edifact-encode-doc] | 
| [![API Icon][flat-file-decode-icon]<br/>**düz dosya</br> kod çözme**][flat-file-decode-doc] | [![API Icon][flat-file-encode-icon]<br/>**düz dosya</br> kodlama**][flat-file-encode-doc] | [![API Icon][integration-account-icon]<br/>**tümleştirme<br/>hesabı**][integration-account-doc] | [![API Icon][liquid-icon]<br/>**sıvı**</br>**dönüştürür**][json-liquid-transform-doc] | 
| [![API Icon][x12-icon]<br/>**X12</br> kod çözme**][x12-decode-doc] | [![API Icon][x12-icon]<br/>**X12</br> kodlama**][x12-encode-doc] | [![API Icon][xml-transform-icon]<br/>**XML**</br>**dönüştürür**][xml-transform-doc] | [![API Icon][xml-validate-icon]<br/>**XML <br/>doğrulama**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Mantıksal uygulamalarınızı SAP ve IBM MQ gibi Kurumsal sistemler erişebilirsiniz:

|   |   | 
|---|---| 
| [![API Icon][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![API Icon][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

## <a name="more-about-triggers-and-actions"></a>Tetikleyiciler ve eylemler hakkında daha fazla

Bazı bağlayıcılar sağlamak *Tetikleyicileri* , bildirim mantıksal uygulamanızı belirli olaylar oluştuğunda. Bu nedenle bu olaylar gerçekleştiğinde, tetikleyici oluşturur ve mantıksal uygulama örneğini çalıştırır. Örneğin, FTP Bağlayıcısı mantıksal uygulamanızı bir dosya güncelleştirildiğinde başlayan bir "dosya eklenen veya değiştirilirken" tetikleyicisi sağlar. 

Logic Apps bu tür Tetikleyiciler sağlar:  

* *Yoklama Tetikleyicileri*: Bu Tetikleyiciler hizmetinizin belirtilen sıklıkta ve denetimleri en yeni verileri yoklamak. 

  Yeni veriler kullanılabilir olduğunda, uygulamanızın yeni bir örneğini oluşturulan ve giriş olarak geçirilen verileri ile çalıştırır. 

* *Tetikleyiciler anında*: Bu Tetikleyiciler yeni verileri bir uç noktada veya bir olay gerçekleşecek şekilde, oluşturur ve mantıksal uygulamanızı yeni bir örneğini çalıştıran dinleme.

* *Yineleme tetikleyici*: Bu tetikleyici oluşturur ve belirtilen bir zamanlamaya göre mantıksal uygulama örneğini çalıştırır.

Bağlayıcılar da sağlamak *Eylemler* mantığı uygulamanızın iş akışında görevleri gerçekleştirebilir. Örneğin, mantıksal uygulamanızı veri okumak ve mantıksal uygulamanızı daha sonraki adımlarda bu verileri kullanın. Daha belirgin olarak mantıksal uygulamanızı bir SQL veritabanında müşteri verilerini bulun ve bu verileri daha sonra iş akışında mantığı uygulamanızın işlem. 

Tetikleyiciler ve eylemler hakkında daha fazla bilgi için bkz: [bağlayıcılar genel bakış](connectors-overview.md). 

## <a name="custom-apis-and-connectors"></a>Özel API'ları ve bağlayıcılar 

Özel kod çalıştırmak veya bağlayıcıları olarak kullanılabilen olmayan API'leri çağırmak için Logic Apps platformu tarafından genişletebilirsiniz [özel API uygulamaları oluşturma](../logic-apps/logic-apps-create-api-app.md). Ayrıca [özel Bağlayıcıyı oluşturmak](../logic-apps/custom-connector-overview.md) için *herhangi* REST ya da bu API'leri, Azure aboneliğinizde mantıksal uygulama kullanılabilir hale getirmek SOAP tabanlı API'ler.
Özel API uygulamaları ya da bağlayıcıların Azure'da herkes için genel hale getirmek için [Microsoft Sertifika bağlayıcılarının gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Gönderme veya Azure Logic Apps ve bağlayıcıları için oy fikir edinmek için şurayı ziyaret edin [Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

* Makaleler veya düşündüğünüz ayrıntıları eksik belgeleri önemli misiniz? Yanıt Evet ise, varolan makaleleri ekleyerek veya kendi yazarak yardımcı olabilir. Belgelere açık bir kaynaktır ve Github'da barındırılır. Azure belgelerine ait başlama [GitHub deposunu](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Sonraki adımlar

* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Özel bağlayıcılar mantıksal uygulamalar için oluşturma](https://docs.microsoft.com/connectors/custom-connectors/)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Şirket içi veri ağ geçidiyle mantıksal uygulamalardan şirket içi veri kaynaklarına bağlanın"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure İşlevleri ile tümleştirin"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Yönetme ve, API yayımlamak için bir Azure API Management hizmet örneği oluşturma"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Service Bus Kuyruklarından ve Konu Başlıklarından iletiler gönderin ve Service Bus Kuyrukları ile Aboneliklerinden iletiler alın"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Gruplar veya toplu olarak iletilerini işleme"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Bir koşulu değerlendirir ve olup olmadığına göre dayanan çalışma farklı eylemler koşul true veya false olduğunda"
[delay-doc]: ./connectors-native-delay.md "Gecikmeli eylemleri gerçekleştirin"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Dizideki her öğe aynı eylemleri gerçekleştirme"
[http-doc]: ./connectors-native-http.md "HTTP bağlayıcısı ile HTTP aramaları yapın"
[http-request-doc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-response-doc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-swagger-doc]: ./connectors-native-http-swagger.md "HTTP + Swagger bağlayıcısı ile HTTP çağrıları yapın"
[http-webook-doc]: ./connectors-native-webhook.md "HTTP Web kancası eylemleri ve Tetikleyicileri mantıksal uygulamalarınızı ekleme"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[query-doc]: ./connectors-native-query.md "Sorgu eylemi ile dizileri seçip filtreleyin"
[recurrence-doc]:  ./connectors-native-recurrence.md "Mantıksal uygulamalar için yinelenen eylemleri tetikleyin"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Eylemler grubundaki Eylemler çalışan işlemini tamamladıktan sonra kendi durumunu Al gruplar halinde düzenleme" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Benzersiz değerler atanır durumlarda eylemlere düzenleyin. Yalnızca bir ifade, nesne ya da belirteci sonuç değeri eşleşen servis talebi çalıştırın. Herhangi bir eşleşme varsa, varsayılan çalışması çalıştırma"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Durdurmak veya etkin olarak çalışan bir iş akışı mantığı uygulamanız için İptal"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Eylemler belirtilen koşulun doğru olması veya bazı durumu değişti kadar yineleyin."
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Değişkenleriyle başlatılamadı, kümesi, artırma ve azaltma, gibi işlemleri gerçekleştirmek ve dize veya dizi değişkeni ekleme"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md " Azure kaynakları veya üçüncü taraf kaynakları değiştirdiğinizde, bir olay kılavuz, örneğin, yayımlanan olaylarını izleme"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Azure Event Hubs’a bağlanma. Logic Apps ile Event Hubs arasında olay gönderip alma"
[box-doc]: ./connectors-create-api-box.md "Box’a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Dropbox'a bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "CRM Online verileriyle çalışabilmek için Dynamics CRM Online’a bağlanın"
[facebook-doc]: ./connectors-create-api-facebook.md "Facebook’a bağlanın. Zaman tünelinde gönderi yapın, sayfa akışı alın ve daha fazlasını yapın"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Şirket içi dosya sistemine bağlanın"
[ftp-doc]: ./connectors-create-api-ftp.md "Dosyaları karşıya yükleme, alma, silme ve diğer FTP görevleri için bir FTP / FTPS sunucusuna bağlanın"
[github-doc]: ./connectors-create-api-github.md "GitHub’a bağlanın ve sorunları izleyin"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Google Takvime bağlanır ve takvimi yönetebilir."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Verilerinizle çalışabilmek için GoogleDrive’a bağlanın"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Sayfalarınızı değiştirebileceğiniz Google sayfalara bağlanın"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmek için Google Görevler’e bağlanın"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2’ye bağlanın. Bir satırı güncelleştirin, bir tabloyu alın ve diğer işlemleri yapın"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix’e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[ibm-mq-doc]: ./connectors-create-api-mq.md "IBM MQ şirket içi veya ileti gönderme ve alma için azure'da bağlanmak"
[instagram-doc]: ./connectors-create-api-instagram.md "Instagram’a bağlanın. Olayları tetikleyin veya üzerinde işlem yapın"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "MailChimp hesabınıza bağlanın. Postaları yönetin ve otomatikleştirin"
[mandrill-doc]: ./connectors-create-api-mandrill.md "İletişim için Mandrill’e bağlanın"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Microsoft Translator’a bağlanın. Metinleri çevirin, dilleri algılayın ve daha fazlasını yapın" 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Office 365 hesabınıza bağlanın. E-posta gönderip alın, takvim ve kişilerinizi yönetin ve daha fazlasını yapın"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Office 365 için video bilgileri, video listeleri ile kanallar ve kayıttan yürütme URL'lerini alın"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Kişisel Microsoft OneDrive’ınıza bağlanın. Dosyaları karşıya yükleyin, silin, listeleyin ve daha fazlasını yapın"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "İş için Microsoft OneDrive’ınıza bağlanın. Dosyalarınızı karşıya yükleyin, silin, listeleyin ve diğer işlemleri yapın"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Bir Oracle veritabanına bağlanarak satır ekleme, silme ve daha fazlası"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Outlook posta kutunuza bağlanın. E-posta, takvim, kişi ve diğerlerini yönetin"
[project-online-doc]: ./connectors-create-api-projectonline.md "Microsoft Project Online’a bağlanın. Proje, görev, kaynaklar ve daha fazlasını yönetin"
[rss-doc]: ./connectors-create-api-rss.md "Akış öğeleri yayımlayıp alın, RSS akışında yeni bir öğe yayımlandığında işlemleri tetikleyin."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Salesforce hesabınıza bağlanın. Hesapları, müşteri adaylarını, fırsatları ve daha fazlasını yönetin"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Şirket içi SAP sistemine bağlanın"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "SendGrid’e bağlanın. E-posta gönderebilir ve alıcı listeleri yönetme"
[sftp-doc]: ./connectors-create-api-sftp.md "SFTP hesabınıza bağlanın. Dosyalarınızı karşıya yükleyin, alın, silin ve diğer işlemleri yapın"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Şirket içi SharePoint sunucusuna bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "SharePoint Online’a bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[slack-doc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtp-doc]: ./connectors-create-api-smtp.md "Bir SMTP sunucusuna bağlanmak ve ekler içeren e-posta Gönder"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Azure SQL veritabanı veya SQL Server bağlanın. SQL veritabanı tablosunda girdiler oluşturun, güncelleştirin, alın ve silin."
[trello-doc]: ./connectors-create-api-trello.md "Trello’ya bağlanın. Projelerinizi yönetin ve herhangi bir şeyi herhangi bir kimseyle düzenleyin"
[twilio-doc]: ./connectors-create-api-twilio.md "Twilio’ya bağlanın. İletiler gönderip alın, mevcut numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[twitter-doc]: ./connectors-create-api-twitter.md "Twitter’a bağlanın. Zaman çizelgelerini alın, tweetler gönderin ve daha fazlasını yapın"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Wunderlist’e bağlanın. Görevlerinizi ve yapılacaklar listenizi yönetin, tüm işlerinizi eşitleyin ve daha fazlasını yapın"
[yammer-doc]: ./connectors-create-api-yammer.md "Yammer’a bağlanın. İleti gönderin, yeni iletiler alın ve daha fazlasını yapın"
[youtube-doc]: ./connectors-create-api-youtube.md "YouTube’a bağlanın. Videolarınızı ve kanallarınızı yönetin"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Enterprise integration AS2 hakkında bilgi edinin."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Enterprise integration AS2 kod çözme hakkında bilgi edinin"
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Enterprise integration AS2 kodlama hakkında bilgi edinin"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Enterprise integration EDIFACT kod çözme hakkında bilgi edinin"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Enterprise integration EDIFACT kodlama hakkında bilgi edinin"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Enterprise integration düz dosyası hakkında bilgi edinin."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Tümleştirme hesabınızda şemalar, haritalar, iş ortakları ve daha fazlasını arayın"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Liquid ile JSON dönüştürmeleri hakkında bilgi edinin"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Enterprise integration X12 hakkında bilgi edinin."
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Enterprise integration X12 kod çözme hakkında bilgi edinin"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Enterprise integration X12 kodlama hakkında bilgi edinin"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Enterprise integration dönüştürmeleri hakkında bilgi edinin."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Enterprise integration XML doğrulaması hakkında bilgi edinin."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png