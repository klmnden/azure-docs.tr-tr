---
title: Uygulamaları, BizTalk Services Azure Logic Apps'e taşıyın. | Microsoft Docs
description: Azure BizTalk Services (MABS) Azure Logic Apps'e geçirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jonfancey
ms.author: jonfan
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 05/30/2017
ms.openlocfilehash: f813cb5d8d5c442fc17f126c3a2ff6de7b0bdde1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61323099"
---
# <a name="migrate-from-biztalk-services-to-azure-logic-apps"></a>BizTalk Services'tan Azure Logic Apps'e geçiş

Microsoft Azure BizTalk Services (MABS) devre dışı bırakılıyor. MABS tümleştirme çözümlerinizi taşımak [Azure Logic Apps](../logic-apps/logic-apps-overview.md), bu makaledeki yönergeleri izleyin. 

## <a name="introduction"></a>Giriş

BizTalk Services'ın iki alt Servisleri oluşur:

* Microsoft BizTalk Hizmetleri karma bağlantılar
* EAI ve EDI köprüye dayalı tümleştirme

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) BizTalk Hizmetleri karma bağlantılar değiştirir. Azure karma bağlantılar, Azure portalı üzerinden Azure App Service ile kullanılabilir. Mevcut BizTalk Hizmetleri karma bağlantılar ve ayrıca, portalda oluşturduğunuz yeni karma bağlantılar yönetebilmesi bu hizmet bir karma Bağlantı Yöneticisi'ni sağlar. 

[Logic Apps](../logic-apps/logic-apps-overview.md) EAI ve EDI köprüye dayalı tümleştirme BizTalk Hizmetleri ve diğer tüm aynı özellikleri değiştirir. Bu hizmet, bulut ölçeğinde tüketim temelli iş akışı ve düzenleme özellikleri, bir tarayıcı aracılığıyla veya Visual Studio ile karmaşık tümleştirme çözümlerini hızla ve kolayca oluşturmanızı sağlar.

Bu tablo, Logic Apps için BizTalk Services'ın özellikleri eşler.

| BizTalk Services   | Logic Apps            | Amaç                      |
| ------------------ | --------------------- | ---------------------------- |
| Bağlayıcı          | Bağlayıcı             | Veri göndermek ve almak   |
| Köprü             | Logic App             | İşlem hattı işlemci           |
| Aşama doğrula     | XML doğrulaması eylemi | Bir XML belgesi bir şemaya karşı doğrulama | 
| Aşama zenginleştirin       | Veri belirteçleri           | İletilere veya yönlendirme kararları için özelliklerini Yükselt |
| Aşama dönüştürme    | Eylem dönüştürme      | XML iletileri bir biçimden diğerine dönüştürme |
| Aşama kodunu çözme       | Düz dosya kodunu çözme eylemi | Düz dosyadan XML biçimine Dönüştür |
| Aşama kodlayın       | Düz dosya kodlama eylemi | XML'den dönüştürmek için düz dosya |
| İleti denetçisi  | Azure işlevleri veya API uygulamaları | İçinde tümleştirmeler özel kod çalıştırma |
| Yönlendirme eylemini       | Koşul veya anahtar | Belirtilen bağlayıcıların birine yönlendirme iletileri |
|||| 

## <a name="biztalk-services-artifacts"></a>BizTalk Hizmetleri yapıtları

BizTalk Services çeşitli yapıtlar vardır. 

## <a name="connectors"></a>Bağlayıcılar

BizTalk Hizmetleri bağlayıcılar, istek/yanıt HTTP tabanlı etkileşimler sağlayan iki yönlü köprüleri dahil olmak üzere, veri göndermek ve almak köprüleri yardımcı olur. Logic Apps kullandığı aynı terminoloji ve çok çeşitli teknolojiler ve hizmetler için bağlanarak aynı amaca hizmet 180 fazla bağlayıcıya sahiptir. Örneğin, bağlayıcılar SaaS bulut için kullanılabilir ve PaaS Hizmetleri, OneDrive, Office 365, Dynamics CRM ve diğer gibi yanı sıra şirket içi sistemler ile BizTalk hizmetlerini BizTalk bağdaştırıcı hizmeti değiştirir On-Premises Data Gateway. BizTalk Services kaynakları, FTP, SFTP ve Service Bus kuyruğu veya konuyu aboneliğine sınırlıdır.

![](media/logic-apps-move-from-mabs/sources.png)

Varsayılan olarak, her köprüsü çalışma zamanı adresi ve köprüyü göreli adres özelliklerini ile yapılandırılmış bir HTTP uç noktası vardır. Logic Apps ile aynı sonuçları elde etmek için kullanın [istek ve yanıt](../connectors/connectors-native-reqres.md) eylemler.

## <a name="xml-processing-and-bridges"></a>XML işleme ve köprüleri

BizTalk Hizmetleri'nde bir köprü işleme ardışık düzenine benzer. Bir köprü Bağlayıcısı'ndan alınan veriler alabilir, bazı verilerle çalışmak ve sonuçları başka bir sisteme gönderir. Logic Apps, BizTalk Hizmetleri olarak aynı işlem hattı tabanlı etkileşim desenleri destekleyen ve ayrıca diğer tümleştirme düzenleri sağlayarak aynı yapar. [XML istek-yanıt köprüsü](https://msdn.microsoft.com/library/azure/hh689781.aspx) BizTalk Hizmetleri bu görevleri gerçekleştirmek aşamalardan oluşan bir VETER işlem hattı bilinir:

* (V) doğrulama
* (E) zenginleştirin
* (T) dönüştürme
* (E) zenginleştirin
* (R) rota

Bu görüntü nasıl işleme istek ve istek üzerine denetim sağlayan yanıt arasında bölünür ve yanıt yolları ayrı olarak, örneğin, farklı kullanarak eşler için her bir yol gösterir:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Ayrıca, tek yönlü köprüsü XML işleme başlangıç ve sonunda kod çözme ve kodla aşamaları ekler. Doğrudan köprüsü, tek bir zenginleştirin aşamasını içerir.

### <a name="message-processing-decoding-and-encoding"></a>İleti işleme, kod çözme ve kodlama

BizTalk Services hizmetinde farklı türde XML iletileri almak ve alınan ileti için eşleşen şema belirleme. Bu iş içinde gerçekleştirilen *ileti türlerini* alma işleme ardışık düzen aşaması. Kod çözme aşama, algılanan ileti türü ardından sağlanan şema kullanarak iletisinin kodunu çözün için kullanır. Şema düz dosya şemasının ise, bu aşama, gelen düz dosya XML biçimine dönüştürür. 

Logic Apps benzer yetenekler sağlar. (Dosya sistemi, FTP, HTTP ve benzeri) farklı bağlayıcı Tetikleyicileri kullanarak farklı protokoller üzerinden düz dosya alma ve kullanma [düz dosya kodunu çözme](../logic-apps/logic-apps-enterprise-integration-flatfile.md) XML gelen verileri dönüştürmek için eylem. Logic Apps herhangi bir değişiklik yapmadan doğrudan, mevcut bir düz dosya şemasına taşıyın ve şemaları tümleştirme hesabınıza yükleyin.

### <a name="validation"></a>Doğrulama

Gelen veri dönüştürüldükten sonra XML (veya XML ileti biçimi alındı olup olmadığını), doğrulama iletisi, XSD şeması uyar, belirlemek için çalışır. Logic Apps'te bu görevi gerçekleştirmek için [XML doğrulama](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) eylem. BizTalk Hizmetleri aynı şemalardan herhangi bir değişiklik yapmadan kullanabilirsiniz.

### <a name="transform-messages"></a>İletileri dönüştürme

BizTalk Services hizmetinde dönüştürme aşaması bir XML tabanlı ileti biçimi diğerine dönüştürür. Bu iş TRFM tabanlı Eşleyici kullanarak bir harita, uygulama tarafından gerçekleştirilir. Logic Apps'te işlemi benzer. Dönüştürme eylemi bir harita tümleştirme hesabınızdan yürütür. İkisi arasındaki temel fark haritalar Logic apps'teki XSLT biçiminde olmasıdır. XSLT, İşlevsiler bulunduğu için BizTalk Server oluşturulan eşlemeleri de dahil olmak üzere zaten var olan XSLT yeniden kullanabilme içerir. 

### <a name="routing-rules"></a>Yönlendirme kuralları

BizTalk Hizmetleri, gelen iletileri veya veri göndermek için hangi uç nokta veya Bağlayıcısı yönlendirme karar verir. Önceden yapılandırılmış uç noktalarından seçebilme yönlendirme filtre seçeneğini kullanarak mümkündür:

![](media/logic-apps-move-from-mabs/route-filter.png)

BizTalk Services hizmetinde, yalnızca iki seçenek varsa, kullanarak bir *koşul* BizTalk Hizmetleri, yönlendirme filtreleri dönüştürmek için en iyi yoludur. Varsa ikiden, ardından kullanmak bir **geçiş**.

Logic Apps, Gelişmiş mantık özellikleri yanı sıra gelişmiş denetim akışı ve ile yönlendirme sağlar [koşullu deyimler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [switch ifadeleri](../logic-apps/logic-apps-control-flow-switch-statement.md).

### <a name="enrich"></a>Enrich

BizTalk Hizmetleri işlemede zenginleştirin aşama alınan verilerle ilişkili ileti bağlamı özellikler ekler. Örneğin, bir veritabanı araması veya bir XPath ifadesi kullanarak bir değer ayıklama yönlendirme için kullanılacak bir özellik yükseltiliyor. Logic Apps, Eylemler aynı davranışı çoğaltmak basit hale getirme, önceki gelen tüm bağlamsal veriler çıktıları erişim sağlar. Örnek olarak, `Get Row` SQL bağlantı eylemi, bir SQL Server veritabanından veri döndürmek ve yönlendirme için verileri bir karar eylemini kullanın. Benzer şekilde, Service Bus gelen özellikleri bir tetikleyici tarafından sıraya alınan iletileri olan adreslenebilir yanı sıra iş akışı tanımı dil xpath ifadesi kullanarak XPath.

### <a name="run-custom-code"></a>Özel kod çalıştırma

BizTalk Services sayesinde [özel kod çalıştıran](https://msdn.microsoft.com/library/azure/dn232389.aspx) kendi derlemeler yüklenir. Bu işlev tarafından uygulanan [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) arabirimi. Köprü her aşamasında bu arabirimi uygulayan oluşturduğunuz .NET türü sağlayan iki özellikleri (üzerinde denetçisi girin ve çıkış Inspector'ı üzerinde) içerir. Özel kod daha karmaşık bir işlem veri gerçekleştirmenize olanak tanıyan ve ortak iş mantığını gerçekleştirebilirsiniz derlemelerde mevcut kodu yeniden olanak tanır. 

Logic Apps özel kod yürütmek için iki birincil yöntem sağlar: Azure işlevleri ve API Apps. Azure işlevleri oluşturulur ve logic apps'ten çağrılır. Bkz: [ekleme ve çalıştırma, Azure işlevleri ile logic apps için özel kod](../logic-apps/logic-apps-azure-functions.md). API Apps, Azure App Service'in bir parçası, kendi Tetikleyicileri ve eylemleri oluşturmak için kullanın. Daha fazla bilgi edinin [Logic Apps ile kullanılacak özel bir API oluşturma](../logic-apps/logic-apps-create-api-app.md). 

BizTalk Services'ı çağırın derlemelerde özel kodunuz varsa, Azure işlevleri için bu kodu Taşı veya ne uyguladığınız bağlı olarak, API Apps, özel API'ler oluşturma. Örneğin, Logic Apps bağlayıcı yok başka bir hizmete sarmalar kodunuz varsa, ardından API uygulaması oluşturma ve mantıksal uygulamanızın içinde API uygulamanızı sağlar eylemlerini kullanın. Ardından Azure işlevleri, büyük olasılıkla yardımcı işlevleri veya kitaplıkları varsa, en uygun olacaktır.

### <a name="edi-processing-and-trading-partner-management"></a>EDI işleme ve ticari ortak Yönetimi

BizTalk Hizmetleri ve Logic Apps dahil EDI ve B2B işleme desteğiyle AS2 (Uygulanabilirlik deyimi 2) X12 ve EDIFACT. BizTalk Hizmetleri, EDI köprüleri oluşturup oluşturmak veya ticari iş ortaklarının ve sözleşmelerin adanmış izleme ve Yönetim Portalı'nda yönetin.
Bu işlev, Logic Apps'te alabilirsiniz [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md). EIP sağlar [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve B2B eylemlerinin EDI ve B2B işleme. Tümleştirme hesabı oluşturmak ve yönetmek için de [ortaklar](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [sözleşmeleri](../logic-apps/logic-apps-enterprise-integration-agreements.md). Tümleştirme hesabı oluşturduktan sonra bir veya daha fazla mantıksal uygulamalar hesabınıza bağlayabilirsiniz. B2B eylemlerinin sonra mantıksal uygulamanızdan ticaret iş ortağı bilgilerine erişmek için de kullanabilirsiniz. Aşağıdaki eylemleri sağlanır:

* AS2 kodlama
* AS2 kod çözme
* X12 kodlayın
* X12 kodunu çözme
* EDIFACT kodlama
* EDIFACT kodunu çözme

BizTalk Services'ın bu Eylemler, aktarım protokolleri arasından birbirinden ayrılmıştır. Logic apps'ı oluşturduğunuzda, hangi bağlayıcı daha fazla esnekliğe sahip veri göndermek ve almak için kullanın. Örneğin, alabileceğiniz X12 dosyaları e-posta ve sonra işlem ek olarak bir mantıksal uygulama içinde bu dosyaları. 

## <a name="manage-and-monitor"></a>Yönetme ve izleme

BizTalk Services hizmetinde, adanmış bir portal izlemek ve sorunlarını gidermek için izleme özellikleri sağlanan. Daha zengin izleme ve izleme özellikleri ile Logic Apps sağlar [Azure portalında](../logic-apps/logic-apps-monitor-your-logic-apps.md)ve hareket ederken şeylere göz önünde tutmak için bir mobil uygulama içerir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Kullanılabilirlik düzeyi yüksek (HA) BizTalk Services, belirli bir bölgede birden fazla örneği'ni kullanarak işlem yükü paylaşabilirsiniz. Logic Apps, ek ücret ödemeden bölge HA sağlar. 

BizTalk Hizmetleri'nde B2B işleme için bölge kullanıma olağanüstü durum kurtarma bir yedekleme ve geri yükleme işlemi gerektirir. İş sürekliliği için Logic Apps, bölgeler arası Aktif/Pasif sağlar [DR özelliği](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md), olanak sağlayan tümleştirme hesapları farklı bölgelerde arasında B2B verileri eşitleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps nedir?](../logic-apps/logic-apps-overview.md)
* [İlk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [önceden oluşturulmuş bir şablon](../logic-apps/logic-apps-create-logic-apps-from-templates.md) kullanarak hızlı şekilde çalışmaya başlayın  
* [Tüm bağlayıcıları görüntüleyin](../connectors/apis-list.md) logic apps'te kullanabileceğiniz