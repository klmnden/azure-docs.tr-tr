---
title: "BizTalk Server EDI çözümleri BizTalk Services Teknik Kılavuzu'na geçirme | Microsoft Docs"
description: "EDI için MABS; yine de geçir istiyor musunuz? Microsoft Azure BizTalk Hizmetleri"
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 1b70fc3d199d7f1521acb534dafec8fb3e69500e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="migrating-biztalk-server-edi-solutions-to-biztalk-services-technical-guide"></a>BizTalk Server EDI çözümleri için BizTalk Services geçirme: teknik Kılavuzu

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Yazar: Tim Wieman ve Nitin Mehrotra

İnceleme: Karthik Bharthy

Kullanılarak yazılmış: Microsoft Azure BizTalk Services – Şubat 2014 serbest bırakın.

## <a name="introduction"></a>Giriş
Elektronik Veri Değişimi (EDI) hangi işletmeler exchange verileri en yaygın yollarla elektronik olarak da işletmeler için veya B2B işlemler olarak gösterilen biridir. BizTalk Server ilk BizTalk Server sürüm bu yana bir on üzerinden EDI desteği oluşturdu. BizTalk Services ile Microsoft EDI çözümleri için destek Microsoft Azure platformunda devam eder. B2B işlemler çoğunlukla kuruluş için dış ve bu nedenle bir bulut platformunda uygulanırsa uygulamak daha kolay olur. Microsoft Azure BizTalk Services aracılığıyla bu yeteneği sağlar.

Yeni EDI çözümleri için "greenfield" platform olarak bazı müşteriler BizTalk Services bakın, ancak birçok müşteri Azure'a geçirmek isteyebileceğiniz geçerli BizTalk Server EDI çözümleri sahiptir. BizTalk Services EDI tasarlanmış çünkü aynı anahtar varlıkları (iş ortakları, varlıklar, anlaşmalar ticaret), BizTalk Server EDI mimari olarak göre BizTalk Services BizTalk Server EDI yapıtları geçirmek mümkündür.

Bu belge farklılıklarını geçirme BizTalk Server EDI yapıtlarıyla BizTalk Services için bazıları açıklanır. Bu belge, BizTalk Server EDI işlemeyi ve ticari ortak sözleşmeleri bilgili varsayar. BizTalk Server EDI hakkında daha fazla bilgi için bkz: [iş ortağı yönetimi kullanarak BizTalk Server ticaret](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-to-biztalk-services"></a>BizTalk Server EDI yapıları'nın hangi sürümü BizTalk Services geçirilebilecek?
Bu iş ortakları, Profiller ve anlaşmaları içerecek şekilde yeniden modellenmiş zaman BizTalk Server EDI modülü BizTalk Server 2010 için önemli ölçüde geliştirilmiştir. BizTalk Services ticari ortaklar ve iş bölümler bu ticari ortaklar içinde düzenlemek için aynı modelini kullanır. Sonuç olarak, EDI yapıları BizTalk Server 2010 ve sonraki sürümler için BizTalk Services geçiş, bir düz çok daha ileri bir işlemdir. BizTalk Server 2010'den önceki sürümleri ile ilişkili EDI yapıtları geçirmek için önce BizTalk Server 2010'a yükseltme ve EDI yapıtları BizTalk Services geçirmenize gerekir.

## <a name="scenariosmessage-flow"></a>Senaryo/ileti akışı
BizTalk Server ile olarak BizTalk Services'da işleme EDI bir ticari ortak Yönetimi (TPM) çözüm oluşturulmuştur. TPM çözüm, aşağıdaki anahtar bileşeni vardır:

* Ticaret ortakları, hangi B2B işlem kuruluşta temsil eder.
* Profilleri, ticari ortak içinde bölümler temsil eder.
* Ticari ortak sözleşmeleri (veya anlaşmaları) iki iş ortakları/profilleri arasında iş anlaşmayı temsil eder.

BizTalk Server EDI çözüm ve BizTalk Services EDI çözümü arasındaki farklar yanı sıra benzerlikler aşağıda gösterilmektedir:

![][EDImessageflow]

BizTalk Server EDI çözüm akışında ve BizTalk Services arasındaki benzerlikler ve anahtar farkları şunlardır:

* Yalnızca bir EDIReceive ardışık EDI ileti ve bir EDISend ardışık EDI ileti göndermek için almak için kullandığı BizTalk Server gibi BizTalk Services bir almaya köprüsü EDI almak ve EDI gönderme köprüsü EDI iletilerini göndermek için kullanır. BizTalk Server ardışık düzen gönderme kullanarak bir Sözleşmesi ile ilişkili olan veya geri alma bağlantı noktalarının. BizTalk hizmetleri sözleşmesi gönder gösterir veya köprüsü alırsınız.
* EDI ileti EDIReceive ardışık düzen işledikten sonra ileti BizTalk Server ' SQL Server veritabanına yazılan. EdiSend ardışık düzen ileti SQL Server veritabanından alır, işler ve ardından ticaret ortağına gönderir.
  
    BizTalk Services EDI aldıktan sonra köprüsü EDI ileti işleme, bir dış işlem iletiyi yönlendirir. Dış işlem Microsoft Azure veya şirket içi çalıştırıyor. Dış işlem ileti EDI gönderme köprüsü için rota; Gönderme köprüsü ileti kendiliğinden çekme değil. İleti işlendikten sonra EDI gönderme köprüsü ticaret ortağına iletiyi yönlendirir.

BizTalk Services hızlı bir şekilde oluşturmak ve tüm Microsoft Azure işlem yapılandırma örnekleri (Web veya çalışan rolleri) olmadan ticaret ortakları, tüm Microsoft Azure SQL veritabanları veya arasında herhangi bir B2B anlaşması dağıtmak için kullanımı kolay yapılandırma deneyimi sağlar Microsoft Azure depolama hesabı. Daha karmaşık senaryolara iş akışları veya diğer hizmetini işleme bağlamadan gerektirir "kenarlarının etrafına" başka bir deyişle, bir ticari ortak sözleşmesi önce veya sonra ticari ortak sözleşmesi EDI köprüsü işleme. Ayrıntılı olarak, aşağıdaki olaylar dizisi bir EDI ileti BizTalk Services'da işleme sırasında oluşur.

1. İş ortağı, Fabrikam ticari bir EDI ileti alındı.  Ticaret ortaklarından EDI iletileri almak için BizTalk Services FTP, SFTP, AS2 ve HTTP/s gibi aktarım protokollerini destekler
2. Ticari ortak sözleşmesi alma tarafı işleme XML biçimine EDI ileti ayrıştırır.  Service Bus Uç noktalara bir Service Bus geçişi uç noktası, hizmet veri yolu konusu, hizmet veri yolu kuyruğu ya da BizTalk Services köprü gibi (XML biçiminde) artık hata ayıklayabildiğinize EDI ileti yönlendirebilir.
3. Daha fazla özel işleme için uç noktasından sonra artık hata ayıklayabildiğinize XML iletileri alınması.  Bu uç noktalar bir şirket içi bileşeni ya da daha fazla işlem bir Windows iş akışı (WF) veya Windows Communication Foundation (WCF) hizmetini iletisinde Microsoft Azure işlem örneğine örneğin işlenemedi.
4. "Gönderme tarafı işleme" ticari ortak sözleşmesi sonra EDI formatına XML ileti derler ve ticaret ortağına Contoso gönderir.  Ticari ortaklar EDI ileti göndermek için BizTalk Services olarak EDI iletileri almak için kullanılanlarla aynı protokollerini destekler.

Bu belgede daha fazla kavramsal bir Rehber sağlanır üzerinde geçirme bazı farklı BizTalk Server EDI yapılarının BizTalk Services.

## <a name="sendreceive-ports-to-trading-partners"></a>Gönderme/alma bağlantı noktalarının ticaret ortakları
BizTalk Server alma konumları ve alma bağlantı noktalarının ticaret ortaklarından EDI/XML iletileri alacak şekilde ayarlamanız ve ticari ortak EDI/XML iletileri göndermek için Gönder bağlantı noktalarını ayarlayın. Ardından bu bağlantı noktaları BizTalk Server Yönetim Konsolu kullanılarak ticari ortak sözleşmesi için yukarı bağlayın. BizTalk Services, ticaret ortaklarından gelen iletileri almanıza konumları ve burada gönderdiğiniz ticaret ortaklarına iletileri ticari ortak sözleşmesi kendisini (parçası olarak aktarım ayarları) BizTalk Services Portalı'nda bir parçası olarak yapılandırılır.  Bu nedenle, gerçekten "gönderme bağlantı noktaları" ve "konumları Al" kavramı başına uzatılmasında BizTalk Services'da sahip değilsiniz. Daha fazla bilgi için bkz: [oluşturma anlaşmaları](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Ardışık Düzen (köprü)
BizTalk Server EDI içinde ardışık düzen da uygulamanın gerektirdiği gibi belirli işleme özelliklerine yönelik özel mantık içerebilir ileti işleme varlıklardır. BizTalk Services için eşdeğer bir EDI köprüsü olacaktır. Ancak, şimdilik BizTalk Services EDI köprüleri "kapalı".  Diğer bir deyişle, kendi özel etkinlikler EDI köprüsüne ekleyemezsiniz. Özel işlem yapmadan önce veya iletinin ticari ortak sözleşmesi bir parçası olarak yapılandırılmış köprüsü girdikten sonra uygulamanızda EDI köprüsü dışında yapılmalıdır. EAI köprüleri özel işleme yapma seçeneğiniz vardır. Özel işleme istiyorsanız, önce veya sonra EDI köprüsü tarafından işlenen iletisi EAI köprüleri kullanabilirsiniz. Daha fazla bilgi için bkz: [özel kodda dahil köprüleri nasıl](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Özel kod ve/veya hizmet veri yolu kuyrukları ve konularından ticari ortak sözleşmesi iletisini almadan önce veya sonra anlaşmayı iletisini işler ve Service Bus uç noktasına yönlendirir ileti kullanarak bir Yayımla ve abone akışla ekleyebilirsiniz.

Bkz: **senaryoları/ileti akışı** ileti akışı deseni için bu konudaki.

## <a name="agreements"></a>Anlaşmaları
BizTalk Server 2010 ticaret ortak EDI işleme için kullanılan sözleşmeleri biliyorsanız, BizTalk ticari ortak sözleşmeleri Hizmetleri bilgili arayın. Anlaşma ayarların çoğu aynıdır ve aynı terimleri kullanın. Bazı durumlarda, anlaşma ayarları çok daha basittir BizTalk Server aynı ayarlarında karşılaştırılan. AS2, EDIFACT ve Microsoft Azure BizTalk Services destekler X12 taşıma.

Microsoft Azure BizTalk Services de sağlayan bir **TPM veri geçişi** ticaret iş ortakları ve anlaşmaları BizTalk Server ticari ortak modülünden BizTalk Services Portal geçirmek için aracı. TPM veri geçiş aracı kullanılabilir Araçlar paketinin bir parçası olarak, hangi adresinden yüklenebilir [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). Paket Ayrıca aracı ve temel sorun giderme bilgileri için Aracı'nı kullanma hakkında yönergeler sağlayan bir benioku dosyası içerir.

## <a name="schemas"></a>Şemalar
BizTalk Services BizTalk Services çözümlerinde kullanılan EDI şemaları sağlar.  Ayrıca, BizTalk hizmetlerinin yanı sıra BizTalk Server arasında EDI şema kök düğümü aynı olduğu için BizTalk Server EDI şemaları da BizTalk Services ile kullanılabilir. Bu nedenle, doğrudan, BizTalk Server EDI şemaları alma ve bunları BizTalk Services'ı kullanarak geliştirme EDI çözümlerinde kullanma olanağınız olur. Gelen şemaları de indirebilirsiniz [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Eşlemeleri (dönüşümler)
BizTalk Server'ın eşlemeleri dönüşümler BizTalk Services'da denir. BizTalk Server geçirme haritaları BizTalk Services için (harita karmaşıklığına bağlı olarak) elde etmek için daha karmaşık görevleri biri olabilir. BizTalk Hizmetleri için kullanılan eşleme BizTalk Eşleyici farklı bir araçtır. Eşleyici çoğunlukla aynı görünse de, temel alınan eşleme biçiminde farklıdır. İşlevsiler (adlı **eşleme işlemleri** BizTalk Services) kullanımına kullanıcılar da farklıdır.  Uygulamada bir BizTalk harita doğrudan BizTalk Services'da kullanamazsınız. Ayrıca, BizTalk Server içinde kullanılabilir tüm İşlevsiler BizTalk Services harita işlemlerinde olarak kullanılabilir.

### <a name="new-transform-operations"></a>Yeni dönüştürme işlemleri
Dönüştürme eşleme işlemleri listesi BizTalk Server Eşleyici oldukça farklı görünebilir, ancak BizTalk Services dönüştüren aynı görevleri gerçekleştirmeye yeni yöntemler vardır. Örneğin, BizTalk Services dönüştüren sahip **listeleme işlemleri** kullanılabilir. Bu, BizTalk eşleyicisinde kullanılabilir değildi.  **Listeleme işlemleri** oluşturmak ve bir liste öğesi (olarak da bilinen "satır") kümesi ve her öğesi, birden çok üye (olarak da bilinen "sütunlar") sahip olduğu bir "listesinde", işletmek olanak sağlar.  Sıralamak, bağlı bir koşul, vb. öğeleri seçin.

BizTalk Services dönüştüren içindeki yeni işlevsellik başka bir örnektir **döngüsü işlemleri**.  BizTalk Server eşleyicisinde iç içe geçmiş döngüler oluşturmak zordur.  Bu nedenle, döngü eşleme işlemleri için BizTalk Services dönüştüren eklenir.

Başka bir örnek henüz **If-Then-Else** ifade eşleme işlemi.  IF-then-başka bir işlem yapmakla BizTalk eşleyicisinde mümkün olsa da, görünen basit bir görevi gerçekleştirmek için birden çok İşlevsiler gerekli.

### <a name="migrating-biztalk-server-maps"></a>BizTalk sunucusunu taşıma eşlemeleri
Microsoft Azure BizTalk Services BizTalk Server geçirmek için bir aracı için BizTalk Services dönüşümler eşlemeleri sağlar. **BTMMigrationTool** olarak kullanılabilir parçası **Araçları** ile sağlanan paketini [BizTalk Services SDK'sını indirme](http://go.microsoft.com/fwlink/p/?LinkId=235057). Aracı hakkında daha fazla bilgi için bkz: [BizTalk Services dönüştürmek için BizTalk harita Dönüştür](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Bir örneğe Sandro Pereira, BizTalk MVP tarafından nasıl de bakabilirsiniz [BizTalk Services dönüşümler BizTalk Server eşlemeleri geçirmek](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Düzenlemelerin
BizTalk Server orchestration Microsoft Azure için işleme geçirmek gerekiyorsa, düzenlemelerin Microsoft Azure çalışan BizTalk Server düzenlemelerin desteklemediğinden yazılması gerekir.  Windows Workflow Foundation 4.0 (WF4) hizmetinin orchestration işlevleri yeniden yazabilirsiniz.  Şu anda hiçbir geçiş BizTalk Server düzenlemelerin WF4 olduğundan bu tam bir yeniden yazma olacaktır. Windows iş akışı için bazı kaynaklar aşağıda verilmiştir:

* [*Bir WCF iş akışı hizmeti Service Bus kuyrukları ve konularından ile tümleştirmek nasıl* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) Paolo Salvatori tarafından. 
* [*Windows Workflow Foundation ve Microsoft Azure ile uygulamaları oluşturmaya* oturum](http://go.microsoft.com/fwlink/p/?LinkId=237314) yapı 2011 konferans gelen.
* [*Windows Workflow Foundation Geliştirici Merkezi* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) konusuna bakın.
* [*Windows Workflow Foundation 4 (WF4) belgeleri* ](https://msdn.microsoft.com/library/dd489441.aspx) konusuna bakın.

## <a name="other-considerations"></a>Diğer konular
BizTalk Services kullanırken yapmanız gereken birkaç dikkat edilecek noktalar aşağıda verilmiştir.

### <a name="fallback-agreements"></a>Geri dönüş sözleşmeleri
BizTalk Server EDI işlemeyi "Geri dönüş anlaşmaları" kavramı vardır.  BizTalk Services mu **değil** bir geri dönüş sözleşmesi kavramı kadarki sahip.  BizTalk belgeleri konulara bakın [rolü, sözleşmelerde EDI işlemeyi](http://go.microsoft.com/fwlink/p/?LinkId=237317) ve [yapılandırma genel ya da geri dönüş sözleşmesi özelliklerini](https://msdn.microsoft.com/library/bb245981.aspx) geri dönüş anlaşmaları BizTalk nasıl kullanıldığı hakkında bilgi için Sunucu.

### <a name="routing-to-multiple-destinations"></a>Birden çok hedefe yönlendirme
BizTalk Services köprüleri, geçerli durumunda bir yayımlama kullanarak birden çok hedefe yönlendirme iletileri desteklemiyor-model abone olun. Bunun yerine bir BizTalk Services köprüsü iletilerden sonra birden fazla uç noktada ileti almak için birden fazla abonelik olabilir bir Service Bus konu için rota.

## <a name="conclusion"></a>Sonuç
Microsoft Azure BizTalk Services daha fazla özellik ve yetenekler eklemek için normal kilometre güncelleştirilir. Her güncelleştirme ile BizTalk Services ve diğer Azure teknolojilerini kullanarak uçtan uca çözümler oluşturma kolaylaştırmak için işlevsellik destek umuyoruz.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure ile Kurumsal uygulamaları geliştirme](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
