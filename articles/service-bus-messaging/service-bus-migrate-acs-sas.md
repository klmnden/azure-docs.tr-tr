---
title: "Paylaşılan erişim imzası yetkilendirme için Azure Active Directory erişim denetimi Hizmeti'nden geçirme | Microsoft Docs"
description: "Uygulamalar için SAS erişim denetimi Hizmeti'nden geçirme"
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 7a2a55a6ad6a721a39c9f064aad817f841dd3235
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Paylaşılan erişim imzası yetkilendirme için Azure Active Directory erişim denetimi Hizmeti'nden geçirme

Hizmet veri yolu uygulamaları iki farklı yetkilendirme modeli kullanılarak seçimini daha önce sahip: [paylaşılan erişim imzası (SAS)](service-bus-sas.md) doğrudan Service Bus tarafından sağlanan belirteç modeli ve bir Federasyon modeli burada Yönetimi Yetkilendirme kuralları yönetilir içinde [Azure Active Directory](/azure/active-directory/) erişim denetimi Hizmeti'nden (ACS) ve ACS elde belirteçleri geçirilir Service Bus istenen özelliklere erişim yetkisi vermek için.

ACS yetkilendirme modelini uzun almıştır [SAS yetkilendirme](service-bus-authentication-and-authorization.md) tercih edilen modeldir ve tüm belgeleri olarak yönergeler ve örnekler yalnızca SAS bugün kullanın. Ayrıca, artık ACS ile eşleştirilmiş yeni hizmet veri yolu ad alanları oluşturmak mümkündür.

Başka bir hizmete hemen bağımlı değildir, ancak bir istemci tüm aracılar olmadan doğrudan SAS kural ad ve kural anahtarı istemci erişim vererek kullanılabilir, SAS avantajına sahiptir. SAS ayrıca bir istemci ilk yetkilendirme denetimi başka bir hizmet ile geçirmek sahip ve bir belirteç sonra verilen bir yaklaşım ile kolayca tümleştirilebilir. İkinci yaklaşımı ACS kullanım desenine benzer, ancak ACS express zor olan uygulamaya özgü koşullara bağlı veren erişim belirteçleri sağlar.

Üzerinde ACS bağımlı olan tüm mevcut uygulamalar için SAS yerine yararlanmayı uygulamalarını geçirmeyi müşteriler yönlendirmeye.

## <a name="migration-scenarios"></a>Geçiş senaryoları

ACS ve Service Bus ile paylaşılan bilgisi tümleşik bir *imzalama anahtarı*. İmzalama anahtarı yetkilendirme Belirteçleri imzalamak için bir ACS ad alanı tarafından kullanılır ve belirteç eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için hizmet veri yolu tarafından kullanılır. ACS ad alanı, hizmet kimliği ve yetkilendirme kuralları tutar. Yetkilendirme kuralları, hangi hizmet kimliği veya erişim hizmet veri yolu ad alanının bir parçası için hangi tür grafiği, bir uzun önekte eşleşme biçiminde kimlik sağlayıcısı tarafından bir dış hangi belirteç alır tanımlayın.

Örneğin, bir ACS kuralı verebilir **Gönder** yol öneki talep `/` bir hizmet kimliği için başka bir deyişle, bu kurala dayalı ACS tarafından verilen bir belirteç istemci ad alanındaki tüm varlıklara göndermek için bir hak vermez. Yol öneki ise `/abc`, kimlik adlı varlıklara göndermeyi sınırlandırılır `abc` veya bu önek altında düzenlenmiştir. Bu Geçiş Kılavuzu okuyucular zaten bu kavramlarını varsayılır.

Geçiş senaryoları üç ana kategoriye ayrılır:

1.  **Değişmeden varsayılanları**. Bazı müşterilerin kullandığı bir [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi, otomatik olarak oluşturulan geçirme **sahibi** hizmet kimliği ve gizli anahtarını Service Bus ad alanı ile eşleştirilmiş ACS ad alanı için ve yeni kurallar eklemeyin.

2.  **Özel hizmet kimlikleri basit kurallarla**. Bazı müşteriler yeni hizmet kimlikleri eklemek ve her yeni hizmet kimliği vermek **Gönder**, **dinleme**, ve **Yönet** belirli bir varlık için izinleri.

3.  **Karmaşık kurallar özel hizmet kimliklerle**. Çok az sayıda müşteriniz karmaşık kural kümeleri hangi dışarıdan verilen belirteçler geçiş hakları eşlendiği ya da tek hizmet kimliği burada atanmış birden çok kural birden fazla ad alanı yoldan hakları Ayrıştırılan yok.

Karmaşık kural kümeleri geçiş ile daha fazla yardım için sizinle [Azure Destek](https://azure.microsoft.com/support/options/). Diğer iki senaryo basit geçişi etkinleştirin.

### <a name="unchanged-defaults"></a>Değişmeden Varsayılanları

Uygulamanızı ACS Varsayılanları değişmemişse tüm değiştirebilirsiniz [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımı ile bir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesne ve önceden yapılandırılmış ad alanı kullanın **RootManageSharedAccessKey** ACS yerine **sahibi** hesabı. ACS ile bile unutmayın **sahibi** hesabı, bu yapılandırma (ve hala) bu hesabı/kural silmek için izni de dahil olmak üzere ad tam yönetim yetkilisi sağladığından değil genellikle, önerilen varlıklar.

### <a name="simple-rules"></a>Basit kuralları

Uygulama basit kurallarla özel hizmet kimlikleri kullanıyorsa, geçiş, belirli bir kuyruğa üzerinde erişim denetimi sağlamak için bir ACS hizmet kimliği oluşturulduğu durumda basittir. Bu senaryo genellikle SaaS stili çözümlerinde her sırası, belirli bir site için bir kiracı sitesi veya şube ofisi ve hizmet kimliği köprüsüne oluşturuldu olarak kullanıldığı bir durumdur. Bu durumda, ilgili hizmet kimliği sıranın üzerinde doğrudan bir paylaşılan erişim imzası kural geçirilemez. Hizmet kimlik adı SAS kural adı olabilir ve hizmet kimlik anahtarı SAS kural anahtarı haline gelir. Sırasıyla geçerli ACS yapılandırılmış eşdeğer kural sonra varlık için SAS kuralının haklarıdır.

Bu yeni ve ek yapılandırma yerinde SAS, ACS ile Federasyon varolan tüm ad alanı yapabilirsiniz ve ACS çıktığınızda geçiş, sonradan kullanılarak gerçekleştirilir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) yerine [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). Ad alanı ACS bağlantısız olması gerekmez.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kuralları hesapları değildir, ancak haklarıyla ilişkilendirilmiş İmzalama anahtarları adlı. Bu nedenle, uygulama birçok hizmet kimliği oluşturur ve bunları veren senaryoları erişim haklarını birkaç varlıklar için veya tüm ad alanı hala belirteci veren bir aracı gerektirir. Yönergeler için bu tür bir aracı tarafından elde edebilirsiniz [Destek](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar

Hizmet veri yolu kimlik doğrulaması hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Hizmet veri yolu kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)
* [Paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](service-bus-sas.md)
* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)

