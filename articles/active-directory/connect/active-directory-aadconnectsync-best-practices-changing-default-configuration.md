---
title: "Azure AD Connect eşitleme: varsayılan yapılandırmasını değiştirme | Microsoft Docs"
description: "Azure AD Connect eşitleme varsayılan yapılandırmasını değiştirmek için en iyi yöntemler sağlar."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 86b9af69063ded2740ac353b863442725104c071
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect eşitleme: en iyi uygulamalar varsayılan yapılandırmasını değiştirmek için
Azure AD Connect eşitleme için desteklenen ve desteklenmeyen değişiklikleri açıklamak için bu konunun amacı budur.

Azure AD Connect tarafından oluşturulan yapılandırma, Azure AD ile şirket içi Active Directory eşitleme çoğu ortam için "olduğu gibi" çalışır. Ancak, bazı durumlarda, bazı değişiklikler gereksiniminizi veya gereksinimi karşılamak için bir yapılandırma uygulamak gereklidir.

## <a name="changes-to-the-service-account"></a>Hizmet hesabı yapılan değişiklikler
Azure AD Connect eşitleme, Yükleme Sihirbazı tarafından oluşturulan bir hizmet hesabı altında çalışıyor. Bu hizmet hesabının, eşitleme tarafından kullanılan veritabanı için şifreleme anahtarları tutar. İle 127 karakterden uzun bir parola oluşturulur ve parolası olmayan süresi dolacak şekilde ayarlanır.

* Bu **desteklenmeyen** değiştirme veya hizmet hesabı parolasını sıfırlama. Bunun yapılması şifreleme anahtarları yok eder ve hizmet veritabanına erişmek mümkün değildir ve başlatmanız mümkün değil.

## <a name="changes-to-the-scheduler"></a>Zamanlayıcı yapılan değişiklikler
Yapı 1.1 sürümlerden itibaren yapılandırabileceğiniz (Şubat 2016) [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) 30 dakika varsayılandan farklı eşitleme döngüsü için.

## <a name="changes-to-synchronization-rules"></a>Eşitleme kuralları yapılan değişiklikler
Yükleme Sihirbazı'nı en yaygın senaryolar için çalışması için gereken bir yapılandırma sağlar. Yapılandırma değişiklikleri yapmanız durumunda, bu kurallar hala desteklenen yapılandırmaya sahip izlemeniz gerekir.

* Yapabilecekleriniz [öznitelik akışları değiştirmek](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) varsayılan doğrudan öznitelik akışları, kuruluşunuz için uygun değilse.
* İsterseniz [bir öznitelik akışı değil](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) ekleme ve kaldırma varolan bir özniteliğe değerleri Azure AD içinde sonra da, bu senaryo için bir kural oluşturmanız gerekir.
* [İstenmeyen bir eşitleme kuralı devre dışı](#disable-an-unwanted-sync-rule) silmeden yerine. Kuralı silindi yükseltme sırasında yeniden oluşturulur.
* İçin [bir out-of-box kuralı değiştirme](#change-an-out-of-box-rule), özgün kuralı bir kopyasını alın ve out-of-box kuralını devre dışı bırakmak gerekir. Eşitleme kuralı Düzenleyicisi'ni ister ve yardımcı olur.
* Özel eşitleme kurallarınızı eşitleme kuralları Düzenleyicisi'ni kullanarak dışarı aktarın. Düzenleyici kolayca bir olağanüstü durum kurtarma senaryosunda yeniden oluşturmak için kullanabileceğiniz bir PowerShell Betiği sağlar.

> [!WARNING]
> Out-of-box eşitleme kuralı bir parmak izine sahip. Bu kural için bir değişiklik yaparsanız, parmak izi artık eşleşen. Gelecekte Azure AD Connect yeni bir sürüm uygulamaya çalıştığınızda sorunlar yaşayabilirsiniz. Yalnızca bu açıklandığı şekilde bu makalede değişiklik.

### <a name="disable-an-unwanted-sync-rule"></a>İstenmeyen bir eşitleme kuralı devre dışı bırak
Bir out-of-box eşitleme kuralı silmeyin. Bir sonraki yükseltme sırasında yeniden oluşturulur.

Bazı durumlarda, Yükleme Sihirbazı'nı topolojiniz için çalışmayan bir yapılandırma üretilen. Örneğin, bir hesap-kaynak orman topolojisini var ancak hesap ormanındaki şema Exchange şeması genişletilmiş ardından kuralları Exchange için hesap ormanı ve kaynak ormanı için oluşturulur. Bu durumda, Exchange için eşitleme kuralı devre dışı bırakmanız gerekir.

![Devre dışı eşitleme kuralı](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Yukarıdaki resimde, Yükleme Sihirbazı'nı hesap ormanında bir eski Exchange 2003 şeması buldu. Kaynak orman Fabrikam'ın ortamında sunulmadan önce bu şema uzantısı eklendi. Eski Exchange uygulamadan özniteliklere eşitlenir emin olmak için eşitleme kuralı gösterildiği gibi devre dışı gerekir.

### <a name="change-an-out-of-box-rule"></a>Out-of-box kuralı değiştirme
Birleştirme kuralı değiştirmeniz gerektiğinde bir out-of-box kuralı değiştirmelisiniz yalnızca bir kez gösterilecektir. Bir öznitelik akışı değiştirmeniz gerekiyorsa, out-of-box kurallarından daha yüksek önceliği olan bir eşitleme kuralı oluşturmanız gerekir. Pratikte gereksinim kopyalamak için yalnızca kural kuralıdır **içinde AD'den - kullanıcı katılma**. Daha yüksek bir öncelik kural ile tüm diğer kuralları geçersiz kılabilirsiniz.

Ardından bir out-of-box kuralı değişiklikleri yapmanız gerekirse, out-of-box kuralı bir kopyasını alın ve özgün kuralı devre dışı bırakmak gerekir. Ardından kopyalanan kuralı değişiklikleri yapın. Eşitleme kuralı Düzenleyicisi'ni bu adımlara yardımcı oluyor. Bir out-of-box kuralı açtığınızda, bu iletişim kutusu sunulur:  
![Uyarı kutusu kural dışında](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Seçin **Evet** kuralın bir kopyası oluşturulamıyor. Kopyalanan kuralı daha sonra açılır.  
![Kopyalanan kuralı](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Kopyalanan bu kuralı, kapsam, birleştirme ve dönüştürme gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
