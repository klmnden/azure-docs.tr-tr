---
title: 'Azure AD Connect eşitleme: Varsayılan yapılandırmanın değiştirilmesine | Microsoft Docs'
description: Azure AD Connect eşitleme varsayılan yapılandırmasını değiştirmeye yönelik en iyi uygulamalar sağlanır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 940a35d89996b1eb9600fe4214863d2b5304750e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60242152"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect eşitleme: Varsayılan yapılandırmanın değiştirilmesine ilişkin en iyi yöntemler
Azure AD Connect eşitlemesi için desteklenen ve desteklenmeyen değişiklikleri açıklamak için bu konunun amacı olan.

Azure AD Connect tarafından oluşturulan yapılandırma, "olduğu gibi" Azure AD ile şirket içi Active Directory eşitleme, çoğu ortam için çalışır. Ancak, bazı durumlarda, belirli bir gereksinim ya da gereksinimi karşılamak için bir yapılandırma için bazı değişiklikleri uygulamak gerekli.

## <a name="changes-to-the-service-account"></a>Hizmet hesabı değişiklikleri
Azure AD Connect eşitleme, Yükleme Sihirbazı tarafından oluşturulan bir hizmet hesabı altında çalışıyor. Bu hizmet hesabı, eşitleme tarafından kullanılan veritabanı şifreleme anahtarları tutar. 127 karakter uzunluğunda bir parola ile oluşturulur ve parolayı dolmayacak şekilde ayarlayın.

* Bu **desteklenmeyen** değiştirme veya hizmet hesabı parolasını sıfırlayın. Bunun yapılması, şifreleme anahtarlarını yok eder ve hizmet veritabanına erişmek mümkün değildir ve başlatmak mümkün değildir.

## <a name="changes-to-the-scheduler"></a>Zamanlayıcı değişiklikleri
Derleme 1.1 sürümlerden başlayarak yapılandırabileceğiniz (Şubat 2016) [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) 30 dakika bir varsayılandan farklı bir eşitleme döngüsü için.

## <a name="changes-to-synchronization-rules"></a>Eşitleme kuralları için değişiklikler
Yükleme Sihirbazı en yaygın senaryolar için çalışması için gereken bir yapılandırma sağlar. Ardından yapılandırmada değişiklikler yapmanız durumunda, desteklenen bir yapılandırma için hala şu kurallara uymalıdır.

> [!WARNING]
> Varsayılan eşitleme kuralları için değişiklik yaparsanız bu değişiklikler Azure AD Connect, eşitleme beklenmeyen ve olası istenmeyen sonuçları elde edilen sonraki güncelleştirildiğinde yazılır.

* Yapabilecekleriniz [öznitelik akışları değiştirme](how-to-connect-sync-change-the-configuration.md#other-common-attribute-flow-changes) varsayılan doğrudan öznitelik akışları kuruluşunuz için uygun değilse.
* İsterseniz [bir öznitelik akışı değil](how-to-connect-sync-change-the-configuration.md#do-not-flow-an-attribute) ekleme ve kaldırma herhangi bir mevcut öznitelik değerleri Azure AD'de, bu senaryo için bir kural oluşturmanız gerekir.
* [İstenmeyen bir eşitleme kuralı devre dışı bırak](#disable-an-unwanted-sync-rule) silmek yerine. Silinen bir kural, yükseltme sırasında yeniden oluşturulur.
* İçin [kullanıma hazır bir kuralı değiştirmek](#change-an-out-of-box-rule), özgün kuralı bir kopyasını alın ve kullanıma hazır kuralı devre dışı bırak. Eşitleme kuralı Düzenleyicisi ister ve yardımcı olur.
* Eşitleme kuralları Düzenleyicisi'ni kullanarak özel bir eşitleme kurallarınızı dışarı aktarın. Düzenleyici kolayca bir olağanüstü durum kurtarma senaryosunda yeniden oluşturmak için kullanabileceğiniz bir PowerShell komut dosyası sağlar.

> [!WARNING]
> Kullanıma hazır eşitleme kuralları bir parmak izi var. Bu kural için bir değişiklik yaparsanız, parmak izini artık eşleşen. Gelecekte yeni Azure AD Connect sürümü uygulamaya çalıştığınızda sorunlar yaşayabilirsiniz. Yalnızca bu anlatılan şekilde bu makalede değişiklik.

### <a name="disable-an-unwanted-sync-rule"></a>İstenmeyen bir eşitleme kuralı devre dışı bırak
Bir kullanıma hazır eşitleme kuralı silmeyin. Bu, sonraki yükseltme sırasında yeniden oluşturulur.

Bazı durumlarda, Yükleme Sihirbazı'nı topolojiniz için çalışmayan bir yapılandırma hazırlamıştır. Örneğin, bir hesap-kaynak orman topolojisini varsa ancak Exchange şema hesabı ormanındaki şemasıyla genişletilmiş ardından Exchange kuralları hesap ormanı ve kaynak ormanı için oluşturulur. Bu durumda, Exchange için eşitleme kuralı devre dışı bırakmak gerekir.

![Devre dışı eşitleme kuralı](./media/how-to-connect-sync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Yukarıdaki resimde, Yükleme Sihirbazı, eski bir Exchange 2003 şema hesabı ormanında buldu. Bu şema uzantısı, kaynak ormanı Fabrikam'ın ortamında sunulmadan önce eklendi. Öznitelikleri eski Exchange uygulamasından eşitlendiğinden emin olmak için eşitleme kuralı gösterildiği gibi devre dışı gerekir.

### <a name="change-an-out-of-box-rule"></a>Kullanıma hazır bir kuralı değiştirin
Birleştirme kuralı değiştirmek, ihtiyacınız olduğunda, bir out-of-box kuralı değiştirmelisiniz tek zamandır. Bir öznitelik akışı değiştirmeniz gerekiyorsa, kullanıma hazır kurallardan daha yüksek önceliğe sahip bir eşitleme kuralı oluşturmanız gerekir. Pratikte ihtiyacınız kopyalamak için yalnızca kural kuralıdır **içinde ad - kullanıcı katılın**. Diğer tüm kurallar daha yüksek bir öncelik kuralı ile geçersiz kılabilirsiniz.

Ardından bir out-of-box kuralı değişiklikler yapmanız gerekirse, kullanıma hazır kuralı bir kopyasını oluşturun ve özgün kuralı devre dışı bırakmak gerekir. Ardından kopyalanan kuralı değişiklikleri yapın. Eşitleme kuralı Düzenleyicisi bu adımlarla yardımcı oluyor. Bir kullanıma hazır kuralı açtığınızda, bu iletişim kutusu ile sunulur:  
![Uyarı kutusunu kural dışı](./media/how-to-connect-sync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Seçin **Evet** kuralın bir kopyası oluşturmasını ister. Kopyalanan bir kuralı sonra açılır.  
![Kopyalanan bir kuralı](./media/how-to-connect-sync-best-practices-changing-default-configuration/clonedrule.png)

Kopyalanan bu kuralı, kapsam, birleştirme ve dönüştürme için gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
