---
title: Azure Blockchain çalışma ekranı mimarisi
description: Azure Blockchain çalışma ekranı mimarisi ve bileşenleri genel bakış.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a6c415123a8e1254e214af69711fc4143a929a58
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-blockchain-workbench-architecture"></a>Azure Blockchain çalışma ekranı mimarisi

Azure Blockchain çalışma ekranı, çeşitli Azure bileşenleri kullanan bir çözüm sağlayarak blockchain uygulama geliştirme basitleştirir. Blockchain çalışma ekranı Azure Marketi'nde bir çözüm şablonu kullanılarak dağıtılabilir. Şablon Blockchain çalışma ekranı ile istemci uygulama, türünü blockchain yığını gibi dağıtıp IOT tümleştirmeyi bileşenleri ve modülleri seçmelerini sağlar. Uygulama dağıtıldıktan sonra Blockchain çalışma ekranı bir web uygulaması, iOS uygulaması ve Android uygulaması erişim sağlar.

![Blockchain çalışma ekranı mimarisi](media/blockchain-workbench-architecture/architecture.png)

## <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulama

Blockchain çalışma ekranı kullanarak, Azure Active Directory (Azure AD) kullanarak kurumsal kimlikleri Konsorsiyumu devredebilir. Çalışma ekranı Azure AD'de depolanan Kurumsal kimlikleri zincir çubuğunda kimliklerle yeni kullanıcı hesaplarını oluşturur. Kimlik eşleme kimliği doğrulanan oturum açma istemci API ve uygulamalara kolaylaştırır ve kimlik doğrulama ilkeleri, kuruluşların kullanır. Çalışma ekranı Ayrıca belirli bir akıllı sözleşme içinde belirli rollere Kurumsal kimlikleri ilişkilendirmek olanağı sağlar. Ayrıca, çalışma ekranı ayrıca eylemleri tanımlamak için bir mekanizma roller sağlar alabilir ve ne zaman.

Blockchain çalışma ekranı dağıtıldıktan sonra kullanıcılar ya da istemci uygulamaları, REST tabanlı istemci API ya da ileti API aracılığıyla Blockchain çalışma ekranı etkileşim. Her durumda, etkileşimleri, Azure Active Directory (Azure AD) veya aygıta özgü kimlik bilgileri üzerinden kimliğinin doğrulanması gerekir.

Kullanıcılar kendi e-posta adresinde bir e-posta davet katılımcılara göndererek kimliklerini Konsorsiyumu Azure AD birleştirmeyi. Oturum açarken bu kullanıcı adı, parola ve ilkeleri kullanılarak doğrulanır. Örneğin, iki faktörlü kimlik doğrulaması, kuruluşları.

Azure AD Blockchain çalışma ekranı erişimi olan tüm kullanıcıları yönetmek için kullanılır. Bir akıllı sözleşmesine bağlanan her aygıt, Azure AD ile de ilişkilidir.

Azure AD, özel yönetici grubuna kullanıcılara atamak için de kullanılır. Yönetici grubu ile ilişkili kullanıcılar yönetici hakları/Eylemler Blockchain sözleşmeleri dağıtma ve bir sözleşme erişmek için bir kullanıcı için izinleri vermeyi gibi çalışma ekranının içinden, sağlarlar. Bu grup dışındaki kullanıcılar yönetici eylemleri erişimi.

## <a name="client-applications"></a>İstemci uygulamaları

Çalışma ekranı web ve mobil (iOS, Android), doğrulama, test ve blockchain uygulamaları görüntülemek için kullanılabilecek otomatik olarak oluşturulan istemci uygulamalarını sağlar. Uygulama arabirimi akıllı sözleşme meta verileri temel alarak dinamik olarak oluşturulur ve herhangi bir kullanım durumu barındırabilir. İstemci uygulamaları kullanıcı dönük ön uç Blockchain çalışma ekranı tarafından oluşturulan tam blockchain uygulamalara sunar. İstemci uygulamaları Azure Active Directory (Azure AD) aracılığıyla kullanıcıların kimliğini doğrulamak ve akıllı sözleşme iş bağlamı için tasarlanmış bir kullanıcı deneyimi sunar. Kullanıcı deneyimi, yetkili kişiler tarafından yeni akıllı sözleşme örnekleri oluşturmanıza olanak tanıyan ve akıllı sözleşme temsil eden iş işleminde bazı türlerdeki işlemlerin uygun noktalarda yürütme yeteneğini gösterir.

Web uygulamasında, yetkili kullanıcıların Yönetici Konsolu erişebilir. Konsol Azure AD'de Yönetici grubundaki kullanıcılar tarafından kullanılabilir ve aşağıdaki işlevlere erişim sağlar:

* Yaygın senaryolar için akıllı sözleşmeleri sağlanan Microsoft dağıtın. Örneğin, bir varlık aktarımı senaryosu.
* Karşıya yükleme ve kendi akıllı sözleşmeleri dağıtın.
* Kullanıcı erişimi belirli bir rolü bağlamındaki akıllı sözleşme atayın.

## <a name="gateway-service-api"></a>Ağ Geçidi Hizmeti API'si

Blockchain çalışma ekranı REST tabanlı ağ geçidi hizmeti API'si içerir. Bir blockchain yazarken, API oluşturur ve bir olay Aracısı iletileri sunar. Veri API tarafından istendiğinde, sorgular zinciri kapalı SQL veritabanına gönderilir. SQL veritabanı bir zincir üzerinde verileri ve desteklenen akıllı sözleşmeleri için içerik ve yapılandırma bilgileri sağlar meta verileri içerir. Sorguları sözleşme için meta veriler tarafından haberdar bir biçimde zinciri kapalı çoğaltmadan gerekli verileri döndürür.

Geliştiriciler, yapı veya Blockchain çalışma ekranı istemci uygulamaları kullanmadan blockchain çözümlerini tümleştirmek için ağ geçidi hizmeti API'si erişebilir.

> [!NOTE]
> API kimlik doğrulamalı erişimi etkinleştirmek için iki istemci uygulamaları Azure Active Directory'ye kaydedilir. Azure Active Directory ayrı uygulama kayıtlar her uygulama türü gerektirir (yerel ve web). 

## <a name="message-broker-for-incoming-messages"></a>Gelen iletiler için ileti Aracısı

Doğrudan Blockchain çalışma ekranına iletileri göndermek için isteyen geliştiriciler, doğrudan Service Bus ileti gönderebilir. Örneğin, iletileri API sistem için tümleştirme veya IOT cihazları için kullanılabilir.

## <a name="message-broker-for-downstream-consumers"></a>İleti aracısı için aşağı akış tüketicileri

Uygulama yaşam döngüsü sırasında olayları oluşur. Olaylar, ağ geçidi API tarafından ya da muhasebe tetiklenebilir. Olay bildirimleri olayına göre aşağı akış kod başlatabilirsiniz.

Blockchain çalışma ekranı otomatik olarak iki tür olay tüketicileri dağıtır. Bir zincir kapalı SQL deposuna doldurmak için blockchain olaylar tarafından tetiklenir. Diğer meta verileri karşıya yükleme ve belgelerin depolama ilgili API tarafından oluşturulan olaylar için yakalamaktır.

## <a name="message-consumers"></a>İleti tüketicileri

 İleti tüketiciye hizmet yolundan iletileri alın. Temel olay modeli ileti tüketicileri için ek hizmetler ve sistemler için uzantıları sağlar. Örneğin, CosmosDB doldurmak veya Azure akış analizi kullanarak iletileri değerlendirmek için destek ekleyebilirsiniz. Aşağıdaki bölümlerde Blockchain çalışma ekranı dahil ileti tüketicileri açıklanmaktadır.

### <a name="distributed-ledger-consumer"></a>Dağıtılmış defter tüketici

Dağıtılmış defter teknolojisi (DLT) iletileri blockchain yazılacak işlemler için meta veriler içerir. Tüketici iletileri alır ve verileri bir işlem Oluşturucu, imzalayan ve yönlendirici iter.

### <a name="database-consumer"></a>Veritabanı tüketici

Veritabanı tüketici hizmeti veri yolundan iletileri alır ve SQL veritabanı gibi ekli bir veritabanına veri iter.

### <a name="storage-consumer"></a>Depolama tüketici

Depolama tüketici hizmeti veri yolundan iletileri alır ve bağlı bir depolama alanına veri iter. Örneğin, karma belgeleri Azure Storage'da depolamak.

## <a name="transaction-builder-and-signer"></a>İşlem oluşturucusu ve imzalayan

Gelen ileti Aracısı iletide blockchain yazılması gerekiyorsa DLT tüketici tarafından işlenir. Yürütmek istenen işlem için meta verileri içeren iletiyi alır ve bilgi gönderdiği bir hizmet DLT tüketicisidir *işlem oluşturucusu ve imzalayan*. *İşlem oluşturucusu ve imzalayan* veri ve istenen blockchain hedef dayalı bir blockchain işlem derler. Birleştirilen sonra işlem imzalanır. Özel anahtarları Azure anahtar Kasası ' depolanır.

 Blockchain çalışma ekranı uygun özel anahtarı anahtar Kasası'nı alır ve anahtar kasası dışında hareket imzalar. Oturum sonra işlem işlem yönlendiriciler ve defterleri gönderilir.

## <a name="transaction-routers-and-ledgers"></a>İşlem yönlendiriciler ve defterleri

İşlem yönlendiriciler ve defterleri imzalı işlemleri alın ve bunları uygun blockchain yol. Şu anda Blockchain çalışma ekranı Ethereum kendi hedef blockchain destekler.

## <a name="dlt-watcher"></a>DLT İzleyicisi

Dağıtılmış defter teknolojisi (DLT) İzleyici Blockchain çalışma ekranına bağlı blok zincirleri üzerinde gerçekleşen olayların izler.
Olaylar, kişiler ve sistemler ilgili bilgileri yansıtır. Örneğin, yeni sözleşme örnekleri, işlemleri yürütülmesi ve durumu değişiklikleri oluşturma. Olayları yakalanan ve aşağı akış tüketiciler tarafından tüketilen giden ileti aracısı için gönderilebilir.

Örneğin, SQL tüketici olayları izler, bunları kullanır ve SQL veritabanı dahil edilen değerlerde ile doldurur. Kopya bir zincir kapalı deposundaki zinciri üzerindeki veri çoğaltmasını dinlenme sağlar.

## <a name="azure-sql-database"></a>Azure SQL veritabanı

Azure SQL veritabanı Blockchain çalışma ekranına ekli sözleşme tanımları, yapılandırma meta verilerini ve blockchain depolanan verileri SQL erişilebilir bir kopyasını depolar. Bu verileri kolayca sorgulanan, görselleştirilen veya veritabanına doğrudan erişerek analiz edilir. Geliştiriciler ve diğer kullanıcıların veritabanı raporlama, analytics veya diğer veri merkezli tümleştirmeleri için kullanabilirsiniz. Örneğin, kullanıcıların Power BI kullanarak işlem verileri görselleştirebilirsiniz.

Bu zincir devre dışı depolama kuruluşlar veri SQL yerine blockchain defter yeteneği sağlar. Ayrıca, blockchain teknoloji yığını bağımsızdır standart bir şema üzerinde standart hale getirerek, zinciri devre dışı depolama raporları ve diğer yapıları kullanılmasını projeleri, senaryoları ve kuruluşlar arasında sağlar.

## <a name="azure-storage"></a>Azure Storage

Azure depolama sözleşmeler ve sözleşmelerle ilişkili meta verileri depolamak için kullanılır.

Satınalma siparişi ve konşimentoları, haber ve tıbbi görüntülerin video Polis gövde kameraları ve ana hareket resim dahil continuum kaynaklanan için kullanılan görüntülerine belgeleri birçok blockchain merkezli senaryolarda bir rol oynar. Belgeleri doğrudan üzerinde blockchain yerleştirmek uygun değildir.

Blockchain çalışma ekranı belgeleri veya diğer medya içeriği blockchain iş mantığı ile ekleme yeteneği destekler. Belge veya medya içeriğin karmasını blockchain depolanır ve gerçek belge veya medya içeriği Azure depolama alanında depolanır. İlişkili işlem bilgileri gelen ileti aracısı için teslim, paketlenir, imzalı ve blockchain yönlendirilir. Bu işlem giden ileti Aracısı paylaşılan olayları tetikler. SQL DB bu bilgiyi kullanır ve daha sonra sorgulamak için DB gönderir. Aşağı Akış sistemleri de uygun olarak davranacak şekilde bu olayları tüketebilir.

## <a name="monitoring"></a>İzleme

Çalışma ekranı Application Insights ve Azure İzleyicisi'ni kullanarak uygulama günlüğü sağlar. Application Insights Blockchain çalışma ekranı günlüğe kaydedilen tüm bilgileri depolamak için kullanılır ve hatalar, uyarılar ve başarılı işlemleri içerir. Application Insights geliştiriciler tarafından Blockchain çalışma ekranı ile ilgili sorunları hata ayıklamak için kullanılabilir. 

Azure İzleyicisi blockchain ağ sistem durumu hakkında bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain çalışma ekranı dağıtma](blockchain-workbench-deploy.md)