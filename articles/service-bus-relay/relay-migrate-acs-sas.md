---
title: "Paylaşılan erişim imzası yetkilendirme için Azure Active Directory erişim denetimi Hizmeti'nden geçirme | Microsoft Docs"
description: "Uygulamalar için SAS erişim denetimi Hizmeti'nden geçirme"
services: service-bus-relay
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: 7a2674ad4db9749b0a2d9342017a230797514763
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Paylaşılan erişim imzası yetkilendirme için Azure Active Directory erişim denetimi Hizmeti'nden geçirme

Azure geçiş uygulamalarını geçmişte iki farklı yetkilendirme modeli kullanılarak seçimini vardı: [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) doğrudan geçiş hizmeti tarafından sağlanan belirteç modeli ve bir Federasyon modeli burada Yönetimi Yetkilendirme kuralları yönetilir içinde [Azure Active Directory](/azure/active-directory/) erişim denetimi Hizmeti'nden (ACS) ve ACS elde belirteçleri geçirilir geçişi istenen özelliklere erişim yetkisi vermek için.

ACS yetkilendirme modelini uzun almıştır [SAS yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md) tercih edilen modeldir ve tüm belgeleri olarak yönergeler ve örnekler yalnızca SAS bugün kullanın. Ayrıca, artık ACS ile eşleştirilmiş yeni geçiş ad alanları oluşturmak mümkündür.

Başka bir hizmete hemen bağımlı değildir, ancak bir istemci tüm aracılar olmadan doğrudan SAS kural ad ve kural anahtarı istemci erişim vererek kullanılabilir, SAS avantajına sahiptir. SAS ayrıca bir istemci ilk yetkilendirme denetimi başka bir hizmet ile geçirmek sahip ve bir belirteç sonra verilen bir yaklaşım ile kolayca tümleştirilebilir. İkinci yaklaşımı ACS kullanım desenine benzer, ancak ACS express zor olan uygulamaya özgü koşullara bağlı veren erişim belirteçleri sağlar.

Üzerinde ACS bağımlı olan tüm mevcut uygulamalar için SAS yerine yararlanmayı uygulamalarını geçirmeyi müşteriler yönlendirmeye.

## <a name="migration-scenarios"></a>Geçiş senaryoları

ACS ve geçiş paylaşılan bilgisi tümleşik bir *imzalama anahtarı*. İmzalama anahtarı yetkilendirme Belirteçleri imzalamak için bir ACS ad alanı tarafından kullanılır ve belirteç eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için Azure geçiş tarafından kullanılır. ACS ad alanı, hizmet kimliği ve yetkilendirme kuralları tutar. Yetkilendirme kuralları, hangi hizmet kimliği veya erişim geçişi ad alanının bir parçası için hangi tür grafiği, bir uzun önekte eşleşme formunda kimlik sağlayıcısı tarafından bir dış hangi belirteç alır tanımlayın.

Örneğin, bir ACS kuralı verebilir **Gönder** yol öneki talep `/` bir hizmet kimliği için başka bir deyişle, bu kurala dayalı ACS tarafından verilen bir belirteç istemci ad alanındaki tüm varlıklara göndermek için bir hak vermez. Yol öneki ise `/abc`, kimlik adlı varlıklara göndermeyi sınırlandırılır `abc` veya bu önek altında düzenlenmiştir. Bu Geçiş Kılavuzu okuyucular zaten bu kavramlarını varsayılır.

Geçiş senaryoları üç ana kategoriye ayrılır:

1.  **Değişmeden varsayılanları**. Bazı müşterilerin kullandığı bir [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi, otomatik olarak oluşturulan geçirme **sahibi** hizmet kimliği ve geçiş ad alanı ile eşleştirilmiş ACS ad alanı için gizli anahtarını ve yapın Yeni Kural Ekle değil.

2.  **Özel hizmet kimlikleri basit kurallarla**. Bazı müşteriler yeni hizmet kimlikleri eklemek ve her yeni hizmet kimliği vermek **Gönder**, **dinleme**, ve **Yönet** belirli bir varlık için izinleri.

3.  **Karmaşık kurallar özel hizmet kimliklerle**. Çok az sayıda müşteriniz karmaşık kural kümeleri hangi dışarıdan verilen belirteçler geçiş hakları eşlendiği ya da tek hizmet kimliği burada atanmış birden çok kural birden fazla ad alanı yoldan hakları Ayrıştırılan yok.

Karmaşık kural kümeleri geçiş ile daha fazla yardım için sizinle [Azure Destek](https://azure.microsoft.com/support/options/). Diğer iki senaryo basit geçişi etkinleştirin.

### <a name="unchanged-defaults"></a>Değişmeden Varsayılanları

Uygulamanızı ACS Varsayılanları değişmemişse tüm değiştirebilirsiniz [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımı ile bir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesne ve önceden yapılandırılmış ad alanı kullanın **RootManageSharedAccessKey** ACS yerine **sahibi** hesabı. ACS ile bile unutmayın **sahibi** hesabı, bu yapılandırma (ve hala) bu hesabı/kural silmek için izni de dahil olmak üzere ad tam yönetim yetkilisi sağladığından değil genellikle, önerilen varlıklar.

### <a name="simple-rules"></a>Basit kuralları

Uygulama basit kurallarla özel hizmet kimlikleri kullanıyorsa, geçiş, belirli bir geçiş üzerinde erişim denetimi sağlamak için bir ACS hizmet kimliği oluşturulduğu durumda basittir. Bu senaryo genellikle SaaS stili çözümlerinde her geçiş, belirli bir site için bir kiracı sitesi veya şube ofisi ve hizmet kimliği köprüsüne oluşturuldu olarak kullanıldığı bir durumdur. Bu durumda, ilgili hizmet kimliği bir paylaşılan erişim imzası kuralı, doğrudan geçiş geçirilemez. Hizmet kimlik adı SAS kural adı olabilir ve hizmet kimlik anahtarı SAS kural anahtarı haline gelir. Sırasıyla geçerli ACS yapılandırılmış eşdeğer kural sonra varlık için SAS kuralının haklarıdır.

Bu yeni ve ek yapılandırma yerinde SAS, ACS ile Federasyon varolan tüm ad alanı yapabilirsiniz ve ACS çıktığınızda geçiş, sonradan kullanılarak gerçekleştirilir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) yerine [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). Ad alanı ACS bağlantısız olması gerekmez.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kuralları hesapları değildir, ancak haklarıyla ilişkilendirilmiş İmzalama anahtarları adlı. Bu nedenle, uygulama birçok hizmet kimliği oluşturur ve bunları veren senaryoları erişim haklarını birkaç varlıklar için veya tüm ad alanı hala belirteci veren bir aracı gerektirir. Yönergeler için bu tür bir aracı tarafından elde edebilirsiniz [Destek](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar

Azure geçiş kimlik doğrulaması hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure geçiş kimlik doğrulama ve yetkilendirme](relay-authentication-and-authorization.md)
* [Paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md)


