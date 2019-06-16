---
title: 'Azure AD Connect: Office 365 kaynaklar için tercih edilen veri konumu yapılandırın'
description: Office 365 kullanıcı kaynaklarınızı Azure Active Directory Connect eşitlemesi ile kullanıcı yakın put işlemini açıklamaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 927987237b51a47d0c8b7c66054842b0a7ff09a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473032"
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure Active Directory Connect eşitleme: Office 365 kaynaklar için tercih edilen veri konumu yapılandırın
Bu konunun amacı, öznitelik tercih edilen veri konumu için Azure Active Directory (Azure AD) Connect eşitleme yapılandırma konusunda rehberlik sağlamaktır. Birisi çoklu coğrafi özellikleri Office 365'te kullandığında, kullanıcının Office 365 verilerine coğrafi konumunu belirlemek için bu öznitelik kullanın. (Koşulları *bölge* ve *coğrafi* birbirinin yerine kullanılır.)

## <a name="enable-synchronization-of-preferred-data-location"></a>Tercih edilen veri konumu, eşitlemeyi etkinleştirme
Varsayılan olarak, Office 365 kaynaklarına kullanıcılarınız için Azure AD kiracınız ile aynı coğrafi bölgede yer alır. Örneğin, kiracınız Kuzey Amerika'da yer alıyorsa kullanıcıların Exchange posta kutuları da Kuzey Amerika'da bulunur. Çok uluslu bir kuruluş için bu en uygun olmayabilir.

Öznitelik ayarlayarak **preferredDataLocation**, kullanıcının coğrafi tanımlayabilirsiniz. Kullanıcının Office 365 kaynaklarına, posta kutusu ve OneDrive gibi bir kullanıcı, aynı coğrafi bölgede olması ve bir kiracı, kuruluşunuzun tamamı için çözümlenmedi.

> [!IMPORTANT]
> Çoklu coğrafi şu anda en düşük 2.500 Office 365 hizmetlerine abonelikleri olan müşteriler için kullanılabilir. Ayrıntılar için Microsoft temsilcinize konuşun.
>
>

Office 365 için tüm coğrafi bölgeler listesi bulunabilir [bulunan verileriniz nerede?](https://aka.ms/datamaps).

Office 365 için çoklu coğrafi kullanılabilir bölgelerde şunlardır:

| Coğrafi | preferredDataLocation değeri |
| --- | --- |
| Asya Pasifik | APC |
| Avustralya | AU |
| Kanada | OLABİLİR |
| Avrupa Birliği | EUR |
| Fransa | FRA |
| Hindistan | UL |
| Japonya | JPN |
| Güney Kore | KOR |
| Birleşik Krallık | GBR |
| Amerika Birleşik Devletleri | ADI |

* Ardından bu tablodaki (örneğin, Güney Amerika) bir coğrafi listede yoksa, çoklu coğrafi için kullanılamaz.

* Tüm Office 365 iş yükleri, kullanıcının coğrafi ayarlamanın kullanımını destekler.

### <a name="azure-ad-connect-support-for-synchronization"></a>Eşitleme için Azure AD Connect desteği

Azure AD Connect eşitleme destekler **preferredDataLocation** özniteliğini **kullanıcı** nesneleri sürüm 1.1.524.0 ve üzeri. Bu avantajlar şunlardır:

* Nesne türünün şeması **kullanıcı** eklemek için Azure AD bağlayıcısında Genişletilmiş **preferredDataLocation** özniteliği. Tek değerli dize türünde özniteliğidir.
* Nesne türünün şeması **kişi** meta veri deposunda içerecek şekilde Genişletilmiş **preferredDataLocation** özniteliği. Tek değerli dize türünde özniteliğidir.

Varsayılan olarak, **preferredDataLocation** eşitleme için etkin değil. Bu özellik, büyük kuruluşlar için tasarlanmıştır. Olduğundan, kullanıcılarınız için Office 365 coğrafi tutmak için bir özniteliği de belirlemeniz gerekir hiçbir **preferredDataLocation** şirket içi Active Directory özniteliği. Bu, her kuruluş için farklı olacak şekilde geçiyor.

> [!IMPORTANT]
> Azure AD'ye verir **preferredDataLocation** özniteliği **kullanıcı, nesneyi bulut** doğrudan Azure AD PowerShell kullanarak yapılandırılmış olması. Artık Azure AD'ye verir **preferredDataLocation** özniteliği **eşitlenmiş kullanıcı, nesneyi** doğrudan Azure AD PowerShell kullanarak yapılandırılmış olması. Bu öznitelik yapılandırma **eşitlenmiş kullanıcı, nesneyi**, Azure AD Connect kullanmanız gerekir.

Eşitleme etkinleştirmeden önce:

* Kaynak özniteliği olarak kullanılmak üzere hangi şirket içi Active Directory öznitelik karar verin. Türünde olmalıdır **tek değerli dize**. İçinde biri, aşağıdaki adımları **extensionAttributes** kullanılır.
* Daha önce yapılandırdıysanız **preferredDataLocation** mevcut öznitelik **eşitlenmiş kullanıcı, nesneyi** Azure AD PowerShell kullanarak Azure AD'de öznitelik değerleri için backport gerekir karşılık gelen **kullanıcı** şirket içi Active Directory içindeki nesneleri.

    > [!IMPORTANT]
    > Bu değerlerin değil backport bunu yaparsanız, Azure AD Connect Azure AD'de mevcut öznitelik değerleri kaldırır. zaman eşitleme için **preferredDataLocation** özniteliği etkindir.

* Kaynak özniteliği, en az birkaç şirket içi Active Directory kullanıcı nesneleri üzerinde şimdi yapılandırın. Daha sonra doğrulama için bunu kullanabilirsiniz.

Aşağıdaki bölümlerde, eşitlemesini adımları **preferredDataLocation** özniteliği.

> [!NOTE]
> Bir Azure AD dağıtım olmadan özel eşitleme kuralları ve tek ormanlı bir topolojiye bağlamında adımları açıklanmaktadır. Çok ormanlı topolojisi varsa, özel bir eşitleme kuralları yapılandırılmış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1\. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden eşitleme olduğunu doğrulayın
Azure AD'ye dışarı aktarılan istenmeyen değişiklikleri önlemek için eşitleme kuralları güncelleniyor ortasında durumdayken eşitleme yer aldığını emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

1. Azure AD Connect sunucusunda bir PowerShell oturumu başlatın.
2. Bu cmdlet'i çalıştırarak zamanlanmış eşitleme devre dışı bırak: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Başlangıç **Eşitleme Hizmeti Yöneticisi** giderek **Başlat** > **eşitleme hizmeti**.
4. Seçin **işlemleri** sekmesini tıklatıp durumundaki işlemi yok onaylayın *sürüyor*.

![Eşitleme Hizmeti Yöneticisi'nin ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>2\. adım: Kaynak özniteliği, şirket içi Active Directory Bağlayıcısı şemasına ekleyin
Tüm Azure AD öznitelikleri, şirket içi Active Directory Bağlayıcısı alanına içeri aktarılır. Varsayılan olarak eşitlenmemiş ise bir özniteliği kullanacak şekilde seçtiyseniz, içeri aktarılması gerekir. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

1. Seçin **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Şirket içi Active Directory Bağlayıcısı'nı sağ tıklatıp seçin **özellikleri**.
3. Açılan iletişim kutusuna gidin **öznitelikleri Seç** sekmesi.
4. Kullanmayı seçtiğiniz kaynak özniteliğinde öznitelik listesinde seçeneğinin işaretli olduğundan emin olun. Özniteliğinizi görmüyorsanız seçin **Tümünü Göster** onay kutusu.
5. Seçin kaydetmek için **Tamam**.

![Eşitleme Hizmeti Yöneticisi ekran görüntüsü ve özellikleri iletişim kutusu](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>3\. adım: Ekleme **preferredDataLocation** Azure AD Bağlayıcısı şema
Varsayılan olarak, **preferredDataLocation** özniteliği Azure AD bağlayıcı alanına alınmadı. İçeri aktarılan öznitelikleri listesine eklemek için:

1. Seçin **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
2. Azure AD Bağlayıcısı'nı sağ tıklatın ve seçin **özellikleri**.
3. Açılan iletişim kutusuna gidin **öznitelikleri Seç** sekmesi.
4. Seçin **preferredDataLocation** listesindeki özniteliği.
5. Seçin kaydetmek için **Tamam**.

![Eşitleme Hizmeti Yöneticisi ekran görüntüsü ve özellikleri iletişim kutusu](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule"></a>4\. Adım: Bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralını şirket içi Active Directory'de kaynak özniteliğinden için meta veri akışı için öznitelik değeri verir.

1. Başlangıç **eşitleme kuralları Düzenleyicisi** giderek **Başlat** > **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.
3. Yeni bir gelen kuralı oluşturmak için Seç **Yeni Kural Ekle**.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "içinde ad – kullanıcı preferredDataLocation" |
    | Açıklama | *Özel bir açıklama sağlayın* |  |
    | Bağlı sistem | *Şirket içi Active Directory bağlayıcısını seçin* |  |
    | Bağlı sistem nesnesi türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Kişi** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Tutun **Scoping filtre** tüm nesneleri eklemek için boş. Azure AD Connect dağıtımınıza göre kapsam belirleme filtresi ince yapmanız gerekebilir.
6. Git **dönüştürme sekmesini**ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Hedef öznitelik | source | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    |Doğrudan | preferredDataLocation | Kaynak öznitelik seçin | Denetlenmeyen | Güncelleştirme |

7. Gelen kuralı oluşturmak için Seç **Ekle**.

![Gelen eşitleme kuralı oluştur ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule"></a>5\. Adım: Giden eşitleme kuralı oluşturma
Giden eşitleme kuralı için aramasındaki akış için öznitelik değeri verir **preferredDataLocation** Azure AD'de öznitelik:

1. Git **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **giden**.
3. Seçin **Yeni Kural Ekle**.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "Out Azure AD'ye – kullanıcı preferredDataLocation" |
    | Açıklama | *Bir açıklama sağlayın* ||
    | Bağlı sistem | *Azure AD Bağlayıcısı'nı seçin* ||
    | Bağlı sistem nesnesi türü | **Kullanıcı** ||
    | Meta veri deposu nesne türü | **Kişi** ||
    | Bağlantı türü | **Birleştir** ||
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesini ve iki yan tümceyi ile tek bir kapsam belirleme filtre grubu Ekle:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam belirleme filtresi, hangi Azure AD giden eşitleme kuralının uygulanacağı nesneleri belirler. Bu örnekte, "Out AD – kullanıcı kimliği için" aynı kapsam belirleme filtresi kullandığımız OOB (kullanıma hazır) eşitleme kuralı. Eşitleme kuralı için uygulanmasını önleyen **kullanıcı** şirket içi Active Directory'nizden eşitlenen olmayan nesneler. Azure AD Connect dağıtımınıza göre kapsam belirleme filtresi ince yapmanız gerekebilir.

6. Git **dönüştürme** sekmesini tıklatıp şu dönüştürme kuralı uygulayın:

    | Akış türü | Hedef öznitelik | source | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | preferredDataLocation | preferredDataLocation | Denetlenmeyen | Güncelleştirme |

7. Kapat **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı oluştur ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>6\. Adım: Tam eşitleme döngüsü çalıştırın
Genel olarak, tam eşitleme döngüsü gereklidir. Active Directory ve Azure AD Bağlayıcısı şema için yeni özellikler eklendi ve özel eşitleme kuralları sunulan olmasıdır. Değişiklikler Azure AD'ye dışarı aktarmadan önce doğrulayın. El ile yaptığınız bir tam eşitleme döngüsü adımları çalıştırırken, değişiklikleri doğrulamak için aşağıdaki adımları kullanabilirsiniz.

1. Çalıştırma **tam içeri aktarma** üzerinde şirket içi Active Directory Bağlayıcısı:

   1. Git **işlemleri** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
   2. Sağ **şirket içi Active Directory Bağlayıcısı**seçip **çalıştırma**.
   3. İletişim kutusunda **tam içeri aktarma**seçip **Tamam**.
   4. İşlemin tamamlanmasını bekleyin.

      > [!NOTE]
      > Kaynak özniteliği zaten içeri aktarılan öznitelikler listesinde yer alıyorsa, şirket içi Active Directory Bağlayıcısı üzerinde tam içeri aktarma atlayabilirsiniz. Diğer bir deyişle, bu makalenin önceki kısımlarında adımı 2 sırasında herhangi bir değişiklik yapmak yoktu.

2. Çalıştırma **tam içeri aktarma** Azure AD Bağlayıcısı üzerinde:

   1. Sağ **Azure AD Bağlayıcısı**seçip **çalıştırma**.
   2. İletişim kutusunda **tam içeri aktarma**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

3. Var olan bir eşitleme kuralı değişiklikleri doğrulamak **kullanıcı** nesne.

   Şirket içi Active Directory'den kaynak özniteliği ve **preferredDataLocation** Azure AD'den her ilgili bağlayıcı alanına içeri aktarıldığından. Tam eşitleme adımına devam etmeden önce var olan bir önizlemesini yapın **kullanıcı** şirket içi Active Directory Bağlayıcısı alanına nesnesi. Seçtiğiniz nesne doldurulmuş kaynak özniteliğine sahip olmamalıdır. İle başarılı bir önizleme **preferredDataLocation** meta veri deposunda eşitlemenin yapılandırdığınız iyi bir göstergesi kuralları doğru şekilde doldurulur. Bir önizleme yapma hakkında daha fazla bilgi için bkz. [değişikliği doğrulayın](how-to-connect-sync-change-the-configuration.md#verify-the-change).

4. Çalıştırma **tam eşitleme** üzerinde şirket içi Active Directory Bağlayıcısı:

   1. Sağ **şirket içi Active Directory Bağlayıcısı**seçip **çalıştırma**.
   2. İletişim kutusunda **tam eşitleme**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

5. Doğrulama **bekleyen dışarı aktarmaların** Azure AD'ye:

   1. Sağ **Azure AD Bağlayıcısı**seçip **arama bağlayıcı alanı**.
   2. İçinde **arama bağlayıcı alanı** iletişim kutusunda:

        a. Ayarlama **kapsam** için **bekleyen dışa aktarım**.<br>
        b. Tüm üç onay kutusunu da dahil olmak üzere, işaretleyin **eklemek, değiştirmek ve silmek**.<br>
        c. Nesneleri dışarı değişikliklerle listesini görüntülemek için seçin **arama**. Belirli bir nesne için değişiklikleri incelemek için nesneyi çift tıklayın.<br>
        d. Değişiklikleri beklendiğini doğrulayın.

6. Çalıştırma **dışarı** üzerinde **Azure AD Bağlayıcısı**

   1. Sağ **Azure AD Bağlayıcısı**seçip **çalıştırma**.
   2. İçinde **çalıştırma bağlayıcı** iletişim kutusunda **dışarı**seçip **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

> [!NOTE]
> Adımları Azure AD Bağlayıcısı üzerinde tam eşitleme adım ve dışarı aktarma adımında Active Directory Bağlayıcısı içermediğini fark edebilirsiniz. Öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akışa çünkü adımlar gerekli değildir.

## <a name="step-7-re-enable-sync-scheduler"></a>7\. Adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. Bir PowerShell oturumu başlatın.
2. Zamanlanan eşitleme şu cmdlet'i çalıştırarak yeniden etkinleştirin: `Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>8\. adım: Sonucu doğrulayın
Bu, artık yapılandırmasını doğrulayın ve kullanıcılarınız için etkinleştirme zamanı geldi.

1. Coğrafi bir kullanıcı seçili özniteliğini ekleyin. Hizmetin kullanılabildiği coğrafyalar listesi bu tabloda bulunabilir.  
![Ekran görüntüsü AD özniteliği için bir kullanıcı eklendi](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Azure AD ile eşitlenecek öznitelik bekleyin.
3. Exchange Online PowerShell kullanarak, posta kutusu bölgesi doğru şekilde ayarlandığını doğrulayın.  
![Exchange Online PowerShell ekran görüntüsü](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Kiracınızda bu özelliği kullanabilmek için işaretlenmiş olduğundan varsayıldığında, posta kutusu için doğru coğrafi taşınır. Bu posta kutusu bulunduğu sunucu adına bakarak doğrulanabilir.
4. Bu ayar birçok posta etkin olduğunu doğrulamak için komut dosyasını kullanın. [TechNet Galerisi](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Bu betik, ayrıca tüm Office 365 veri merkezi sunucusu önekleri listesini sahiptir ve hangi coğrafi bulunduğu içinde. Bu önceki adımda bir başvuru olarak posta kutusunun konumunu doğrulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Office 365'te çoklu coğrafi hakkında daha fazla bilgi edinin:

* [Ignite çoklu coğrafi oturumları](https://aka.ms/MultiGeoIgnite)
* [Çoklu coğrafi onedrive](https://aka.ms/OneDriveMultiGeo)
* [SharePoint Online'daki çoklu coğrafi](https://aka.ms/SharePointMultiGeo)

Eşitleme altyapısı yapılandırma modeli hakkında daha fazla bilgi edinin:

* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).
* İfade dili, hakkında daha fazla bilgiyi [bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).

Genel bakış konuları:

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
