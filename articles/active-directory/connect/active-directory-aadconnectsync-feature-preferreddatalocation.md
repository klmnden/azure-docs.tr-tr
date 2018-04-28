---
title: "Azure Active Directory Connect eşitleme: çoklu coğrafi özellikleri için tercih edilen veri konumu Office 365'te yapılandırma | Microsoft Docs"
description: Office 365 kullanıcı kaynaklarınızı Azure Active Directory Connect eşitleme kullanıcıyla yakın put açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: billmath
ms.openlocfilehash: 0020ed42baaa32fbc5ae2d62b37558e491842d67
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure Active Directory Connect eşitleme: Office 365 kaynaklar için tercih edilen veri konumu yapılandırın
Bu konunun amacı, Azure Active Directory (Azure AD) Connect eşitleme öznitelik tercih edilen veri konumu için yapılandırma konusunda size yol sağlamaktır. Birisi çok coğrafi özellikleri Office 365'te kullandığında, kullanıcının Office 365 verilerin coğrafi konumunu belirtmek için bu öznitelik kullanın. (Koşulları *bölge* ve *coğrafi* birbirlerinin yerine kullanılır.)

## <a name="enable-synchronization-of-preferred-data-location"></a>Tercih edilen veri konumu eşitlemeyi etkinleştir
Varsayılan olarak, Office 365 kaynakları kullanıcılarınız için Azure AD kiracınıza aynı coğrafi bölgede bulunur. Örneğin, Kuzey Amerika'da Kiracı yer alıyorsa, kullanıcıların Exchange posta kutularına de Kuzey Amerika'da bulunur. Çokuluslu bir kuruluş için bu en iyi olmayabilir.

Öznitelik ayarlayarak **preferredDataLocation**, bir kullanıcının coğrafi tanımlayabilirsiniz. Kullanıcının Office 365 kaynaklarında, posta kutusu ve OneDrive gibi aynı coğrafi bölgede kullanıcı varsa ve kuruluşunuz için bir kiracı devam ediyor.

> [!IMPORTANT]
> Birden çok coğrafi 5.000 Office 365 Hizmetleri abonelikleri en az müşterilerle şu anda kullanılabilir. Ayrıntılar için Microsoft temsilcinize konuşun.
>
>

Office 365 için tüm geos listesini bulunabilir [bulunan, verilerinizin nerede olduğuna?](https://aka.ms/datamaps).

Office 365 çoklu coğrafi için kullanılabilir bölgelerde şunlardır:

| Coğrafi | preferredDataLocation değeri |
| --- | --- |
| Asya Pasifik | APC |
| Avustralya | AVUSTRALYA |
| Kanada | CAN |
| Avrupa Birliği | EUR |
| Hindistan | UL |
| Japonya | JPN |
| Kore | KOR |
| Birleşik Krallık | GBR |
| Amerika Birleşik Devletleri | ADI |

* Ardından bir coğrafi bu tabloda (örneğin, Güney Amerika) listelenmiyorsa, çoklu coğrafi için kullanılamaz.
* Hindistan coğrafi yalnızca fatura adresini ve bu coğrafi bölgede satın alınan lisans sahip müşteriler için kullanılabilir.
* Tüm Office 365 iş yükleri bir kullanıcının coğrafi ayarı kullanımını destekler.

### <a name="azure-ad-connect-support-for-synchronization"></a>Eşitleme için Azure AD Connect desteği

Azure AD Connect eşitleme destekleyen **preferredDataLocation** için öznitelik **kullanıcı** sürüm 1.1.524.0 nesneleri ve daha sonra. Bu avantajlar şunlardır:

* Nesne türü şeması **kullanıcı** Azure AD bağlayıcısında içerecek şekilde Genişletilmiş **preferredDataLocation** özniteliği. Öznitelik, tek değerli dize türünde değil.
* Nesne türü şeması **kişi** meta veri deposunda içerecek şekilde Genişletilmiş **preferredDataLocation** özniteliği. Öznitelik, tek değerli dize türünde değil.

Varsayılan olarak, **preferredDataLocation** eşitleme için etkin değil. Bu özellik, büyük kuruluşlar için tasarlanmıştır. Olduğundan, kullanıcılarınızın Office 365 coğrafi tutmak için bir öznitelik de tanımlamalıdır hiçbir **preferredDataLocation** şirket içi Active Directory'de öznitelik. Bu her kuruluş için farklı olacak.

> [!IMPORTANT]
> Azure AD verir **preferredDataLocation** özniteliği **kullanıcı nesneleri bulut** doğrudan Azure AD PowerShell kullanarak yapılandırılmalıdır. Azure AD artık verir **preferredDataLocation** özniteliği **kullanıcı nesneleri eşitlenen** doğrudan Azure AD PowerShell kullanarak yapılandırılacak. Bu öznitelik yapılandırmak için **kullanıcı nesneleri eşitlenen**, Azure AD Connect kullanmanız gerekir.

Eşitleme etkinleştirmeden önce:

* Kaynak özniteliği olarak kullanılmak üzere hangi şirket içi Active Directory öznitelik karar verin. Türünde olmalı **tek değerli dize**. Aşağıdakilerden birini izleyin adımlarda **extensionAttributes** kullanılır.
* Daha önce yapılandırdıysanız **preferredDataLocation** varolan üzerinde özniteliği **kullanıcı nesneleri eşitlenen** Azure AD PowerShell kullanarak Azure AD içinde öznitelik değerleri için backport gerekir karşılık gelen **kullanıcı** şirket içi Active Directory içindeki nesneleri.

    > [!IMPORTANT]
    > Bu değerleri değil backport bunu yaparsanız, Azure AD Connect varolan öznitelik değerleri Azure AD'de kaldırır. zaman eşitleme için **preferredDataLocation** özniteliği etkindir.

* Kaynak özniteliği, en az birkaç şirket içi Active Directory kullanıcı nesneleri üzerinde şimdi yapılandırın. Bunu daha sonra doğrulama için kullanabilirsiniz.

Aşağıdaki bölümlerde biri eşitlenmesine olanak tanımak için adımlar sağlar **preferredDataLocation** özniteliği.

> [!NOTE]
> Bir Azure AD dağıtımı tek orman topolojisi ile ve özel eşitleme kuralları olmadan bağlamında adımları açıklanmaktadır. Çoklu orman topolojisini varsa, özel eşitleme kuralları yapılandırılmış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
Azure AD dışarı aktarılan istenmeyen değişiklikleri önlemek için eşitleme kuralları güncelleştiriliyor ortasında durumdayken eşitleme yer alan emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

1. Bir PowerShell oturumunda Azure AD Connect bir sunucuda başlatın.
2. Bu cmdlet'i çalıştırarak zamanlanmış eşitleme devre dışı bırak: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Başlat **Eşitleme Hizmeti Yöneticisi'ni** giderek **Başlat** > **eşitleme hizmeti**.
4. Seçin **Operations** sekmesini tıklatın ve işlem durumundaki yok onaylayın *devam eden*.

![Eşitleme Hizmeti Yöneticisi'nin ekran görüntüsü](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>2. adım: kaynak özniteliği şirket içi Active Directory Bağlayıcısı şemasına ekleme
Tüm Azure AD öznitelikleri şirket içi Active Directory Bağlayıcısı alanına alınır. Varsayılan olarak eşitlenmemiş bir öznitelik kullanmayı seçtiyseniz, bu içeri aktarmanız gerekir. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

1. Seçin **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Şirket içi Active Directory Bağlayıcısı sağ tıklatın ve seçin **özellikleri**.
3. Açılan iletişim kutusunda Git **öznitelikleri Seç** sekmesi.
4. Kullanmak için seçtiğiniz kaynak özniteliği öznitelik listesinde işaretli olduğundan emin olun. Özniteliğinizi görmüyorsanız seçin **Tümünü Göster** onay kutusu.
5. Kaydetmek için seçin **Tamam**.

![Eşitleme Hizmeti Yöneticisi ekran ve özellikleri iletişim kutusu](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>3. adım: Ekleme **preferredDataLocation** Azure AD Bağlayıcısı şemaya
Varsayılan olarak, **preferredDataLocation** özniteliği, Azure AD bağlayıcı alanına değil alınır. İçeri aktarılan öznitelikleri listesine eklemek için:

1. Seçin **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
2. Azure AD Bağlayıcısı'nı sağ tıklatın ve seçin **özellikleri**.
3. Açılan iletişim kutusunda Git **öznitelikleri Seç** sekmesi.
4. Seçin **preferredDataLocation** listesindeki özniteliği.
5. Kaydetmek için seçin **Tamam**.

![Eşitleme Hizmeti Yöneticisi ekran ve özellikleri iletişim kutusu](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule"></a>4. adım: bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralını şirket içi Active Directory'de kaynak özniteliğinden için meta veri akışı için öznitelik değeri verir.

1. Başlat **eşitleme kuralları Düzenleyicisi** giderek **Başlat** > **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.
3. Yeni bir gelen kuralı oluşturmak için seçin **Yeni Kural Ekle**.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "içinde AD'den – kullanıcı preferredDataLocation" |
    | Açıklama | *Özel bir açıklama belirtin* |  |
    | Bağlı sistem | *Şirket içi Active Directory bağlayıcısını seçin* |  |
    | Bağlı sistem nesne türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Kişi** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Tutmak **Scoping filtre** tüm nesneleri dahil edecek boş. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.
6. Git **dönüştürme sekmesi**ve aşağıdaki dönüştürme kuralı uygular:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    |Doğrudan | PreferredDataLocation | Kaynak özniteliği seçin | İşaretli | Güncelleştirme |

7. Gelen kuralı oluşturmak için seçin **Ekle**.

![Gelen eşitleme kuralı, ekran oluşturma](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule"></a>5. adım: bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı öznitelik değerini için aramasındaki akış verir **preferredDataLocation** Azure AD özniteliği:

1. Git **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **giden**.
3. Seçin **Yeni Kural Ekle**.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "Out Azure AD – kullanıcı preferredDataLocation" |
    | Açıklama | *Bir açıklama belirtin* ||
    | Bağlı sistem | *Azure AD Bağlayıcısı seçin* ||
    | Bağlı sistem nesne türü | **Kullanıcı** ||
    | Meta veri deposu nesne türü | **Kişi** ||
    | Bağlantı türü | **Birleştir** ||
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesinde ve iki yan tümceleri içeren tek bir kapsam filtresi gruba ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam Filtresi hangi Azure AD bu giden eşitleme kuralının uygulandığı nesneleri belirler. Bu örnekte, "Dışı" AD – kullanıcı kimliği için aynı kapsam filtresinden kullanırız OOB (out-of-box) eşitleme kuralı. İçin uygulanan eşitleme kuralı engeller **kullanıcı** şirket içi Active Directory'den eşitlenmez nesneleri. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.

6. Git **dönüştürme** sekmesini tıklatın ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | PreferredDataLocation | PreferredDataLocation | İşaretli | Güncelleştirme |

7. Kapat **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı, ekran görüntüsü oluştur](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>6. adım: Tam eşitleme döngüsü çalıştırın
Genel olarak, tam eşitleme döngüsü gereklidir. Active Directory ve Azure AD Bağlayıcısı şema için yeni öznitelikler eklenir ve özel eşitleme kuralları sunulan olmasıdır. Azure AD dışarı aktarmadan önce bu değişiklikleri doğrularsınız. Bir tam eşitleme döngüsü yapmak adımları el ile çalışırken değişiklikleri doğrulamak için aşağıdaki adımları kullanın.

1. Çalıştırma **tam alma** şirket içi Active Directory Bağlayıcısı üzerinde:

   1. Git **Operations** sekme Eşitleme Hizmeti Yöneticisi'nde.
   2. Sağ **şirket içi Active Directory Bağlayıcısı**seçip **çalıştırmak**.
   3. İletişim kutusunda **tam içeri aktarma**seçip **Tamam**.
   4. İşlemin tamamlanmasını bekleyin.

    > [!NOTE]
    > Kaynak özniteliği zaten içe aktarılmış öznitelikleri listesinde yer alıyorsa, şirket içi Active Directory Bağlayıcısı üzerinde tam içeri aktarma atlayabilirsiniz. Diğer bir deyişle, bu makalenin 2 adımı sırasında herhangi bir değişiklik yapmak yoktu.

2. Çalıştırma **tam alma** Azure AD Bağlayıcısı üzerinde:

   1. Sağ **Azure AD Bağlayıcısı**seçip **çalıştırmak**.
   2. İletişim kutusunda **tam içeri aktarma**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

3. Var olan üzerinde eşitleme kuralı değişiklikleri doğrulamak **kullanıcı** nesnesi.

   Şirket içi Active Directory'den kaynak özniteliği ve **preferredDataLocation** Azure AD'den her ilgili bağlayıcı alanına alındı. Tam eşitleme adımla devam etmeden önce var olan üzerinde önizlemesini yapın **kullanıcı** şirket içi Active Directory Bağlayıcısı alan nesne. Seçtiğiniz nesne doldurulmuş kaynak özniteliği olmalıdır. Başarılı bir önizleme ile **preferredDataLocation** meta veri deposunda eşitleme yapılandırdığınız iyi bir gösterge kuralları doğru şekilde doldurulur. Önizleme yapma hakkında daha fazla bilgi için bkz: [değişikliği doğrulayın](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Çalıştırma **tam eşitleme** şirket içi Active Directory Bağlayıcısı üzerinde:

   1. Sağ **şirket içi Active Directory Bağlayıcısı**seçip **çalıştırmak**.
   2. İletişim kutusunda **tam eşitleme**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

5. Doğrulama **bekleyen dışarı aktarmaların** Azure ad:

   1. Sağ **Azure AD Bağlayıcısı**seçip **arama bağlayıcı alanı**.
   2. İçinde **arama bağlayıcı alanı** iletişim kutusunda:

        a. Ayarlama **kapsam** için **dışa aktarma bekleyen**.<br>
        b. Tüm üç onay dahil olmak üzere kutularını seçin **ekleme, değiştirme ve silme**.<br>
        c. Nesneleri dışarı değişikliklerle listesini görüntülemek için seçin **arama**. Belirli nesne değişiklikleri incelemek için nesne çift tıklayın.<br>
        d. Değişiklikleri beklenir doğrulayın.

6. Çalıştırma **verme** üzerinde **Azure AD Bağlayıcısı**

   1. Sağ **Azure AD Bağlayıcısı**seçip **çalıştırmak**.
   2. İçinde **çalıştırmak bağlayıcı** iletişim kutusunda **verme**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

> [!NOTE]
> Adımları Azure AD Bağlayıcısı üzerinde tam eşitleme adım veya Active Directory Bağlayıcısı verme adımla eklemeyin fark edebilirsiniz. Öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akmaktadır çünkü adımlar gerekli değildir.

## <a name="step-7-re-enable-sync-scheduler"></a>7. adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. Bir PowerShell oturumu başlatın.
2. Zamanlanan eşitleme Bu cmdlet'i çalıştırarak yeniden etkinleştirin: `Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>8. adım: sonucu doğrulayın
Şimdi yapılandırmasını doğrulayın ve kullanıcılarınız için etkinleştirme zamanı gelmiş demektir.

1. Bir kullanıcı seçili öznitelik için coğrafi ekleyin. Kullanılabilir geos listesi bulunabilir [Bu tablo](#enable-synchronization-of-preferreddatalocation).  
![Bir kullanıcı için eklenen AD özniteliğinin ekran görüntüsü](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Azure AD ile eşitlenecek öznitelik bekleyin.
3. Exchange Online PowerShell kullanarak, posta kutusu bölge doğru ayarlandığını doğrulayın.  
![Exchange Online PowerShell ekran görüntüsü](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Kiracı bu özelliği kullanabilmek için işaretlenmiş varsayıldığında, posta kutusu doğru coğrafi taşınır. Bu, posta kutusu bulunduğu sunucu adına bakarak doğrulanabilir.
4. Bu ayar çok sayıda posta kutusunu etkin olduğunu doğrulamak için komut dosyasındaki kullanın [TechNet Galerisi](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Bu komut dosyası ayrıca tüm Office 365 veri merkezleri sunucu önekleri listesini içeren ve hangi coğrafi bulunduğu içinde. Bu önceki adımda bir başvuru olarak posta kutusunun konumunu doğrulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Office 365'te birden çok coğrafi hakkında daha fazla bilgi edinin:

* [Birden çok coğrafi oturumların Ignite](https://aka.ms/MultiGeoIgnite)
* [Birden çok coğrafi onedrive'da](https://aka.ms/OneDriveMultiGeo)
* [SharePoint Online çoklu coğrafi](https://aka.ms/SharePointMultiGeo)

Eşitleme altyapısı yapılandırma modelleri hakkında daha fazla bilgi edinin:

* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

Genel bakış konuları:

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
