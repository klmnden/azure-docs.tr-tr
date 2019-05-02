---
title: Azure Logic Apps için bağlayıcılar
description: Yerleşik, yönetilen, şirket içi, tümleştirme hesabı ve kurumsal bağlayıcılar dahil olmak üzere Azure Logic Apps için bağlayıcılar ile iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: e008d9fd2734af6a355771c321ecaea9150bcc33
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722981"
---
# <a name="connectors-for-azure-logic-apps"></a>Azure Logic Apps için bağlayıcılar

Bağlayıcılar hızlı erişim Azure Logic Apps'ten olayları, veri ve eylemleri için diğer uygulamaları, hizmetleri, sistemleri, protokoller ve platformlar arasında sağlar. Logic apps bağlayıcıları kullanarak, özellikleri oluşturan ve zaten sahip verilerle görevleri gerçekleştirmek Bulut ve şirket içi uygulamalarınız için genişletin.

Logic Apps teklifler while [~ 200'den fazla bağlayıcı](https://docs.microsoft.com/connectors), başarıyla binlerce uygulama ve milyonlarca yürütme işlemi tarafından veri ve bilgi işlem için kullanılan popüler ve yaygın olarak kullanılan bağlayıcılar bu makalede açıklanır. Bağlayıcılar ve bağlayıcıların başvuru bilgileri, sınırları, tetikleyiciler ve Eylemler gibi tam listesini bulmak için bağlayıcı başvuru sayfaları'nın altında gözden [bağlayıcılara genel bakış](https://docs.microsoft.com/connectors). Ayrıca, daha fazla bilgi edinin [tetikleyiciler ve Eylemler](#triggers-actions).

> [!NOTE]
> Bir hizmet ya da bağlayıcı yok API ile tümleştirmek için doğrudan bir protokol gibi HTTP üzerinden hizmet çağrısı veya oluşturma bir [özel bağlayıcı](#custom).

Bağlayıcılar, yerleşik tetikleyiciler ve Eylemler veya yönetilen bağlayıcılar olarak kullanılabilir:

* [**Yerleşik olanları**](#built-ins): Bu yerleşik Eylemler ve Tetikleyiciler "Azure Logic Apps ve Yardım özel zamanlamalara göre çalıştırın, diğer uç noktalar ile iletişim kurmak, alabilir ve isteklere yanıt ve Azure işlevleri, Azure API Apps (Web uygulamaları), kendi API çağrısı, mantıksal uygulamalar oluşturmak için yerel" Yönetilen ve yayımlanan ile Azure API Management ve istekleri alabilecek iç içe mantıksal uygulamalar. Ayrıca yerleşik kullanabileceğiniz yardımcı eylemleri düzenlemek ve mantıksal uygulamanızın iş akışı denetim ve ayrıca verileri ile çalışma.

* **Yönetilen Bağlayıcılar**: Bu bağlayıcılar, dağıtılan ve Microsoft tarafından yönetilen, tetikleyiciler ve Eylemler cloud services, şirket içi sistemler veya her ikisi de Office 365, Azure Blob Depolama, SQL Server, Dynamics, Salesforce, SharePoint ve daha fazlası dahil olmak üzere erişim sağlar. Bazı bağlayıcılar, özellikle işletmeler arası (B2B) iletişimi senaryoları desteklemek ve gerektiren bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) mantıksal uygulamanıza bağlı. Bazı bağlayıcılar kullanmadan önce ilk Azure Logic Apps tarafından yönetilen bağlantıları oluşturmanız gerekebilir. 

  Örneğin, Microsoft BizTalk Server kullanıyorsanız, logic apps için bağlanabilir ve kullanarak, BizTalk Server ile iletişim [BizTalk Server şirket içi Bağlayıcısı](#on-premises-connectors). 
  Genişletme veya kullanarak, logic apps BizTalk benzeri işlemleri gerçekleştirmesi [tümleştirme hesabı bağlayıcıları](#integration-account-connectors).

  Bağlayıcılar, standart veya Kurumsal sınıflandırılır. 
  [Kurumsal Bağlayıcılar](#enterprise-connectors) Kurumsal sistemlere SAP, IBM MQ ve IBM 3270 gibi ek ücret karşılığında erişim sağlar. Bir bağlayıcı Standard veya Enterprise olup olmadığını belirlemek için her bağlayıcı'nın altında başvuru sayfası teknik ayrıntılara bakın [bağlayıcılara genel bakış](https://docs.microsoft.com/connectors). 
  
  Bazı bağlayıcılar birden çok kategori çapraz olsa da bu kategorileri kullanarak bağlayıcılar da tanımlayabilirsiniz. 
  Örneğin, SAP, Kurumsal bağlayıcı ve bir şirket içi Bağlayıcısı şöyledir:

  |   |   |
  |---|---|
  | [**Yönetilen API bağlayıcıları**](#managed-api-connectors) | Azure Blob Depolama, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online ve çok daha fazlası gibi hizmetleri kullanan mantıksal uygulamalar oluşturun. |
  | [**Şirket içi bağlayıcılar**](#on-premises-connectors) | Yükleme ve kurma sonra [şirket içi veri ağ geçidi][gateway-doc], logic apps erişiminizi şirket içi SQL Server, SharePoint Server, Oracle DB, dosya paylaşımları ve diğerleri gibi sistemleri bu bağlayıcılar Yardım. |
  | [**Tümleştirme hesabı bağlayıcıları**](#integration-account-connectors) | Oluştururken ve bu bağlayıcılar dönüştürme bir tümleştirme hesabı için ödeme yaparsınız ve XML doğrulama, kodlamak ve düz dosyaları kodlayıp kod çözebilirsiniz, ve işletmeler arası işlem kullanılabilir (B2B) AS2, EDIFACT ve X12 protokolleri iletileri. |
  |||

> [!NOTE]
> Bir Openapı tarafından tanımlanan bağlayıcılar ve Eylemler ve hiçbir tetikleyici gibi her bir bağlayıcının başvuru bilgileri tam listesi için (önceki adıyla Swagger) açıklama yanı sıra, herhangi bir sınırlama altında tam listesini bulabilirsiniz [bağlayıcılara genel bakış ](/connectors/). Fiyatlandırma bilgileri için bkz: [Logic Apps fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/logic-apps/) ve [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Yerleşik olanları

Logic Apps yerleşik Tetikleyiciler sağlar ve zamanlama tabanlı iş akışları oluşturmak için diğer uygulamaları ve Hizmetleri, denetim akışı, logic apps ile iletişim kurmak ve yönetmek veya verileri işlemek logic apps eylemleri yardımcı.

|   |   |   |   | 
|---|---|---|---| 
| [![API simgesi][schedule-icon]<br/>**zamanlaması**][recurrence-doc] | -İle temel karmaşık yinelenme için arasında belirli bir zamanlamaya göre mantıksal uygulamanızı çalıştırma **yinelenme** tetikleyici. <p>-Belirtilen bir süre ile mantıksal uygulamanızı duraklatma **gecikme** eylem. <p>-Belirtilen bir tarihe kadar mantıksal uygulamanızı duraklatma ve ile zaman **Geciktir** eylem. | [![API simgesi][http-icon]<br/>**HTTP**][http-doc] | Herhangi bir uç noktası ile HTTP tetikleyiciler ve Eylemler HTTP, HTTP + Swagger, için ve HTTP + Web kancası iletişim kurar. | 
| [![API simgesi][http-request-icon]<br/>**iste**][http-request-doc] | -Mantıksal uygulamanızı diğer uygulamalar veya hizmetler, Event Grid kaynak olayları Tetikle ya da Azure Güvenlik Merkezi uyarıları ile yanıtlarını Tetikle çağrılabilir olun **istek** tetikleyici. <p>-Uygulama yanıtlar göndermek veya hizmete **yanıt** eylem. | [![API simgesi][batch-icon]<br/>**Batch**][batch-doc] | -İletileri ile toplu işleme **toplu iletiler** tetikleyici. <p>-Var olan mantıksal uygulamalar çağrı Tetikleyiciler ile batch **toplu ileti gönderme** eylem. | 
| [![API simgesi][azure-functions-icon]<br/>**Azure işlevleri**][azure-functions-doc] | Mantıksal uygulamalarınızı özel kod parçacıkları (C# veya Node.js) çalışan Azure işlevleri'ni arayın. | [![API simgesi][azure-api-management-icon]</br>**Azure API Management**][azure-api-management-doc] | Yönetmek ve yayımlamak ve Azure API Management ile kendi Apı'lerinize tarafından tanımlanan tetikleyiciler ve eylemlerin çağırın. | 
| [![API simgesi][azure-app-services-icon]<br/>**Azure uygulama hizmetleri**][azure-app-services-doc] | Azure API Apps veya Azure App Service üzerinde barındırılan Web uygulamaları arayın. Swagger eklendiğinde Tetikleyicileri ve eylemleri bu uygulamaları tarafından tanımlanan diğer birinci sınıf tetikleyiciler ve Eylemler gibi görünür. | [![API simgesi][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | İstek tetikleyicisi ile başlayan diğer mantıksal uygulamaları çağırma. | 
||||| 

### <a name="control-workflow"></a>Denetim iş akışı

Yapılandırma ve mantıksal uygulamanızın iş akışında eylemleri denetlemek için yerleşik eylemler şunlardır:

|   |   |   |   | 
|---|---|---|---| 
| [![Yerleşik simgeyi][condition-icon]<br/>**koşulu**][condition-doc] | Bir koşulu değerlendirmek ve bağlı çalışma farklı eylemlerin koşul true veya false. | [![Yerleşik simgeyi][for-each-icon]</br>**her**][for-each-doc] | Bir dizideki her öğeye aynı eylemleri gerçekleştirin. | 
| [![Yerleşik simgeyi][scope-icon]<br/>**kapsamı**][scope-doc] | Grup eylemlere *kapsamları*, çalıştırma kapsamı eylemleri tamamladıktan sonra kendi durumunu alın. | [![Yerleşik simgeyi][switch-icon]</br>**anahtarı**][switch-doc] | Grup eylemlere *çalışmaları*, varsayılan durum dışında benzersiz değerler atanır. Yalnızca bu durumda, atanan değeri bir ifade, nesne veya belirteç sonucu eşleşen çalıştırın. Herhangi bir eşleşme varsa, varsayılan durumda çalıştırın. | 
| [![Yerleşik simgeyi][terminate-icon]<br/>**Sonlandır**][terminate-doc] | Etkin olarak çalışan bir mantıksal uygulama iş akışı durdurun. | [![Yerleşik simgeyi][until-icon]<br/>**kadar**][until-doc] | Eylemler belirtilen koşulun true olması veya bazı durumu değişti kadar tekrarlayın. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Verileri değiştirebilen veya yönetme

Veri çıktıları ve bunların biçimleri ile çalışmak için yerleşik eylemler şunlardır:  

|   |   | 
|---|---| 
| [![Yerleşik simgeyi][data-operations-icon]<br/>**veri işlemleri**][data-operations-doc] | Veri işlemlerini gerçekleştirin: <p>- **Compose**: Tek bir çıkış çeşitli türleri ile birden çok giriş oluşturun. <br>- **CSV tablosu oluşturma**: Bir dizi JSON nesnesi içeren bir virgülle ayrılmış değer (CSV) tablo oluşturun. <br>- **HTML tablosu oluşturma**: Bir dizi JSON nesnesi içeren bir HTML tablosu oluşturun. <br>- **Filtre dizisi**: Ölçütlerinizi karşılayan öğeleri başka bir dizide bir dizi oluşturun. <br>- **Birleştirme**: Bir dizideki tüm öğeler bir dize oluşturma ve bu öğeler belirtilen bir sınırlayıcı ile ayırın. <br>- **JSON Ayrıştır**: Bu özellikler, iş akışında kullanabilmeniz için kullanıcı dostu belirteçleri özelliklerini ve değerlerini JSON içeriği oluşturun. <br>- **Seçin**: Bir dizi, öğeleri veya başka bir dizideki değerleri dönüştürme ve öğelerle eşleme için belirtilen özellikleri JSON nesneleriyle oluşturun. | 
| ![Yerleşik simgesi][date-time-icon]<br/>**Tarih saat** | Zaman damgalı işlemleri gerçekleştirin: <p>- **Saate Ekle**: Bir zaman damgası için belirtilen birim sayısını ekleyin. <br>- **Saat dilimini Dönüştür**: Bir zaman damgasını kaynak saat diliminden hedef saat dilimine dönüştürür. <br>- **Geçerli saati**: Geçerli zaman damgasını dize olarak döndürür. <br>- **Gelecekteki saat Al**: Yanı sıra belirtilen zaman birimi geçerli zaman damgasını döndürür. <br>- **Geçmişteki saati Al**: Belirtilen zaman birimi eksi geçerli zaman damgasını döndürür. <br>- **Saatten çıkar**: Bir zaman damgası saat birimleri sayısı çıkarın. |
| [![Yerleşik simgeyi][variables-icon]<br/>**değişkenleri**][variables-doc] | Değişkenleri ile işlemleri gerçekleştirin: <p>- **Dizi değişkenine Ekle**: Bir dizi değişkeni tarafından depolanan son öğe olarak değer Ekle. <br>- **Dize değişkenine Ekle**: Bir değişkeni tarafından depolanan bir dizedeki son karakter olarak değer Ekle. <br>- **Değişkeni Azalt**: Bir değişken bir sabit değere göre azaltın. <br>- **Artış değişkeni**: Bir değişken bir sabit değere göre artırın. <br>- **Değişkeni başlatmak**: Bir değişken oluşturun ve başlangıç değeri ve veri türü bildirin. <br>- **Değişken Ayarla**: Farklı bir değer var olan bir değişkene atayın. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Yönetilen API bağlayıcıları

Görevler, işlemler ve bu hizmetleri ve sistemleri ile iş akışlarını otomatik hale getirmek için daha popüler bağlayıcılar şunlardır:

|   |   |   |   | 
|---|---|---|---| 
| [![API simgesi][azure-service-bus-icon]<br/>**Azure Service Bus**][azure-service-bus-doc] | Zaman uyumsuz iletiler, oturumları ve Logic Apps içinde en sık kullanılan bağlayıcısıyla konu aboneliklerini yönetin. | [![API simgesi][sql-server-icon]<br/>**SQL Server**][sql-server-doc] | Kayıtlarını yönetme, saklı yordamları çalıştırmak veya sorguları gerçekleştirmek için SQL Server şirket içi veya buluttaki Azure SQL veritabanı bağlayın. | 
| [![API simgesi][office-365-outlook-icon]<br/>**Office 365<br/>Outlook**][office-365-outlook-doc] | Oluşturabilir ve e-postaları, görevler, Takvim etkinlikleri ve toplantıları, kişiler, istekleri ve diğer yönetmek için Office 365 e-posta hesabınıza bağlanın. | [![API simgesi][azure-blob-storage-icon]<br/>**Azure Blob<br/>depolama**][azure-blob-storage-doc] | Oluşturabilir ve blob içeriğini yönetmek için depolama hesabınıza bağlanın. | 
| [![API simgesi][sftp-icon]<br/>**SFTP**][sftp-doc] | Dosya ve klasörlerinizi ile çalışabilmek internet'ten erişmek SFTP sunucularına bağlayın. | [![API simgesi][sharepoint-online-icon]<br/>**SharePoint<br/>çevrimiçi**][sharepoint-online-doc] | Dosyaları, ekleri, klasörleri ve daha fazlasını yönetmek için SharePoint Online'a bağlanın. | 
| [![API simgesi][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Oluşturabilir ve kayıtları, öğeleri ve daha fazlasını yönetmek için Dynamics 365 hesabınıza bağlanın. | [![API simgesi][ftp-icon]<br/>**FTP**][ftp-doc] | FTP sunucuları, dosya ve klasörlerle çalışabilmesi için internet'ten erişmek için bağlanın. | 
| [![API simgesi][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Oluşturabilir ve kayıtları, işleri, nesneleri ve diğer öğeleri yönetmek için Salesforce hesabınıza bağlanın. | [![API simgesi][twitter-icon]<br/>**Twitter**][twitter-doc] | Tweetleri, takipçi, zaman çizelgeniz ve yönetmek için Twitter hesabınıza bağlanın. SQL, Excel veya SharePoint tweetlerinizi kaydedin. | 
| [![API simgesi][azure-event-hubs-icon]<br/>**Azure Event Hubs**][azure-event-hubs-doc] | Kullanma ve bir olay hub'ı üzerinden olayları yayımlama. Örneğin, Event Hubs ile mantıksal uygulamanızdan çıkış alırsınız ve çıkarılan bir gerçek zamanlı bir analiz sağlayıcısına gönderebilirsiniz. | [![API simgesi][azure-event-grid-icon]<br/>**Azure olay**</br>**kılavuz**][azure-event-grid-doc] | Olayları bir Event Grid tarafından Örneğin, Azure veya üçüncü taraf kaynaklar değiştirdiğinizde yayımlanan izleyin. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Şirket içi bağlayıcılar 

Şirket içi sistemlerde veri ve kaynaklara erişim sağlayan bazı yaygın olarak kullanılan bağlayıcılar aşağıda verilmiştir. Bir şirket içi sistemi bağlantısı oluşturabilmeniz için önce [indirin, yükleyin ve bir şirket içi veri ağ geçidi ayarlama][gateway-doc]. Bu ağ geçidi, gerekli ağ altyapısını ayarlamak zorunda kalmadan güvenli bir iletişim kanalı sağlar. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![API simgesi][biztalk-server-icon]<br/>**BizTalk**</br> **Sunucu** | [![API simgesi][file-system-icon]<br/>**dosya</br> sistem**][file-system-doc] | [![API simgesi][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![API simgesi][ibm-informix-icon]<br/>**IBM** </br> **Informix**][ibm-informix-doc] | ![API simgesi][mysql-icon]<br/>**MySQL** | 
| [![API simgesi][oracle-db-icon]<br/>**Oracle DB**][oracle-db-doc] | ![API simgesi][postgre-sql-icon]<br/>**PostgreSQL** | [![API simgesi][sharepoint-server-icon]<br/>**SharePoint</br> sunucusu**][sharepoint-server-doc] | [![API simgesi][sql-server-icon]<br/>**SQL</br> sunucusu**][sql-server-doc] | ![API simgesi][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Tümleştirme hesabı bağlayıcıları 

Bağlayıcılar oluştururken ve ödeme logic apps ile işletmeden işletmeye (B2B) çözümleri oluşturmak için İşte bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), azure'da Enterprise Integration Pack (EIP) aracılığıyla kullanılabilen. Bu Hesapla oluşturabilir ve iş ortakları, sözleşmeler, haritalar, şemalar, sertifikaları ve benzeri ticari gibi B2B yapıtları depolayın. Bu yapılar kullanmak için logic apps tümleştirmesi hesabınızla ilişkilendirir. BizTalk Server'ı şu anda kullanıyorsanız, bu bağlayıcıları zaten alışık olduğunuz görünebilir.

|   |   |   |   | 
|---|---|---|---| 
| [![API simgesi][as2-icon]<br/>**AS2</br> kodunu çözme**][as2-doc] | [![API simgesi][as2-icon]<br/>**AS2</br> kodlama**][as2-doc] | [![API simgesi][edifact-icon]<br/>**EDIFACT</br> kodunu çözme**][edifact-decode-doc] | [![API simgesi][edifact-icon]<br/>**EDIFACT</br> kodlama**][edifact-encode-doc] | 
| [![API simgesi][flat-file-decode-icon]<br/>**düz dosya</br> kodunu çözme**][flat-file-decode-doc] | [![API simgesi][flat-file-encode-icon]<br/>**düz dosya</br> kodlama**][flat-file-encode-doc] | [![API simgesi][integration-account-icon]<br/>**tümleştirme<br/>hesabı**][integration-account-doc] | [![API simgesi][liquid-icon]<br/>**Liquid**</br>**dönüştürür**][json-liquid-transform-doc] | 
| [![API simgesi][x12-icon]<br/>**X12</br> kodunu çözme**][x12-decode-doc] | [![API simgesi][x12-icon]<br/>**X12</br> kodlama**][x12-encode-doc] | [![API simgesi][xml-transform-icon]<br/>**XML**</br>**dönüştürür**][xml-transform-doc] | [![API simgesi][xml-validate-icon]<br/>**XML <br/>doğrulama**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Kurumsal bağlayıcılar

Logic apps, Kurumsal sistemleri, SAP ve IBM MQ gibi erişebilirsiniz:

|   |   |   | 
|---|---|---| 
| [![API simgesi][ibm-3270-icon]<br/>**IBM 3270**][ibm-3270-doc] | [![API simgesi][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![API simgesi][sap-icon]<br/>**SAP**][sap-connector-doc] |
|||| 

<a name="triggers-actions"></a>

## <a name="triggers-and-actions---more-info"></a>Tetikleyiciler ve Eylemler - daha fazla bilgi

Bağlayıcılar sağlayabilir *Tetikleyicileri*, *eylemleri*, veya her ikisini de. A *tetikleyici* genellikle tetikleyici ve çalışan mantıksal uygulamanızı başlatan olay belirtme, herhangi bir mantıksal uygulama ilk adımdır. Örneğin, FTP Bağlayıcısı "bir dosya değiştirildiğinde veya eklendiğinde" mantıksal uygulamanızı başlatan bir tetikleyici vardır. Bazı tetikleyiciler belirtilen olay veya verileri düzenli aralıklarla denetleyin ve sonra belirtilen olay veya veri algıladığınızda yangın. Diğer Tetikleyiciler bekleyin, ancak zaman belirli bir olay meydana instantly veya yeni veriler kullanılabilir olduğunda. Tetikleyiciler, ayrıca mantıksal uygulamanız tüm gerekli veri geçirin. Mantıksal uygulamanızı okuyabilir ve iş akışı genelinde bu verileri kullanın.
Örneğin, bir tetikleyici Twitter Bağlayıcısı sahip "Yeni bir tweet, geçen bir tweet gönderildiğinde" mantıksal uygulamanızın iş akışınıza içerik. 

Azure Logic Apps bir tetikleyici sonra mantıksal uygulamanızın bir örneğini oluşturur ve çalışmaya başladıktan *eylemleri* mantıksal uygulamanızın iş akışında. Tetikleyici izleyin ve mantıksal uygulamanızın iş akışında görevlerini gerçekleştirme adımları eylemlerdir. Örneğin, müşteri verilerini bir SQL veritabanı'ndan alır bir mantıksal uygulama oluşturma ve sonraki eylemlerde bu verisi işleme. 

Azure Logic Apps sağlayan Tetikleyicileri genel türleri şunlardır:

* *Yinelenme tetikleyicisini*: Bu tetikleyici, belirtilen bir zamanlamaya göre çalışır ve belirli bir hizmet veya sistem ile sıkı bir şekilde ilişkili değildir.

* *Yoklama tetikleyici*: Bu tetikleyici, belirli bir hizmet veya sistem olup belirli bir olayın veya yeni veriler için denetleme belirli bir zamanlamaya göre düzenli aralıklarla yoklar. Yeni veriler kullanılabilir veya belirli bir olayın gerçekleştiği, tetikleyici oluşturur ve giriş olarak geçirilen verileri hemen kullanabileceğiniz mantıksal uygulamanızın yeni bir örneğini çalıştırır.

* *Tetikleyici anında iletme*: Bu tetikleyiciyi bekler ve bir olayın meydana gelmesine veya yeni veriler için dinler. Yeni veriler kullanılabilir olduğunda veya olay meydana geldiğinde, tetikleyici oluşturur ve giriş olarak geçirilen verileri hemen kullanabileceğiniz mantıksal uygulamanızın yeni bir örneğini çalıştırır.

<a name="custom"></a>

## <a name="connector-configuration"></a>Bağlayıcı yapılandırması

Her bağlayıcı'nın tetikleyiciler ve Eylemler kendi özelliklerini yapılandırmanıza olanak sağlar. Çok sayıda bağlayıcı ayrıca ilk oluşturmanızı gerektirir bir *bağlantı* hedef hizmet veya sistem için ve mantıksal uygulamanızda bir tetikleyici veya eylemi kullanmadan önce kimlik doğrulama bilgilerini veya diğer yapılandırma ayrıntılarını sağlayın. Örneğin, bir Twitter hesabıyla verilerine erişmek için veya sizin adınıza gönderilecek bir bağlantı yetkilendirmeniz gerekir. 

Bağlantı oluşturma OAuth kullanan bağlayıcıları için Office 365, Salesforce veya erişim belirtecinizin burada şifrelenir ve güvenli bir şekilde Azure bir gizli dizi deposu içinde depolanan GitHub gibi hizmete imzalama anlamına gelir. FTP ve SQL gibi diğer bağlayıcıları sunucu adresi, kullanıcı adı ve parola gibi yapılandırma ayrıntılarını içeren bir bağlantı gerektirir. Bu bağlantı yapılandırma ayrıntılarını da şifrelenir ve güvenli bir şekilde depolanır. 

Bu hizmet veya sistem izin verdiği sürece bağlantıları hedef hizmet veya sistem için erişebilirsiniz. Office 365 ve Dynamics gibi Azure Active Directory (AD) OAuth bağlantıları kullanan Hizmetleri için erişim belirteçleri Azure Logic Apps süresiz olarak yeniler. Diğer hizmetleri Azure Logic Apps bir belirteç yenileme olmadan ne kadar süreyle kullanabileceğiniz sınırları olabilir. Genel olarak, bazı eylemler parolanızı değiştirme gibi tüm erişim belirteçlerini geçersiz kılar.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Özel API'ler ve Bağlayıcılarla

Özel kod çalıştıran veya bağlayıcı olarak kullanılamayan API'leri çağırmak için Logic Apps platformunu genişletebilirsiniz [özel API Apps oluşturarak](../logic-apps/logic-apps-create-api-app.md). Ayrıca [özel bağlayıcılar oluşturma](../logic-apps/custom-connector-overview.md) için *herhangi* REST veya bu API'lerle Azure aboneliğinizdeki herhangi bir mantıksal uygulama için kullanılabilir hale getirmek SOAP tabanlı API'ler.
Özel API Apps veya bağlayıcıları herkesin Azure'da kullanması genel hale getirmek için [bağlayıcıları Microsoft sertifikası için gönderme](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Gönderin veya Azure Logic Apps ve bağlayıcıları için fikirleri oylamak için şurayı ziyaret edin [Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* Bulma [bağlayıcılar tam listesi](https://docs.microsoft.com/connectors)
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Logic apps için özel bağlayıcılar oluşturma](https://docs.microsoft.com/connectors/custom-connectors/)
* [Mantıksal uygulamalar için özel API’ler oluşturma](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Şirket içi veri ağ geçidiyle mantıksal uygulamalardan şirket içi veri kaynaklarına bağlanın"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Mantıksal uygulamaları Azure İşlevleri ile tümleştirin"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Apı'lerinizi yayımlama ve yönetme için bir Azure API Management hizmet örneği oluşturma"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Mantıksal uygulamaları App Service API Apps ile tümleştirin"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Service Bus Kuyruklarından ve Konu Başlıklarından iletiler gönderin ve Service Bus Kuyrukları ile Aboneliklerinden iletiler alın"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Gruplar veya toplu olarak iletilerini işleme"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Bir koşulu değerlendirmek ve bağlı çalışma farklı eylemlerin koşul true veya false"
[delay-doc]: ./connectors-native-delay.md "Gecikmeli eylemleri gerçekleştirin"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Bir dizideki her öğe aynı eylemleri gerçekleştirin"
[http-doc]: ./connectors-native-http.md "HTTP bağlayıcısı ile HTTP aramaları yapın"
[http-request-doc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-response-doc]: ./connectors-native-reqres.md "HTTP istekleri ve mantıksal uygulama yanıtları için eylemler ekleyin"
[http-swagger-doc]: ./connectors-native-http-swagger.md "HTTP + Swagger bağlayıcısı ile HTTP çağrıları yapın"
[http-webook-doc]: ./connectors-native-webhook.md "Logic apps için HTTP Web kancası eylemleri ve tetikleyiciler ekleyin"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Mantıksal uygulamaları iç içe geçmiş iş akışları ile tümleştirin"
[query-doc]: ./connectors-native-query.md "Sorgu eylemi ile dizileri seçip filtreleyin"
[recurrence-doc]:  ./connectors-native-recurrence.md "Mantıksal uygulamalar için yinelenen eylemleri tetikleyin"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Eylem grubu eylemleri çalıştırma işlemini tamamladıktan sonra kendi durumunu Al gruplar halinde düzenleyin" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Benzersiz değerler atanır durumlarda eylemlere düzenleyin. Yalnızca bir ifade, nesne veya belirteç sonuç değeri eşleşen servis talebi çalıştırın. Herhangi bir eşleşme varsa, varsayılan çalışmasını çalıştırın"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Durdurma veya etkin olarak çalışan bir iş akışı mantıksal uygulamanız için İptal Et"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Eylemler belirtilen koşulun true olması veya bazı durumu değişti kadar yineleyin."
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Veri işlemleri gibi diziler filtreleme veya CSV ve HTML tabloları oluşturma"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Başlatma, set, artırma, azaltma, gibi değişkenleri ile işlemlerini gerçekleştirmek ve dize veya dizi değişkenine Ekle"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Blob kapsayıcınızdaki dosyaları Azure blob depolama bağlayıcısı ile yönetin"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md " Olayları bir Event Grid tarafından Örneğin, Azure veya üçüncü taraf kaynaklar değiştirdiğinizde yayımlanan izleyin"
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
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Google e-tablolarınızı değiştirebilmesi bağlanma"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Görevlerinizi yönetebilmek için Google Görevler’e bağlanın"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "IBM ana bilgisayarları üzerinde 3270 uygulamalara bağlanma"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Bulutta veya şirket içinde IBM DB2’ye bağlanın. Bir satırı güncelleştirin, bir tabloyu alın ve diğer işlemleri yapın"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Bulutta veya şirket içinde Informix’e bağlanın. Bir satırı okuyun, tabloları listeleyin ve daha fazlasını yapın"
[ibm-mq-doc]: ./connectors-create-api-mq.md "IBM MQ şirket içi ya da ileti göndermek ve almak için Azure"
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
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "SendGrid’e bağlanın. E-posta gönderin ve alıcı listelerini yönetin"
[sftp-doc]: ./connectors-create-api-sftp.md "SFTP hesabınıza bağlanın. Dosyalarınızı karşıya yükleyin, alın, silin ve diğer işlemleri yapın"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Şirket içi SharePoint sunucusuna bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "SharePoint Online’a bağlanın. Belgeleri yönetin, öğeleri listeleyin ve daha fazlasını yapın"
[slack-doc]: ./connectors-create-api-slack.md "Slack’e bağlanın ve Slack kanallarında iletiler yayınlayın"
[smtp-doc]: ./connectors-create-api-smtp.md "Bir SMTP sunucusuna bağlanın ve ekler içeren e-posta Gönder"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "İletişim için SparkPost’a bağlanın"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Azure SQL veritabanı veya SQL sunucusuna bağlanın. SQL veritabanı tablosunda girdiler oluşturun, güncelleştirin, alın ve silin."
[trello-doc]: ./connectors-create-api-trello.md "Trello’ya bağlanın. Projelerinizi yönetin ve herhangi bir şeyi herhangi bir kimseyle düzenleyin"
[twilio-doc]: ./connectors-create-api-twilio.md "Twilio’ya bağlanın. İletiler gönderip alın, mevcut numaraları alın, gelen telefon numaralarını yönetin ve daha fazlasını yapın"
[twitter-doc]: ./connectors-create-api-twitter.md "Twitter’a bağlanın. Zaman çizelgelerini alın, tweetler gönderin ve daha fazlasını yapın"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Wunderlist’e bağlanın. Görevlerinizi ve yapılacaklar listenizi yönetin, tüm işlerinizi eşitleyin ve daha fazlasını yapın"
[yammer-doc]: ./connectors-create-api-yammer.md "Yammer’a bağlanın. İleti gönderin, yeni iletiler alın ve daha fazlasını yapın"
[youtube-doc]: ./connectors-create-api-youtube.md "YouTube’a bağlanın. Videolarınızı ve kanallarınızı yönetin"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Enterprise integration AS2 hakkında bilgi edinin."
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
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
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
