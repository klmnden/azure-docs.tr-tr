---
title: "Azure Mobile Engagement benzer diğer Azure hizmetleriyle karşılaştırma"
description: "Diğer benzer Azure hizmetleriyle - HockeyApp, Appınsights'dan, bildirim hub'ları Azure Mobile Engagement karşılaştırma"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f114775-3a9a-4dd4-8d59-b10d1da9a68b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7df2eb9ecebe3313dad9c15171552a084787f6b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure Mobile Engagement benzer diğer Azure hizmetleriyle karşılaştırma
Microsoft Azure tarafından sunulan hizmetlerin listesini her zamankinden genişletme ve bazen, nasıl Azure Mobile Engagement yalnızca okuma veya hakkında duymak bu hizmet farklı merak ediyor olabilirsiniz. Bu makalede, karışıklığı temizlemek çalışır ve kullanımınız için en uygun olduğunda Azure Mobile Engagement seçmenizi yönlendirir. 

Azure Mobile Engagement, özellikle hedeflenen bir hizmettir **dijital Pazarlamacılar/CMOs için** tarafından kullanılabilir ancak **mobil uygulama sahibi veya yayımcı** isteyen kullanım, bekletme ve mobil uygulamalarını farklılaştırabilmelerini artırmak. 

*Veriye dayalı Öngörüler gerçek zamanlı kullanıcı Segmentasyonu, uygulama kullanımı sağlar ve bağlamsal anında iletme bildirimleri ve uygulama içi Mesajlaşma sağlayan bir hizmet olarak yazılım (SaaS) kullanıcı etkileşimi platformudur.* 

Bu tanım parçalamak, biz, ayrıca kendi benzersiz değer teklifinde önemli noktalar aşağıdaki anahtar özelliklere sahiptir:

1. **Bağlamsal anında iletme bildirimleri ve uygulama içi Mesajlaşma**
   
   Bu ürünün çekirdek odak - hedeflenen ve kişiselleştirilmiş anında iletme bildirimleri gerçekleştirin. Bunun yapılması, biz zengin davranış analizi veri toplamak ve. 
2. **Veriye dayalı Öngörüler uygulama kullanımı**
   
   Platformlar arası uygulama kullanıcılar hakkında davranış analizi toplamak için SDK'ları sunuyoruz. Terim davranış analizi (as against Performans Analizi) uygulama kullanıcıların uygulamayı nasıl kullandığını odaklanmak için unutmayın. Hataları, kilitlenme vb. ancak başka bir deyişle değil çekirdek odak ürünün hakkında temel performans analizi verileri toplarız. 
3. **Gerçek zamanlı kullanıcı Segmentasyonu**
   
   Uygulama kullanıcıların davranış analizi veri toplandığında, biz kitlenizi çeşitli parametreleri temel alınarak ve toplanan verileri hedeflenen anında iletme kampanyalarını çalıştırmanıza olanak sağlamak için kesim sağlar. 
4. **Yazılım olarak-hizmet (SaaS):**
   
   Hangi etkileşim ve uygulama kullanıcılar hakkında zengin davranış analizi görüntüleyebilir ve anında iletme kampanyalarını pazarlama çalıştırmak için optimize Azure Yönetim Portalı'ndan ayrı bir portal sahibiz. Bir süre içinde giderek sağlamak için ürün sağlamıştır!   

Bu kümesi farklılık el ile İşte nasıl biz karşı görünen benzer diğer Azure teklifleri karşılaştırmak - makaleyi özellik düzeyi karşılaştırması yapmaz ancak daha fazla senaryo alır Not hangi ürün en iyi tanımlamak için bir yaklaşım tabanlı:

1. [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) Microsoft'un mobil DevOps çözümüdür. Bununla, derlemeleri beta test edenlere dağıtabilir, kilitlenme verilerini toplayabilir ve kullanıcılara geri bildirim sağlayabilirsiniz. Ayrıca, Visual Studio Team kolay derleme dağıtımları ve çalışma öğesi tümleştirmeyi Etkinleştirme Hizmetleri ile tümleşir. 
   
   DevOps ve mobil uygulamalar hakkında performans analizi veri toplama burada odak noktasıdır. Her iki HockeyApps tümleştirme ile anlamayabilir ve Mobile Engagement uygulamanız ve olmasına karşın bazı örtüşme toplanan veriler, ürünlerin çekirdek odak farklı olduğundan ve sizin için farklı hedefler elde etmeye yardımcı olağan dışı olmaz.  
2. [Application Insights](../application-insights/app-insights-overview.md) mobil uygulamanıza bir sunucu tarafı sahip sonra uygulamanızın web sunucusu tarafını izlemek için Application Insights kullanır ancak istemci tarafı mobil uygulamalar için HockeyApp kullanmanız gerekir. 
3. [Bildirim hub'ları](https://azure.microsoft.com/services/notification-hubs/) mobil uygulamaya tümleşik bir SDK gerekmez ve mobil uygulamanızı üzerinde tam denetime sahip olabilir ve temel etiketleme özellikleri ile anında iletme Bildirimleri göndermeye izin verir herkese açık basit bir hizmet sunar. Bu, daha az hedefleme hakkında ve gönderme ve iletişim bilgileri anında kullanıcılara kendi uygulama (büyük bir popülasyonun bir yayına) hakkında daha fazla cares tüm uygulama sahibi için mükemmeldir. 
   
   Hızlı bildirimleri temel kesimleme özelliğine sahip üstün gönderme odağı burada unutmayın. 

Bazı senaryolar atalım:

1. Tim kullanıcılar için mobil uygulamalar yayımlayan Adventure Works deposunda dijital pazarlama ekibinin bir parçasıdır. Tim'ın hedeftir mobil uygulamalarını indirme kullanıcılar kullanmaya devam etmesini sağlamak için ve sık. Bu yalnızca artar bir marka Ekle uygulama kullanıcılarınızın de artar farklılaştırabilmelerini ile uygulama kullanıcıların mobil uygulamayı kullanarak satın alma işlemleri yaptığınızda. Uygulamasını açın ve buna genellikle döndürülmesini kendisine teşvik eden yararlı buldukları uygulama kullanıcılara hedeflenen bildirim göndermek için bu Tim gerekir. Tim Mobile Engagement yararlı bulabileceğiniz budur. 
2. Joann Adventure Works, mobil uygulamaları geliştirme ekibi bir parçasıdır ve günlüğü kilitlenmeler, özel durumlar hakkında ayrıntılar ve performans telemetrisini bir uygulamadan elde etmek bir ürün için aramaktadır. Joann HockeyApp yararlı bulabileceğiniz budur. Joann hem kendi odaklanmış Geliştirici senaryoları için HockeyApp ve Azure Mobile Engagement Tim için hem en iyi almak için aynı uygulamada tümleştirin. 
3. Bir kez deneme Contoso haber ağ ve tüm haber uyarıyı tetikleyen hemen haber uyarıları kadar kesimleme olmadan tüm kullanıcılara kesmeden göndermek için istediği mobil uygulamaları geliştirme ekibi bir parçasıdır. Bir kez deneme bildirim hub'ları yararlı bulabileceğiniz budur. 
   Bu senaryoda, ancak mobil uygulama geliştikçe olduğundan çok daha zengin kesimleme yapın ve uygulama kullanıcının davranışı hakkında bilgi almak için bir zorunluluk mümkündür. Şu anda Azure Mobile Engagement değerlendirmek bir kez deneme gerekir. 

Olduðunu için Mobile Engagement amacı değil sadece analiz bilgileri toplamak için - değil "henüz başka bir Analytics ürün Microsoft". Hedeflenen anında iletme bildirimleri gönderme hakkında olduğu ve bu hedeflemek için davranış analizi veri topladığımız ancak uygulama kullanıcılar için en anlamlı ve böylece, istenmeyen posta olarak gelmez anında iletme bildirimleri gönderme odağı kalır. 

Daha fazla ayrıntı için - bu bir göz atalım [hızlı video](mobile-engagement-overview.md) Mobile Engagement kısaca hakkında. 

