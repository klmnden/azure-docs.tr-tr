---
title: "Azure AD Connect eşitleme: çoklu coğrafi özellikleri için tercih edilen veri konumu Office 365'te yapılandırma | Microsoft Docs"
description: "Office 365 kullanıcı kaynaklarınızı Azure AD Connect eşitleme kullanıcıyla yakın put açıklar."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: billmath
ms.openlocfilehash: 8a36fc45334a2f1d12e6eabbfb16731ccc9998bf
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure AD Connect eşitleme: Office 365 kaynaklar için tercih edilen veri konumu yapılandırın
Bu konunun amacı, Azure AD Connect eşitleme içinde PreferredDataLocation yapılandırma konusunda size yol sağlamaktır. Bir müşteri Office 365'te birden çok coğrafi özellikleri kullandığında, bu öznitelik kullanıcının Office 365 verilerin coğrafi konumunu belirlemek için kullanılır.

> [!IMPORTANT]
> Birden çok coğrafi şu anda önizlemede değil. Önizleme programına katılma istiyorsanız, lütfen Microsoft temsilcinize başvurun.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>PreferredDataLocation eşitlemeyi etkinleştir
Varsayılan olarak, Office 365 kaynakları kullanıcılarınız için Azure AD kiracınıza ile aynı bölgede yer alır. Örneğin, Kuzey Amerika'da Kiracı bulunuyorsa kullanıcıların Exchange posta kutuları da Kuzey Amerika'da bulunur. Birden çok ulusal bir kuruluş için bu en iyi olmayabilir. Öznitelik preferredDataLocation ayarlayarak kullanıcının bölge tanımlanabilir.

Bu öznitelik ayarları tarafından kullanabilirsiniz posta kutusu ve OneDrive gibi kullanıcının Office 365 kaynaklar kullanıcı ile aynı bölgede olması ve tüm kuruluşunuz için bir kiracı çözümlenmedi.

> [!IMPORTANT]
> Birden çok coğrafi için uygun olması için en az 5000 kişilik Office 365 aboneliğinizin olması gerekir
>
>

Office 365 çoklu coğrafi için kullanılabilir bölgeleri şunlardır:

| Bölge | Açıklama |
| --- | --- |
| ADI | Kuzey Amerika |
| EUR | Avrupa |
| APC | Asya Pasifik |
| JPN | Japonya |
| AVUSTRALYA | Avustralya |
| CAN | Kanada |
| GBR | Büyük Britanya |
| LAM | Latin Amerika |

Tüm Office 365 iş yükleri bir kullanıcının bölge ayarı kullanımını destekler.

Azure AD Connect eşitleme destekleyen **PreferredDataLocation** için öznitelik **kullanıcı** sürüm 1.1.524.0 ve sonra nesneleri. Daha açık belirtmek gerekirse, aşağıdaki değişiklikleri sunulmuştur:

* Nesne türü şeması **kullanıcı** Azure AD Bağlayıcısı türü tek değerli dizesidir PreferredDataLocation özniteliği kapsayacak şekilde genişletilir.
* Nesne türü şeması **kişi** meta veri deposunda türü dize olan ve tek değerli PreferredDataLocation özniteliği kapsayacak şekilde genişletilir.

Varsayılan olarak, PreferredDataLocation özniteliği için eşitleme etkin değil. Bu özellik, büyük kuruluşlar için tasarlanmıştır ve herkes ondan yararlı. Ayrıca, şirket içi Active Directory'de PreferredDataLocation özniteliği yok olduğundan, kullanıcılar için Office 365 bölge tutmak için bir öznitelik tanımlamanız gerekir. Bu her kuruluş için farklı olacak.

> [!IMPORTANT]
> Şu anda Azure AD, Azure AD PowerShell kullanarak doğrudan olacak şekilde yapılandırılmış PreferredDataLocation özniteliği eşitlenmiş kullanıcı nesneleri hem bulut kullanıcı nesneleri sağlar. PreferredDataLocation öznitelik eşitlemesi etkinleştirildiğinde, öznitelik yapılandırmak için Azure AD PowerShell kullanarak durdurmalısınız **kullanıcı nesneleri eşitlenen** Azure AD Connect bunları şirket içi Active Directory'de kaynak öznitelik değerleri temel alarak geçersiz kılar.

> [!IMPORTANT]
> 1 Eylül 2017'dan itibaren Azure AD artık PreferredDataLocation öznitelik sağlar. **kullanıcı nesneleri eşitlenen** doğrudan Azure AD PowerShell kullanarak yapılandırılmalıdır. Eşitlenen kullanıcı nesnelerindeki PreferredLocation özniteliği yapılandırmak için Azure AD Connect kullanmanız gerekir.

Eşitleme PreferredDataLocation özniteliğinin etkinleştirmeden önce aşağıdakileri yapmalısınız:

* İlk olarak, kaynak özniteliği olarak kullanılmak üzere hangi şirket içi Active Directory öznitelik karar verin. Türünde olmalı **tek değerli dize**. ExtensionAttributes aşağıdaki adımları kullanılır.
* Daha önce PreferredDataLocation özniteliği üzerinde yapılandırdıysanız Azure AD PowerShell kullanarak Azure AD içinde eşzamanlı kullanıcı nesneleri varolan, şunları yapmalısınız **backport** karşılık gelen kullanıcı nesnelerine şirket içi Active Directory'deki öznitelik değerleri.

    > [!IMPORTANT]
    > Şirket içi Active Directory'de karşılık gelen kullanıcı nesneleri için öznitelik değerlerini değil backport bunu yaparsanız, Azure AD Connect eşitleme PreferredDataLocation özniteliği için etkinleştirildiğinde Azure AD'de mevcut öznitelik değerlerini kaldırın.

* Kaynak özniteliği yapılandırmanız önerilir daha sonra doğrulama için kullanılabilecek artık, şirket içi en az bir birkaç AD kullanıcı nesneleri.

Eşitleme PreferredDataLocation özniteliğinin etkinleştirme adımları olarak özetlenebilir:

1. Eşitleme Zamanlayıcı'yı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
2. Şirket içi EKLER bağlayıcı şemaya kaynak öznitelik Ekle
3. Azure AD Bağlayıcısı şemaya PreferredDataLocation Ekle
4. Şirket içi Active Directory'den öznitelik değeri akışı için bir gelen eşitleme kuralı oluşturma
5. Azure AD öznitelik değerini akışı için bir giden eşitleme kuralı oluşturma
6. Tam eşitleme döngüsü çalıştırın
7. Eşitleme Zamanlayıcı etkinleştir
8. Sonucu doğrulayın

> [!NOTE]
> Bu bölümde rest Ayrıntıları'nda aşağıdaki adımları kapsar. Özel eşitleme kuralları olmadan tek orman topolojisi ile bir Azure AD dağıtımı bağlamında açıklanmıştır. Çoklu orman topolojisini varsa, özel eşitleme kuralları yapılandırılmış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız gerekir.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
Azure AD dışarı aktarılan istenmeyen değişiklikleri önlemek için eşitleme kuralları güncelleştiriliyor ortasında durumdayken eşitleme gerçekleşir emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

1. Bir PowerShell oturumunda Azure AD Connect bir sunucuda başlatın.
2. Cmdlet'ini çalıştırarak zamanlanmış eşitleme devre dışı bırak: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Başlat **Eşitleme Hizmeti Yöneticisi'ni** giderek **Başlat** > **eşitleme hizmeti**.
4. Git **Operations** sekmesinde ve işlem durumundaki yok onaylayın *devam eden*.

![Eşitleme Hizmeti Yöneticisi - devam eden hiçbir işlemleri denetleyin](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>2. adım: kaynak özniteliği şirket içi EKLER bağlayıcı şemaya ekleyin.
Tüm AD öznitelikleri şirket alınır AD bağlayıcı alanı. Varsayılan olarak eşitlenmez bir öznitelik kullanmayı seçtiyseniz, bu içeri aktarmanız gerekir. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Sağ **şirket içi AD Bağlayıcısı** seçip **özellikleri**.
3. Açılan iletişim kutusunda, Git **öznitelikleri Seç** sekmesi.
4. Kullanmak için seçtiğiniz kaynak özniteliği öznitelik listesinde işaretli olduğundan emin olun. Özniteliğinizi görmüyorsanız, ardından "Tümünü Göster" onay kutusuna tıklayın.
5. Tıklatın **Tamam** kaydetmek için.

![Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şeması](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>3. adım: Azure AD Bağlayıcısı şemaya PreferredDataLocation ekleme
Varsayılan olarak, Azure AD bağlayıcı alanına PreferredDataLocation özniteliği alınmaz. İçeri aktarılan öznitelikleri listesi PreferredDataLocation özniteliği eklemek için:

1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Sağ **Azure AD Bağlayıcısı** seçip **özellikleri**.
3. Açılan iletişim kutusunda, Git **öznitelikleri Seç** sekmesi.
4. Öznitelik listesi preferredDataLocation özniteliği seçin.
5. Tıklatın **Tamam** kaydetmek için.

![Azure AD Bağlayıcısı şemaya kaynak öznitelik Ekle](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>4. adım: şirket içi Active Directory'den öznitelik değeri akışı için bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı öznitelik değerini meta veri deposu için şirket içi Active Directory'den kaynak özniteliğinden akış verir:

1. Başlat **eşitleme kuralları Düzenleyicisi** giderek **Başlat** > **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.
3. Tıklatın **Yeni Kural Ekle** düğmesi yeni gelen kuralı oluşturun.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *"içinde AD'den – kullanıcı PreferredDataLocation"* |
    | Açıklama | *Özel bir açıklama belirtin* |  |
    | Bağlı sistem | *Şirket içi çekme AD Bağlayıcısı* |  |
    | Bağlı sistem nesne türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Person** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Tutmak **Scoping filtre** tüm nesneleri dahil edecek boş. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.
6. Git **dönüştürme sekmesi** ve aşağıdaki dönüştürme kuralı uygular:

    | Akış türü | Hedef Öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    |Doğrudan | PreferredDataLocation | Kaynak özniteliği seçin | İşaretli | Güncelleştirme |

7. Tıklatın **Ekle** gelen kuralı oluşturmak için.

![Gelen eşitleme kuralı oluşturma](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>5. adım: Azure AD öznitelik değerini akışı için bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı öznitelik değerini meta veri deposu Azure AD'de PreferredDataLocation öznitelik akış verir:

1. Git **eşitleme kuralları** Düzenleyici.
2. Arama filtresi ayarlamak **yönü** olmasını **giden**.
3. Tıklatın **Yeni Kural Ekle** düğmesi.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "çıkışı için AAD – kullanıcı PreferredDataLocation" |
    | Açıklama | *Bir açıklama belirtin* ||
    | Bağlı sistem | *AAD bağlayıcı seçin* ||
    | Bağlı sistem nesne türü | Kullanıcı ||
    | Meta veri deposu nesne türü | **Person** ||
    | Bağlantı türü | **Birleştir** ||
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesinde ve ekleme bir **iki maddeleri tek kapsam filtresi grubu**:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam Filtresi hangi Azure AD bu giden eşitleme kuralının uygulandığı nesneleri belirler. Bu örnekte, "Dışı" AD – kullanıcı kimliği için aynı kapsam filtresinden kullanırız OOB eşitleme kuralı. Eşitleme kuralı şirket içi Active Directory'den eşitlenmez kullanıcı nesnelerine uygulanan engeller. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.

6. Git **dönüştürme** sekmesinde ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Hedef Öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | PreferredDataLocation | PreferredDataLocation | İşaretli | Güncelleştirme |

7. Kapat **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı oluştur](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>6. adım: Çalıştır tam eşitleme döngüsü
AD için yeni öznitelikler ekledik genel olarak, tam eşitleme döngüsü gerekli olduğu ve Azure AD Bağlayıcısı şema ve sunulan özel eşitleme kuralları. Azure AD dışarı aktarmadan önce değişiklikleri doğrulamak önerilir. Bir tam eşitleme döngüsü yapmak adımları el ile çalışırken değişiklikleri doğrulamak için aşağıdaki adımları kullanın.

1. Çalıştırma **tam alma** üzerinde Adım **şirket içi AD Bağlayıcısı**:

   1. Git **Operations** sekme Eşitleme Hizmeti Yöneticisi'nde.
   2. Sağ **şirket içi AD Bağlayıcısı** seçip **Çalıştır...** .
   3. Açılan iletişim kutusunda seçin **tam içeri aktarma** tıklatıp **Tamam**.
   4. İşlemin tamamlanmasını bekleyin.

    > [!NOTE]
    > Tam içeri aktarma atlayabilirsiniz şirket içi AD kaynak özniteliği zaten listesinde yer alıyorsa Bağlayıcısı öznitelikleri alındı. Diğer bir deyişle, sırasında herhangi bir değişiklik yapmak zorunda kalmadığımıza [2. adım: şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema).

2. Çalıştırma **tam alma** üzerinde Adım **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **Çalıştır...**
   2. Açılan iletişim kutusunda seçin **tam içeri aktarma** tıklatıp **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

3. Eşitleme kuralı değişiklikleri var olan bir kullanıcı nesnesi üzerinde doğrulayın:

Kaynak özniteliği Active Directory ve Azure AD'den PreferredDataLocation ilgili bağlayıcı alanına içeri aktarıldığını şirket içi. Tam eşitleme adımla devam etmeden önce bunu yapmanız önerilir bir **Önizleme** üzerinde var olan bir kullanıcı nesnesi şirket içi AD bağlayıcı alanı. Seçtiğiniz nesne doldurulmuş kaynak özniteliği olmalıdır. Başarılı bir **Önizleme** eşitleme yapılandırdığınız iyi bir gösterge kuralları doğru meta veri deposunda doldurulmuş PreferredDataLocation olduğu. Nasıl yapılacağı hakkında bilgi için bir **Önizleme**, bölümüne bakın [değişikliği doğrulayın](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Çalıştırma **tam eşitleme** üzerinde Adım **şirket içi AD Bağlayıcısı**:

   1. Sağ **şirket içi AD Bağlayıcısı** seçip **Çalıştır...** .
   2. Açılan iletişim kutusunda seçin **tam eşitleme** tıklatıp **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

5. Doğrulama **bekleyen dışarı aktarmaların** Azure ad:

   1. Sağ **Azure AD Bağlayıcısı** seçip **arama bağlayıcı alanı**.
   2. Bağlayıcı alanı arama açılan iletişim kutusunda:

      1. Ayarlama **kapsam** için **dışa aktarma bekleyen**.
      2. Dahil olmak üzere, tüm üç checkboxes denetleyin **ekleme, değiştirme ve silme**.
      3. Tıklatın **arama** dışarı değişikliklerle nesneleri listesini almak için düğmesi. Belirli nesne değişiklikleri incelemek için nesne çift tıklayın.
      4. Değişiklikleri beklenen doğrulayın.

6. Çalıştırma **verme** üzerinde Adım **Azure AD Bağlayıcısı**

   1. Sağ **Azure AD Bağlayıcısı** seçip **Çalıştır...** .
   2. Bağlayıcı çalıştırmak açılan iletişim kutusunda, seçin **verme** tıklatıp **Tamam**.
   3. Tamamlamak için Azure ad dışarı aktarma bekleyin.

> [!NOTE]
> Adımları Azure AD Bağlayıcısı üzerinde tam eşitleme adım ve dışarı aktarma AD Bağlayıcısı üzerinde içermez fark edebilirsiniz. Adımları öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akmaktadır gerekli değildir.

## <a name="step-7-re-enable-sync-scheduler"></a>7. adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. PowerShell oturumu başlatın.
2. Zamanlanan eşitleme cmdlet'ini çalıştırarak yeniden etkinleştirin:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>8. adım: sonucu doğrulayın
Şimdi yapılandırmasını doğrulayın ve kullanıcılarınız için etkinleştirme zamanı gelmiş demektir.

1. Bir kullanıcı seçili öznitelik için bölge ekleyin. Kullanılabilir bölgelerin listesini bulunabilir [Bu tablo](#enable-synchronization-of-preferreddatalocation).  
![Bir kullanıcı için eklenen AD özniteliği](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Azure AD ile eşitlenecek öznitelik bekleyin.
3. Exchange Online PowerShell kullanarak, posta kutusu bölge doğru ayarlandığını doğrulayın.  
![Posta kutusu bölge bir kullanıcı Exchange Online ayarlayın.](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Kiracı bu özelliği kullanabilmek için işaretlenmiş varsayıldığında, posta kutusu doğru bölgesine taşınır. Bu, posta kutusu bulunduğu sunucu adına bakarak doğrulanabilir.
4. Bu ayar çok sayıda posta kutusunu etkin olduğunu doğrulamak için komut dosyasındaki kullanın [Technet Galerisi](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Bu komut dosyası ayrıca tüm Office 365 veri merkezleri sunucu önekleri ve hangi listesini içeren bölge içinde bulunur. Bu önceki adımda bir başvuru olarak posta kutusunun konumunu doğrulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

**Office 365'te birden çok coğrafi hakkında daha fazla bilgi edinin:**

* Birden çok coğrafi oturumların Ignite: https://aka.ms/MultiGeoIgnite
* Birden çok coğrafi onedrive'da: https://aka.ms/OneDriveMultiGeo
* SharePoint Online çoklu coğrafi: https://aka.ms/SharePointMultiGeo

**Eşitleme altyapısı yapılandırma modelleri hakkında daha fazla bilgi edinin:**

* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
