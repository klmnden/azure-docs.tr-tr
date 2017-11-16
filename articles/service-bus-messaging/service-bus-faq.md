---
title: "Azure Service Bus sık sorulan sorular (SSS) | Microsoft Docs"
description: "Azure Service Bus hakkında bazı sık sorulan sorular yanıtlanmaktadır."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: sethm
ms.openlocfilehash: e64e7d9f203debe19dfa222f501c7902cfe2ae98
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="service-bus-faq"></a>Hizmet Veri Yolu SSS
Bu makalede, Microsoft Azure Service Bus hakkında sık sorulan bazı sorular açıklanmaktadır. Ayrıca, ziyaret edebilirsiniz [Azure destek SSS](http://go.microsoft.com/fwlink/?LinkID=185083) genel Azure fiyatlandırma ve destek bilgileri için.

## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus hakkında genel sorular
### <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?
[Azure Service Bus](service-bus-messaging-overview.md) ayrılmış sistemleri arasında veri göndermenizi sağlayan bir zaman uyumsuz Mesajlaşma bulut platformudur. Microsoft, bu özelliği kullanmak için kendi donanımınızın herhangi biri ana bilgisayar gerekmez anlamına gelen bir hizmet olarak sunar.

### <a name="what-is-a-service-bus-namespace"></a>Bir hizmet veri yolu ad alanı nedir?
A [ad alanı](service-bus-create-namespace-portal.md) uygulamanızı Service Bus kaynaklarını adreslemek için kapsam bir kapsayıcı sağlar. Bir ad alanı oluşturma Service Bus hizmetini kullanmak gereklidir ve Başlarken ilk adımlar biridir.

### <a name="what-is-an-azure-service-bus-queue"></a>Bir Azure hizmet veri yolu kuyruğu nedir?
A [Service Bus kuyruğuna](service-bus-queues-topics-subscriptions.md) iletileri depolandığı bir varlıktır. Sıralar, birden çok uygulama veya birbirleri ile iletişim kurması gereken dağıtılmış bir uygulama birden fazla bölümü olduğunda faydalıdır. Birden fazla ürün (iletileri) alınan ve sonra o konumdan gönderilen sıra için bir dağıtım merkezi benzer.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus konuları ve abonelikleri nelerdir?
Bir konu sırası olarak canlandırılabilir ve birden çok abonelik kullanırken, bu daha zengin bir Mesajlaşma modeli olur; bir çok iletişim aracı temelde. Bu yayımlama/abonelik modelini (veya *pub/alt*) birden çok uygulama tarafından alınan ileti sağlamak için birden çok abonelik ile bir konuya ileti gönderir bir uygulama sağlar.

### <a name="what-is-a-partitioned-entity"></a>Bölümlenmiş bir varlık nedir?
Geleneksel kuyruk veya konu tek ileti aracısı tarafından işlenen ve bir Mesajlaşma deposunda depolanır. A [bölümlenmiş kuyruk veya konu](service-bus-partitioning.md) birden çok ileti aracıları tarafından işlenen ve birden çok Mesajlaşma deposunda depolanır. Başka bir deyişle, genel üretilen işi bölümlenmiş kuyruk veya konu artık tek ileti Aracısı ya da ileti deposu performans ile sınırlıdır. Ayrıca, bir Mesajlaşma deposu geçici bir kesinti bölümlenmiş kuyruk veya konu kullanılamaz işlemez.

Sıralama kullanırken sağlanmaz Not varlıklar bölümlenmiş. Bir bölüm kullanılamıyor gelmesi durumunda, hala gönderebilir ve diğer bölümlerden iletileri alacak.

## <a name="best-practices"></a>En iyi uygulamalar
### <a name="what-are-some-azure-service-bus-best-practices"></a>Bazı Azure Service Bus en iyi uygulamalar nelerdir?
Bkz: [Service Bus kullanarak performans iyileştirmeleri için en iyi uygulamaları] [ Best practices for performance improvements using Service Bus] – bu makalede, ileti alışverişi sırasında performansı iyileştirmek açıklar.

### <a name="what-should-i-know-before-creating-entities"></a>Ne ı varlıklar oluşturmadan önce bilmeniz gerekenler?
Bir kuyruk ve konu aşağıdaki özelliklerini değişmez. Varlıklarınızı sağlarken bu sınırlamaya yeni bir yedek varlık oluşturmadan bu özellikleri değiştirilemez olarak göz önünde bulundurun.

* Boyut
* Bölümleme
* Oturumlar
* Yinelenen algılama
* Varlık express

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde fiyatlandırma yapısına Service Bus hakkında sık sorulan bazı sorular yanıtlanmaktadır.

[Service fiyatlandırma ve faturalama Bus](service-bus-pricing-billing.md) makale, Service Bus içinde fatura ölçümler açıklar. Service Bus seçenekleri fiyatlandırma hakkında ayrıntılı bilgi için bkz: [Service Bus fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/service-bus/).

Ayrıca, ziyaret edebilirsiniz [Azure desteği ile ilgili SSS](http://go.microsoft.com/fwlink/?LinkID=185083) genel Azure fiyatlandırma bilgileri için. 

### <a name="how-do-you-charge-for-service-bus"></a>Hizmet veri yolu için nasıl ücret?
Hizmet veri yolu fiyatlandırma hakkında tam bilgi için bkz: [Service Bus fiyatlandırma ayrıntıları][Pricing overview]. Not ettiğiniz fiyatların yanı sıra, ilişkili veri aktarımları, uygulamanızın sağlanan veri merkezi dışında çıkışı için ücretlendirilirsiniz.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Hizmet veri yolu hangi kullanımını veri aktarımı tabi mi? Ne değil misiniz?
Belirli bir Azure bölgesi içinde herhangi bir veri aktarımı yanı gelen veri aktarımı hiçbir ücret ödemeden sağlanır. Veri aktarımı bir bölge dışında bulunabilir çıkış ücretlerini tabi olan [burada](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Hizmet veri yolu, depolama için ücretli mi?
Hayır, hizmet veri yolu için depolama ücret değil. Ancak, bir kota en fazla sıra/konu kalıcı veri miktarı sınırlama yoktur. Sonraki SSS Bölümüne bakın.

## <a name="quotas"></a>Kotalar

Hizmet veri yolu sınırlarını ve kotaları listesi için bkz: [Service Bus kotaları genel bakış][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Hizmet veri yolu olan kullanım kotaları var mı?
Varsayılan olarak, her bulut için Microsoft hizmet tüm müşteri'nin abonelikler arasında hesaplanan bir toplama aylık kullanım kotası ayarlar. Bu sınırları birden fazla gerekebilir anlamak için böylece biz gereksinimlerinizi anlamak ve bu sınırları uygun şekilde ayarlayın Müşteri Hizmetleri herhangi bir zamanda başvurabilirsiniz. Hizmet veri yolu için toplam kullanım kotası ayda 5 milyar iletileri ' dir.

Kullanım kotalarını belirtilen aydaki aştı bir müşteri hesabı devre dışı bırakmak için sağ yedek olsa da, biz e-posta bildirimi sağlayın ve herhangi bir işlem gerçekleştirmeden önce bağlantı kurmak için birden çok deneme yapmanız gerekir. Bu kotalar aşan müşterileri kotaları aşan ücretler hala sorumludur.

Diğer hizmetleri gibi Azure üzerinde hizmet veri yolu Orta kaynak kullanımını olduğundan emin olmak için özel kotalar bir dizi zorlar. Bu Kotalar hakkında daha fazla ayrıntı bulabilirsiniz [Service Bus kotaları genel bakış][Quotas overview].

## <a name="troubleshooting"></a>Sorun giderme
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Service Bus API'lerine ve bunların önerilen eylemleri tarafından oluşturulan özel durumları bazıları nelerdir?
Olası Service Bus özel durumlar listesi için bkz: [özel durumlar genel bakış][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Paylaşılan erişim imzası nedir ve hangi dilleri imza oluşturma destekliyor?
Paylaşılan erişim imzaları SHA-256 güvenli karmaları veya URI'ler için temel kimlik doğrulama mekanizması değil. Düğüm, PHP, Java ve C kendi imzaları üretmek hakkında bilgi için\#, bkz: [paylaşılan erişim imzaları] [ Shared Access Signatures] makalesi.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Başka bir Azure aboneliğine nasıl bir ad alanı geçişini?

Bir ad alanı bir Azure aboneliğinden diğerine kullanarak taşıyabilirsiniz [Azure portal](https://portal.azure.com) veya PowerShell komutları. İşlemi yürütmek için ad alanı zaten etkin olması gerekir. Komutlar yürütülürken kullanıcının kaynak ve hedef abonelikler üzerinde bir yönetici olması gerekir.

#### <a name="portal"></a>Portal

Hizmet veri yolu ad alanları başka bir aboneliği taşımak için Azure Portalı'nı kullanmak için yönergeleri izleyin [burada](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell komut dizisi bir ad alanı bir Azure aboneliğinden diğerine taşır. Bu işlemi yürütmek için ad alanı zaten etkin olması gerekir ve PowerShell komutları çalıştıran kullanıcının kaynak ve hedef abonelikler üzerinde bir yönetici olması gerekir.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Service Bus Premium (blog yayını) Tanıtımı](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus Premium (Channel9) Tanıtımı](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Hizmet veri yolu genel bakış](service-bus-messaging-overview.md)
* [Azure Service Bus mimarisine genel bakış](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
