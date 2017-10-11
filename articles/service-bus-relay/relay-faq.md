---
title: "Azure geçiş ile ilgili SSS | Microsoft Docs"
description: "Azure geçiş hakkında bazı sık sorulan soruların yanıtlarını alın."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: e8c146f4b6d02449be6ad9e991e52db8dfd58e04
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="azure-relay-faqs"></a>Azure geçiş SSS

Bu makalede hakkında bazı sık sorulan sorular (SSS) yanıtları [Azure geçiş](https://azure.microsoft.com/services/service-bus/). Genel Azure fiyatlandırma ve destek bilgileri için bkz: [Azure destek SSS](http://go.microsoft.com/fwlink/?LinkID=185083).

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-relay"></a>Azure Geçiş nedir?
[Azure geçiş hizmeti](relay-what-is-it.md) karma uygulamalarınızı daha güvenli bir şekilde kullanıma sunma genel bulutta Kurumsal işletme ağında bulunan Hizmetleri yardımcı olarak kolaylaştırır. Bir güvenlik duvarı bağlantı açmadan ve bir kurumsal ağ altyapısına müdahale eden değişiklikler gerektirmeden hizmetlerin getirebilir.

### <a name="what-is-a-relay-namespace"></a>Bir geçiş ad alanı nedir?
A [ad alanı](relay-create-namespace-portal.md) uygulamanızdaki adresi geçiş kaynakları için kullanabileceğiniz kapsanan bir kapsayıcıdır. Geçiş kullanmak için bir ad alanı oluşturmanız gerekir. Bu, Başlarken ilk adımlar biridir.

### <a name="what-happened-to-service-bus-relay-service"></a>Service Bus geçişi hizmetini ne oldu?
Daha önce adlandırılmış Service Bus geçişi hizmetini şimdi WCF geçiş adı verilir. Bu hizmet normal şekilde kullanmaya devam edebilirsiniz. Karma bağlantılar, Azure BizTalk Services transplanted bir hizmeti güncelleştirilmiş bir sürümünü özelliğidir. WCF geçiş ve karma bağlantılar desteklenmeye devam edilir.

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde fiyatlandırma yapısına geçiş hakkında sık sorulan bazı sorular yanıtlanmaktadır. Ayrıca bkz [Azure desteği ile ilgili SSS](http://go.microsoft.com/fwlink/?LinkID=185083) genel Azure fiyatlandırma bilgileri için. Geçiş fiyatlandırma hakkında tam bilgi için bkz: [Service Bus fiyatlandırma ayrıntıları][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Karma bağlantılar ve WCF geçiş için nasıl ücret?
Geçiş fiyatlandırma hakkında tam bilgi için bkz: [karma bağlantılar ve WCF geçişler] [ Pricing overview] fiyatlandırma ayrıntıları sayfası Service Bus üzerinde tablo. Bu sayfada not ettiğiniz fiyatlar yanı sıra, ilişkili veri aktarımları, uygulamanızın sağlanan veri merkezi dışında çıkışı için ücretlendirilirsiniz.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Karma bağlantılar için nasıl faturalandırılır?
Karma bağlantılar için üç örnek fatura senaryoları şunlardır:

*   Senaryo 1:
    *   Karma Bağlantı Yöneticisi yüklü ve tüm ay için sürekli olarak çalışan bir örneği gibi tek bir dinleyicisi var.
    *   3 GB veri ay bağlantısı üzerinden gönder. 
    *   Toplam ücret $5'tir.
*   Senaryo 2:
    *   Karma Bağlantı Yöneticisi yüklü ve tüm ay için sürekli olarak çalışan bir örneği gibi tek bir dinleyicisi var.
    *   10 GB veri ay bağlantısı üzerinden gönder.
    *   Toplam ücret $7.50 ' dir. Olan ilk 5 GB ve bağlantı için 5 + 2,50 ek 5 GB'lık veri için.
*   Senaryo 3:
    *   A ve B, karma bağlantılar yüklenir ve tüm ay için sürekli çalışan Manager'ın iki örneği vardır.
    *   Bağlantı bir ay boyunca 3 GB veri gönderin.
    *   6 GB veri Ay B bağlantısı üzerinden gönder.
    *   Toplam ücret $10,50 ' dir. Olan 5 bağlantısının için 5 bağlantı B için + 0,50 (için B bağlantıda altıncı gigabayt).

Örneklerde kullanılan fiyatlar yalnızca karma bağlantılar Önizleme dönemi boyunca geçerli olduğunu unutmayın. Karma bağlantılar, genel kullanılabilirlik değişikliklerinde fiyatlar tabidir.

### <a name="how-are-hours-calculated-for-relay"></a>Saat geçiş için nasıl hesaplanır?

WCF geçiş yalnızca standart katmanı ad alanları ile kullanılabilir. Fiyatlandırma ve [bağlantı kotaları](../service-bus-messaging/service-bus-quotas.md) geçişler aksi değişip değişmediğini için. Uygulanacak geçişler devam Bunun anlamı iletileri (olmayan işlemler) ve geçiş saatleri sayısına göre. Daha fazla bilgi için bkz: ["Karma bağlantılar ve WCF geçişler"](https://azure.microsoft.com/pricing/details/service-bus/) fiyatlandırma ayrıntıları sayfasında tablo.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-specific-relay"></a>Belirli bir geçiş bağlı birden çok dinleyici yoksa ne miyim?
Bazı durumlarda, tek bir geçiş birden fazla bağlı dinleyicileri olabilir. En az bir geçiş dinleyicisi bağlı olduğunda geçiş açık olarak kabul edilir. Ekleme dinleyicileri ek geçiş saatleri açık geçiş sonuçlanır. Geçiş Gönderenler (çağırma veya geçişler için ileti gönderme istemciler) sayısı için bağlı bir geçiş geçiş saatleri hesaplama etkilemez.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>İletileri ölçer WCF geçişler için nasıl hesaplanır?
(**Bu yalnızca WCF geçişler için geçerlidir. İletileri karma bağlantılar için bir maliyet değildir.** )

Genel olarak, Faturalanabilir iletileri geçişler için daha önce açıklanan aracılı varlıklar için (kuyruklar, konular ve abonelikler) kullanılan aynı yöntemi kullanılarak hesaplanır. Ancak, önemli bazı farklar vardır.

"Tam yoluyla" iletisini alır geçiş dinleyicisi göndermek gibi bir hizmet veri yolu geçişi ileti gönderme kabul edilir. Geçiş dinleyicisi teslim arkasından Service Bus geçişi için gönderme işlemi olarak işlenmez. İstek-yanıt geçişine karşı hizmet çalıştırılışı (en çok 64 KB) iki Faturalanabilir ileti dinleyicisi sonuçlarında stil: istek ve yanıt için bir Faturalanabilen iletisini için bir Faturalanabilen iletisini (yanıt varsayılarak olduğunu da 64 KB veya daha küçük). Bu, bir istemci ve hizmet arasında aktarmak için bir sıra kullanmaktan farklıdır. Bir istemci ve hizmet arasında aktarmak için bir sıra kullanırsanız, aynı istek-yanıt düzeni dequeue/teslim tarafından sıradan hizmete ardından sıranın bir istek Gönder gerektirir. Bu yanıt gönderme tarafından başka bir sıra ve dequeue/teslim istemciye bu sırasından izler. (En çok 64 KB) boyunca aynı boyutu varsayımlar kullanarak mediated sıra düzeni 4 Faturalanabilir iletilerinde sonuçlanır. İki kez geçişi kullanarak gerçekleştirmek aynı desen uygulamak için ileti sayısı için fatura. Elbette, dayanıklılık gibi bu deseni elde etmek ve Yük Dengeleme için kuyrukları kullanma avantajları vardır. Avantajlar ek gider justify.

Kullanarak açık geçişler **netTCPRelay** WCF bağlama tek bir ileti olarak değil, ancak sistem üzerinden akan bir veri akışı olarak iletileri kabul eder. Bu bağlama kullandığınızda, yalnızca gönderen ve dinleyici gönderilen ve alınan tek bir ileti çerçeveleme görünürlüğe sahip. Geçişleri kullanan **netTCPRelay** bağlama, tüm verileri olarak değerlendirilir Faturalanabilir iletileri hesaplamak için bir akış. Bu durumda, Service Bus toplam gönderilen veya alınan 5 dakikalık aralıklarla tek tek her geçiş aracılığıyla veri miktarını hesaplar. Ardından, bu süre içinde bu geçiş Faturalanabilir ileti sayısını belirlemek için 64 KB toplam veri miktarı böler.

## <a name="quotas"></a>Kotalar
| Kota adı | Kapsam | Tür | Aşıldığında davranışı | Değer |
| --- | --- | --- | --- | --- |
| Bir geçiş üzerinde eşzamanlı dinleyicileri |Varlık |Statik |Sonraki istekleri için ek bağlantıları reddedilir ve arama kodun bir özel durum aldı. |25 |
| Eşzamanlı geçiş dinleyicileri |Sistem çapında |Statik |Sonraki istekleri için ek bağlantıları reddedilir ve arama kodun bir özel durum aldı. |2,000 |
| Bir hizmet ad alanındaki tüm geçiş uç noktaları başına eşzamanlı geçiş bağlantıları |Sistem çapında |Statik |- |5,000 |
| Hizmet ad alanı başına geçiş uç noktaları |Sistem çapında |Statik |- |10,000 |
| İleti boyutu için [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) ve [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) geçişleri |Sistem çapında |Statik |Bu kotalar aşan gelen iletileri reddedilir ve arama kodun bir özel durum aldı. |64 KB |
| İleti boyutu için [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) ve [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) geçişleri |Sistem çapında |Statik |- |Sınırsız |

### <a name="does-relay-have-any-usage-quotas"></a>Geçiş tüm kullanım kotalarını var mı?
Varsayılan olarak, tüm bulut hizmeti için tüm müşteri'nin abonelikler arasında hesaplanan bir toplama aylık kullanım kotası Microsoft ayarlar. Bazen gereksinimlerinizi bu sınırları aşabilir olduğunu anlayın. Biz gereksinimlerinizi anlamak ve bu sınırları uygun şekilde ayarlamak için Müşteri Hizmetleri herhangi bir zamanda başvurabilirsiniz. Hizmet veri yolu için toplam kullanım kotalarını aşağıdaki gibidir:

* 5 milyar ileti
* 2 milyon geçiş saati

Biz aylık kullanım kotalarını aşan bir hesap devre dışı hakkını saklı, e-posta bildirimi sağladığımız ve birden çok vermiyoruz ancak herhangi bir işlem gerçekleştirmeden önce bağlantı kurmak çalışır. Bu kotalar aşan müşterileri için aşırı ücretler hala sorumludur.

### <a name="naming-restrictions"></a>Adlandırma kısıtlamaları
Bir geçiş ad alanı adı uzunluğu 6 ile 50 karakter arasında olmalıdır.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Başka bir Azure aboneliğine nasıl bir ad alanı geçişini?

Bir ad alanı başka bir abonelik için bir Azure aboneliğinden diğerine taşımak için kullanabilir [Azure portal](https://portal.azure.com) veya PowerShell komutlarını kullanın. Başka bir abonelik için bir ad alanı taşımak için ad alanı zaten etkin olması gerekir. Komutları çalıştıran kullanıcı bir yönetici kullanıcı kaynak ve hedef abonelikler üzerinde olmalıdır.

#### <a name="azure-portal"></a>Azure portalına

Azure geçiş ad alanları bir abonelikten başka bir aboneliği taşımak için Azure Portalı'nı kullanmak için bkz: [bir yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Bir ad alanı başka bir abonelik için bir Azure aboneliğinden diğerine taşımak için PowerShell kullanmak için aşağıdaki komut dizisi kullanın. Bu işlemi yürütmek için ad alanı zaten etkin olması gerekir ve PowerShell komutları çalıştıran kullanıcı bir yönetici kullanıcı kaynak ve hedef abonelikler üzerinde olmalıdır.

```powershell
# Create a new resource group in the target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move the namespace from the source subscription to the target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Sorun giderme
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Bazı özel durumlar nelerdir Azure geçiş API'leri tarafından oluşturulan ve önerilen eylemler eylemleri gerçekleştirebilirsiniz?
Genel özel durumlar ve önerilen eylemler gerçekleştirebileceğiniz bir açıklaması için bkz: [geçiş özel durumları][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-to-generate-a-signature"></a>Paylaşılan erişim imzası nedir ve hangi dilleri bir imzayı üretmek için kullanabilir miyim?
Paylaşılan erişim imzaları (SAS), SHA-256 güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. Düğüm, PHP, Java, C ve C# içinde kendi imzaları üretmek hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması][Shared Access Signatures].

### <a name="is-it-possible-to-whitelist-relay-endpoints"></a>Beyaz liste geçiş Uç noktalara mümkün mü?
Evet. Geçiş istemcisi, tam etki alanı adlarını kullanarak Azure geçiş hizmetine bağlantı sağlar. Müşteriler için bir giriş ekleyebilirsiniz `*.servicebus.windows.net` duvarlarındaki DNS uygulamaları güvenilir listeye almayı destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared access signatures]: ../service-bus-messaging/service-bus-sas.md