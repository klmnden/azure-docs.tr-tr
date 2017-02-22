---
title: "İş akışları ile uygulamaları bağlama ve verileri tümleştirme - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps ile uygulamaları bağlayıp verileri tümleştirerek iş akışları oluşturun ve işlemleri otomatik hale getirin."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
translationtype: Human Translation
ms.sourcegitcommit: 11aa3f74d112244cd96278c2b2e3d701e031aee8
ms.openlocfilehash: 6415b0e1c7feb744c6c13a0ae19ed434b6e9befc


---
# <a name="what-are-logic-apps"></a>Logic Apps nedir?
Logic Apps ölçeklenebilir tümleştirmeleri ve iş akışlarını basitleştirmenin ve buluta uygulamanın bir yolunu sağlar. İşleminizi iş akışı olarak bilinen bir dizi adım olarak modelleyen ve otomatikleştiren bir görsel tasarımcı sağlar.  Bulutta ve şirket içinde hizmet ve protokolleri hızlıca tümleştirmeye yönelik [çok sayıda bağlayıcı](../connectors/apis-list.md) vardır.  Mantıksal uygulama bir tetikleyici ile başlar ('Dynamics CRM’e bir hesabın eklenmesi' gibi) ve başlatma sonrasında çok sayıda birleştirme eylemi, dönüştürme ve koşul mantığı başlayabilir.

Logic Apps kullanmanın avantajları şunlardır:  

* Anlaşılması kolay tasarım araçları kullanan karmaşık işlemler tasarlayarak zaman kazandırma
* Aksi takdirde koda uygulanması zor olan desenleri ve iş akışlarını sorunsuz bir şekilde uygulama
* Şablonlardan hızlı bir şekilde çalışmaya başlama
* Mantıksal uygulamanızı kendi özel API'leriniz, kodunuz ve eylemlerinizle özelleştirme
* Ayrık sistemleri şirket içinde ve bulutta bağlama ve eşitleme
* BizTalk server, API Management, Azure İşlevleri ve Azure Service Bus’ı birinci sınıf tümleştirme desteği ile derleme

Logic Apps, geliştiricilerin barındırma, ölçeklenebilirlik, kullanılabilirlik ve yönetim oluşturma endişesi taşımamasına imkan tanıyan, tam yönetilen bir iPaaS (Hizmet olarak Tümleştirme Platformu) hizmetidir.  Logic Apps talebi karşılamak üzere otomatik olarak ölçeklenir.

![Akış uygulama tasarımcısı](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Daha önce bahsedildiği gibi Logic Apps ile iş süreçlerini otomatik hale getirebilirsiniz. Aşağıda birkaç örnek verilmiştir:  

* Bir FTP sunucusuna yüklenmiş dosyaları Azure Storage’a taşıma
* Şirket işi ve bulut sistemlerinde siparişleri işleme ve yönlendirme
* Belirli bir konu hakkındaki tüm tweet izleme, düşünceleri çözümleme ve takip gerektiren öğeler için uyarılar ve görevler oluşturma.

Bunlar gibi senaryoların tümü görsel tasarımcıdan ve tek bir satır kod yazmadan yapılandırılabilir. [Mantıksal uygulamanızı derlemeye][create] hemen başlayın.  Mantıksal uygulama yazıldıktan sonra birden fazla ortam ve bölgeye [hızlı bir şekilde dağıtılabilir ve yeniden yapılandırılabilir](../logic-apps/logic-apps-create-deploy-template.md).

## <a name="why-logic-apps"></a>Logic Apps neden kullanılmalıdır?
Logic Apps kuruluş tümleştirme alanına hız ve ölçeklenebilirlik kazandırır.  Tasarımcının kullanım kolaylığı, kullanılabilir tetikleyici ve eylemlerin çeşitliliği ve güçlü yönetim araçları API’lerinizi merkezi hale getirmeyi hiç olmadığı kadar kolaylaştırır.  İşletmeler dijitalleştirmeye doğru ilerlerken Logic Apps eski ve modern sistemleri birbirine bağlamanıza olanak sağlar.

Ayrıca, [Enterprise Integration Hesabı][biztalk] ile [XML mesajlaşması][xml], [ticari ortak yönetimi][tpm] ve daha fazlasının gücü sayesinde olgun tümleştirme senaryolarında ölçeklendirme yapabilirsiniz.

* **Kullanımı kolay tasarım araçları** - Logic Apps tarayıcıda veya Visual Studio araçları ile uçtan uca tasarlanabilir. Tetikleyici ile başlayın: basit bir zamanlamadan bir GitHub sorunu oluşturulmasına kadar. Ardından zengin bağlayıcı galerisini kullanarak dilediğiniz sayıda eylemi düzenleyin.
* **API’leri kolayca bağlayın** - Açıklanması kolay olan oluşturma görevlerinin bile koda uygulanması zordur. Logic Apps farklı sistemleri birbirine bağlamayı kolaylaştırır. Bulut pazarlama çözümünüzü şirket içi faturalama sisteminize bağlamak mı istiyorsunuz? API’ler ile sistemler arasında mesajlaşmayı bir Enterprise Service Bus ile merkezi hale getirmek mi istiyorsunuz? Logic Apps bu sorunlara çözüm getirmenin en hızlı ve en güvenilir yoludur.
* **Şablonlardan hızlıca başlayın** - Başlamanıza yardımcı olmak üzere bazı yaygın çözümleri hızlıca oluşturmanıza imkan tanıyan bir [şablon galerisi][templates] sunulmuştur. Galeri, Gelişmiş B2B çözümlerinden basit SaaS bağlantısına ve hatta 'eğlencelik' olan birkaç özelliğe kadar her şey için, Logic Apps’in gücünü kullanmaya başlamanın en hızlı yoludur.
* **Desteklenmiş genişletilebilirlik** - İhtiyaç duyduğunuz bağlayıcıyı görmüyor musunuz? Logic Apps kendi API’leriniz ve kodunuzla çalışacak şekilde tasarlanmıştır; kendi API uygulamanızı özel bir bağlayıcı kullanacak şekilde kolayca oluşturabilir veya bir [Azure İşlevi](https://functions.azure.com)’ne çağırarak kod parçacıklarını talep üzerine yürütebilirsiniz. 
* **Gerçek tümleştirme gücü** - Basitten başlayın ve gerektikçe büyütün. Logic Apps, profesyonellerin ihtiyaç duydukları çözümleri oluşturmasını sağlayan sektör lideri Microsoft çözümü BizTalk’tan kolayca yararlanır. [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin.

## <a name="logic-app-concepts"></a>Mantıksal Uygulama Kavramları
Logic Apps deneyimini oluşturan bazı temel parçalar aşağıda verilmiştir. 

* **İş Akışı** - MLogic Apps ş süreçlerinizi bir dizi adım veya bir iş akışı olarak modellemenizi sağlayan grafiksel bir yöntem sunar.
* **Yönetilen Bağlayıcılar** - Mantıksal uygulamalarınızın veri ve hizmetlere erişmesi gerekir. Yönetilen bağlayıcılar, verilerinize bağlanırken ve verilerinizle çalışırken size yardımcı olmak üzere özel olarak oluşturulur. Şu anda kullanılabilen bağlayıcıların listesi için bkz. [yönetilen bağlayıcılar][managedapis].
* **Tetikleyiciler** - Bazı Yönetilen Bağlayıcılar tetikleyici olarak da hareket edebilir. Tetikleyici bir e-postanın gelmesi veya Azure Storage hesabınızdaki bir değişiklik gibi belirli bir olaya göre bir iş akışının yeni bir örneğini başlatır.
* **Eylemler** - Bir iş akışında tetikleyiciden sonraki her adım eylem olarak adlandırılır. Her eylem genellikle yönetilen bağlayıcı veya özel API uygulamalarınızdaki bir işlemle eşlenir.
* **Enterprise Integration Pack** - Daha gelişmiş tümleştirme senaryoları için Logic Apps, BizTalk özelliklerini içerir. BizTalk, Microsoft'un sektör lideri tümleştirme platformudur. Enterprise Integration Pack bağlayıcıları Logic App iş akışlarınıza doğrulama, dönüşüm ve daha fazlasını kolayca dahil etmenize imkan tanır.

## <a name="getting-started"></a>Başlarken
* Logic Apps’i kullanmaya başlamak için [Mantıksal Uygulama oluşturma][create] öğreticisini izleyin.  
* [Sık rastlanan örnekleri ve senaryoları inceleyin](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Logic Apps ile iş süreçlerini otomatik hale getirebilirsiniz](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Sistemlerinizi Logic Apps ile nasıl tümleştireceğinizi öğrenin](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md



<!--HONumber=Jan17_HO4-->


