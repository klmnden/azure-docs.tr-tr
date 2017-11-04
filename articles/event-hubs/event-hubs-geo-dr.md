---
title: "Azure Event Hubs coğrafi olağanüstü durum kurtarma | Microsoft Docs"
description: "Coğrafi bölgeler için yük devretme kullanın ve Azure Event Hubs olağanüstü durum kurtarma gerçekleştirmek nasıl"
services: event-hubs
documentationcenter: 
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sethm
ms.openlocfilehash: 94c2782b3166fbc65ae755291a82a2a14556b96f
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery-preview"></a>Azure Event Hubs coğrafi olağanüstü durum kurtarma (Önizleme)

Bölgesel veri merkezleri kapalı kalma süresi karşılaştığınızda farklı bir bölge veya veri merkezi içinde çalışmaya devam etmek veri işleme için önemlidir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* ve *coğrafi çoğaltma* tüm kuruluş için önemli özelliklerdir. Azure Event Hubs coğrafi olağanüstü durum kurtarma ve coğrafi çoğaltma, ad alanı düzeyinde destekler. 

Azure Event Hubs coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve değil geçici ya da geçici kesintileri uygulanır.

Microsoft Azure olağanüstü durum kurtarma hakkında ayrıntılı bilgi için bkz: [bu makalede](/azure/architecture/resiliency/disaster-recovery-azure-applications). 

## <a name="terminology"></a>Terminoloji

**Eşleştirme**: birincil ad alanı olarak adlandırılır *etkin* ve iletileri alır. Yük devretme ad alanı *pasif* ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitleme, olduğundan her ikisi de iletileri uygulama kod değişiklikleri olmadan sorunsuz bir şekilde kabul edebilir. Etkin Bölge ve edilgen bölge arasında olağanüstü durum kurtarma yapılandırması oluşturma olarak bilinir *eşleştirme* bölgeleri.

**Diğer ad**: sizin ayarladığınız bir olağanüstü durum kurtarma yapılandırması için bir ad. Diğer adı tek bir tutarlı tam etki alanı adı (FQDN) bağlantı dizesini sağlar. Uygulamalar, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesini kullanın.

**Meta veri**: olay hub'ı adları, tüketici grupları, bölümler, üretilen iş birimleri, varlıkları ve ad alanınızla ilişkilendirilen özellikleri başvuruyor.

## <a name="enable-geo-disaster-recovery"></a>Coğrafi olağanüstü durum kurtarmayı etkinleştir

Olay hub'ları coğrafi olağanüstü durum kurtarma 3 adımda etkinleştirin: 

1. Bir coğrafi-bir diğer ad bağlantı dizesi oluşturur ve canlı meta veri çoğaltma sağlayan eşleme oluşturun. 
2. 1. adımda oluşturduğunuz diğer mevcut istemci bağlantı dizelerini güncelleştirin.
3. Yük devretme başlatın: coğrafi eşleştirme bozuluyor ve diğer ikincil ad alanı, yeni birincil ad alanı olarak işaret eder.

Aşağıdaki şekilde, bu iş akışı gösterilmektedir:

![Akış coğrafi eşleştirme][1] 

### <a name="step-1-create-a-geo-pairing"></a>1. adım: coğrafi çifti oluşturma

İki bölgeler arasında eşleştirme oluşturmak için birincil ad alanı ve ikincil bir ad alanı gerekir. Coğrafi çifti oluşturmak için bir diğer ad oluşturursunuz. Ad bir diğer ad ile eşleştirilmiş sonra meta verileri düzenli aralıklarla her iki ad alanlarında çoğaltılır. 

Aşağıdaki kod bunun nasıl yapılacağı gösterilmektedir:

```csharp
ArmDisasterRecovery adr = await client.DisasterRecoveryConfigs.CreateOrUpdateAsync(
                                    config.PrimaryResourceGroupName,
                                    config.PrimaryNamespace,
                                    config.Alias,
                                    new ArmDisasterRecovery(){ PartnerNamespace = config.SecondaryNamespace});
```

### <a name="step-2-update-existing-client-connection-strings"></a>2. adım: var olan istemci bağlantı dizelerini güncelleştirmek

Coğrafi eşleştirme tamamlandıktan sonra birincil ad alanlarına noktası bağlantı dizeleri diğer ad ile bağlantı dizesi işaret edecek şekilde güncelleştirilmesi gerekir. Bağlantı dizeleri, aşağıdaki örnekte gösterildiği gibi alın:

```csharp
var accessKeys = await client.Namespaces.ListKeysAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
```

### <a name="step-3-initiate-a-failover"></a>3. adım: bir yük devretme başlatın

Bir olağanüstü durum oluştuğunda ya da ikincil ad alanı için bir yük devretme başlatın karar verirseniz, verileri ve meta verileri ikincil ad alanına akan başlatın. Uygulamalar diğer bağlantı dizeleri kullandığından, otomatik olarak okuma ve ikincil ad alanında olay hub'ları yazma başlangıç olarak başka bir eylem gereklidir. 

Aşağıdaki kod, yük devretme tetiklemek gösterilmektedir:

```csharp
await client.DisasterRecoveryConfigs.FailOverAsync(config.SecondaryResourceGroupName,
                                                   config.SecondaryNamespace, config.Alias);
```

Veri ayıklamak için birincil ad alanında mevcut verileri gerekir ve yük devretme işlemi tamamlandıktan sonra açık bir bağlantı dizesi olay hub'ları için birincil ad alanında kullanmanız gerekir.

### <a name="other-operations-optional"></a>Diğer işlemleri (isteğe bağlı)

Ayrıca, coğrafi eşleştirme bölün veya bir diğer ad aşağıdaki kodda gösterildiği gibi silebilirsiniz. Not: bir diğer ad bağlantı dizesi silmek için önce coğrafi paring bölmeniz gerekir

```csharp
// Break pairing
await client.DisasterRecoveryConfigs.BreakPairingAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, config.Alias);

// Delete alias connection string
// Before the alias is deleted, pairing must be broken
await client.DisasterRecoveryConfigs.DeleteAsync(config.PrimaryResourceGroupName,
                                                 config.PrimaryNamespace, config.Alias);
```

## <a name="considerations-for-public-preview"></a>Genel Önizleme için ilgili önemli noktalar

Bu sürüm için aşağıdaki konuları göz önünde bulundurun:

1. Coğrafi olağanüstü durum kurtarma özelliği yalnızca Kuzey Orta ABD ve orta Güney ABD bölgelerde kullanılabilir. 
2. Bu özellik yalnızca yeni oluşturulan ad alanları için desteklenir.
3. Önizleme sürümü için yalnızca meta veri çoğaltma etkinleştirilir. Gerçek veri çoğaltılmaz.
4. Önizleme sürümü ile özelliği etkinleştirmek için herhangi bir ücret yoktur. Ancak, hem birincil hem de ikincil ad alanları için ayrılmış üretilen iş birimleri ücret uygulanabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Github'da örnek](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) coğrafi çifti oluşturur ve bir olağanüstü durum kurtarma senaryosunda bir yük devretme başlatır basit bir iş akışı anlatılmaktadır.
* [REST API Başvurusu](/rest/api/eventhub/disasterrecoveryconfigs) coğrafi olağanüstü durum kurtarma yapılandırması gerçekleştirmek için API'ları açıklar.

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/eh-geodr1.png

