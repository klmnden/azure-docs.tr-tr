---
title: "Azure veri Kataloğu genel senaryoları | Microsoft Docs"
description: "Azure veri kaydı ve yüksek değerli veri kaynaklarını bulma gibi Self Servis iş zekası etkinleştirme ve veri kaynakları ve işlemleri hakkında mevcut bilgilerini yakalama Kataloğu genel senaryoları genel bakış."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 156710ad50349e8a3632e31c7752387d4449a65d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="azure-data-catalog-common-scenarios"></a>Azure Veri Kataloğu genel senaryoları
Bu makalede Azure veri Kataloğu, var olan veri kaynaklarından daha fazla değer almak, kuruluşunuzun burada yardımcı olabilir yaygın senaryolar sunar.

## <a name="scenario-1-registration-of-central-data-sources"></a>Senaryo 1: Kayıt Merkezi veri kaynakları
Kuruluşlar, çok yüksek değerli veri kaynağı genellikle sahiptir. Bu veri kaynaklarının iş satır, çevrimiçi işlem işleme (OLTP) sistemleri, veri ambarlarında ve iş zekası/analytics veritabanlarını içerir. Sistemleri ve aralarındaki çakışma sayısı, genellikle gelişmesi iş gereksinimleriniz ve örneğin, birleşmeler veya satın almalar sonucunda, iş dönüşmesi zamanla artar.

Bu veri kaynaklarının içinde verileri bulmak nereye bilmesi kuruluş üyeleri için zor olabilir. Aşağıdaki gibi soruları tüm çok ortaktır:

* Şirket içinde kullanılan üç HR sistemlerinin, bu tür bir rapor oluşturmak için kullanmalıyım?
* Sertifikalı satış numaraları yalnızca sona erdi mali yılın almak için nereye gitmek?
* Kimin ı istemelisiniz ya da veri ambarını erişmek için kullanması gereken işlemi nedir?
* Bu değerler doğru olup olmadığını bilmek yok. Kimin ı öngörüleri için nasıl bu verileri ı my ekibi ile Bu panoyu paylaşmak önce kullanılması gerektiği üzerinde sorabilir miyim?

Azure veri Kataloğu, bunlar ve diğer sorular için yanıtlar sağlayabilir. Kuruluş genelinde kullanılan merkezi yüksek değerli, BT tarafından yönetilen veri kaynakları genellikle katalog doldurmak için mantıksal başlangıç noktasıdır. Herhangi bir kullanıcı bir veri kaynağı kaydedebilirsiniz rağmen kullanıcılar için en büyük sayı değeri sağlamak büyük olasılıkla veri kaynaklarıyla kick-started katalog sahip benimsenmesini ve sistem kullanımı yardımcı olur. 

Azure veri Kataloğu ile başlıyorsanız tanımlayarak ve veri tüketicileri, birçok farklı ekip tarafından kullanılan önemli veri kaynaklarını kaydederek ilk adımınız başarılı olabilir.

Bu senaryo ayrıca anlamak ve erişim daha kolay hale getirmek için yüksek değerli veri kaynaklarına açıklama fırsatı sunar. Temel bir yönü bu çaba, kullanıcılar veri kaynağına erişim nasıl isteyebilir bilgi eklemektir. Azure veri Kataloğu ile e-posta adresi kullanıcı veya mevcut araçlar veya belge için veri kaynağı erişimi denetleme, bağlantılar için sorumlu bir ekip veya erişim isteği işlemini açıklar serbest metin sağlayabilir. Bu bilgiler kim kayıtlı veri kaynaklarını bulmasına ancak henüz kolayca tanımlanır ve veri kaynağı sahipleri tarafından denetlenen işlemleri kullanarak erişim istemek için verilere erişim izni etkinleştirmemiş üyeleri yardımcı olur.

## <a name="scenario-2-self-service-business-intelligence"></a>Senaryo 2: Self Servis iş zekası
Birçok kuruluş veri Windows'un çok değerli yapan bir parçası olarak geleneksel kurumsal iş zekası çözümleri devam etmesine rağmen ayak iş kolu değiştirme BI Self Servis ve daha önemli yaptı. Self Servis BI kullanarak, bilgi çalışanlarının ve analistleri kendi raporları, çalışma kitaplarına ve panolar merkezi BT ekibi veya bu BT ekibin zamanlama ve kullanılabilirlik ile sınırlı kalmayarak öğesine bağlı kalmadan oluşturabilirsiniz.

Self Servis BI senaryolarda, kullanıcıların yaygın olarak birçok önceden BI ve analiz için kullanılmamış birden fazla kaynaktan veri birleştirin. Bu veri kaynaklarının bazıları zaten biliniyor olabilir ancak bulun ve olası veri kaynakları için belirli bir görevi değerlendirmek için yapmanız gerekenler bulmak için zor.

Geleneksel olarak, bu bulma işlemi el ile bir bilgisayardır: analistleri diğer kişilerin Aranan verilerle çalışmak tanımlamak için kendi eş ağ bağlantılarını kullanın. Bir veri kaynağı bulundu ve kullanılan sonra işlem tekrar her sonraki Self Servis BI çabayla, birden çok kullanıcı bulma yedekli el ile işlemi gerçekleştirmek için yinelenir.

Azure veri Kataloğu ile kuruluşunuz bu döngüsü çaba bozulabilir. Bir veri kaynağı geleneksel araçlarla öğrendiğinizde bir analist daha kolay bulunabilir diğer kullanıcılar tarafından gelecekte yapmak için bunu kaydedebilirsiniz. Analist kayıtlı veri varlıklarına açıklama tarafından daha fazla değer ekleyebilirsiniz, ancak bu ek açıklama kayıt aynı zamanda gerçekleşmesi gerekmez. Kullanıcılar kendi zamanlamaları izin olarak zaman içinde kademeli olarak ve kataloğa kayıtlı veri kaynaklarına değer ekleme katkıda bulunabilir.

Katalog içeriğinin organik bu büyüme merkezi veri kaynakları önceden kaydını tamamlayan bir doğal ' dir. Çok sayıda kullanıcı gerekir veri Kataloğu önceden doldurmak motivator ilk kullanım ve bulma olabilir. Kaydolun ve ek kaynaklarına açıklama bağlanmalarını sağlayarak bunları ve diğer kuruluş üyeleri bağlı tutmak için bir yol olabilir.

Bu, aynı desenleri ve zorluklar bu senaryo özellikle Self Servis BI odaklanıyor olsa da, büyük ölçekli şirket BI projeleri için de geçerlidir dikkate değerdir. Veri Kataloğu'nu kullanarak, kuruluşunuz veri kaynağı bulma işlemini elle içerir çaba artırabilir.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Senaryo 3: Yakalama grupsal bilgilere
İşinizi ve bu verileri nerede gerçekleştirmeniz gereken hangi verilerin nasıl bilebilirsiniz?

Bir süredir işinizde bırakıldı, büyük olasılıkla yalnızca bildiğiniz. İşlem öğrenme gradual gitti ve zaman içinde günlük iş anahtarına olan veri kaynakları hakkında öğrendiniz.

Nasıl yeni bir çalışan ekibinizin katıldığında, o kişiyi hangi verilerin iş için gereklidir ve nerede bulacağını biliyor?

Büyük olasılıkla olan yeni bir kişiye size bu sorularla birlikte gelir.

Devam eden bu grupsal bilgilere aktarımını küçük ve büyük kuruluşların veri kaynağı bulma işlemi bir parçasıdır. Daha üst düzey ve deneyimli takım üyeleri bilgi yıllar içinde yerleşik ve yeni ekip üyelerinin sorularınız varsa bunları sorulacak öğrendiniz. Yalnızca birkaç anahtar kişinin kafa sayısı genellikle en önemli bilgiler bulunmaktadır ve bu kişilere tatile olan veya takım bırakın, kuruluşun yükselmesine.

Veri uzmanlar normalde bilgilerini, belge için bir e-posta yoluyla veya takım SharePoint sitesinde Word belgelerinde paylaşımı çabayı. Bu yaklaşım değerli olabilse de, yeni bir bulma sorun sunar: nasıl kişiler bilmeniz hangi belge var ve bu nerede?

Azure veri Kataloğu ile tek, merkezi konumda depolamak ve bu grupsal bilgilere paylaşımı ve kolayca bulunabilir yapmak için kuruluşunuzun yok. Veri Kataloğu'nda, veri uzmanlar doğrudan veri varlıklarına açıklama ve var olan belgeler için bağlantılar sağlar. Kuruluş üyeleri bir veri kaynağı bulmak için katalog kullandığınızda, bunlar yalnızca kaynak kendisi, aynı zamanda daha önceden varolan Bilgi Bankası yalnızca kuruluşunuzun uzmanlar minds içinde bulabilirsiniz.
