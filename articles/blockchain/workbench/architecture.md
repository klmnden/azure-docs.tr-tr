---
title: Azure Blockchain Workbench mimarisi
description: Azure Blockchain Workbench mimarisini ve bileşenlerini genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 83c5e1405c402a1c6c98f9dbcaaf74891eb75e6d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61437772"
---
# <a name="azure-blockchain-workbench-architecture"></a>Azure Blockchain Workbench mimarisi

Azure Blockchain Workbench, çeşitli Azure bileşenlerinden kullanarak bir çözüm sağlayarak blockchain uygulama geliştirmeyi basitleştirir. Blockchain Workbench'i Azure Marketi'nde bir çözüm şablonu kullanılarak dağıtılabilir. Şablon, modüller ve istemci uygulaması türü blok zinciri yığın dahil olmak üzere, dağıtma ve IOT tümleştirmeyi bileşenleri seçmenize olanak sağlar. Blockchain Workbench'i, uygulama dağıtıldıktan sonra bir web uygulaması, iOS uygulaması ve Android uygulaması erişim sağlar.

![Blockchain Workbench'i mimarisi](./media/architecture/architecture.png)

## <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulaması

Blockchain Workbench'i kullanan, Kurumsal kimlikleri Azure Active Directory (Azure AD) kullanarak bir konsorsiyum ad'sini birleştirebilir. Workbench, Azure AD'de depolanan Kurumsal kimlikleri zincir kimliklerle yeni kullanıcı hesaplarını oluşturur. Kimlik eşleme kimliği doğrulanmış oturum açma için istemci API'leri ve uygulamaları kolaylaştırır ve kimlik doğrulama ilkeleri, kuruluşların kullanır. Workbench, Kurumsal kimlikleri verilen bir akıllı sözleşme belirli rollere ilişkilendirmek için özelliği de sağlar. Ayrıca, Workbench ayrıca eylemleri tanımlamak için bir mekanizma roller sağlar alabilir ve ne zaman.

Blockchain Workbench'i dağıtıldıktan sonra kullanıcılara istemci uygulamaları, REST tabanlı istemci API veya Mesajlaşma API'si aracılığıyla Blockchain Workbench ile etkileşim kurun. Her durumda, etkileşimleri, Azure Active Directory (Azure AD) veya cihaza özel kimlik bilgileri aracılığıyla kimliğinin doğrulanması gerekir.

Kullanıcılar kimliklerini bir konsorsiyum Azure AD için kendi e-posta adresinde bir e-posta davetiyesi katılımcılara göndererek federasyona. Oturum açılırken, bu kullanıcı adı, parola ve ilkeleri kullanarak kimlik doğrulaması yapılır. Örneğin, iki öğeli kimlik doğrulama kendi kuruluşları.

Azure AD, Blockchain Workbench'i erişimi olan tüm kullanıcıları yönetmek için kullanılır. Her cihaz için akıllı bir sözleşme bağlanma, Azure AD ile de ilişkilidir.

Azure AD, özel yönetici grubuna kullanıcılara atamak için de kullanılır. Yönetici grubu ile ilişkili kullanıcı hakları ve Eylemler içinde Blockchain Workbench'i sözleşmeleri dağıtma ve bir kullanıcı için izinleri vermeyi de dahil olmak üzere bir sözleşme erişmek için erişim verilir. Bu grup dışındaki kullanıcılara yönetici eylemlerine erişim yok.

## <a name="client-applications"></a>İstemci uygulamaları

Workbench, web ve mobile (iOS, Android), doğrulama, test etme ve blok zinciri uygulamaları görüntülemek için kullanılan otomatik olarak oluşturulan istemci uygulamalar sağlar. Uygulama arabirimi akıllı sözleşme meta verileri temel alarak dinamik olarak oluşturulur ve tüm kullanım örneği barındırabilir. İstemci uygulamaları, kullanıcıya yönelik bir ön uç Blockchain Workbench tarafından oluşturulan tüm blok zinciri uygulamaları sunun. İstemci uygulamaları, Azure Active Directory (Azure AD) ile kullanıcıların kimliğini doğrulamak ve ardından uyarlanmış akıllı sözleşme iş bağlamında bir kullanıcı deneyimi sunar. Kullanıcı deneyimi yeni akıllı sözleşme örnekleri yetkili kişiler tarafından oluşturulmasını sağlar ve ardından akıllı sözleşmeyi temsil eden iş işleminde bazı türlerdeki işlemlerin uygun noktalarda yürütme yeteneği sunar.

Web uygulamasında, Yönetici konsolunda yetkili kullanıcılar erişebilir. Konsolu, Azure AD'de Yönetici grubundaki kullanıcılar tarafından kullanılabilir ve aşağıdaki işlevlere erişim sağlar:

* Nitelikli akıllı anlaşmalar popüler senaryolar için sağlanan Microsoft dağıtın. Örneğin, bir varlık aktarımı senaryo.
* Karşıya yükleme ve kendi nitelikli akıllı anlaşmalar dağıtın.
* Kullanıcı erişimi belirli bir rolü bağlamında akıllı sözleşme atayın.

Daha fazla bilgi için [github'daki Azure Blockchain Workbench örnek istemci uygulamaları](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-development-kit/connect/mobile/blockchain-workbench/workbench-client).

## <a name="gateway-service-api"></a>Ağ Geçidi Hizmeti API'si

Blockchain Workbench'i REST tabanlı ağ geçidi hizmeti API'si içerir. Bir blok zincirine yazarken API oluşturur ve iletileri için bir olay aracısından teslim eder. Veri API'si tarafından istendiğinde, sorgular dışı SQL veritabanına gönderilir. SQL veritabanı bir zincir içi verileri zincir ve desteklenen nitelikli akıllı anlaşmalar için içerik ve yapılandırma bilgileri sağlayan meta verileri içerir. Sorguları, sözleşme için meta verileri tarafından sürekli bir biçimde dışı çoğaltmadan gerekli verileri döndürür.

Geliştiricilerin, derleme veya Blockchain Workbench'i istemci uygulamalar üzerinde bağlı olmadan blockchain çözümlerini tümleştirmek için ağ geçidi hizmeti API'si erişebilirsiniz.

> [!NOTE]
> API'sine kimliği doğrulanmış erişimi etkinleştirmek için iki istemci uygulamaları Azure Active Directory'ye kaydedilir. Azure Active Directory, her uygulama türü farklı uygulama kayıtları gerektirir (yerel ve web). 

## <a name="message-broker-for-incoming-messages"></a>Gelen iletiler için ileti Aracısı

Blockchain Workbench'i doğrudan ileti göndermek için isteyen geliştiriciler, doğrudan Service Bus iletileri gönderebilir. Örneğin, iletileri API sistem için tümleştirme veya IOT cihazları için kullanılabilir.

## <a name="message-broker-for-downstream-consumers"></a>Aşağı Akış Tüketiciler için ileti Aracısı

Uygulama yaşam döngüsü boyunca, olaylar gerçekleşir. Olaylar, ağ geçidi API'si tarafından veya bir kayıt defteri tetiklenebilir. Olay bildirimleri, olay tabanlı akış kod başlatabilirsiniz.

Blockchain Workbench'i iki türde olay tüketicileri otomatik olarak dağıtır. Tek bir tüketici dışı SQL deposunun doldurulması için blok zinciri olayları tarafından tetiklenir. Diğer tüketici meta verilerini karşıya yükleme ve belgelerin depolanması ilgili API tarafından oluşturulan olayları yakalama sağlamaktır.

## <a name="message-consumers"></a>İleti tüketiciler

 İleti tüketiciler Service Bus'tan ileti alın. İleti Tüketiciler için temel alınan olay modeli için ek hizmetleri ve sistemleri uzantıları sağlar. Örneğin, CosmosDB doldurmak veya Azure akış analizi kullanarak iletileri değerlendirmek için destek ekleyebilirsiniz. Aşağıdaki bölümlerde, Blockchain Workbench'i dahil ileti tüketiciler açıklanmaktadır.

### <a name="distributed-ledger-consumer"></a>Dağıtılmış bir defter tüketici

Dağıtılmış kayıt defteri teknolojisini (DLT) iletiler için blok zinciri yazılacak işlemleri için meta verileri içerir. Tüketici, iletileri alır ve bir işlem Oluşturucu, imzalayan ve yönlendirici verileri gönderir.

### <a name="database-consumer"></a>Veritabanı tüketici

Veritabanı tüketici, Service Bus iletileri alır ve SQL veritabanı gibi ekli bir veritabanına veri gönderir.

### <a name="storage-consumer"></a>Depolama tüketici

Depolama tüketici, Service Bus iletileri alır ve bağlı bir depolama alanına veri gönderir. Örneğin, karma belgeleri, Azure depolamada depolamak.

## <a name="transaction-builder-and-signer"></a>İşlem oluşturucusu ve imzalayan

Gelen ileti Aracısı ileti blok zincirine yazılması, DLT tüketici tarafından işlenir. Yürütmek için istenen işlem için meta veriler içeren ileti alır ve ardından bilgileri gönderir, bir hizmet DLT tüketicisidir *işlem oluşturucusu ve imzalayan*. *İşlem oluşturucusu ve imzalayan* verileri ve istenen blockchain hedef dayalı bir blok zinciri işlem birleştirir. İşlem, birleştirilmiş bir kez imzalanır. Özel anahtarları Azure Key Vault'ta saklanır.

 Blockchain Workbench'i Key Vault'tan uygun özel anahtarı alır ve Key Vault dışında hareket imzalar. İmzalandıktan sonra işlemin işlem yönlendiriciler ve defterler gönderilir.

## <a name="transaction-routers-and-ledgers"></a>İşlem yönlendiriciler ve defterler

İşlem yönlendiriciler ve defterler işaretli işlemleri alın ve bunları uygun blok zinciri için yönlendirir. Şu anda, Blockchain Workbench'i Ethereum, hedef blok zinciri destekler.

## <a name="dlt-watcher"></a>DLT İzleyicisi

Dağıtılmış kayıt defteri teknolojisini (DLT) İzleyici Blockchain Workbench'i bağlı blok zincirleri üzerinde gerçekleşen olayları izler.
Olaylar, kişiler ve sistemleri ilgili bilgileri yansıtır. Örneğin, yeni sözleşme örnekleri, yürütme işlemlerinin yanı sıra, durum değişiklikleri oluşturma. Olayları yakalanır ve aşağı akış tüketiciler tarafından tüketilebilecek giden ileti aracısı için gönderilebilir.

Örneğin, SQL tüketici olayları izler, bunları kullanır ve SQL veritabanı dahil değerlerle doldurur. Kopya verileri zincir dışı deposunda çoğaltmasını mpgo.exe'nin sağlar.

## <a name="azure-sql-database"></a>Azure SQL veritabanı

Blockchain Workbench'i bağlı Azure SQL veritabanı, sözleşme tanımları, yapılandırma meta verilerini ve blok zincirine yatırım depolanan verilerin SQL erişilebilir bir çoğaltma depolar. Bu verileri kolayca sorgulanan, görselleştirilmiş veya veritabanına doğrudan erişerek analiz edilir. Geliştiriciler ve diğer kullanıcılar veritabanı, raporlama, analiz veya veri merkezli diğer tümleştirmeler için kullanabilirsiniz. Örneğin, kullanıcılar işlem verilerini Power bı'da görselleştirebilirsiniz.

Bu dışı depolama, veri SQL yerine bir blok zinciri defter büyük kuruluşlar için özelliği sağlar. Ayrıca, blockchain teknolojisi yığını belirsiz olduğundan standart bir şema üzerinde standart hale getirilirse dışı depolama raporları ve diğer yapıtları yeniden projeleri, senaryolar ve kuruluşlar arasında sağlar.

## <a name="azure-storage"></a>Azure Storage

Azure depolama, sözleşmeler ve sözleşmeleri ile ilişkili meta verileri depolamak için kullanılır.

Satın alma siparişleri ve konşimentoları, haber ve tıbbi tanımayı Polis gövdesi kameralar ve ana hareket resimler gibi bir continuum video kaynak kullanılan görüntülerine belgeleri blockchain merkezli senaryolarda bir rol oynar. Belgeleri doğrudan üzerinde blok zinciri yerleştirmek uygun değildir.

Blockchain Workbench belgeleri veya diğer medya içeriği blok zinciri iş mantığı ile ekleme özelliği destekler. Bir belge veya medya içeriğin karmasını, blok zincirine yatırım depolanır ve gerçek belge veya medya içeriği, Azure Depolama'da saklanır. İlişkili işlem bilgilerini gelen ileti aracısı için teslim, paketlenir, imzalı ve blok zincirine yönlendirilir. Bu işlemi giden ileti Aracısı paylaşılan olayları tetikler. SQL DB, bu bilgileri kullanır ve daha sonra sorgulamak için DB gönderir. Aşağı Akış sistemlerine Ayrıca bu olaylar uygun şekilde hareket tüketebilir.

## <a name="monitoring"></a>İzleme

Workbench, Application Insights ile Azure İzleyici uygulama günlüğü sağlar. Application Insights, Blockchain Workbench'i günlüğe kaydedilen tüm bilgileri depolamak için kullanılır ve hataları, uyarıları ve başarılı işlemleri içerir. Application Insights, Blockchain Workbench ile hata ayıklamak için geliştiriciler tarafından kullanılabilir. 

Azure İzleyici, ağın blok zinciri durumuyla ilgili bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’i dağıtma](../../blockchain-workbench/blockchain-workbench-deploy.md)