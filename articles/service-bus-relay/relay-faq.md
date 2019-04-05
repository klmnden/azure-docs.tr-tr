---
title: Azure geçişi ile ilgili SSS | Microsoft Docs
description: Azure geçişi hakkında sık sorulan soruların yanıtlarını alın.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: 2433f4b3563cc8b301d1815cccf5ab24406e8662
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045586"
---
# <a name="azure-relay-faqs"></a>Azure geçişi ile ilgili SSS

Bu makalede hakkında sık sorulan bazı sorular (SSS) yanıtlarını [Azure geçişi](https://azure.microsoft.com/services/service-bus/). Genel Azure fiyatlandırma ve destek için bilgi [Azure desteği SSS](https://azure.microsoft.com/support/faq/).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-relay"></a>Azure Geçiş nedir?
[Azure geçişi hizmetini](relay-what-is-it.md) karma uygulamalarınızı daha güvenli bir şekilde kullanıma sunma genel bulutta Kurumsal işletme ağında bulunan Hizmetleri yardımcı olarak gerçekleştirir. Bir güvenlik duvarı bağlantısı açmak ve kurumsal ağ altyapısına müdahale eden değişikliklere gerek olmadan Hizmetleri üzerinden kullanıma sunabilirsiniz.

### <a name="what-is-a-relay-namespace"></a>Bir geçiş ad alanı nedir?
A [ad alanı](relay-create-namespace-portal.md) uygulamanızdaki adresi geçişi kaynakları için kullanabileceğiniz kapsayan bir kapsayıcıdır. Geçiş ad alanı oluşturmanız gerekir. Bu Başlarken içinde ilk adımlarından biridir.

### <a name="what-happened-to-service-bus-relay-service"></a>Service Bus geçişi hizmetini ne oldu?
Daha önce adlandırılmış Service Bus geçişi hizmetini şimdi adlı [WCF geçişi](relay-wcf-dotnet-get-started.md). Bu hizmet zamanki gibi kullanmaya devam edebilirsiniz. Karma bağlantılar özelliği, Azure BizTalk Services transplanted bir hizmet, güncelleştirilmiş bir sürümüdür. WCF geçişi ve karma bağlantılar desteklemeye devam eder.

## <a name="pricing"></a>Fiyatlandırma
Bu bölümde, fiyatlandırma yapısı geçişi hakkında sık sorulan bazı sorular yanıtlanmaktadır. Ayrıca bkz [Azure desteği SSS](https://azure.microsoft.com/support/faq/) genel Azure fiyatlandırma bilgileri için. Geçişi fiyatlandırması hakkında tam bilgi için bkz. [Service Bus fiyatlandırma ayrıntıları][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Nasıl karma bağlantılar ve WCF geçişi için ücretlendirme yapılır?
Geçişi fiyatlandırması hakkında tam bilgi için bkz. [karma bağlantılar ve WCF geçişleri] [ Pricing overview] Service Bus fiyatlandırma ayrıntıları sayfasına tablosunda. Bu sayfada bahsi geçen ücretler ek olarak, ilişkili veri aktarımları, uygulamanızın sağlandığı veri merkezi dışında çıkışı için ücretlendirilirsiniz.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Karma bağlantılar için nasıl faturalandırılırım?
Karma bağlantılar için üç örnek fatura senaryoları şunlardır:

*   Senaryo 1:
    *   Karma bağlantılar yöneticisinin yüklü ve tüm ay boyunca sürekli olarak çalışan bir örneğini gibi tek bir dinleyiciniz varsa var.
    *   Ay boyunca bağlantı üzerinden 3 GB veri gönderin. 
    *   Toplam ödeyeceğiniz ücret, 5 ABD Doları ' dir.
*   Senaryo 2:
    *   Karma bağlantılar yöneticisinin yüklü ve tüm ay boyunca sürekli olarak çalışan bir örneğini gibi tek bir dinleyiciniz varsa var.
    *   Bağlantı ay boyunca 10 GB veri gönderdiyseniz.
    *   Toplam ödeyeceğiniz ücret $7.50 ' dir. Bu bağlantı ve ilk 5 GB için 5 + 2,50 ek 5 GB veri için şeklindedir.
*   Senaryo 3:
    *   A ve B, yüklü ve tüm ay boyunca sürekli çalışan karma bağlantılar yöneticisinin iki örneği var.
    *   Bir ay boyunca bağlantı üzerinden 3 GB veri gönderin.
    *   Ay sırasında B bağlantısı üzerinden 6 GB veri gönderin.
    *   Toplam ödeyeceğiniz ücret $10.50 olan. Olan 5 A bağlantısı için + B bağlantısı için 5 ABD doları (için B bağlantısında altıncı gigabayt) 0,50 ABD Doları.

Örneklerde kullanılan fiyatların yalnızca karma bağlantılar Önizleme dönemi boyunca geçerli olduğunu unutmayın. Fiyatlar, karma bağlantılar genel kullanım sonrasında değiştirilebilir.

### <a name="how-are-hours-calculated-for-relay"></a>Geçiş saatleri nasıl hesaplanır?

WCF geçişi, yalnızca standart katman ad alanlarında kullanılabilir. Fiyatlandırma ve [bağlantı kotalar](../service-bus-messaging/service-bus-quotas.md) geçişleri aksi değişip değişmediğini için. Geçişleri ücretlendirilmeye devam Bunun anlamı, iletileri (değil işlemi) ve geçiş saatleri sayısına göre. Daha fazla bilgi için ["Karma bağlantılar ve WCF geçişleri"](https://azure.microsoft.com/pricing/details/service-bus/) fiyatlandırma ayrıntıları sayfasına tablosunda.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-specific-relay"></a>Belirli bir geçiş için bağlı birden fazla dinleyici varsa ne olur?
Bazı durumlarda tek bir geçiş birden çok bağlı dinleyici olabilir. En az bir geçiş dinleyicisinin bağlı olduğunda geçiş açık olarak kabul edilir. Dinleyicileri bir açık geçiş sonuçları için ek geçiş saatleri ekleniyor. Geçiş Gönderenler (geçişleri için ileti göndermek ya da çağırma istemciler) sayısını bağlı geçiş geçiş saatleri hesaplama etkilemez.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Mesaj ölçütü, WCF geçişleri için nasıl hesaplanır?
(**Bu yalnızca WCF geçişleri için geçerlidir. İletileri karma bağlantılar için bir maliyet değildir.** )

Genel olarak, daha önce açıklanan aracılı varlıklarda (kuyruklar, konular ve abonelikler) aynı yöntemi kullanarak Faturalanabilir mesajlar geçişleri için hesaplanır. Ancak, bazı önemli farklılıklar vardır.

İletiyi alır geçiş dinleyici "tam yoluyla" gönderme, ileti göndermek için Service Bus geçişi kabul edilir. Service Bus geçişi, geçiş dinleyicisinin teslim ardından için bir gönderme işlemi olarak işlenmez. İstek-yanıt geçiş karşı hizmet çağırma (of, 64 KB'a kadar) iki Faturalanabilir ileti dinleyici sonuçlarında stil: isteği ve yanıtı için bir adet Faturalanabilir mesaj bir adet Faturalanabilir mesaj (yanıt varsayarak, da 64 KB veya daha küçük). Bu, bir istemci ve hizmet arasında aktarmak için bir kuyruk kullanmaktan farklıdır. Bir istemci ve hizmet arasında aktarmak için bir kuyruk kullanırsanız, aynı istek-yanıt desen tarafından bir kuyruktan çıkarma/teslim kuyruktan hizmetine ve ardından sıranın bir istek Gönder gerektirir. Bu yanıt gönderme tarafından başka bir kuyruk ve kuyruktan çıkarma/teslim istemciye bu kuyruktan takip eder. Aynı boyut varsayımların boyunca (64 KB'a kadar) kullanarak, cout kuyruk düzeni 4 Faturalanabilir mesaj sonuçlanır. İki kez geçişi kullanarak gerçekleştirmek aynı deseni uygulamak için ileti sayısı için faturalandırılırsınız. Elbette, bu düzen, dayanıklılık gibi ulaşmasını sağlamak ve Yük Dengeleme kuyrukları kullanmanın avantajları vardır. Bu avantajlara ek masraf Yasla.

Kullanılarak açılan geçişleri **netTCPRelay** WCF bağlama tek bir ileti olarak değil, ancak sistem üzerinden akıyor veri akışı olarak ileti kabul et. Bu bağlama kullandığınızda, yalnızca gönderen ve dinleyici gönderilen ve alınan bireysel iletilerin çerçeveleme görünürlük sahiptir. Geçişleri kullanan **netTCPRelay** bağlama, tüm veriler olarak kabul edildiği Faturalanabilir mesajlar hesaplamak için bir akış. Bu durumda, Service Bus toplam gönderilen veya alınan 5 dakikalık aralıklarla tek tek her geçiş aracılığıyla veri miktarını hesaplar. Ardından, 64 KB'ın ilgili süre içinde bu geçiş Faturalanabilir mesajların sayısı belirlemek için bu toplam veri miktarı böler.

## <a name="quotas"></a>Kotalar
| Kota adı | Kapsam |  Notlar | Değer |
| --- | --- | --- | --- |
| Bir geçiş üzerinde eşzamanlı dinleyicileri |Varlık |Bir ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. |25 |
| Tüm geçiş uç noktaların bir hizmet ad alanı başına eşzamanlı geçiş bağlantıları |Ad alanı |- |5.000 |
| Hizmet ad alanı başına geçiş uç noktaları |Ad alanı |- |10,000 |
| İleti boyutu için [NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) ve [NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) geçişleri |Ad alanı |Bu kotaları aşan gelen iletileri reddedilir ve bir özel durum çağıran kod tarafından alınır. |64 KB |
| İleti boyutu için [HttpRelayTransportBindingElement](/dotnet/api/microsoft.servicebus.httprelaytransportbindingelement) ve [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) geçişleri |Ad alanı |İleti boyutu sınırı yoktur. |Sınırsız |

### <a name="does-relay-have-any-usage-quotas"></a>Geçiş herhangi bir kullanım kotalarını var mı?
Varsayılan olarak herhangi bir bulut hizmeti için Microsoft, tüm müşteri abonelikleri hesaplanan bir toplam aylık kullanım kotası ayarlar. Bazen gereksinimlerinizi bu limitlerin olduğunu biliyoruz. Biz de ihtiyaçlarınızı anlayabilmemiz ve bu limitleri uygun şekilde ayarlamak için Müşteri Hizmetleri herhangi bir zamanda başvurabilirsiniz. Service Bus için toplam kullanım kotalarını aşağıdaki gibidir:

* 5 milyar ileti
* 2 milyon geçiş saati

Aylık kullanım kotalarını aşan bir hesap devre dışı bırakmak için hakkımızı saklı tutarız, e-posta bildirimi sağlarız ve birden çok vermiyoruz rağmen müşteri herhangi bir işlem gerçekleştirmeden önce bağlantı kurmayı dener. Bu kotaları aşan müşteriler, aşırı ücretlerden için yine de sorumludur.

### <a name="naming-restrictions"></a>Adlandırma kısıtlamaları
Bir geçiş ad alanı adı uzunluğu 6 ile 50 karakter arasında olmalıdır.

## <a name="subscription-and-namespace-management"></a>Abonelik ve ad alanı yönetimi
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Bir ad alanı için başka bir Azure aboneliğine nasıl geçirebilirim?

Bir ad alanı bir Azure aboneliğine ait başka bir aboneliğe taşımak için kullanabilir [Azure portalında](https://portal.azure.com) veya PowerShell komutlarını kullanın. Bir ad alanı başka bir aboneliğe taşımak için ad alanı zaten etkin olması gerekir. Komutları çalıştıran kullanıcının, hem kaynak hem de hedef Aboneliklerde yönetici kullanıcı olması gerekir.

#### <a name="azure-portal"></a>Azure portal

Azure geçişi ad alanlarını başka bir aboneliğe bir abonelikten geçirmek için Azure portalını kullanmak için bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Bir ad alanı bir Azure aboneliğine ait başka bir aboneliğe taşımak için PowerShell kullanmak için aşağıdaki komutları dizisini kullanın. Bu işlemi yürütmek için ad alanı zaten etkin olması gerekir ve PowerShell komutları çalıştıran kullanıcının kaynak ve hedef Aboneliklerde yönetici kullanıcı olması gerekir.

```azurepowershell-interactive
# Create a new resource group in the target subscription.
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move the namespace from the source subscription to the target subscription.
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Sorun giderme
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Bazı özel durumlar nedir, Azure geçişi API'leri tarafından oluşturulan ve önerilen gerçekleştirebileceğiniz eylemler?
Sık karşılaşılan özel durumlar ve önerilen eylemler gerçekleştirebileceğiniz açıklaması için bkz: [geçiş özel durumları][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-to-generate-a-signature"></a>Paylaşılan erişim imzası nedir ve hangi dillerin bir imzayı üretmek için kullanabilir miyim?
Paylaşılan erişim imzaları (SAS), SHA-256'yı güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. Düğüm, PHP, Java, C ve C# içinde kendi imzaları üretmek hakkında daha fazla bilgi için bkz: [paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması][Shared Access Signatures].

### <a name="is-it-possible-to-whitelist-relay-endpoints"></a>Beyaz liste geçiş uç noktaları için olası mı?
Evet. Geçiş istemcisi, tam etki alanı adlarını kullanarak Azure geçişi hizmetini bağlantıları sağlar. Müşteriler için bir giriş ekleyebilirsiniz `*.servicebus.windows.net` DNS beyaz listeye ekleme desteği güvenlik.

## <a name="next-steps"></a>Sonraki adımlar
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md
