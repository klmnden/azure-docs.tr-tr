<properties 
    pageTitle="Logic Apps nedir?" 
    description="App Service Logic Apps hakkında daha fazla bilgi edinin" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="04/07/2016"
    ms.author="klam"/>

#Logic Apps nedir?

| Hızlı Başvuru |
| --------------- |
| [Logic Apps Tanımlama Dili](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Logic Apps Bağlayıcı Belgeleri](../connectors/apis-list.md) |
| [Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Azure App Service, geliştiriciler için web, mobil ve tümleştirme uygulamalarını derlemeyi kolaylaştıran, tam yönetilen bir PaaS (Hizmet Olarak Platform) örneğidir. Logic Apps bu paketin bir parçasıdır ve teknik kullanıcıların ve geliştiricilerin, kullanımı kolay bir görsel tasarımcı kullanarak iş süreçlerini yürütmesini ve iş akışını otomatikleştirmesini sağlar.

En önemlisi, Logic Apps beceri gerektiren tümleştirme senaryolarını bile çözmeye yardımcı olan yerleşik [Yönetilen API’ler][managedapis] ile birlikte kullanılabilir: 

![Akış uygulama tasarımcısı](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Daha önce bahsedildiği gibi mantıksal uygulamalarla iş süreçlerini otomatik hale getirebilirsiniz. Aşağıda birkaç örnek verilmiştir:  
 
* SQL veritabanınızdaki yeni kayıtları otomatik olarak çoğaltabilir ve ardından danışmaya e-posta gönderebilirsiniz.   
* Olumsuz tweet’leri otomatik olarak bulun ve bir slack kanalına gönderin.

Bunlar gibi senaryoların tümü görsel tasarımcıdan ve tek bir satır kod yazmadan yapılandırılabilir. [Mantıksal uygulamanızı derlemeye][oluşturmaya] hemen başlayın.

## Logic Apps neden kullanılmalıdır?

Logic Apps, geliştiricilerin bir tetikleyiciden başlayıp bir dizi adım yürüten iş akışları tasarlamasına imkan tanır. Her adım kimlik doğrulaması ve denetim noktası ile dayanıklı yürütme gibi en iyi yöntemleri gerçekleştirirken bir API çağırır.

Herhangi bir iş sürecini otomatik hale getirmek isterseniz (ör. olumsuz tweet’leri bulup iç Slack kanalınıza göndermek veya SQL’deki yeni müşteri kayıtlarını geldikleri anda CRM sisteminize çoğaltmak) Logic Apps farklı veri kaynaklarını buluttan şirket içine tümleştirmeyi kolaylaştırır. Daha fazla fikir için [yönetilen API’ler][managedapis] bölümüne ve neler yapabileceğinizi görmek için [başlarken][oluşturmaya] bölümüne bakın. 

Ayrıca, [BizTalk yönetilen API’ler][biztalk] ile [kurallar altyapısı][rules], [ticari ortak yönetimi][tpm] ve daha fazlasının gücü sayesinde olgun tümleştirme senaryolarına geçiş yapabilirsiniz.

- **Kullanımı kolay tasarım araçları** - Logic Apps tarayıcıda uçtan uca tasarlanabilir. Basit bir zamanlamadan şirketiniz hakkında bir tweet’in göründüğü her duruma kadar değişen bir tetikleyici ile başlayın. Ardından zengin bağlayıcı galerisini kullanarak dilediğiniz sayıda eylemi düzenleyin.

- **SaaS’ı kolayca oluşturun** - Açıklanması kolay olan oluşturma görevlerinin bile koda uygulanması zordur. Logic Apps farklı sistemleri birbirine bağlamayı kolaylaştırır. CRM yazılımınızda Facebook ya da Twitter hesaplarınızdaki etkinliği temel alan bir görev mi oluşturmak istiyorsunuz? Bulut pazarlama çözümünüzü şirket içi faturalama sisteminize bağlamak mı istiyorsunuz? Logic Apps bu sorunlara çözüm getirmenin en hızlı ve en güvenilir yoludur.

- **Şablonlardan hızlıca başlayın** - Başlamanıza yardımcı olmak üzere bazı yaygın çözümleri hızlıca oluşturmanıza imkan tanıyan bir [şablon galerisi][templates] sunulmuştur. Galeri, Gelişmiş BizTalk çözümlerinden basit SaaS bağlantısına ve hatta 'eğlencelik' olan birkaç özelliğe kadar her şey için, Logic Apps’in gücünü anlamanın en hızlı yoludur.

- **Desteklenmiş genişletilebilirlik** - İhtiyaç duyduğunuz API’yi görmüyor musunuz? Logic Apps, API uygulamalarıyla çalışacak şekilde tasarlanmıştır; özel bir API kullanmak üzere kendi API uygulamanızı kolayca oluşturabilirsiniz. Yalnızca sizin için yeni bir uygulama oluşturun veya markette paylaşıp para kazanın.

- **Gerçek tümleştirme gücü** - Basitten başlayın ve gerektikçe büyütün. Logic Apps, profesyonellerin ihtiyaç duydukları çözümleri oluşturmasını sağlayan sektör lideri Microsoft çözümü BizTalk’tan kolayca yararlanır. [Logic Apps ile sağlanan BizTalk özellikleri][biztalk] hakkında daha fazla bilgi edinin.

## Mantıksal Uygulama Kavramları

Logic Apps deneyimini oluşturan bazı temel parçalar aşağıda verilmiştir. 

- **İş Akışı** - MLogic Apps ş süreçlerinizi bir dizi adım veya bir iş akışı olarak modellemenizi sağlayan grafiksel bir yöntem sunar.
- **Yönetilen API’ler** - Mantıksal uygulamalarınızın veri ve hizmetlere erişmesi gerekir. Yönetilen API'ler, verilerinize bağlanırken ve verilerinizle çalışırken size yardımcı olmak üzere özel olarak oluşturulur. Şu anda kullanılabilen API’lerin listesi için bkz. [yönetilen API’ler][managedapis].
- **Tetikleyiciler** - Bazı Yönetilen API’ler tetikleyici olarak da hareket edebilir. Tetikleyici bir e-postanın gelmesi veya Azure Storage hesabınızdaki bir değişiklik gibi belirli bir olaya göre bir iş akışının yeni bir örneğini başlatır.
-  **Eylemler** - Bir iş akışında tetikleyiciden sonraki her adım eylem olarak adlandırılır. Her eylem genellikle yönetilen veya özel API uygulamalarınızdaki bir işlemle eşlenir.
- **BizTalk** - Daha gelişmiş tümleştirme senaryoları için Logic Apps, Biztalk özelliklerini içerir. Biztalk, Microsoft'un sektör lideri tümleştirme platformudur. BizTalk API’si Mantıksal Uygulama iş akışlarınıza doğrulama, dönüşüm, kurallar ve daha fazlasını kolayca dahil etmenize imkan tanır. [BizTalk API uygulamaları nelerdir][biztalk] içinde daha fazla bilgi bulabilirsiniz.

## Başlarken  

 - Logic Apps’i kullanmaya başlamak için [Logic App oluşturma][oluşturmaya] öğreticisini izleyin.  
 - [Yayın örnekleri ve senaryoları görüntüleyin](app-service-logic-examples-and-scenarios.md)
 - [Logic Apps ile iş süreçlerini otomatik hale getirebilirsiniz](http://channel9.msdn.com/Events/Build/2016/T694) 
 - [Sistemlerinizi Logic Apps ile nasıl tümleştireceğinizi öğrenin](http://channel9.msdn.com/Events/Build/2016/P462)
- Azure App Service platformu hakkında daha fazla bilgi için bkz. [Azure App Service][appservice].

[biztalk]: app-service-logic-what-are-biztalk-api-apps.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[oluşturmaya]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-create-a-trading-partner-agreement.md
[rules]: app-service-logic-use-biztalk-rules.md
[templates]: app-service-logic-use-logic-app-templates.md



<!---HONumber=Jun16_HO2-->


