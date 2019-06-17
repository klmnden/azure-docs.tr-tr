---
title: Azure Active Directory erişim denetimi hizmetinden alınan paylaşılan erişim imzası yetkisi geçirme | Microsoft Docs
description: Uygulamaları, erişim denetimi hizmetinden SAS'ye geçiş
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 746b19062c3014caa37c6668e6c41df054a47e25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60868171"
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Azure Active Directory erişim denetimi hizmetinden alınan paylaşılan erişim imzası yetkisi geçirme

Hizmet veri yolu uygulamaları iki farklı yetkilendirme modeli kullanarak bir seçeneği önceden sahip: [paylaşılan erişim imzası (SAS)](service-bus-sas.md) doğrudan hizmet veri yolu tarafından sağlanan belirteç modelini ve Federasyon modeli nerede Yönetimi Yetkilendirme kuralları yönetilir içinde [Azure Active Directory](/azure/active-directory/) Access Control Service (ACS) ve ACS'den alınan belirteçlerin geçirilir Service Bus'a istenen özelliklere erişim yetkisi vermek için.

ACS yetkilendirme modelini uzun yerini almıştır [SAS yetkilendirme](service-bus-authentication-and-authorization.md) tercih edilen modeli ve tüm belgeler yönergelerinin ve SAS bugün özel kullanımda. Üstelik, artık ACS ile eşleştirilmiş yeni hizmet veri yolu ad alanları oluşturmak mümkündür.

Hemen başka bir hizmete bağımlı değil ancak bir istemci tüm aracılar olmadan doğrudan SAS kural adı ve kural anahtarı istemci erişim vererek kullanılabilir SAS avantajına sahiptir. SAS, bir istemci bir yetkilendirme denetimi başka bir hizmet ile ilk geçirilecek olan ve ardından bir belirteç verilir bir yaklaşım ile aynı zamanda bir kolayca tümleştirilebilir. İkinci yaklaşımda ACS kullanım desenine benzer, ancak ACS'de express zor olan uygulama özel koşullara bağlı veren erişim belirteçleri sağlar.

Üzerinde ACS bağımlı olan tüm mevcut uygulamalar için biz bunun yerine SAS yararlanmayı uygulamalarını konusunda müşterileri bağlantısını okumanızı tavsiye.

## <a name="migration-scenarios"></a>Geçiş senaryoları

ACS ve Service Bus ile paylaşılan bilgisi tümleşik bir *imzalama anahtarı*. İmzalama anahtarı yetkilendirme Belirteçleri imzalamak için bir ACS ad alanı tarafından kullanılır ve belirteç eşleştirilmiş ACS ad alanı tarafından verildiğini doğrulamak için Service Bus tarafından kullanılır. ACS ad alanı, hizmet kimlikleri ve yetkilendirme kuralları içerir. Yetkilendirme kuralları, hangi hizmet kimliği veya bir dış tarafından verilen bir belirteç hangi kimlik sağlayıcısı hangi tür bir erişimin bir parçası bir Service Bus ad alanı grafik, en uzun ön ek eşleştirmesi biçiminde alır tanımlayın.

Örneğin, bir ACS kuralı vermek **Gönder** yol ön ek talep `/` için hizmet kimliği, yani bu kurala dayalı ACS tarafından verilmiş bir belirteç istemci ad alanındaki tüm varlıklara Gönder hakkı verir. Yol ön eki ise `/abc`, kimlik adlı varlıklara göndermeyi sınırlıdır `abc` veya bu ön eki altında düzenlenir. Bu Geçiş Kılavuzu okuyucular zaten bu kavramlarla ilgili bilgi sahibi olduğunuz varsayılır.

Geçiş senaryoları üç ana kategoriye ayrılır:

1.  **Varsayılan değerleri değiştirmeden**. Bazı müşteriler bir [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) nesnesi, otomatik olarak oluşturulan geçirme **sahibi** servis kimlik ve Service Bus ad alanı ile eşleştirilmiş ACS ad alanı için gizli anahtarı ve yeni kurallar eklemeyin.

2.  **Özel hizmet kimlikleri basit kurallarla**. Bazı müşteriler yeni hizmet kimlikleri ekleyin ve her yeni bir hizmet kimliği vermek **Gönder**, **dinleme**, ve **Yönet** belirli bir varlık için izinleri.

3.  **Özel hizmet kimlikleri karmaşık kurallar ile**. Çok az sayıda müşterilerin karmaşık kural kümeleri hangi harici olarak verilen belirteçler geçiş hakları eşlendiğine ya da burada tek hizmet kimliği atanır birden çok kural birden fazla ad alanı yollarını hakları Ayrıştırılan var.

Karmaşık kural kümeleri geçişi ile ilgili Yardım almak için sizinle [Azure Destek](https://azure.microsoft.com/support/options/). Diğer iki senaryoda basit geçişi etkinleştirin.

### <a name="unchanged-defaults"></a>Değiştirilmemiş Varsayılanları

Uygulamanızı ACS Varsayılanları değişmemişse tüm değiştirebilirsiniz [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) kullanımı ile bir [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) nesne ve önceden yapılandırılmış ad alanı kullanın **RootManageSharedAccessKey** ACS yerine **sahibi** hesabı. ACS ile bile unutmayın **sahibi** hesabının, bu yapılandırma (ve hala) bu hesabı/kural silmek için izni de dahil olmak üzere ad alanı üzerinde tam yönetim yetkilisi sağladığından değil genel olarak, önerilen varlıklar.

### <a name="simple-rules"></a>Basit kural

Uygulama basit kurallarıyla özel hizmet kimlikleri kullanıyorsa, geçiş, belirli bir kuyruğa üzerindeki erişim denetimi sağlamak için bir ACS hizmet kimliği oluşturulduğu durumda basittir. Bu senaryo genellikle SaaS stili çözümlerde, belirli bir site için bir kiracı sitesi veya şube ile hizmet kimliği için bir köprü oluşturulduğundan her kuyruk kullanıldığı durumdur. Bu durumda, ilgili hizmet kimliği doğrudan kuyruktaki bir paylaşılan erişim imzası kural geçirilebilir. Hizmet kimliği adı, SAS kural adı haline gelir ve hizmet kimlik anahtarı SAS kural anahtarı haline gelir. Varlık için yapılandırılmış eşdeğer sırasıyla geçerli ACS kural ardından SAS kuralın haklarıdır.

ACS ile Federasyon tüm var olan bir ad alanında yerinde SAS'ın bu yeni ve ek yapılandırma yapabilir ve ACS uzağa geçiş kullanarak daha sonra gerçekleştirilen [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) yerine [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). Ad alanı ACS'den bağlantısı olması gerekmez.

### <a name="complex-rules"></a>Karmaşık kurallar

SAS kuralları hesapları olması beklenmez, ancak haklarıyla ilişkili İmzalama anahtarları olarak adlandırılır. Bu nedenle, uygulama birçok hizmet kimlikleri oluşturur ve bunları veren senaryoları erişim haklarını çeşitli varlıklar için veya tüm ad alanı yine de bir belirteci verilirken aracı gerektirir. Yönergeler için bu tür bir aracı tarafından edinebilirsiniz [destekle iletişim kurarak](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar

Service Bus kimlik doğrulaması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)
* [Paylaşılan erişim imzaları ile Service Bus kimlik doğrulaması](service-bus-sas.md)

