<properties 
    pageTitle="Bağlayıcılar ve BizTalk API Apps nedir?" 
    description="API Apps, Bağlayıcılar ve BizTalk API Uygulamaları hakkında bilgi edinin" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/01/2016" 
    ms.author="mandia"/>


# Bağlayıcılar ve BizTalk API Apps nedir?

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


*Bağlayıcı*, bağlantı üzerine odaklanan bir API uygulaması türüdür. Bağlayıcılar da diğer tüm API uygulamaları gibi Web Apps, Mobile Apps ve Logic Apps hizmetlerinde kullanılır. Bağlayıcılar mevcut hizmetlere bağlanmayı kolaylaştırır ve kimlik doğrulamayı yönetmeye yardımcı olur, izleme, analiz ve daha fazlasını sağlar.

Herhangi bir geliştirici, kendi API uygulamalarını oluşturabilir ve özel olarak dağıtabilir. Gelecekte, geliştiriciler özel olarak oluşturdukları API Uygulamalarını market üzerinden paylaşıp para kazanabilecektir. 

![API Uygulamaları Marketi](./media/app-service-logic-what-are-biztalk-api-apps/Marketplace.png)

Geliştiricilerin çözümler oluşturmasını hızlandırmak amacıyla Azure ekibi, markete çok sayıda yaygın senaryoyu karşılayan birkaç bağlayıcı eklemiştir. Ayrıca, karmaşık ve gelişmiş tümleştirme senaryolarına ulaşımı artırmak için birkaç Premium ve BizTalk özelliği de mevcuttur.

Farklı Hizmet "Katmanları" vardır. Tüm Katmanlar tam işlevleriyle birlikte bütün bağlayıcıları ve API uygulamalarını içerir.  

[App Service Fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/) bu Hizmet Katmanlarını açıklar ve ayrıca bu katmanların içindekileri listeler. Aşağıdaki bölümlerde BizTalk API Uygulamaları ve Bağlayıcıların çeşitli kategorileri açıklanmaktadır.


## Karma Bağlayıcılar 
Karma bağlayıcılar [DB2](app-service-logic-connector-db2.md), [Informix](app-service-logic-connector-informix.md) ve WebSphere MQ bağlantısı ile kuruluş içinde kapsamı artırır. 

## EAI ve EDI Hizmetleri
Kritik iş uygulamalarının oluşturulması bağlantıdan fazlasını gerektirir. Microsoft'un sektör lideri tümleştirme platformu BizTalk Server’ın altyapısını temel alan BizTalk API Uygulamaları; Web, Mobil ve Logic Uygulamalarına kolayca eklenebilen gelişmiş tümleştirme özellikleri sağlar. Bu tümleştirme özelliklerinden bazıları [Doğrulama](app-service-logic-xml-validator.md), [Ayıklama](app-service-logic-xpath-extract.md), [Dönüşüm](app-service-logic-transform-xml-documents.md), [Kodlayıcılar](app-service-logic-connector-jsonencoder.md), [Ticari Ortak Yönetimi](app-service-logic-connector-tpm.md) ve [X12](app-service-logic-connector-x12.md), [EDIFACT](app-service-logic-connector-edifact.md) ile [AS2](app-service-logic-connector-as2.md) gibi EDI biçimleridir.

Ek kaynaklar: [İşletmeler arası bağlayıcılar ve API uygulamaları](app-service-logic-b2b-connectors.md)  
[B2B işlemi oluşturma](app-service-logic-create-a-b2b-process.md)  
[Ticari ortak sözleşmesi oluşturma](app-service-logic-create-a-trading-partner-agreement.md)  
[B2B iletilerinizi izleme](app-service-logic-track-b2b-messages.md)  


## Kurallar
İş kuralları, iş süreçlerini denetleyen ilk ve kararları kapsar. Genellikle, kurallar dinamiktir ve iş planları, düzenlemeler ve diğer birçok nedenden dolayı zaman içinde değişir. [BizTalk Kuralları](app-service-logic-use-biztalk-rules.md) bu ilkeleri uygulama kodunuzdan ayırmanıza ve değişiklik işlemini daha kolay ve hızlı hale getirmenize imkan tanır.

## Bağlayıcı ve API uygulamaları listesi
Standart Bağlayıcılar, BizTalk EAI, Premium Bağlayıcılar gibi her bir kategoride yer alan bağlayıcılar ve API Uygulamalarının tam listesi için bkz. [Bağlayıcılar ve API Uygulamaları Listesi](app-service-logic-connectors-list.md).
 



<!--HONumber=Sep16_HO3-->


