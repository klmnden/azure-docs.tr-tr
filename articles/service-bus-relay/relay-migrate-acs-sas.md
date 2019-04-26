---
title: Azure Active Directory erişim denetimi hizmetinden alınan paylaşılan erişim imzası yetkisi geçirme | Microsoft Docs
description: Uygulamaları, erişim denetimi hizmetinden SAS'ye geçiş
services: service-bus-relay
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: 7f71b6884413309e6806658f25313c22e074a71b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419637"
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Azure Active Directory erişim denetimi hizmetinden alınan paylaşılan erişim imzası yetkisi geçirme

Azure geçişi uygulamaları geçmişe yönelik olarak iki farklı yetkilendirme modeli kullanarak bir seçim vardı: [paylaşılan erişim imzası (SAS)](../service-bus-messaging/service-bus-sas.md) doğrudan geçiş hizmeti tarafından sağlanan belirteç modelini ve Federasyon modeli nerede Yönetimi Yetkilendirme kuralları yönetilir içinde [Azure Active Directory](/azure/active-directory/) Access Control Service (ACS) ve ACS'den alınan belirteçlerin geçirilir geçişi istenen özelliklere erişim yetkisi vermek için.

ACS yetkilendirme modelini uzun yerini almıştır [SAS yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md) tercih edilen modeli ve tüm belgeler yönergelerinin ve SAS bugün özel kullanımda. Üstelik, artık, ACS ile eşleştirilmiş durumda yeni bir geçiş ad alanı oluşturabileceğiniz mümkündür.

Hemen başka bir hizmete bağımlı değil ancak bir istemci tüm aracılar olmadan doğrudan SAS kural adı ve kural anahtarı istemci erişim vererek kullanılabilir SAS avantajına sahiptir. SAS, bir istemci bir yetkilendirme denetimi başka bir hizmet ile ilk geçirilecek olan ve ardından bir belirteç verilir bir yaklaşım ile aynı zamanda bir kolayca tümleştirilebilir. İkinci yaklaşımda ACS kullanım desenine benzer, ancak ACS'de express zor olan uygulama özel koşullara bağlı veren erişim belirteçleri sağlar.

Üzerinde ACS bağımlı olan tüm mevcut uygulamalar için biz bunun yerine SAS yararlanmayı uygulamalarını konusunda müşterileri bağlantısını okumanızı tavsiye.

## <a name="migration-scenarios"></a>Geçiş senaryoları

ACS ve geçiş paylaşılan bilgisi tümleşik bir *imzalama anahtarı*. İmzalama anahtarı yetkilendirme Belirteçleri imzalamak için bir ACS ad alanı tarafından kullanılır ve belirteç eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için Azure geçişi tarafından kullanılır. ACS ad alanı, hizmet kimlikleri ve yetkilendirme kuralları içerir. Yetkilendirme kuralları, hangi hizmet kimliği veya bir dış tarafından verilen bir belirteç hangi kimlik sağlayıcısı hangi tür bir erişimin bir parçası bir geçiş ad alanı grafik, en uzun ön ek eşleştirmesi biçiminde alır tanımlayın.

Örneğin, bir ACS kuralı vermek **Gönder** yol ön ek talep `/` için hizmet kimliği, yani bu kurala dayalı ACS tarafından verilmiş bir belirteç istemci ad alanındaki tüm varlıklara Gönder hakkı verir. Yol ön eki ise `/abc`, kimlik adlı varlıklara göndermeyi sınırlıdır `abc` veya bu ön eki altında düzenlenir. Bu Geçiş Kılavuzu okuyucular zaten bu kavramlarla ilgili bilgi sahibi olduğunuz varsayılır.

Geçiş senaryoları üç ana kategoriye ayrılır:

1.  **Varsayılan değerleri değiştirmeden**. Bazı müşteriler bir [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi, otomatik olarak oluşturulan geçirme **sahibi** kimliği ve gizli anahtarını geçiş ad alanı ile eşleştirilmiş ACS ad alanı için hizmet ve yapın Yeni Kural Ekle değil.

2.  **Özel hizmet kimlikleri basit kurallarla**. Bazı müşteriler yeni hizmet kimlikleri ekleyin ve her yeni bir hizmet kimliği vermek **Gönder**, **dinleme**, ve **Yönet** belirli bir varlık için izinleri.

3.  **Özel hizmet kimlikleri karmaşık kurallar ile**. Çok az sayıda müşterilerin karmaşık kural kümeleri hangi harici olarak verilen belirteçler geçiş hakları eşlendiğine ya da burada tek hizmet kimliği atanır birden çok kural birden fazla ad alanı yollarını hakları Ayrıştırılan var.

Karmaşık kural kümeleri geçişi ile ilgili Yardım almak için sizinle [Azure Destek](https://azure.microsoft.com/support/options/). Diğer iki senaryoda basit geçişi etkinleştirin.

### <a name="unchanged-defaults"></a>Değiştirilmemiş Varsayılanları

Uygulamanızı ACS Varsayılanları değişmemişse tüm değiştirebilirsiniz [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımı ile bir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesne ve önceden yapılandırılmış ad alanı kullanın **RootManageSharedAccessKey** ACS yerine **sahibi** hesabı. ACS ile bile unutmayın **sahibi** hesabının, bu yapılandırma (ve hala) bu hesabı/kural silmek için izni de dahil olmak üzere ad alanı üzerinde tam yönetim yetkilisi sağladığından değil genel olarak, önerilen varlıklar.

### <a name="simple-rules"></a>Basit kural

Uygulama basit kurallarıyla özel hizmet kimlikleri kullanıyorsa, geçiş, belirli bir geçiş üzerinde erişim denetimi sağlamak için bir ACS hizmet kimliği oluşturulduğu durumda basittir. Bu senaryo genellikle SaaS stili çözümlerde, belirli bir site için bir kiracı sitesi veya şube ile hizmet kimliği için bir köprü oluşturulduğundan her geçişi kullanıldığı durumdur. Bu durumda, ilgili hizmet kimliği geçişi üzerinde doğrudan bir paylaşılan erişim imzası kural geçirilebilir. Hizmet kimliği adı, SAS kural adı haline gelir ve hizmet kimlik anahtarı SAS kural anahtarı haline gelir. Varlık için yapılandırılmış eşdeğer sırasıyla geçerli ACS kural ardından SAS kuralın haklarıdır.

ACS ile Federasyon tüm var olan bir ad alanında yerinde SAS'ın bu yeni ve ek yapılandırma yapabilir ve ACS uzağa geçiş kullanarak daha sonra gerçekleştirilen [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) yerine [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). Ad alanı ACS'den bağlantısı olması gerekmez.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kuralları hesapları olması beklenmez, ancak haklarıyla ilişkili İmzalama anahtarları olarak adlandırılır. Bu nedenle, uygulama birçok hizmet kimlikleri oluşturur ve bunları veren senaryoları erişim haklarını çeşitli varlıklar için veya tüm ad alanı yine de bir belirteci verilirken aracı gerektirir. Yönergeler için bu tür bir aracı tarafından edinebilirsiniz [destekle iletişim kurarak](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar

Azure geçişi kimlik doğrulaması hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Azure geçişi kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md)
* [Paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](../service-bus-messaging/service-bus-sas.md)


