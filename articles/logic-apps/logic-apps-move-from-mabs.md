---
title: "Uygulamaları Azure mantıksal uygulamaları için BizTalk Services taşıma | Microsoft Docs"
description: "Taşıma veya Azure mantıksal uygulamaları için Azure BizTalk Services (MABS) geçirme"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: jonfan; LADocs
ms.openlocfilehash: 6e00e62e60c059a16731a77e529b4b93f50802e9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="move-from-biztalk-services-to-azure-logic-apps"></a>BizTalk Services Azure mantıksal uygulamaları taşıma

Microsoft Azure BizTalk Services'ı (MABS) devre dışı bırakılıyor. MABS tümleştirme çözümlerinizi taşımak için [Azure Logic Apps](../logic-apps/logic-apps-overview.md), bu makalede yer alan yönergeleri izleyin. 

## <a name="introduction"></a>Giriş

BizTalk Services iki alt Servisleri oluşur:

* Karma bağlantılar Microsoft BizTalk Hizmetleri
* EAI ve EDI köprüsü tabanlı tümleştirmesi

[Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) BizTalk Services karma bağlantılar yerini alır. Azure karma bağlantılar, Azure Portalı aracılığıyla Azure App Service ile kullanılabilir. Bu hizmet bir karma Bağlantı Yöneticisi sunar, böylece mevcut BizTalk Services karma bağlantılar ve ayrıca portalında oluşturduğunuz yeni karma bağlantılar yönetebilirsiniz. 

[Logic Apps](../logic-apps/logic-apps-overview.md) EAI ve EDI köprüsü dayalı tümleştirme aynı olanaklarına BizTalk Services ve daha fazla ile değiştirir. Bu hizmet, bulut ölçekli tüketim tabanlı iş akışı ve orchestration özellikleri karmaşık tümleştirme çözümleri bir tarayıcı yoluyla veya Visual Studio ile hızlı ve kolay bir şekilde oluşturmanızı sağlar.

Bu tablo BizTalk Services özellikleri Logic Apps eşler.

| BizTalk Services   | Logic Apps            | Amaç                      |
| ------------------ | --------------------- | ---------------------------- |
| Bağlayıcı          | Bağlayıcı             | Veri göndermek ve almak   |
| Köprüsü             | Logic App             | Ardışık Düzen işlemci           |
| Aşama doğrula     | XML doğrulaması eylemi | Bir XML belgesi bir şemaya karşı doğrulama | 
| Aşama zenginleştirmek       | Veri belirteçleri           | İletilere veya yönlendirme kararları için özelliklerini Yükselt |
| Aşama dönüştürme    | Eylem dönüştürme      | XML iletileri bir biçimden diğerine dönüştürme |
| Aşama kod çözme       | Düz dosya kod çözme eylemi | Düz dosyadan XML biçimine Dönüştür |
| Aşama kodlama       | Düz dosya kodlamak eylemi | Düz dosya XML'den Dönüştür |
| İleti denetçisi  | Azure işlevleri veya API uygulamaları | Özel kod, tümleştirmeler çalıştırın |
| Rota eylem       | Koşul veya anahtar | Belirtilen bağlayıcılar birine iletileri yönlendirmek |
|||| 

## <a name="biztalk-services-artifacts"></a>BizTalk Services yapıları

BizTalk Services çeşitli türlerde yapıları vardır. 

## <a name="connectors"></a>Bağlayıcılar

BizTalk Services bağlayıcıları HTTP tabanlı istek/yanıt etkileşimleri etkinleştirmek iki yönlü köprüleri dahil olmak üzere veri gönderip köprüleri yardımcı olur. Logic Apps kullandığı aynı terminoloji ve çok çeşitli teknolojiler ve hizmetler için bağlanarak aynı amaca hizmet 180 + bağlayıcılar sahiptir. Örneğin, bağlayıcılar SaaS bulut için kullanılabilir ve PaaS, OneDrive, Office365, Dynamics CRM ve diğerleri gibi hizmetleri plus içi sistemler BizTalk bağdaştırıcı hizmeti BizTalk hizmetlerini değiştirir şirket içi veri geçidi arasında. BizTalk Hizmetleri kaynaklarında, FTP, SFTP ve hizmet veri yolu kuyruğu ya da konu aboneliği sınırlıdır.

![](media/logic-apps-move-from-mabs/sources.png)

Varsayılan olarak, her köprüsü çalışma zamanı adresi ve köprüsü göreli adres özelliklerini ile yapılandırılmış bir HTTP uç noktası vardır. Logic Apps ile aynı sonucu elde etmek için kullanmak [istek ve yanıt](../connectors/connectors-native-reqres.md) eylemler.

## <a name="xml-processing-and-bridges"></a>XML işleme ve köprüleri

BizTalk Services ' bir köprü işleme ardışık düzenine benzerdir. Köprü Bağlayıcısı'ndan alınan verileri alabilir, bazı verilerle çalışmak ve başka bir sisteme sonuçları gönderir. Logic Apps, BizTalk Services olarak aynı ardışık düzen tabanlı etkileşim desenlerini destekleme ve ayrıca diğer tümleştirme desenleri sağlayarak aynı yapar. [XML istek-yanıt köprüsü](https://msdn.microsoft.com/library/azure/hh689781.aspx) BizTalk Services'da bu görevleri gerçekleştiren aşamadan oluşur ve VETER ardışık bilinir:

* (V) doğrulama
* (E) zenginleştirmek
* (T) dönüştürme
* (E) zenginleştirmek
* (R) yolu

Bu görüntü nasıl işleme istek ve istek üzerinde denetim sağlar. yanıt arasında bölünür ve yanıt yolları ayrı olarak, örneğin, farklı kullanarak eşler için her yol göstermektedir:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Ayrıca, tek yönlü köprüsü XML işlem başlangıcı ve sonunda kod çözme ve kodla aşamaları ekler. Doğrudan köprüsü tek bir Enrich aşamasını içerir.

### <a name="message-processing-decoding-and-encoding"></a>İleti işleme, kod çözme ve kodlama

BizTalk Services ' farklı türlerde XML iletileri almak ve alınan ileti eşleşen şeması belirleyin. Bu iş içinde gerçekleştirilir *ileti türlerini* alma işleme ardışık düzen aşaması. Kod çözme aşama, algılanan ileti türü sonra iletiyi sağlanan şema kullanarak kod çözme için kullanır. Şemanın bir düz dosya şema ise, bu aşama, gelen düz dosya XML'e dönüştürür. 

Logic Apps benzer yetenekleri sağlar. Düz dosya farklı bağlayıcı Tetikleyicileri (dosya sistemi, FTP, HTTP vb.) kullanarak farklı protokoller üzerinden almak ve kullanmak [düz dosya kod çözme](../logic-apps/logic-apps-enterprise-integration-flatfile.md) gelen veri XML'e dönüştürmek için eylem. Logic Apps herhangi bir değişiklik yapmadan doğrudan varolan düz dosya şemaları taşıyın ve şemaları tümleştirme hesabınıza yükleyin.

### <a name="validation"></a>Doğrulama

Gelen veri dönüştürüldükten sonra XML (veya XML alınan ileti biçimi olup olmadığı için), doğrulama ileti XSD şemanızı aynılarını varsa belirlemek için çalışır. Logic Apps içinde bu görevi gerçekleştirmek için kullanın [XML doğrulama](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) eylem. BizTalk Services aynı şemaları herhangi bir değişiklik yapılmadan kullanabilirsiniz.

### <a name="transform-messages"></a>İletileri dönüştürme

BizTalk Services ' dönüştürme aşaması bir XML tabanlı ileti biçimi diğerine dönüştürür. Bu iş TRFM tabanlı Eşleyici kullanan bir harita uygulanarak yapılır. Logic Apps içinde benzer işlemidir. Dönüştürme eylem bir harita tümleştirme hesabınızdan yürütür. İkisi arasındaki temel fark Logic Apps eşlemelerinin XSLT biçiminde olmasıdır. XSLT, İşlevsiler içeren BizTalk Server için oluşturulan eşlemeleri dahil olmak üzere zaten varolan XSLT yeniden yeteneğini içerir. 

### <a name="routing-rules"></a>Yönlendirme kuralları

BizTalk Services gelen iletileri veya veri göndermek için hangi uç nokta veya bağlayıcı üzerinde bir yönlendirme karar verir. Önceden yapılandırılmış uç noktalarından seçebilme yönlendirme filtre seçeneğini kullanarak mümkündür:

![](media/logic-apps-move-from-mabs/route-filter.png)

Yalnızca iki seçenek yoksa, BizTalk Hizmetleri'ni kullanarak bir *koşulu* BizTalk Services yönlendirme filtreleri dönüştürmek için en iyi yoludur. Varsa ikiden daha sonra kullanmak bir **geçiş**.

Logic Apps, Gelişmiş mantık özellikleri yanı sıra gelişmiş denetim akışı ve ile yönlendirme sağlar [koşullu deyimler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [switch ifadeleri](../logic-apps/logic-apps-control-flow-switch-statement.md).

### <a name="enrich"></a>Zenginleştirmek

BizTalk Services işlemede Enrich aşama alınan verilerle ilişkili ileti bağlamı özellikler ekler. Örneğin, bir veritabanı araması veya bir XPath ifadesi kullanarak bir değer ayıklanıyor tarafından yönlendirme için kullanmak üzere bir özelliğe yükseltme. Logic Apps, Eylemler aynı davranışı çoğaltmak basit hale getirme, önceki tüm bağlamsal verileri çıktıları erişim sağlar. Örneğin, kullanarak `Get Row` SQL bağlantı eylem, bir SQL Server veritabanından veri dönün ve yönlendirme için verileri bir karar eylemi kullanın. Benzer şekilde, gelen Service Bus özelliklerini Tetikleyici tarafından sıraya alınan iletileri xpath iş akışı tanımı dili ifade kullanılarak XPath yanı sıra olan adreslenebilir.

### <a name="run-custom-code"></a>Özel kod çalıştırma

BizTalk Services olanak tanır [özel kodu çalıştırmak](https://msdn.microsoft.com/library/azure/dn232389.aspx) kendi derlemelerde karşıya. Bu işlev tarafından uygulanan [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) arabirimi. Köprüsü her aşamada bu arabirimi gerçekleştiren oluşturduğunuz .NET türü sağlayan iki özellikleri (üzerinde denetçisi girin ve üzerinde çıkış denetçisi) içerir. Özel kod, verileri daha karmaşık işleme gerçekleştirmenize olanak sağlar ve ortak iş mantığını gerçekleştirirler derlemelerde varolan kodunu yeniden olanak tanır. 

Logic Apps özel kod yürütmek için iki birincil yol sağlar: Azure işlevleri ve API Apps. Azure işlevleri oluşturulur ve mantığı uygulamalardan çağrılır. Bkz: [ekleme ve Azure işlevleri aracılığıyla mantıksal uygulamalar için çalışma özel kod](../logic-apps/logic-apps-azure-functions.md). API uygulamaları, Azure App Service, parçası kendi tetikleyiciler ve Eylemler oluşturmak için kullanın. Daha fazla bilgi edinmek [Logic Apps ile kullanmak için özel bir API oluşturma](../logic-apps/logic-apps-create-api-app.md). 

BizTalk Services çağrı derlemelerde özel kod varsa, bu kodu Azure işlevleri taşıyın veya ne, uygulama bağlı olarak API uygulamaları ile özel API'leri oluşturabilirsiniz. Örneğin, başka bir service Logic Apps bağlayıcı yok sarmalar kodu varsa, API uygulaması oluşturma sonra mantıksal uygulamanızı içinde API uygulamanızı sağlar eylemlerini kullanın. Yardımcı işlevleri veya kitaplıkları varsa, Azure işlevleri büyük olasılıkla en uygun değil.

### <a name="edi-processing-and-trading-partner-management"></a>İşleme ve ticari ortak Yönetimi EDI

BizTalk Services ve Logic Apps dahil EDI ve B2B işleme desteğiyle AS2 (Uygulanabilirlik deyimi 2) X12 ve EDIFACT. BizTalk Services, EDI köprüleri oluşturma ve oluşturun veya ticaret iş ortakları ve ayrılmış izleme ve Yönetim Portalı'nda sözleşmelerini yönetme.
Logic Apps içinde bu işlevselliği elde [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md). EIP sağlar [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve EDI ve B2B işleme B2B eylemler. Ayrıca bir tümleştirme hesabı oluşturmak ve yönetmek için kullandığınız [ticaret ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmaları](../logic-apps/logic-apps-enterprise-integration-agreements.md). Bir tümleştirme hesabı oluşturduktan sonra bir veya daha fazla logic apps hesabınıza bağlayabilirsiniz. B2B Eylemler sonra mantıksal uygulamanızı ticaret iş ortağı bilgileri erişmek için de kullanabilirsiniz. Aşağıdaki eylemleri sağlanır:

* AS2 kodlama
* AS2 kod çözme
* X12 kodlama
* X12 kod çözme
* EDIFACT kodlama
* EDIFACT kod çözme

BizTalk Services, bu eylemleri aktarım protokollerinden birbirinden ayrılır. Mantıksal uygulamalarınızı oluşturduğunuzda, hangi bağlayıcılar daha fazla esneklik bulunur şekilde veri göndermek ve almak için kullanın. Örneğin, alabileceğiniz X12 dosyaları e-posta ve sonra işlemi ek olarak bir mantıksal uygulama içinde bu dosyalar. 

## <a name="manage-and-monitor"></a>Yönetme ve izleme

BizTalk Services ' ayrılmış bir portal izlemek ve sorunlarını gidermek için izleme özellikleri sağlıyordu. Logic Apps daha zengin izleme ve izleme kapasiteleri aracılığıyla sağlar [Azure portal](../logic-apps/logic-apps-monitor-your-logic-apps.md)ile [Operations Management Suite B2B çözümü](../logic-apps/logic-apps-monitor-b2b-message.md), bir göz şeylere tutmak için bir mobil uygulama içerir hareket halinde olduğunuzda.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Yüksek kullanılabilirlik için (HA) BizTalk Services, belirli bir bölgede birden fazla örneği kullanarak işlem yükü paylaşabilirsiniz. Logic Apps bölge içinde HA hiçbir ek ücret ödemeden sağlar. 

BizTalk Services ' bölge olağanüstü durum kurtarma B2B işleme için bir yedekleme ve geri yükleme işlemi gerektirir. İş sürekliliği için çapraz bölge Etkin/pasif Logic Apps sağlar [DR yetenek](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md), olanak sağlayan, B2B veri tümleştirme hesapları farklı bölgelerdeki eşitleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps nedir?](../logic-apps/logic-apps-overview.md)
* [İlk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [önceden oluşturulmuş bir şablon](../logic-apps/logic-apps-create-logic-apps-from-templates.md) kullanarak hızlı şekilde çalışmaya başlayın  
* [Tüm kullanılabilir bağlayıcılar görüntülemek](../connectors/apis-list.md) logic apps içinde kullanabileceğiniz