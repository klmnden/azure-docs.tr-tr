---
title: Azure AD Connect - neden userCertificate özniteliğinin yol açtığı LargeObject hatalarını | Microsoft Docs
description: Bu konuda, neden userCertificate özniteliğinin yol açtığı LargeObject hatalarını düzeltme adımları sağlanır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: c851b5ef024e6584e6f8c93995208b08a91fbb60
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095498"
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect eşitleme: Neden userCertificate özniteliğinin yol açtığı LargeObject hatalarını işleme

Azure AD, bir üst sınır uygular **15** sertifika değerleri **userCertificate** özniteliği. Azure AD Connect 15'ten fazla değerleri içeren bir nesne, Azure AD'ye dışarı aktarır, Azure AD'ye verir. bir **LargeObject** hata iletisi:

>*"Sağlanan nesne çok büyük. Özniteliği bu nesnedeki değer sayısını azaltın. ... Sonraki eşitleme döngüsünde işlemin yeniden denenmesi "*

Diğer AD öznitelikleri tarafından LargeObject hataya neden olabilir. Bu gerçekten de neden olur userCertificate özniteliğinin onaylamak için doğrulayın ya da şirket içi AD nesnesi karşı veya içinde gerekir [Eşitleme Hizmeti Yöneticisi meta veri deposu arama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

Kiracınızdaki LargeObject hataları olan nesnelerin listesini almak için aşağıdaki yöntemlerden birini kullanın:

 * Kiracınız için Azure AD Connect Health Eşitleme etkinse başvurabilirsiniz [eşitleme hata raporu](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync) sağlanan.
 
 * Her bir eşitleme döngüsü sonunda gönderilen bildirim e-posta için dizin eşitleme hatalarına LargeObject hataları nesnelerin listesi vardır. 
 * [Eşitleme Hizmeti Yöneticisi işlem sekmesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) son dışarı aktarma işlemi Azure AD için tıklarsanız LargeObject hataları olan nesnelerin listesini görüntüler.
 
## <a name="mitigation-options"></a>Risk azaltma seçenekleri
LargeObject hata çözümlenene kadar aynı nesneye diğer öznitelik değişiklikler Azure AD'ye dışarı aktarılamaz. Hatayı gidermek için aşağıdaki seçenekleri göz atabilirsiniz:

 * 1\.1.524.0 oluşturmak için Azure AD Connect'e yükseltmenin veya sonra. Azure AD Connect'e bağlanan 1.1.524.0, kuralları, öznitelikleri 15'ten fazla değer varsa öznitelikleri userCertificate ve Usersmımecertificate vermemeniz şekilde güncelleştirildi kullanıma hazır eşitleme oluşturun. Azure AD Connect yükseltme hakkında daha fazla ayrıntı için makalesine bakın [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version) makalesine bakın.

 * Uygulama bir **giden eşitleme kuralı** Azure AD CONNECT'te dışarı aktaran bir **değeri 15'ten fazla sertifika değerleri ile nesneler için gerçek değerleri yerine null**. Bu seçenek, herhangi bir sertifika değeri 15'ten fazla değerleri ile nesneler için Azure AD'ye aktarılacak gerekmiyorsa uygundur. Nasıl bu eşitleme kuralının uygulanacağı hakkında daha fazla bilgi için sonraki bölüme bakın [dışarı aktarma userCertificate özniteliğinin sınırlamak için uygulama eşitleme kuralı](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Şirket içi sertifika değerlerin sayısını azaltmak kuruluşunuz tarafından artık kullanılmayan değerleri kaldırarak AD nesnesi (15 veya daha az). Bu öznitelik Şişirme tarafından kullanılmayan ya da süresi dolmuş sertifikaları neden oluyorsa uygundur. Kullanabileceğiniz [PowerShell Betiği buradan kullanılabilir](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) bulmak için yedekleme ve şirket içi süresi dolan sertifikaları sil AD. Sertifikaları silmeden önce kuruluşunuzda ile ortak anahtar altyapısı yöneticilerinin doğrulamanız önerilir.

 * Azure AD Connect userCertificate özniteliğinin Azure AD'ye aktarılan hariç tutmak için yapılandırın. Öznitelik belirli senaryoları etkinleştirmek için Microsoft Online Services tarafından kullanılabilir olduğundan genel olarak, bu seçenek önermiyoruz. Özellikle:
    * Kullanıcı nesnesindeki userCertificate özniteliğinin yol açtığı ileti imzalama ve şifreleme için Exchange Online ve Outlook istemcileri tarafından kullanılır. Bu özellik hakkında daha fazla bilgi için makalesine bakın. [S/MIME iletisi imzalama ve şifreleme için](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * Bilgisayar nesnesinde userCertificate özniteliğinin yol açtığı, Windows 10 şirket içi etki alanına katılmış cihazların Azure AD'ye bağlanmasına izin vermek için Azure AD tarafından kullanılır. Bu özellik hakkında daha fazla bilgi edinmek için lütfen makaleye başvurun [deneyimleri Windows 10 için etki alanına katılan cihazları Azure AD'ye bağlanma](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-to-limit-export-of-usercertificate-attribute"></a>Dışarı aktarma userCertificate özniteliğinin sınırlamak için eşitleme kuralı uygulama
UserCertificate özniteliğinin neden olduğu LargeObject hatası gidermek için dışarı aktarır Azure AD Connect giden eşitleme kuralı uygulayabilirsiniz. bir **değeri 15'ten fazla sertifika değerlerlenesneleriçingerçekdeğerleriyerinenull**. Bu bölüm için eşitleme kuralı uygulamak için gereken adımları açıklar **kullanıcı** nesneleri. Adımlar için uyarlanabilir **kişi** ve **bilgisayar** nesneleri.

> [!IMPORTANT]
> Null değer dışarı aktarma, değerleri daha önce başarıyla Azure AD'ye aktarılan sertifika kaldırır.

Adımları olarak özetlenebilir:

1. Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden eşitleme olduğunu doğrulayın.
3. Mevcut giden eşitleme kuralı için userCertificate özniteliğinin bulun.
4. Gerekli giden eşitleme kuralı oluşturun.
5. Yeni eşitleme kuralı LargeObject hatası ile var olan bir nesne üzerinde doğrulayın.
6. Yeni eşitleme kuralı LargeObject hatası kalan nesneler için geçerlidir.
7. Azure AD'ye aktarılacak bekleyen değişiklik yok beklenmeyen doğrulayın.
8. Değişiklikler Azure AD'ye dışarı aktarın.
9. Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1\.Adım Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden eşitleme olduğunu doğrulayın
Azure AD'ye dışarı aktarılan istenmeyen değişiklikleri önlemek için yeni bir eşitleme kuralının uygulanması ortasında durumdayken eşitleme gerçekleşir emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:
1. Azure AD Connect sunucusunda PowerShell oturumu başlatın.

2. Zamanlanan eşitleme cmdlet'ini çalıştırarak devre dışı bırakın: `Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> Önceki adımlarda, yalnızca yerleşik Zamanlayıcı ile Azure AD Connect (1.1.xxx.x) daha yeni sürümleri için geçerlidir. Windows Görev Zamanlayıcı'yı kullanan Azure AD Connect'in eski sürümlerini (1.0.xxx.x) kullandığınız ya da düzenli eşitleme tetiklemek için kendi özel Zamanlayıcı (bilinen) kullanıyorsanız, bunları uygun şekilde devre dışı bırakmanız gerekir.

1. Başlangıç **Eşitleme Hizmeti Yöneticisi** → Başlangıç eşitleme hizmetine giderek.

1. Git **işlemleri** sekme ve durumu olan bir işlemi yok onaylayın *"sürüyor."*

### <a name="step-2-find-the-existing-outbound-sync-rule-for-usercertificate-attribute"></a>2\.Adım Mevcut giden eşitleme kuralı için userCertificate özniteliğinin Bul
Etkin ve kullanıcı nesnelerinin userCertificate özniteliğinin Azure AD'ye dışarı aktarmak için yapılandırılmış mevcut bir eşitleme kuralı olması gerekir. Öğrenmek için bu eşitleme kuralı bulun, **öncelik** ve **kapsam belirleme filtresi** yapılandırma:

1. Başlangıç **eşitleme kuralları Düzenleyicisi** → Başlangıç eşitleme kuralları Düzenleyicisi için giderek.

2. Arama filtreleri, aşağıdaki değerleri yapılandırın:

    | Öznitelik | Değer |
    | --- | --- |
    | Direction |**Giden** |
    | MV nesne türü |**Kişi** |
    | Bağlayıcı |*Azure AD Bağlayıcısı adı* |
    | Bağlayıcı nesnesi türü |**Kullanıcı** |
    | MV özniteliği |**userCertificate** |

3. Kullanıcı, nesneyi userCertficiate özniteliği dışarı aktarmak için Azure AD Bağlayıcısı için OOB (kullanıma hazır) eşitleme kuralları kullanıyorsanız, geri almalısınız *"Out için AAD – kullanıcı ExchangeOnline"* kuralı.
4. Not **önceliği** değeri bu eşitleme kuralı.
5. Eşitleme kuralı seçin ve tıklayın **Düzenle**.
6. İçinde *"Ayrılmış kuralı onayı Düzenle"* açılır iletişim kutusu, tıklayın **Hayır**. (Endişelenmeyin, herhangi bir değişiklik için bu eşitleme kuralı kullanacağız değil).
7. Düzenleme ekranında seçin **Scoping filtre** sekmesi.
8. Kapsam belirleme filtresi yapılandırmayı unutmayın. OOB eşitleme kuralı kullanıyorsanız, tam olarak olması gerektiğini **iki yan tümceyi içeren bir kapsam belirleme filtre grubu**de dahil olmak üzere:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

### <a name="step-3-create-the-outbound-sync-rule-required"></a>Adım 3. Gerekli giden eşitleme kuralı oluşturma
Yeni eşitleme kuralı aynı olmalıdır **kapsam belirleme filtresi** ve **daha yüksek önceliği** daha mevcut eşitleme kuralı. Bu, yeni eşitleme kuralı aynı nesne kümesini mevcut eşitleme kuralı olarak uygulanır ve mevcut eşitleme kuralı userCertificate özniteliği için geçersiz kılmalar sağlar. Eşitleme kuralı oluşturmak için:
1. Eşitleme kuralları Düzenleyicisi'nde **Yeni Kural Ekle** düğmesi.
2. Altında **açıklaması sekmesi**, aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *"Out – AAD için özel geçersiz kılmayı için userCertificate"* |
    | Açıklama | *Bir açıklama sağlayın* | Örneğin, *"UserCertificate özniteliğinin yol açtığı 15'ten fazla değer yoksa NULL verin."* |
    | Bağlı sistem | *Azure AD Bağlayıcısı'nı seçin* |
    | Bağlı sistem nesnesi türü | **Kullanıcı** | |
    | Meta veri deposu nesne türü | **Kişi** | |
    | Bağlantı türü | **Birleştir** | |
    | Öncellik | *1-99 arasında bir sayı seçti* | Seçilen sayı tüm mevcut eşitleme kuralı tarafından kullanılmamalıdır ve daha düşük bir değere sahip (ve bu nedenle, daha yüksek öncelik) varolan eşitleme kuralı daha. |

3. Git **Scoping filtre** sekme ve mevcut eşitleme kuralı kullanarak aynı kapsam belirleme filtresi uygulayın.
4. Skip **birleştirme kuralları** sekmesi.
5. Git **dönüşümleri** yapılandırmayı kullanarak yeni bir dönüştürme eklemek için sekmesinde:

    | Öznitelik | Değer |
    | --- | --- |
    | Akış türü |**İfade** |
    | Hedef öznitelik |**userCertificate** |
    | Kaynak özniteliği |*Şu ifadeyi kullanın*: `IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Tıklayın **Ekle** eşitleme kuralı oluşturma düğmesi.

### <a name="step-4-verify-the-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>4\. adımı. Yeni eşitleme kuralı LargeObject hatası ile var olan bir nesne üzerinde doğrulayın
Bu, diğer nesnelere uygulamadan önce oluşturulan eşitleme kuralı doğru LargeObject hatası ile var olan bir AD nesne üzerinde çalıştığını doğrulayın.
1. Git **işlemleri** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Azure AD işlemi için en son dışarı aktarma seçin ve LargeObject hataları ile nesnelerinden birine tıklayın.
3.  Bağlayıcı alanı nesne özellikleri açılan ekranda tıklayarak **Önizleme** düğmesi.
4. Açılan ekranda Önizleme seçin **tam eşitleme** tıklatıp **işleme Önizleme**.
5. Önizleme ekran ve bağlayıcı alanı nesne özellikleri ekran kapatın.
6. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
7. Sağ **Azure AD** Bağlayıcısı ve select **Çalıştır...**
8. Bağlayıcı çalıştır açılır pencerede seçin **dışarı** adım ve tıklayın **Tamam**.
9. Dışarı aktarma tamamlamak ve bu belirli bir nesnede LargeObject hata hakkında daha fazla olup olmadığını onaylamak için Azure AD için bekleyin.

### <a name="step-5-apply-the-new-sync-rule-to-remaining-objects-with-largeobject-error"></a>5\. adımı. Yeni eşitleme kuralını LargeObject hatası kalan nesneler uygulamak
Eşitleme kuralı eklendiğinde AD Bağlayıcısı üzerinde tam eşitleme adımını çalıştırmak ihtiyacınız vardır:
1. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Sağ **AD** Bağlayıcısı ve select **Çalıştır...**
3. Bağlayıcı çalıştır açılır pencerede seçin **tam eşitleme** adım ve tıklayın **Tamam**.
4. Tam eşitleme adımını tamamlamak bekleyin.
5. Birden fazla AD bağlayıcıları varsa kalan AD bağlayıcıları için yukarıdaki adımları yineleyin. Genellikle, birden çok şirket içi dizin varsa birden çok bağlayıcı gereklidir.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-to-be-exported-to-azure-ad"></a>6\. adım. Azure AD'ye aktarılacak bekleyen değişiklik yok beklenmeyen doğrulayın
1. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Sağ **Azure AD'ye** Bağlayıcısı ve select **arama bağlayıcı alanı**.
3. Arama bağlayıcı alanı açılır pencerede:
    1. Kapsamı ayarlayın **bekleyen dışa aktarım**.
    2. Dahil olmak üzere tüm 3 onay kutusunu işaretleyin **Ekle**, **Değiştir** ve **Sil**.
    3. Tıklayın **arama** tüm nesneleri Azure AD'ye aktarılacak bekleyen değişikliklerle döndürülecek düğmesi.
    4. Beklenmeyen bir değişiklik bulunmamaktadır doğrulayın. Belirli bir nesne için değişiklikleri incelemek için nesneye çift tıklayın.

### <a name="step-7-export-the-changes-to-azure-ad"></a>7\. Adım. Değişiklikler Azure AD'ye dışarı aktarma
Değişiklikler Azure AD'ye dışarı aktarmak için:
1. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Sağ **Azure AD** Bağlayıcısı ve select **Çalıştır...**
4. Bağlayıcı çalıştır açılır pencerede seçin **dışarı** adım ve tıklayın **Tamam**.
5. Daha fazla LargeObject hata onaylayın ve Azure AD'ye dışarı aktarma için bekleyin.

### <a name="step-8-re-enable-sync-scheduler"></a>8\. Adım Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Sorun çözüldüğünde, yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:
1. PowerShell oturumu başlatın.
2. Zamanlanan eşitleme cmdlet'ini çalıştırarak yeniden etkinleştirin: `Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> Önceki adımlarda, yalnızca yerleşik Zamanlayıcı ile Azure AD Connect (1.1.xxx.x) daha yeni sürümleri için geçerlidir. Windows Görev Zamanlayıcı'yı kullanan Azure AD Connect'in eski sürümlerini (1.0.xxx.x) kullandığınız ya da düzenli eşitleme tetiklemek için kendi özel Zamanlayıcı (bilinen) kullanıyorsanız, bunları uygun şekilde devre dışı bırakmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

