---
title: BizTalk Server EDI çözümlerini BizTalk Services'a geçme | Microsoft Docs
description: Microsoft BizTalk Server EDI çözümlerinizi Microsoft Azure BizTalk Services (MABS) nasıl geçirebileceğiniz öğrenin
services: biztalk-services
ms.service: biztalk-services
author: jonfancey
ms.author: jonfan
manager: jeconnoc
ms.topic: article
ms.date: 07/31/2018
ms.reviewer: jonfan, LADocs
ms.suite: integration
ms.openlocfilehash: e6f0b11c99cbe8778b51024c418ffba70da61a77
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917388"
---
# <a name="migrate-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server EDI çözümlerini BizTalk Services'a Geçirme: Teknik kılavuz

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Yazar: Tim Wieman ve Nitin Mehrotra

İnceleme: Karthik Bharthy

Kullanılarak yazılmış:  Microsoft Azure BizTalk Hizmetleri – Şubat 2014 yayın.

## <a name="introduction"></a>Giriş
Elektronik Veri Değişimi (EDI) hangi işletmelerin exchange verilerini en yaygın yöntemlerle elektronik olarak da işletmeler arası veya B2B işlemleri adlandırılır biridir. BizTalk Server EDI için BizTalk Server ilk yayımlandıktan sonra on yıl destek oluşturdu. BizTalk Hizmetleri ile Microsoft Microsoft Azure platformunda EDI çözümlerini desteği devam eder. B2B işlemlerini çoğunlukla bir kuruluş dışında bulunan ve bu nedenle bir bulut platformunun üzerine uyguladığında uygulamak daha kolay olur. Microsoft Azure BizTalk Hizmetleri aracılığıyla bu yeteneği sağlar.

Bazı müşteriler için yeni EDI çözümlerini bir "Yeşil alan" platform olarak BizTalk Hizmetleri arayın, birçok müşteri Azure'a geçirmek isteyebileceğiniz geçerli BizTalk Server EDI çözümlerini bulunur. BizTalk Hizmetleri EDI desteklemesi için tasarlanmıştır (iş ortakları, varlıklar, anlaşmalar ticaret), BizTalk Server EDI mimari olarak aynı temel varlıklarda temel BizTalk Server EDI yapıtları BizTalk hizmetlerine geçirme mümkün olmasıdır.

Bu belge, BizTalk hizmetlerine geçirme BizTalk Server EDI yapıtlarla birlikte ilgili farklılıklar açıklanır. Bu belge, ticari ortak sözleşmeleri ve BizTalk Server EDI işleme bilgisine varsayar. BizTalk Server EDI hakkında daha fazla bilgi için bkz. [iş ortağı yönetim kullanarak BizTalk Server alım-satım](/biztalk/core/trading-partner-management-using-biztalk-server).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>BizTalk Hizmetleri için BizTalk Server EDI yapıtları'nın hangi sürümünün taşınabilir mi?
Bu iş ortakları, Profiller ve anlaşmalar içerecek şekilde yeniden modellenmiş, BizTalk Server EDI modülü BizTalk Server 2010 için önemli ölçüde geliştirilmiştir. BizTalk Hizmetleri, ticari ortaklar ve iş bölümler ticaret iş ortakları içinde düzenlemek için aynı modelini kullanır. Sonuç olarak, EDI yapıtları BizTalk Server 2010 ve sonraki sürümler BizTalk hizmetlerine geçirme, çok daha anlaşılır, bir işlemdir. BizTalk Server 2010'dan önceki sürümleriyle ilişkili EDI yapıtları geçirmek için BizTalk Server 2010'a yükseltmeniz ve ardından EDI yapıtlarınızı BizTalk hizmetlerine geçirme.

## <a name="scenariosmessage-flow"></a>Senaryo/ileti akışı
BizTalk Server ile BizTalk Services'da işleme EDI bir ticari ortak Yönetimi (TPM) çözüm olarak tasarlanmıştır. TPM çözüm aşağıdaki temel bileşenlere sahiptir:

* Ticari ortaklar, hangi kuruluştaki B2B hareket temsil eder.
* Profiller, ticari ortak içinde bölümler temsil eder.
* Ticari ortak sözleşmeleri (veya anlaşmalar) iki iş ortakları/profiller arasındaki ticari sözleşme temsil eder.

Aşağıdaki çizimde, bir BizTalk Server EDI ve BizTalk Hizmetleri EDI çözüm arasındaki farklar yanı sıra benzerlikler gösterilmektedir:

![][EDImessageflow]

Bir BizTalk Server EDI çözüm flow'da ve BizTalk Hizmetleri arasındaki benzerlikler ve farklar şunlardır:

* BizTalk Hizmetleri, yalnızca BizTalk Server EDI iletisi ve bir EDISend işlem hattı bir EDI iletisi göndermek için almak için bir EDIReceive işlem hattı kullandığı gibi bir almak için köprü EDI almak ve EDI gönderme köprüsü EDI iletilerini göndermek için kullanır. BizTalk Server, işlem hatları ile bir anlaşma Gönder'i kullanarak ilişkili veya bağlantı alma. BizTalk Hizmetleri, anlaşma Gönder gösterir veya köprü alırsınız.
* EDI iletisi EDIReceive işlem hattı işledikten sonra iletiyi BizTalk Server'ın içinde bir SQL Server veritabanına yazılan. EdiSend işlem hattı SQL Server veritabanını iletiden sonra seçer, işler ve ardından ticaret ortağına gönderir.
  
    BizTalk hizmetleri de EDI aldıktan sonra köprüsü EDI ileti işleme, dış işleme iletisi yönlendirir. Dış işlem Microsoft Azure'da veya şirket içi üzerinde çalışıyor olabilir. Dış işleme iletisi EDI gönderme köprüsü için yönlendirmek; Gönderme köprüsü ileti kendiliğinden çekmez. İleti işlendikten sonra EDI gönderme köprüsü için ticari ortak ileti yönlendirir.

BizTalk Hizmetleri, hızla oluşturun ve tüm Microsoft Azure bilgi işlem örnekleri (Web veya çalışan rolleri), herhangi bir Microsoft Azure SQL veritabanını veya yapılandırma olmadan ticari ortaklar arasında bir B2B sözleşmesi dağıtmak için bir kolay kullanımlı yapılandırma deneyimi sağlar. Microsoft Azure depolama hesabı. Daha karmaşık senaryolarda, iş akışları veya başka bir hizmet işleme bağlamadan gerektirecek "köşelerindeki" diğer bir deyişle, bir ticaret iş ortağı sözleşmesi önce veya sonra iş ortağı sözleşmesi EDI ticari köprüsü işleme. Ayrıntılı olarak, aşağıdaki olaylar dizisi bir EDI iletisi BizTalk Services'da işleme sırasında oluşur.

1. İş ortağı, Fabrikam alım-satım EDI iletisi aldı.  BizTalk Hizmetleri gibi FTP, SFTP, AS2 ve HTTP/s aktarım protokolünü destekleyen ticari iş ortaklarının sunduğu EDI iletilerini almak için
2. Ticari ortak sözleşmesi alma tarafı işleme XML biçimine EDI iletisi ayrıştırır.  Service Bus uç noktalarına bir Service Bus geçişi uç noktası, Service Bus konusu, hizmet veri yolu kuyruğu ya da BizTalk Hizmetleri köprü gibi ayrıştırılmış EDI iletisi (XML biçimi) yönlendirebilirsiniz.
3. Ayrıştırılmış XML iletileri daha fazla özel işleme için uç noktasından sonra alınan.  Bu uç noktalar şirket içi bir bileşen ya da daha fazla işlem yapılacak bir Windows Workflow (WF) veya Windows Communication Foundation (WCF) hizmetinin Microsoft Azure işlem örneğine örneğin işlenemedi.
4. "Gönderme tarafı işleme" ticari ortak sözleşmesi sonra XML ileti EDI biçime derler ve ticaret iş ortağı için Contoso gönderir.  Ticari ortaklar için EDI iletilerini göndermek için BizTalk Hizmetleri EDI iletilerini almak için kullanılanlarla aynı protokollerini destekler.

Bu belgede daha fazla kavramsal rehberlik sağlanır üzerinde geçirme bazı farklı BizTalk Server EDI yapıtlar için BizTalk Hizmetleri.

## <a name="sendreceive-ports-to-trading-partners"></a>Gönderme bağlantı noktaları ticaret iş ortakları ve alma
BizTalk Server alma konumlarını ve alma bağlantı noktalarının ticari iş ortaklarının sunduğu EDI/XML iletileri alacak şekilde ayarlamanız ve ticaret iş ortağına EDI/XML iletileri göndermek için gönderme bağlantı noktalarını ayarlayın. Ardından bu bağlantı noktaları için BizTalk Server yönetim konsolunu kullanarak ticari ortak sözleşmesi bağlayın. BizTalk Services, ticaret iş ortakları iletileri almanıza konumları ve burada gönderdiğiniz ortaklar için iletileri, ticari ortak sözleşmesi kendisi (aktarım ayarlarının bir parçası) olarak BizTalk Hizmetleri portalında bir parçası olarak yapılandırılır.  Bu nedenle, gerçekten "gönderme bağlantı noktaları" ve "konumları Al" kavramını başına uzatılmasında BizTalk Services hizmetinde yok. Daha fazla bilgi için [sözleşmeleri oluşturma](/previous-versions/azure/hh689908(v=azure.100)).

## <a name="pipelines-bridges"></a>İşlem hatları (köprü)
BizTalk Server EDI işlem hatları da uygulamanın gerektirdiği gibi belirli bir işlemeyi özelliklerin Özel mantık dahil edebileceğiniz ileti işleme varlıklardır. BizTalk Hizmetleri için de bir EDI köprüsü eşdeğer olacaktır. Ancak şimdilik BizTalk Hizmetleri'nde EDI köprüleri "kapatılır".  Diğer bir deyişle, bir EDI köprüsüne özel etkinliklerinizi ekleyemezsiniz. Herhangi bir özel işlem önce ya da ticari ortak sözleşmesi bir parçası olarak yapılandırılmış köprüsü ileti girdikten sonra uygulamanızda EDI köprüsü dışında yapılmalıdır. EAI köprüleri özel işlem yapma seçeneğiniz vardır. Özel işleme istiyorsanız, önce veya sonra EDI köprüsü tarafından işlenen iletisi EAI köprüleri kullanabilirsiniz. Daha fazla bilgi için [özel kodda dahil köprüsü nasıl](/previous-versions/azure/dn232389(v=azure.100)).

Özel kod ve/veya Service Bus kuyrukları ve konuları ticari ortak sözleşmesi iletiyi alır önce veya sonra sözleşmesi iletiyi işler ve bir Service Bus uç noktasına yönlendirir mesajlaşması kullanarak bir yayımlama/abone olma akışla ekleyebilirsiniz.

Bkz: **senaryoları/ileti akışı** ileti akış düzeni için bu makaledeki.

## <a name="agreements"></a>Sözleşmeler
BizTalk Server 2010 ticaret ortak EDI işleme için kullanılan sözleşmeleri biliyorsanız, BizTalk Services'ın ticari ortak sözleşmeleri bilgili arayın. Sözleşme ayarların çoğu aynıdır ve aynı bir terminoloji kullanıyor. Bazı durumlarda, anlaşma ayarları çok daha basittir BizTalk Server aynı ayarları karşılaştırılan. AS2, EDIFACT ve Microsoft Azure BizTalk Services destekler X12 taşıma.

Microsoft Azure BizTalk Services de sağlar bir **TPM veri geçişi** ticaret iş ortakları ve anlaşmalar BizTalk Server ticaret iş ortağı modülünden BizTalk Hizmetleri portalında geçirmek için aracı. TPM veri geçiş aracı kullanılabilir Araçları paketinin bir parçası olarak, hangi nden indirilebilir [MABS SDK](https://go.microsoft.com/fwlink/p/?LinkId=235057). Paketi ayrıca aracı ve temel sorun giderme bilgileri için Aracı'nı kullanma hakkında yönergeler sağlayan bir benioku dosyası içerir.

## <a name="schemas"></a>Şemalar
BizTalk Hizmetleri, BizTalk Services çözümlerinde kullanılan EDI şemaları sağlar.  Ayrıca, BizTalk hizmetlerinin yanı sıra BizTalk Server arasında EDI şemasının kök düğümü aynı olduğu için BizTalk Server EDI şemaları de BizTalk Hizmetleri ile kullanılabilir. Bu nedenle, doğrudan, BizTalk Server EDI şemaları alıp bunları BizTalk Services'ı kullanarak geliştirme EDI çözümlerini kullanmak mümkün olacaktır. Şemalardan da indirebilirsiniz [MABS SDK](https://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Haritalar (dönüşümler)
BizTalk Server maps'a dönüşümler BizTalk Hizmetleri'nde çağrılır. BizTalk hizmetlerine geçirme haritalar'dan BizTalk Server (harita karmaşıklığına bağlı olarak) elde etmek için daha karmaşık görevleri biri olabilir. BizTalk Hizmetleri için kullanılan eşleme BizTalk eşleyicisinden farklı bir araçtır. Temel eşleme biçimi Eşleyici çoğunlukla aynı görünür halde farklıdır. İşlevsiler (adlı **eşleme işlemleri** BizTalk Services) kullanılabilir olan kullanıcıları da farklıdır.  Aslında, BizTalk Services hizmetinde BizTalk harita doğrudan kullanamazsınız. Ayrıca, BizTalk Server kullanılabilir tüm İşlevsiler eşleme işlemleri BizTalk Hizmetleri'nde olarak kullanılabilir.

### <a name="new-transform-operations"></a>Yeni dönüştürme işlemleri
BizTalk Hizmetleri dönüştüren, dönüştürme eşlemesi işlemlerin listesini BizTalk Server eşleyicisinden oldukça farklı görünebilir, ancak aynı görevleri yerine getirmeye yeni yolu vardır. Örneğin, BizTalk Hizmetleri dönüştüren sahip **listeleme işlemleri** kullanılabilir. Bu, BizTalk eşleyicisinde kullanılabilir değildi.  **Listeleme işlemleri** etkinleştirmek, oluşturma ve bir "listesinde", bir liste (diğer adıyla "satır") öğelerin kümesi olduğu ve her öğesi, birden çok üye (diğer adıyla "sütunları") sahip olabilir.  Listeyi sıralayın, bağlı bir koşul, vb. öğeleri seçer.

BizTalk Hizmetleri dönüştüren yeni işlevler başka bir örnektir **döngü Operations**.  BizTalk Server eşleyicisinde iç içe döngüleri oluşturmak zordur.  Bu nedenle, döngü eşleme işlemleri için BizTalk Hizmetleri dönüştüren eklenir.

Başka bir örnek henüz **If-Then-Else** ifade harita işlemi.  İf-then-else bir işlem yapmadan BizTalk eşleyicisinde olası, ancak birden çok İşlevsiler görünüşte basit bir görevi gerçekleştirmek için gerekli.

### <a name="migrating-biztalk-server-maps"></a>BizTalk sunucusunu taşıma eşler
Microsoft Azure BizTalk Services, BizTalk Server'ı geçirmek için bir aracı için BizTalk Hizmetleri dönüşümler eşler sağlar. **BTMMigrationTool** olarak kullanılabilir parçası **Araçları** ile sağlanan paket [BizTalk Hizmetleri SDK'sını indirme](https://go.microsoft.com/fwlink/p/?LinkId=235057). Aracı hakkında daha fazla bilgi için bkz. [BizTalk hizmetlerini dönüştürmek için BizTalk eşleşmesi dönüştürme](/previous-versions/azure/hh949812(v=azure.100)).

Bir örneğe nasıl Sandro Pereira, BizTalk MVP tarafından da bakabilirsiniz [BizTalk Server haritalar için BizTalk Hizmetleri dönüşümler geçirme](https://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Düzenlemeleri
BizTalk Server düzenleme Microsoft Azure'a işleme geçmeniz gerekiyorsa düzenlemeleri Microsoft Azure çalışan BizTalk Server düzenlemeleri desteklemediğinden yazılması gerekir.  Bir Windows Workflow Foundation 4.0 (WF4) hizmetinin düzenleme işlevleri yeniden yazabilirsiniz.  Şu anda hiçbir geçiş BizTalk Server düzenlemeleri WF4 olduğundan bu tam bir yeniden yazma olacaktır. Windows iş akışı için bazı kaynaklar aşağıda verilmiştir:

* [*Bir WCF iş akışı hizmeti, hizmet veri yolu kuyrukları ve konuları ile tümleştirmek nasıl* ](https://blogs.msdn.microsoft.com/paolos/2013/04/09/how-to-integrate-a-wcf-workflow-service-with-service-bus-queues-and-topics/) Paolo Salvatori tarafından. 
* [*Windows Workflow Foundation'ı ve Azure ile uygulama oluşturmaya* oturumu](https://go.microsoft.com/fwlink/p/?LinkId=237314) derleme 2011 konferansına ait.
* [*Windows Workflow Foundation Geliştirici Merkezi*](https://docs.microsoft.com/previous-versions/dotnet/articles/ee342461(v=msdn.10)).
* [*Windows Workflow Foundation 4 (WF4) belgeleri* ](/dotnet/framework/windows-workflow-foundation/index) MSDN'de.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
BizTalk Services'ı kullanırken yapmanız gereken bazı önemli noktalar aşağıda verilmiştir.

### <a name="fallback-agreements"></a>Geri dönüş sözleşmeleri
BizTalk Server EDI işleme "Geri dönüş sözleşmeleri" kavramı vardır.  BizTalk Hizmetleri gerektirmez **değil** bir geri dönüş sözleşmesi kavramı şimdiye sahip.  BizTalk belgeleri konulara bakın [rolü, sözleşmelerde EDI işleme](https://go.microsoft.com/fwlink/p/?LinkId=237317) ve [yapılandırma genel ya da geri dönüş anlaşması özellikleri](/biztalk/core/configuring-global-or-fallback-agreement-properties) geri dönüş anlaşmalar BizTalk nasıl kullanıldığı hakkında bilgi Sunucu.

### <a name="routing-to-multiple-destinations"></a>Birden çok hedefe yönlendirme
BizTalk Hizmetleri köprüleri, geçerli durumunda bir yayımlama kullanarak birden fazla hedefe yönlendirme iletileri desteklemiyor-abonelik modeli. Bunun yerine bir BizTalk Hizmetleri köprü iletilerden sonra birden fazla uç nokta ileti almak için birden fazla abonelik olabilir bir Service Bus konu başlığına yol.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure'da iş KOLU çözümleri](https://azure.microsoft.com/solutions/lob-applications)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
