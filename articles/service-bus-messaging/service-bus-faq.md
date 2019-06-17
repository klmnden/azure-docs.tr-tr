---
title: Azure Service Bus hakkında sık sorulan sorular (SSS) | Microsoft Docs
description: Azure Service Bus hakkında sık sorulan bazı sorular yanıtlanmaktadır.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 8461764a3f1f682ffb97420a4efdf2803f518872
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64707143"
---
# <a name="service-bus-faq"></a>Hizmet Veri Yolu SSS

Bu makalede, Microsoft Azure Service Bus hakkında sık sorulan bazı sorular açıklanmaktadır. Da ziyaret edebilirsiniz [Azure desteği SSS](https://azure.microsoft.com/support/faq/) genel Azure fiyatlandırma ve destek bilgileri.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus hakkında genel sorular
### <a name="what-is-azure-service-bus"></a>Azure Service Bus nedir?
[Azure Service Bus](service-bus-messaging-overview.md) ayrılmış sistemleri arasında veri göndermek sağlayan bir zaman uyumsuz Mesajlaşma bulut platformudur. Microsoft, bu özelliği kullanmak için kendi donanım konak gerekmez anlamına gelir. bir hizmet olarak sunar.

### <a name="what-is-a-service-bus-namespace"></a>Service Bus ad alanı nedir?
A [ad alanı](service-bus-create-namespace-portal.md) uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sağlar. Ad alanı oluşturarak, Service Bus hizmetini kullanmak gereklidir ve Başlarken ilk adımlar biridir.

### <a name="what-is-an-azure-service-bus-queue"></a>Bir Azure Service Bus kuyruğuna nedir?
A [Service Bus kuyruğu](service-bus-queues-topics-subscriptions.md) iletileri depolanan bir varlıktır. Kuyruklar, birden fazla uygulama ya da birbirleri ile iletişim kurması gereken dağıtılmış bir uygulama birden fazla bölümü olduğunda yararlıdır. Birden çok ürünlerin (iletiler) alınan ve ardından o konumdan gönderilen bir dağıtım Merkezi'ne kuyruğa benzer.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus konuları ve abonelikleri nelerdir?
Bir konu bir kuyruk görselleştirilebilir ve birden çok abonelik kullanırken daha zengin bir Mesajlaşma modeli olur; temelde bir-çok iletişim aracıdır. Bu yayımlama/abone olma modelini (veya *pub/sub*) birden çok uygulama tarafından alınan o ileti için birden fazla aboneliğine sahip bir konu başlığına bir ileti gönderen bir uygulama sağlar.

### <a name="what-is-a-partitioned-entity"></a>Bölümlenen bir varlığın nedir?
Geleneksel bir kuyruk veya konuda tek ileti aracısı tarafından işlenmesini ve bir Mesajlaşma deposunda depolanır. Yalnızca temel ve standart Mesajlaşma katmanları, desteklenen bir [bölümlenmiş bir kuyruk veya konuda](service-bus-partitioning.md) birden çok ileti aracıları tarafından işlenmesini ve birden çok Mesajlaşma deposu içinde depolanan. Bu özellik, bir bölümlenmiş kuyruğa veya konuya genel verimini tek ileti aracısı veya ileti deposu performansını artık sınırlı olduğu anlamına gelir. Ayrıca, bir Mesajlaşma deposunun geçici bir kesinti bölümlenmiş bir kuyruk veya konuda kullanılamaz işlemez.

Kullanılarak bölümlenmiş varlıklar, sıralama sağlanmaz. Bir bölüm kullanılamıyor durumunda durumunda, yine de göndermek ve diğer bölümlerden ileti alma.

 Bölümlenen varlıklar artık desteklenmemektedir [Premium SKU](service-bus-premium-messaging.md). 

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>Güvenlik Duvarı'nı açmak hangi bağlantı noktalarını gerekiyor? 
Azure Service Bus ile aşağıdaki protokolleri, ileti göndermek ve almak için kullanabilirsiniz:

- Gelişmiş ileti sıraya alma Protokolü (AMQP)
- Service Bus Mesajlaşma Protokolü (SBMP)
- HTTP

Azure Event Hubs ile iletişim kurmak için bu protokolleri kullanmak için açmanız giden bağlantı noktaları için aşağıdaki tabloya bakın. 

| Protocol | Bağlantı Noktaları | Ayrıntılar | 
| -------- | ----- | ------- | 
| AMQP | 5671 ve 5672 | Bkz: [AMQP protokol Kılavuzu](service-bus-amqp-protocol-guide.md) | 
| SBMP | için 9350 9354 | Bkz: [bağlantı modu](/dotnet/api/microsoft.servicebus.connectivitymode?view=azure-dotnet) |
| HTTP, HTTPS | 80, 443 | 

### <a name="what-ip-addresses-do-i-need-to-whitelist"></a>Hangi IP adreslerini beyaz listeye gerekiyor mu?
Bağlantılarınız için doğru IP adreslerini beyaz listeye bulmak için aşağıdaki adımları izleyin:

1. Bir komut isteminden aşağıdaki komutu çalıştırın: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. Not alın, döndürülen IP adresine `Non-authoritative answer`. Bu IP adresi statiktir. Değiştirmeniz gerekir yalnızca zaman içinde ad farklı bir kümeye açın geri noktasıdır.

Ad alanınız için bölge artıklığı kullanırsanız birkaç ek adımları gerçekleştirmeniz gerekir: 

1. İlk olarak, nslookup ad alanı üzerinde çalıştırın.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. Not alın adında **yetkili olmayan yanıt** bölümünde, aşağıdaki biçimlerden biri: 

    ```
    <name>-s1.servicebus.windows.net
    <name>-s2.servicebus.windows.net
    <name>-s3.servicebus.windows.net
    ```
3. Her biri ile sonekleri s1, s2 ve s3 üç kullanılabilirlik alanında çalışan tüm üç örnek IP adreslerini almak için nslookup Çalıştır 


## <a name="best-practices"></a>En iyi uygulamalar
### <a name="what-are-some-azure-service-bus-best-practices"></a>Azure Service Bus en iyi yöntemlerden bazıları nelerdir?
Bkz: [için Service Bus'ı kullanarak performans geliştirme en iyi yöntemler] [ Best practices for performance improvements using Service Bus] – bu makalede, mesaj alışverişleri sırasında performansı iyileştirmek açıklanır.

### <a name="what-should-i-know-before-creating-entities"></a>Neleri varlık oluşturmadan önce bilmeliyim?
Bir kuyruk ve konu aşağıdaki özelliklerini sabittir. Varlıklarınızı sağladığınızda yeni değiştirme varlık oluşturmadan bu özellikleri değiştirilemez olarak bu sınırlamayı göz önünde bulundurun.

* Bölümleme
* Oturumlar
* Yineleme algılama
* Varlık express

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde, Service Bus fiyatlandırma yapısı hakkında sık sorulan bazı sorular yanıtlanmaktadır.

[Fiyatlandırma ve faturalama Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) makalede, Service Bus, faturalandırma ölçümlerinde açıklanmaktadır. Service Bus fiyatlandırma seçenekleri hakkında ayrıntılı bilgi için bkz: [Service Bus fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/service-bus/).

Da ziyaret edebilirsiniz [Azure desteği SSS](https://azure.microsoft.com/support/faq/) genel Azure fiyatlandırma bilgileri için. 

### <a name="how-do-you-charge-for-service-bus"></a>Nasıl hizmet veri yolu için ücretlendirme yapılır?
Service Bus fiyatlandırması hakkında tam bilgi için bkz. [Service Bus fiyatlandırma ayrıntıları][Pricing overview]. Bahsi geçen ücretler ek olarak, ilişkili veri aktarımları, uygulamanızın sağlandığı veri merkezi dışında çıkışı için ücretlendirilirsiniz.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Service Bus'ın hangi kullanım, veri aktarımı tabi mi? Ne olur?
Belirli bir Azure bölgesi içinde tüm veri aktarımları ücretsiz yanı sıra tüm gelen veri aktarımı sağlanır. Bir bölge dışında veri aktarımı ise bulunabilir çıkış ücretlerini tabi [burada](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Service Bus, depolama için ücretli midir?
Hayır, Service Bus depolama için ücret talep etmez. Ancak, maksimum kuyruk/konu kalıcı veri miktarını sınırlayan bir kota yok. Bir sonraki SSS Bölümüne bakın.

## <a name="quotas"></a>Kotalar

Service Bus limitler ve kotalar listesi için bkz. [Service Bus kotaları genel bakış][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Service Bus tüm kullanım kotalarını var mı?
Varsayılan olarak, herhangi bir bulut hizmeti Microsoft, tüm müşteri abonelikleri hesaplanan bir toplam aylık kullanım kotası ayarlar. Bu limitlerden daha fazlasına ihtiyacınız varsa ihtiyaçlarınızı anlayabilmemiz ve bu limitleri uygun şekilde ayarlamak için istediğiniz zaman Müşteri Hizmetleri başvurabilirsiniz. Service Bus için toplam kullanım kotası ayda 5 milyar ileti ' dir.

Microsoft, belirli bir ay içinde kullanım kotalarını aştı bir müşterinin hesabını devre dışı hakkını saklı tutar, ancak e-posta bildirimi gönderilir ve birden fazla girişimde herhangi bir işlem gerçekleştirmeden önce bağlantı kurmak için sunulur. Bu kotalar aşan müşterileri yine de kotaları aşan ücretler için sorumlu.

Diğer hizmetler gibi Azure üzerinde ile Service Bus kaynakların adil kullanım olduğundan emin olmak için özel kotalar bir dizi zorlar. Bu kotaları hakkında daha fazla ayrıntı bulabilirsiniz [Service Bus kotaları genel bakış][Quotas overview].

### <a name="how-to-handle-messages-of-size--1-mb"></a>> 1 MB boyut iletilerini işlemek için nasıl?
Service Bus Mesajlaşma Hizmetleri (kuyruklar ve konular/abonelikler) kadar boyutta iletileri göndermek uygulama izin 256 KB (standart katman) veya 1 MB (premium katman). Boyutu 1 MB'den büyük iletileri ile uğraşıyorsanız, açıklanan talep denetim desenini kullanın [bu blog gönderisini](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern).

## <a name="troubleshooting"></a>Sorun giderme
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Neden başka abonelikten sildikten sonra bir ad alanı oluşturmak gönderemiyorum? 
Bir abonelikten bir ad alanı sildiğinizde, başka bir Abonelikteki aynı adla yeniden önce 4 saat bekleyin. Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Service Bus API'lerine ve önerilen eylemlerinin tarafından oluşturulan özel durumları bazıları nelerdir?
Olası Service Bus özel durumları listesi için bkz. [özel durumlar genel bakış][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Bir imza oluşturulurken bir paylaşılan erişim imzası nedir ve hangi dilleri destekliyor?
Paylaşılan erişim imzaları, SHA-256'yı güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. Node.js, PHP, Java ve C kendi imzaları üretmek hakkında bilgi için\#, bkz: [paylaşılan erişim imzaları] [ Shared Access Signatures] makalesi.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Bir ad alanı için başka bir Azure aboneliğine nasıl geçirebilirim?

Bir ad alanı bir Azure aboneliğine ait diğerine kullanarak taşıyabilirsiniz [Azure portalında](https://portal.azure.com) veya PowerShell komutları. İşlemi yürütmek için ad alanı zaten etkin olması gerekir. Komutları yürütmeden kullanıcı kaynak ve hedef abonelikler yönetici olması gerekir.

#### <a name="portal"></a>Portal

Service Bus ad alanlarını başka bir aboneliğe geçirme Azure portal'ı kullanmak için yönergeleri izleyin. [burada](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell komutları dizisini bir ad alanı bir Azure aboneliğine ait diğerine taşır. Bu işlemi yürütmek için ad alanı zaten etkin olması gerekir ve PowerShell komutları çalıştıran kullanıcının kaynak ve hedef Aboneliklerde yönetici olmanız gerekir.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Service Bus Premium (blog gönderisi) ile tanışın](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus Premium (Channel9) ile tanışın](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus genel bakış](service-bus-messaging-overview.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
