---
title: "Azure AD Connect eşitleme: userCertificate özniteliği tarafından işleme LargeObject hatalardır | Microsoft Docs"
description: "Bu konu, kullanıcı sertifikasını özniteliği tarafından kaynaklanan LargeObject hataları için düzeltme adımları sağlar."
services: active-directory
documentationcenter: 
author: cychua
manager: mtillman
editor: 
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: fa824448288059aaad164035743982a2c9f20b9c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect eşitleme: userCertificate özniteliği tarafından kaynaklanan işleme LargeObject hataları

Azure AD, bir üst sınır zorlar **15** sertifika değerleri **userCertificate** özniteliği. Azure AD Connect, Azure AD 15'ten fazla değerleri içeren bir nesne aktarır, Azure AD'ye verir. bir **LargeObject** hata iletisi:

>*"Sağlanan nesne çok büyük. Bu nesnedeki öznitelik değerlerinin sayısı kesim. İşlem sonraki eşitleme döngüsünde... denenecek"*

Diğer AD öznitelikleri tarafından LargeObject hataya neden olabilir. Bu gerçekten nedeni userCertificate özniteliği tarafından onaylamak için ya da şirket içi AD nesnesine karşı veya içinde doğrulamamız gerekiyor [Eşitleme Hizmeti Yöneticisi'ni meta veri deposu arama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

Kiracınızda LargeObject hataları olan nesnelerin listesini almak için aşağıdaki yöntemlerden birini kullanın:

 * Kiracınız için Azure AD Connect Health Eşitleme etkinse başvurabilirsiniz [eşitleme hata raporu](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync#object-level-synchronization-error-report-preview) sağlanan.
 
 * Her eşitleme döngüsü sonunda gönderilen bildirim e-posta için dizin eşitleme hatalarına LargeObject hataları nesneleriyle listesine sahiptir. 
 * [Eşitleme Hizmeti Yöneticisi'ni işlemleri sekmesinde](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) Azure AD işlemi son ver tıklatırsanız LargeObject hataları olan nesnelerin listesini görüntüler.
 
## <a name="mitigation-options"></a>Azaltma seçenekleri
LargeObject hata çözümlenene kadar diğer öznitelik değişikliklerini aynı nesne için Azure AD dışarı aktarılamaz. Hatayı gidermek için aşağıdaki seçenekleri dikkate alın:

 * 1.1.524.0 oluşturmak için Azure AD Connect yükseltin ya da sonra. Azure AD Connect içinde 1.1.524.0, kuralları öznitelikleri 15'ten fazla değerleri varsa öznitelikleri userCertificate ve userSMIMECertificate değil verilecek güncelleştirilmiş out-of-box eşitleme oluşturun. Azure AD Connect yükseltme hakkında daha fazla ayrıntı için makalesine başvurun [Azure AD Connect: önceki bir sürümünden yükseltme en son](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Uygulama bir **giden eşitleme kuralı** aktarır Azure AD CONNECT'te bir **null değer 15'ten fazla sertifika değerlerini gerçek değerlerle nesneler için yerine**. Bu seçenek, 15'ten fazla değerlerle nesneler için Azure AD'ye aktarılacak sertifika değerlerden herhangi birini gerektirmiyorsa uygundur. Bu eşitleme kuralı gerçekleştirme hakkında daha fazla bilgi için sonraki bölüme bakın [userCertificate özniteliğinin verme sınırlamak için uygulama eşitleme kuralı](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Şirket içi sertifika değerlerine sayısını azaltın artık kuruluşunuz tarafından kullanılmakta olan değerleri kaldırarak AD nesne (15 veya daha az). Bu öznitelik oluşan şişirmeyi süresi dolmuş ya da kullanılmayan sertifikalar tarafından neden oluyorsa uygundur. Kullanabileceğiniz [PowerShell Betiği kullanılabilir burada](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) bulmak için yedekleme ve şirket içi süresi dolan sertifikaları sil AD. Sertifikaları silmeden önce ortak anahtar altyapısı yöneticilerine, kuruluşunuzdaki olduğunu doğrulamanız önerilir.

 * Azure AD'ye aktarılan userCertificate özniteliği dışlamak için Azure AD Connect yapılandırın. Özniteliği, Microsoft Online Services tarafından belirli senaryoları etkinleştirmek için kullanılabilir olduğundan genel olarak, bu seçenek önermiyoruz. Özellikle:
    * Kullanıcı nesnesindeki kullanıcı sertifikasını öznitelik ileti imzalama ve şifreleme için Exchange Online ve Outlook istemcileri tarafından kullanılır. Bu özellik hakkında daha fazla bilgi için makalesine başvurun [S/MIME ileti imzalama ve şifreleme için](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * Bilgisayar nesnesinde userCertificate özniteliği, Windows 10 şirket içi etki alanına katılmış cihazların Azure AD ile bağlanmasına izin vermek için Azure AD tarafından kullanılır. Bu özellik hakkında daha fazla bilgi için lütfen makalesine başvurun [Windows 10 deneyimleri için etki alanına katılmış cihazlar için Azure AD Connect](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-to-limit-export-of-usercertificate-attribute"></a>Dışarı aktarma kullanıcı sertifikasını özniteliğinin sınırlamak için eşitleme kuralı uygulama
UserCertificate özniteliği tarafından neden LargeObject hatayı gidermek için bir giden eşitleme kuralı aktarır Azure AD Connect uygulayabilirsiniz bir **null değer 15'ten fazla sertifika değerlerini gerçek değerlerle nesneler için yerine**. Bu bölüm için eşitleme kuralı uygulamak için gereken adımları açıklar **kullanıcı** nesneleri. Adımları için uyarlanabilir **kişi** ve **bilgisayar** nesneleri.

> [!IMPORTANT]
> Null değer dışarı aktarma değerleri daha önce başarıyla Azure AD'ye aktarılan sertifika kaldırır.

Adımları olarak özetlenebilir:

1. Eşitleme Zamanlayıcı'yı devre dışı bırakın ve devam eden bir eşitleme doğrulayın.
3. Varolan giden eşitleme kuralı için kullanıcı sertifikasını öznitelik bulunamadı.
4. Gerekli giden eşitleme kuralı oluşturun.
5. Yeni eşitleme kuralı LargeObject hatasıyla var olan bir nesne üzerinde doğrulayın.
6. Yeni eşitleme kuralı LargeObject hatasıyla kalan nesneler için geçerlidir.
7. Azure AD'ye aktarılacak bekleyen beklenmeyen bir değişiklik bulunmamaktadır doğrulayın.
8. Değişiklikleri için Azure AD verin.
9. Eşitleme Zamanlayıcı'yı yeniden etkinleştirin.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. Adım Eşitleme Zamanlayıcı'yı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
Azure AD dışarı aktarılan istenmeyen değişiklikleri önlemek için yeni bir eşitleme kuralı uygulama ortasında durumdayken eşitleme gerçekleşir emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:
1. Azure AD Connect sunucusunda PowerShell oturumu başlatın.

2. Zamanlanan eşitleme cmdlet'ini çalıştırarak devre dışı bırakın:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> Yukarıdaki adımları yalnızca Azure AD Connect yerleşik Zamanlayıcı ile daha yeni sürümleri (1.1.xxx.x) için geçerlidir. Windows Görev Zamanlayıcısı'nı kullanan Azure AD Connect eski sürümleri (1.0.xxx.x) kullanarak veya düzenli aralıklarla eşitleme tetiklemek için kendi özel Zamanlayıcı (ortak değil) kullanıyorsanız, bunları uygun şekilde devre dışı bırakmanız gerekir.

3. Başlat **Eşitleme Hizmeti Yöneticisi'ni** Başlangıç → eşitleme hizmeti giderek.

4. Git **Operations** sekmesinde ve durumu olan işlem yok onaylayın *"sürüyor."*

### <a name="step-2-find-the-existing-outbound-sync-rule-for-usercertificate-attribute"></a>2. Adım Kullanıcı sertifikasını öznitelik için varolan giden eşitleme kuralı bulunamadı
Etkin ve kullanıcı nesneleri için kullanıcı sertifikasını özniteliği için Azure AD dışarı aktarmak için yapılandırılan mevcut bir eşitleme kuralı olması gerekir. Öğrenmek için bu eşitleme kuralı bulun, **öncelik** ve **kapsam filtresi** yapılandırma:

1. Başlat **eşitleme kuralları Düzenleyicisi** Başlangıç → eşitleme kuralları Düzenleyicisi giderek.

2. Arama filtreleri ile aşağıdaki değerleri yapılandırın:

    | Öznitelik | Değer |
    | --- | --- |
    | Yön |**Giden** |
    | MV nesne türü |**Kişi** |
    | Bağlayıcı |*Azure AD Bağlayıcısı adı* |
    | Bağlayıcı nesne türü |**Kullanıcı** |
    | MV özniteliği |**kullanıcı sertifikasını** |

3. Kullanıcı nesnelerinin userCertficiate özniteliği dışarı aktarmak için Azure AD Bağlayıcısı OOB (out-of-box) eşitleme kuralları kullanıyorsanız, bunu geri almanız gerekir *"Çıkışı için AAD – kullanıcı ExchangeOnline"* kuralı.
4. Aşağı Not **öncelik** bu eşitleme kuralı değeri.
5. Eşitleme kuralı seçin ve tıklatın **Düzenle**.
6. İçinde *"Ayrılmış kural onay Düzenle"* açılan iletişim kutusunda, tıklatın **Hayır**. (Endişelenmeyin, biz bu eşitleme kuralı için herhangi bir değişiklik yapmak için yapmayacağınız).
7. Düzen ekranında şunları seçin **Scoping filtre** sekmesi.
8. Kapsam filtresi yapılandırmayı unutmayın. OOB eşitleme kuralı kullanıyorsanız, tam olarak olması gerektiğini **iki yan tümceleri içeren bir kapsam filtresi grubu**dahil:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

### <a name="step-3-create-the-outbound-sync-rule-required"></a>3. Adım Gerekli giden eşitleme kuralı oluşturma
Yeni eşitleme kuralı aynı olmalıdır **kapsam filtresi** ve **daha yüksek önceliği** mevcut eşitleme kuralı daha. Bu, yeni eşitleme kuralı aynı nesne kümesini var olan eşitleme kuralı olarak uygulanır ve mevcut eşitleme kuralı userCertificate özniteliği için geçersiz kılmaları sağlar. Eşitleme kuralı oluşturmak için:
1. Eşitleme kuralları Düzenleyicisi'nde tıklatın **Yeni Kural Ekle** düğmesi.
2. Altında **Açıklama sekmesinin**, aşağıdaki yapılandırmayı sağlar:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *"Out AAD'ye – özel geçersiz kılma için userCertificate"* |
    | Açıklama | *Bir açıklama belirtin* | Örneğin, *"UserCertificate özniteliği 15'ten fazla değerlere sahipse, NULL verin."* |
    | Bağlı sistem | *Azure AD Bağlayıcısı seçin* |
    | Bağlı sistem nesne türü | **Kullanıcı** | |
    | Meta veri deposu nesne türü | **kişi** | |
    | Bağlantı türü | **Birleştir** | |
    | Öncellik | *1-99 arasında bir sayı seçtiniz* | Seçilen sayı varolan herhangi bir eşitleme kural kullanılmamalıdır ve daha düşük bir değere sahip (ve bu nedenle, daha yüksek öncelik) mevcut eşitleme kuralı daha. |

3. Git **Scoping filtre** sekmesinde ve mevcut eşitleme kuralı kullanarak aynı kapsam filtre uygulayabilirsiniz.
4. Atla **katılma kuralları** sekmesi.
5. Git **dönüşümleri** sekmesinde yapılandırmayı kullanarak yeni bir dönüştürme eklemek için:

    | Öznitelik | Değer |
    | --- | --- |
    | Akış türü |**İfade** |
    | Target özniteliği |**kullanıcı sertifikasını** |
    | Kaynak özniteliği |*Aşağıdaki ifade kullanmak*:`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Tıklatın **Ekle** eşitleme kuralı oluşturmak için düğmesi.

### <a name="step-4-verify-the-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>4. Adım. Yeni eşitleme kuralı LargeObject hata ile varolan bir nesnede doğrulayın
Bu diğer nesnelere uygulamadan önce oluşturulan eşitleme kuralı düzgün LargeObject hata sahip mevcut bir AD nesnesi üzerinde çalıştığını doğrulayın.
1. Git **Operations** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Azure AD işlemi için en son verme seçin ve LargeObject hatalarla nesnelerden birini tıklatın.
3.  Bağlayıcı alanı nesne özellikleri açılır ekranında tıklayın **Önizleme** düğmesi.
4. Önizleme açılır ekranında şunları seçin **tam eşitleme** tıklatıp **Commit Önizleme**.
5. Önizleme ekran ve bağlayıcı alanı nesne özellikleri ekran kapatın.
6. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
7. Sağ tıklayın **Azure AD** Bağlayıcısı ve select **Çalıştır...**
8. Çalıştırma bağlayıcı açılır pencerede seçin **dışarı** adım ve tıklatın **Tamam**.
9. Dışarı aktarma tamamlamak ve bu belirli bir nesnede LargeObject hata hakkında daha fazla olduğunu doğrulamak için Azure ad bekleyin.

### <a name="step-5-apply-the-new-sync-rule-to-remaining-objects-with-largeobject-error"></a>5. Adım. LargeObject hatasıyla kalan nesneler için yeni eşitleme kuralı uygula
Eşitleme kuralı eklendiğinde AD Bağlayıcısı üzerinde tam eşitleme adım çalıştırma gerekir:
1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Sağ tıklayın **AD** Bağlayıcısı ve select **Çalıştır...**
3. Bağlayıcı çalıştır açılır pencerede seçin **tam eşitleme** adım ve tıklatın **Tamam**.
4. Tam eşitleme adımının tamamlanması için bekleyin.
5. Birden fazla AD bağlayıcılar varsa kalan AD bağlayıcıları için yukarıdaki adımları yineleyin. Genellikle, birden çok şirket içi dizin varsa, birden çok bağlayıcı gereklidir.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-to-be-exported-to-azure-ad"></a>6. Adım. Azure AD'ye aktarılacak bekleyen beklenmeyen bir değişiklik bulunmamaktadır doğrulayın
1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Sağ **Azure AD** Bağlayıcısı ve select **arama bağlayıcı alanı**.
3. Arama bağlayıcı alanı açılır:
    1. Set Scope **dışa aktarma bekleyen**.
    2. Dahil olmak üzere tüm 3 checkboxes denetleyin **Ekle**, **Değiştir** ve **silmek**.
    3. Tıklatın **arama** Azure AD'ye aktarılacak bekleyen değişikliklerle tüm nesneler döndürmeye düğmesi.
    4. Beklenmeyen bir değişiklik yok doğrulayın. Belirli nesne değişiklikleri incelemek için nesne üzerinde çift tıklatın.

### <a name="step-7-export-the-changes-to-azure-ad"></a>7. Adım. Değişiklikleri için Azure AD dışarı aktarma
Değişiklikleri için Azure AD dışarı aktarmak için:
1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Sağ tıklayın **Azure AD** Bağlayıcısı ve select **Çalıştır...**
4. Çalıştırma bağlayıcı açılır pencerede seçin **dışarı** adım ve tıklatın **Tamam**.
5. Dışarı aktarma tamamlamak ve daha fazla LargeObject hatalar yoktur onaylamak için Azure ad bekleyin.

### <a name="step-8-re-enable-sync-scheduler"></a>8. adım. Eşitleme Zamanlayıcı'yı yeniden etkinleştirin
Sorun çözüldüğünde, yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:
1. PowerShell oturumu başlatın.
2. Zamanlanan eşitleme cmdlet'ini çalıştırarak yeniden etkinleştirin:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> Yukarıdaki adımları yalnızca Azure AD Connect yerleşik Zamanlayıcı ile daha yeni sürümleri (1.1.xxx.x) için geçerlidir. Windows Görev Zamanlayıcısı'nı kullanan Azure AD Connect eski sürümleri (1.0.xxx.x) kullanarak veya düzenli aralıklarla eşitleme tetiklemek için kendi özel Zamanlayıcı (ortak değil) kullanıyorsanız, bunları uygun şekilde devre dışı bırakmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.

