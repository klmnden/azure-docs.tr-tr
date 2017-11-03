---
title: "Uygulamaları Azure mantıksal uygulamaları için BizTalk Services taşıma | Microsoft Docs"
description: "Taşıma veya Azure BizTalk Services MABS Logic Apps geçirme"
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
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: df26e4669158e5aa9e3b9a7af888d0dbbba273dd
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="move-from-biztalk-services-to-logic-apps"></a>BizTalk Services Logic Apps taşıma

Microsoft Azure BizTalk Services'ı (MABS) devre dışı bırakılıyor. Azure mantıksal uygulamaları MABS tümleştirme çözümleri taşımak için bu konuyu kullanın. 

## <a name="overview"></a>Genel Bakış

BizTalk Services iki alt hizmetinden oluşur:

1.  Karma bağlantılar Microsoft BizTalk Hizmetleri
2.  EAI ve EDI köprüsü tabanlı tümleştirmesi

Karma bağlantılar, ardından taşımak, aradığınız [Azure App Service karma bağlantılar](../app-service/app-service-hybrid-connections.md) değişiklikleri ve bu hizmetin özelliklerini açıklar. Karma bağlantılar Azure BizTalk Services karma bağlantılar değiştirir. Azure karma bağlantılar, Azure App Service ile kullanılabilir ve Azure portalında sunulur. Azure karma bağlantılar, mevcut BizTalk Services karma bağlantılar ve portalda oluşturduğunuz yeni karma bağlantıları yönetmek için yeni bir karma Bağlantı Yöneticisi ayrıca sağlar. Azure App Service karma bağlantı genel olarak kullanılabilir (GA) sayısıdır.

EAI ve EDI köprüsü tabanlı tümleştirmesi için Logic Apps yerini alır. Logic Apps, BizTalk Services özelliklerine ve daha fazlasını hepsi aynı sağlar. Logic Apps bulut ölçekli tüketim tabanlı iş akışı ve hızlı bir şekilde izin orchestration özellikleri ve kolay bir tarayıcı veya Visual Studio içinde araçları kullanarak karmaşık tümleştirme çözümleri oluşturma sağlar.

Aşağıdaki tabloda, Logic Apps BizTalk Services özelliklerinin bir eşleme sağlar.

| BizTalk Services   | Logic Apps            | Amaç                  |
| ------------------ | --------------------- | ---------------------------- |
| Bağlayıcı          | Bağlayıcı             | Veri gönderme ve alma   |
| Köprüsü             | Logic App             | Ardışık Düzen işlemci           |
| Aşama doğrula     | XML doğrulaması eylemi      | Bir XML belgesi bir şemaya karşı doğrulama             |
| Aşama zenginleştirmek       | Veri belirteçleri      | İletilere veya yönlendirme kararları için özelliklerini Yükselt             |
| Aşama dönüştürme    | Eylem dönüştürme      | XML iletileri bir biçimden diğerine dönüştürme             |
| Aşama kod çözme       | Düz dosya kod çözme eylemi      | Düz dosyadan XML biçimine Dönüştür             |
| Aşama kodlama       |  Düz dosya kodlamak eylemi      | Düz dosya XML'den Dönüştür             |
| İleti denetçisi       |  Azure işlevleri veya API uygulamaları      | Özel kod, tümleştirmeler çalıştırın             |
| Rota eylem      |  Koşul veya anahtar      | Belirtilen bağlayıcılar birine iletileri yönlendirmek             |

BizTalk Services yapı farklı türlerde mevcuttur.

## <a name="connectors"></a>Bağlayıcılar
BizTalk Services bağlayıcıları HTTP tabanlı istek/yanıt etkileşimleri etkin iki yönlü köprüleri dahil olmak üzere veri göndermek ve almak köprüleri izin verir. Logic Apps içinde aynı terimler kullanılır. Bağlayıcılar mantıksal uygulamalar'ın aynı amaca hizmet eder ve üzerinde teknolojileri ve Hizmetleri, hem şirket içi veri (BizTalk Services tarafından kullanılan BizTalk bağdaştırıcı hizmeti değiştirme) ağ geçidini kullanarak şirket içi geniş bir diziye bağlanabildiğinden 140 de içerir ve bulut OneDrive, Office365, Dynamics CRM ve diğerleri gibi SaaS ve PaaS Hizmetleri.

BizTalk Hizmetleri kaynaklarında, FTP, SFTP ve hizmet veri yolu kuyruğu ya da konu aboneliği sınırlıdır.

![](media/logic-apps-move-from-mabs/sources.png)

Her köprüsü köprüsü göreli adres özelliklerini çalışma zamanı adresi ile yapılandırılmış varsayılan olarak bir HTTP uç noktası vardır. Logic Apps ile aynı elde etmek için kullanmak [istek ve yanıt](../connectors/connectors-native-reqres.md) eylemler.

## <a name="xml-processing-and-bridges"></a>XML işleme ve köprüleri
BizTalk Services köprü işleme ardışık düzenine benzerdir. Köprü Bağlayıcısı'ndan alınan verileri alabilir ve bazı verilerle çalışmak ve başka bir sisteme gönderme. Logic Apps aynı BizTalk Services olarak aynı ardışık düzen tabanlı etkileşim desenlerini destekleme tarafından yapar ve ayrıca diğer tümleştirme desenleri sayısını sağlar. [XML istek-yanıt köprüsü](https://msdn.microsoft.com/library/azure/hh689781.aspx) BizTalk Services'da gerçekleştirebileceğiniz aşamaları oluşan bir VETER ardışık düzeni bilinir:

* (V) doğrulama
* (E) zenginleştirmek
* (T) dönüştürme
* (E) zenginleştirmek
* (R) yolu

Aşağıdaki resimde görüldüğü gibi işleme isteği ve yanıtlama arasında bölünür ve istek ve yanıt yolları (örneğin her biri için farklı haritalar kullanılarak ayrı ayrı) üzerinde denetim sağlar:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Ayrıca, tek yönlü köprüsü XML işleme başında ve sonunda kod çözme ve kodla aşamaları ekler ve geçiş köprüsü tek bir Enrich aşamasını içerir.

### <a name="message-processing-and-decodingencoding"></a>İleti işleme ve kod çözme ve kodlama
BizTalk Services, farklı türlerde XML iletileri almasına ve alınan ileti eşleşen şeması belirleyin. Bu, gerçekleştirilir **ileti türlerini** alma işleme ardışık düzen aşaması. Ardından, kod çözme aşama sağlanan şema kullanarak çözecek algılanan ileti türü kullanır. Şema Yataydosya şema ise, gelen Yataydosya XML'e dönüştürür. 

Logic Apps benzer yetenekleri sağlar. Çok sayıda farklı bağlayıcı Tetikleyicileri (dosya sistemi, FTP, HTTP vb.) kullanarak farklı protokoller bir Yataydosya almak ve kullanmak [düz dosya kod çözme](../logic-apps/logic-apps-enterprise-integration-flatfile.md) gelen veri XML'e dönüştürmek için eylem. Herhangi bir değişiklik gerektirmeden doğrudan mantıksal uygulamalar için varolan düz dosya şemaları taşımak ve şemaları tümleştirme hesabınıza yükleyin.

### <a name="validation"></a>Doğrulama
Gelen veri dönüştürüldükten sonra XML (veya XML alınan ileti biçimi olup olmadığı için), doğrulama ileti XSD şemanızı aynılarını varsa belirlemek için çalışır. Logic Apps içinde Bunu yapmak için kullanın [XML doğrulama](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) eylem. Tekrar şemalar BizTalk hizmetlerinden herhangi bir değişiklik yapılmadan kullanabilirsiniz.

### <a name="transform-messages"></a>İletileri dönüştürme
BizTalk Services ' dönüştürme aşaması bir XML tabanlı ileti biçimi diğerine dönüştürür. Bu, TRFM tabanlı Eşleyici kullanan bir harita uygulanarak yapılır. Logic Apps içinde benzer işlemidir. Dönüştürme eylem bir harita tümleştirme hesabınızdan yürütür. İkisi arasındaki temel fark Logic Apps eşlemelerinin XSLT biçiminde olmasıdır. XSLT, İşlevsiler içeren BizTalk Server için oluşturulan eşlemeleri dahil olmak üzere zaten varolan XSLT yeniden yeteneğini içerir. 

### <a name="routing-rules"></a>Yönlendirme kuralları
BizTalk Services yönlendirme karar bağlayıcıdaki hangi uç nokta/gelen iletileri/veri göndermesini sağlar. Önceden yapılandırılmış uç noktalarının sayısını birini seçebilme yönlendirme filtre seçeneğini kullanarak mümkündür:

![](media/logic-apps-move-from-mabs/route-filter.png)

Logic Apps ile daha karmaşık mantığı yetenekleri sağlar [koşulu](../logic-apps/logic-apps-use-logic-app-features.md) ve [anahtar](../logic-apps/logic-apps-switch-case.md), Gelişmiş denetim akışı etkinleştirme ve yönlendirme. BizTalk Services yönlendirme filtreleri dönüştürme en iyi sonucu elde edilir kullanarak bir **koşulu** *varsa* yalnızca iki seçenek vardır. Varsa ikiden daha sonra kullanmak bir **geçiş**.

### <a name="enrich"></a>Zenginleştirmek
BizTalk Services işleme Enrich aşamasında alınan verilerle ilişkili ileti bağlamı özellikler ekler. Örneğin, (aşağıda bir veritabanı araması veya bir XPath ifadesi kullanarak bir değer ayıklanıyor tarafından ele) yönlendirme için kullanmak üzere bir özelliğe yükseltme. Logic Apps, Eylemler aynı davranışı çoğaltmak basit hale getirme, önceki tüm bağlamsal verileri çıktıları erişim sağlar. Örneğin, kullanarak `Get Row` SQL bağlantı eylem, bir SQL Server veritabanından veri dönün ve yönlendirme için verileri bir karar eylemi kullanın. Benzer şekilde, gelen Service Bus özelliklerini Tetikleyici tarafından sıraya alınan iletileri xpath iş akışı tanımı dili ifade kullanılarak XPath yanı sıra olan adreslenebilir.

### <a name="use-custom-code"></a>Özel kod kullanın
BizTalk hizmetleri sağlayan yeteneği [özel kodu çalıştırmak](https://msdn.microsoft.com/library/azure/dn232389.aspx) kendi derlemelerde karşıya. Bu tarafından uygulanır [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) arabirimi. Köprüsü her aşamada bu arabirimi gerçekleştiren oluşturduğunuz .net türü sağlayan iki özellikleri (üzerinde denetçisi girin ve üzerinde çıkış denetçisi) içerir. Özel kod gibi verileri daha karmaşık yönlendirilmeden ortak iş mantığını gerçekleştirirler derlemelerde varolan kodunu yeniden sağlar. 

Logic Apps özel kod yürütmek için iki birincil yol sağlar: Azure işlevleri ve API Apps. Azure işlevleri oluşturulur ve mantığı uygulamalardan çağrılır. Bkz: [ekleme ve Azure işlevleri aracılığıyla mantıksal uygulamalar için çalışma özel kod](../logic-apps/logic-apps-azure-functions.md). API uygulamaları, Azure App Service, parçası kendi tetikleyiciler ve Eylemler oluşturmak için kullanın. Daha fazla bilgi edinmek [Logic Apps ile kullanmak için özel bir API oluşturma](../logic-apps/logic-apps-create-api-app.md). 

BizTalk Services çağrı assmeblies özel kod varsa, bu kod için Azure işlevleri taşımak veya API uygulamaları ile özel API oluşturma; ne, uygulama bağlı olarak. Örneğin, başka bir service Logic Apps bağlayıcı yok sarmalar kodu varsa, API uygulaması oluşturma sonra mantıksal uygulamanızı içinde API uygulamanızı sağlar eylemlerini kullanın. Yardımcı işlevleri veya kitaplıkları varsa, Azure işlevleri büyük olasılıkla en uygun değil.

### <a name="edi-processing-and-trading-partner-management"></a>İşleme ve ticari ortak Yönetimi EDI
BizTalk Services EDI ve B2B işleme AS2 desteği içerir (Uygulanabilirlik deyimi 2) X12 ve EDIFACT. Bu nedenle Logic Apps yapar. BizTalk Services, EDI köprüleri oluşturma ve yönetme oluşturun/ticaret iş ortakları ve ayrılmış izleme ve Yönetim Portalı'nda anlaşmaları.

Bu işlev Logic Apps içinde bulunan [Kurumsal tümleştirme paketi](../logic-apps/logic-apps-enterprise-integration-overview.md). Bu tümleştirme hesabı ve B2B eylemleri EDI ve B2B işleme için oluşur. [Tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) oluşturmak ve yönetmek için kullanılan [ticaret ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmaları](../logic-apps/logic-apps-enterprise-integration-agreements.md). Bir tümleştirme hesabı oluşturduktan sonra bir veya daha fazla mantıksal uygulamalar hesabı ilişkilendirebilirsiniz. İlişkili sonra B2B Eylemler mantığı uygulamanızda ticari ortak bilgilere erişmek için kullanabilirsiniz. Aşağıdaki eylemleri sağlanır:

* AS2 kodlama
* AS2 kod çözme
* X12 kodlama
* X12 kod çözme
* EDIFACT kodlama
* EDIFACT kod çözme

BizTalk Services, bu eylemleri aktarım protokollerinden birbirinden ayrılır. Bu nedenle, mantıksal uygulamalarınızı oluşturduğunuzda, veri göndermek ve almak için kullandığınız hangi bağlayıcıların daha fazla esnekliğe sahip olursunuz. Örneğin, X12 almak için mümkündür e-posta ve sonra işlemi ek olarak bu mantıksal uygulama içinde dosyaları. 

## <a name="manage-and-monitor"></a>Yönetme ve izleme
Ticari ortak Yönetimi yanı sıra, BizTalk Services için ayrılmış portalı izlemek ve sorunlarını gidermek için izleme özellikleri sağlanır. 

Logic Apps daha zengin izleme ve yetenekleri izleme sağlar [Azure portal](../logic-apps/logic-apps-monitor-your-logic-apps.md)ile [Operations Management Suite B2B çözümü](../logic-apps/logic-apps-monitor-b2b-message.md); hareket halinde olduğunuzda bir göz şeylere tutmak için bir mobil uygulama dahil.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) BizTalk Services'da elde etmek için birden fazla örneği belirli bir bölgede işlem yükü paylaşmak için kullanın. Logic apps ile bölge içinde HA yerleşiktir ve hiçbir ek maliyetlerine neden olur. BizTalk Services B2B işlemek için bölge olağanüstü durum kurtarma için bir yedekleme ve geri yükleme işlemi gereklidir. Logic Apps, çapraz bölge Etkin/pasif olarak [DR yetenek](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) sağlanır; iş sürekliliği için farklı bölgelerdeki tümleştirme hesapları arasında B2B verilerinin eşitlemesine ilişkin veren.

## <a name="next"></a>Sonraki
* [Logic Apps nedir?](logic-apps-what-are-logic-apps.md)
* [İlk mantıksal uygulamanızı oluşturun](logic-apps-create-a-logic-app.md) veya [önceden oluşturulmuş bir şablon](logic-apps-create-logic-apps-from-templates.md) kullanarak hızlı şekilde çalışmaya başlayın  
* Mantıksal uygulamalarda [kullanabileceğiniz tüm bağlayıcıları görüntüleyin](../connectors/apis-list.md)
