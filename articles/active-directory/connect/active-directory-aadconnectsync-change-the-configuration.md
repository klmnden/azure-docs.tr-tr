---
title: "Azure AD Connect eşitleme: bir yapılandırma değişikliği Azure AD Connect eşitleme yaptığınızda | Microsoft Docs"
description: "Azure AD Connect eşitleme yapılandırmasında değişiklik konusunda size yol göstermektedir."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 63a7ae9d39e1a74294637172efd607ee41b2d69b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect eşitleme: varsayılan yapılandırmanın bir değişiklik yapma
Bu konunun amacı, Azure AD Connect eşitleme varsayılan yapılandırmasında değişiklik konusunda size yol sağlamaktır. Bu, bazı ortak senaryolar için adımları sağlar. Bu bilgiyle, kendi iş kurallarına göre kendi yapılandırma bazı basit değişiklikler yapmak yapabiliyor olmanız gerekir.

## <a name="synchronization-rules-editor"></a>Eşitleme kuralları Düzenleyicisi
Eşitleme kuralları Düzenleyicisi görmek ve varsayılan yapılandırmasını değiştirmek için kullanılır. Başlat menüsünün altında bulabilirsiniz **Azure AD Connect** grubu.  
![Başlat menüsü ile eşitleme kural Düzenleyicisi](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Açtığınızda, varsayılan out-of-box kuralları bakın.

![Eşitleme kuralı Düzenleyicisi](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Düzenleyicide gezinme
Aşağı açılan listeler düzenleyicisinin üstünde, belirli bir kural hızla bulmak izin verir. Öznitelik proxyAddresses dahil olduğu kuralları görmek istiyorsanız, aşağı açılan listeler Örneğin, aşağıdaki değiştirin:  
![SRE filtreleme](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Filtreleme sıfırlamak ve yeni yapılandırmayı basın **F5** klavyedeki.

Sağ üst için bir düğmeye sahip **Yeni Kural Ekle**. Bu düğme, kendi özel bir kural oluşturmak için kullanılır.

Alt kısmındaki düğmeleri hareket için bir seçili eşitleme kuralı vardır. **Düzen** ve **silmek** onlara beklediğiniz yapın. **Dışarı aktarma** eşitleme kuralı yeniden oluşturmak için bir PowerShell Betiği oluşturur. Bu yordam, bir eşitleme kuralı bir sunucudan diğerine taşımanıza olanak verir.

## <a name="create-your-first-custom-rule"></a>İlk, özel bir kural oluşturun
Öznitelik akışları değişiklikler en yaygın değişikliktir. Veri kaynağı dizininizde Azure AD olduğu gibi olmayabilir. Bu bölümdeki örnekte, bir kullanıcının verilen adı her zaman içinde olduğundan emin olmak istediğiniz **uygun durumda**.

### <a name="disable-the-scheduler"></a>Zamanlayıcı'yı devre dışı bırak
[Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) varsayılan olarak 30 dakikada bir çalışır. Değişiklik yapmadan ve yeni kurallarınızı sorun giderme, başlangıç değil emin olmak istersiniz. Geçici olarak Zamanlayıcı'yı devre dışı bırakmak için PowerShell'i başlatın ve çalıştırın`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Zamanlayıcı'yı devre dışı bırak](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Kural oluşturma
1. Tıklatın **Yeni Kural Ekle**.
2. Üzerinde **açıklama** sayfasında aşağıdakileri girin:  
   ![Filtreleme kuralını gelen](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Ad: kural açıklayıcı bir ad verin.
   * Açıklama: birisi kural içindir anlamaları için bazı açıklama.
   * Bağlı sistem: sistemi nesnenin bulunabilir. Bu durumda, Active Directory bağlayıcısını seçin.
   * Bağlı sistem/meta veri deposu nesne türü: Seçin **kullanıcı** ve **kişi** sırasıyla.
   * Bağlantı türü: Bu değer ile değiştirmek **katılma**.
   * Öncelik: sistemdeki benzersiz olan bir değer sağlayın. Daha yüksek önceliği daha düşük bir sayısal değer belirtir.
   * Etiket: boş bırakın. Yalnızca out-of-box kuralları Microsoft'tan bir değerle doldurulmuş bu kutu olması gerekir.
3. Üzerinde **Scoping filtre** want **givenName ISNOTNULL**.  
   ![Gelen kuralı kapsamı filtresi](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Bu bölümde, kuralın geçerli olacağı nesneleri tanımlamak için kullanılır. Boş bırakılırsa, kuralı tüm kullanıcı nesneleri için geçerli olur. Ancak, konferans odaları, hizmet hesapları ve diğer kişiler olmayan kullanıcı nesneleri içerir.
4. Üzerinde **katılma kuralları**, bu alanı boş bırakın.
5. Üzerinde **dönüşümleri** sayfasında, FlowType değiştirmek **ifade**. Hedef öznitelik Seç **givenName**ve kaynağında girin `PCase([givenName])`.
   ![Gelen kuralı dönüşümleri](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Eşitleme altyapısı, hem de işlev adı ve öznitelik adı duyarlıdır. Bir sorun yazarsanız, kural eklediğinizde, bir uyarı görürsünüz. Kural yeniden açın ve hatayı düzeltmek gerekir böylece Düzenleyicisi'ni kaydetmek ve devam etmek sağlar.
6. Tıklatın **Ekle** kuralını kaydetmek için.

Yeni özel kuralın sistemdeki diğer eşitleme kuralları görünür olması gerekir.

### <a name="verify-the-change"></a>Değişikliği doğrulayın
Bu yeni değişiklikle hataları atma değil ve beklendiği gibi çalıştığından emin olmak istiyorsunuz. Sahip olduğunuz nesneleri sayısına bağlı olarak, bu adımı tamamlamak için iki farklı yolu vardır.

1. Tüm nesneler üzerinde tam bir eşitleme çalıştırın
2. Tek bir nesne üzerinde bir önizleme ve tam eşitleme çalıştırma

Başlat **eşitleme hizmeti** Başlat menüsünden. Bu bölümde bu araç tüm adımlardır.

1. **Tüm nesneler üzerinde tam eşitleme**  
   Seçin **Bağlayıcılar** üstünde. Bu durumda Active Directory etki alanı Hizmetleri, önceki bölümde bir değişiklik yaptığınız bağlayıcı tanımlamak ve seçin. Seçin **çalıştırmak** Eylemler ve select **tam eşitleme** ve **Tamam**.
   ![Tam eşitleme](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Nesneler meta veri deposunda şimdi güncelleştirilir. Şimdi, meta veri nesnesinde bakmak istiyorsunuz.
2. **Önizleme ve tek bir nesne üzerinde tam eşitleme**  
   Seçin **Bağlayıcılar** üstünde. Bu durumda Active Directory etki alanı Hizmetleri, önceki bölümde bir değişiklik yaptığınız bağlayıcı tanımlamak ve seçin. Seçin **bağlayıcı alanı arama**. Kapsam değişikliği test etmek için kullanmak istediğiniz bir nesneyi bulmak için kullanın. Nesne seçin ve tıklatın **Önizleme**. Yeni ekranında şunları seçin **Commit Önizleme**.  
   ![Önizleme Yürüt](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Değişiklik için meta veri deposu artık kararlıdır.

**Meta veri nesnesinde bakın**  
Şimdi, beklenen değer emin olmak için birkaç örnek nesneleri ve kuralın uygulanacağı çekme istiyorsunuz. Seçin **meta veri deposu arama** üstten. İlgili nesneleri bulmak için gereken herhangi bir filtre ekleyin. Arama sonuçlarından nesneyi açın. Öznitelik değerleri arayın ve ayrıca doğrulayın **eşitleme kuralları** beklendiği gibi kuralın uygulandığı sütun.  
![Meta veri deposu arama](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Zamanlayıcısını Etkinleştir
Her şeyin beklendiği gibi ise, Zamanlayıcı yeniden etkinleştirebilirsiniz. PowerShell çalıştırmak `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Diğer ortak öznitelik akışı değişiklikleri
Önceki bölümde bir öznitelik akışı değişiklik açıklanmıştır. Bu bölümde, bazı ek örnekler verilmiştir. Eşitleme kuralı oluşturmak adımlar kısaltılmış, ancak tam adımlar önceki bölümde bulabilirsiniz.

### <a name="use-another-attribute-than-the-default"></a>Varsayılandan başka bir öznitelik kullanın
Fabrikam'daki, yerel alfabe verilen ad, Soyadı ve görünen ad için kullanıldığı bir orman yoktur. Bu öznitelikler Latin karakterlerini gösterimini uzantı öznitelikleri bulunabilir. Azure AD içinde genel adres listesinde oluştururken ve Office 365, kuruluşun istediği yerine kullanılacak bu öznitelikler.

Varsayılan yapılandırma ile yerel orman nesneden şöyle görünür:  
![Öznitelik akışı 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Diğer öznitelik akışları ile bir kural oluşturmak için aşağıdakileri yapın:

* Başlat **Synchronization Rule Editor** Başlat menüsünden.
* İle **gelen** sola seçiliyken, düğmesini **Yeni Kural Ekle**.
* Kural, bir ad ve açıklama verin. Şirket içi Active Directory ve ilgili nesne türlerini seçin. İçinde **bağlantı türü**seçin **katılma**. Öncelik, başka bir kural tarafından kullanılmayan bir sayı seçin. 50 değeri bu örnekte, böylece out-of-box kuralları 100 ile başlatın.
  ![Öznitelik akışı 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Kapsam boş bırakın getirin (diğer bir deyişle, ormandaki tüm kullanıcı nesneleri uygulanması gereken).
* Birleşim kuralları boş bırakın (diğer bir deyişle, birleşimlerin işlemek out-of-box kural sağlar).
* Dönüşümleri, aşağıdaki akışları oluşturun:  
  ![Öznitelik akışı 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Tıklatın **Ekle** kuralını kaydetmek için.
* Git **Eşitleme Hizmeti Yöneticisi'ni**. Üzerinde **Bağlayıcılar**, kural eklediğimiz burada Bağlayıcısı'nı seçin. Seçin **çalıştırmak**, ve **tam eşitleme**. Tam eşitleme, geçerli kurallarını kullanarak tüm nesneleri yeniden hesaplar.

Bu bu özel kuralın ile aynı nesne için kaynaklanır:  
![Öznitelik akışı 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Öznitelikleri uzunluğu
Dize özniteliklerin dizine olarak ayarlanmalıdır varsayılan ve en fazla 448 karakterdir. Daha içerebilir dizesi öznitelikleri ile çalışıyorsanız, aşağıdaki öznitelik akışı eklediğinizden emin olun:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>UserPrincipalSuffix değiştirme
UserPrincipalName özniteliği Active Directory'de her zaman kullanıcılar tarafından bilinmiyor ve oturum açma kimliği olarak uygun olmayabilir. Örneğin, farklı bir öznitelik çekme eşitleme Yükleme Sihirbazı'nı sağlar Azure AD Connect posta. Ancak bazı durumlarda öznitelik hesaplanması gerekir. Örneğin, Contoso şirket, iki Azure AD dizini, bir üretim için ve test etmek için bir sahiptir. Oturum açma kimliği başka bir sonek kullanmak üzere kendi test Kiracı kullanıcılar istedikleri  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

Bu ifadede her şeyi ele ilk sol @-sign (Word) ve sabit bir dize ile Birleştir.

### <a name="convert-a-multi-value-to-a-single-value"></a>Tek değerli birden çok değerli Dönüştür
Active Directory Kullanıcıları ve Bilgisayarları'nda değerli tek Ara olsa bile Active Directory'de bazı öznitelikler şemada birden çok değerli. Açıklama özniteliği örneğidir.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Bu ifadede öznitelik ilk öğe (öğe) ele bir değer özniteliği sahip olmaması durumunda, baştaki ve sondaki boşlukları (kırpma) kaldırın ve ardından (soldaki) ilk 448 karakter dizesini tutun.

### <a name="do-not-flow-an-attribute"></a>Bir öznitelik akışı değil
Bu bölümde senaryo arka planda için bkz: [öznitelik akışı işlemini denetleyen](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Bir öznitelik akışı olmayan iki yolu vardır. İlk Yükleme Sihirbazı'nda kullanılabilir ve böylece [seçili öznitelikleri kaldırmak](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Bu seçenek, hiçbir zaman önce öznitelik eşitlenmiş durumunda çalışır. Ancak, bu öznitelik eşitlemek ve daha sonra bu özellik ile kaldırmak başlattıysanız, öznitelik ve var olan değerleri yönetme eşitleme altyapısı durakları Azure AD'de bırakılır.

Özniteliğin değerini kaldırın ve gelecekte akmaz emin olmak istiyorsanız, bunun yerine özel bir kural oluşturmak.

Fabrikam'daki, biz bazı biz buluta eşitleme öznitelikleri var olmamalıdır, gerçekleştirilmiş. Bu öznitelikler Azure AD'den kaldırılır emin olmak istiyoruz.  
![Hatalı uzantı öznitelikleri](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Yeni bir gelen eşitleme kuralı oluşturabilir ve bir açıklama doldurabilirsiniz ![açıklamaları](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Öznitelik akışları türü oluşturma **ifade** ve kaynağı ile **AuthoritativeNull**. Sabit **AuthoritativeNull** değerini doldurmak daha düşük bir öncelik eşitleme kuralı çalıştığında olsa bile değerin MV boş olması gerektiğini gösterir.
  ![Uzantı öznitelikleri dönüşümü](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Eşitleme kuralı kaydedin. Başlat **eşitleme hizmeti**, bağlayıcı bulmak, seçmek **çalıştırmak**, ve **tam eşitleme**. Bu adım, tüm öznitelik akışları yeniden hesaplar.
* Hedeflenen değişiklikleri bağlayıcı alanı arama yaparak dışarı aktarılacak olduğunu doğrulayın.
  ![Hazırlanan Sil](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>PowerShell ile kuralları oluşturma
Yalnızca yapmak için birkaç değişiklik olduğunda eşitleme kuralı Düzenleyicisi'ni kullanarak düzgün çalışır. Birçok değişiklik yapmanız gerekirse, PowerShell daha iyi bir seçenek olabilir. Gelişmiş özellikler yalnızca PowerShell ile kullanılabilir.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>PowerShell komut dosyası için bir out-of-box kuralını Al
Bir out-of-box kuralı oluşturulan komut dosyası, eşitleme kuralları Düzenleyicisi'nde kuralı seçin ve tıklatın PowerShell görmek için **verme**. Bu eylem, kural oluşturulan PowerShell komut dosyası sağlar.

### <a name="advanced-precedence"></a>Gelişmiş önceliği
Out-of-box eşitleme kuralları öncelik değeri 100 ile başlatın. Daha sonra birçok ormanlar varsa ve birçok özel değişiklikler yapmanız gerekir, 99 eşitleme kuralları yeterli olmayabilir.

Eşitleme altyapısı out-of-box kurallardan önce eklenen ek kurallar istediğiniz söyleyebilirsiniz. Bu davranışı elde etmek üzere aşağıdaki adımları izleyin:

1. İlk out-of-box eşitleme kuralı işaretlemek (Bu kural **içinde AD kullanıcı katılma gelen**) eşitleme kuralı Düzenleyicisi'ni seçip **verme**. SR tanımlayıcı değeri kopyalayın.  
![PowerShell değişikliği öncesinde](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Yeni eşitleme kuralı oluşturun. Eşitleme kuralı Düzenleyicisi'ni oluşturmak için kullanabilirsiniz. Kural için bir PowerShell betiğini dışarı aktarın.
3. Özelliğindeki **PrecedenceBefore**, out-of-box kuraldan tanımlayıcı değeri ekleyin. Ayarlama **öncelik** için **0**. Benzersiz tanımlayıcı özniteliği ve başka bir kural adresinden bir GUID yeniden kullanma değil emin olun. Da emin olmanız **ImmutableTag** özelliği ayarlı değil; Bu özellik yalnızca bir out-of-box kuralı için ayarlamanız gerekir. PowerShell komut dosyasını kaydedin ve çalıştırın. Özel kural öncelik değeri 100 atanır ve diğer tüm out-of-box kurallar artırılır sonucudur.  
![Değişiklikten sonra PowerShell](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Aynı kullanarak birçok özel eşitleme kuralları olabilir **PrecedenceBefore** değer gerektiğinde.


## <a name="enable-synchronization-of-preferreddatalocation"></a>PreferredDataLocation eşitlemeyi etkinleştir
Azure AD Connect eşitleme destekleyen **PreferredDataLocation** için öznitelik **kullanıcı** sürüm 1.1.524.0 ve sonra nesneleri. Daha açık belirtmek gerekirse, aşağıdaki değişiklikleri sunulmuştur:

* Nesne türü şeması **kullanıcı** Azure AD Bağlayıcısı türü dize olan ve tek değerli PreferredDataLocation özniteliği kapsayacak şekilde genişletilir.

* Nesne türü şeması **kişi** meta veri deposunda türü dize olan ve tek değerli PreferredDataLocation özniteliği kapsayacak şekilde genişletilir.

Şirket içi Active Directory içinde karşılık gelen bir PreferredDataLocation öznitelik olduğundan varsayılan olarak, eşitleme için PreferredDataLocation özniteliği etkin değil. Eşitleme el ile etkinleştirmeniz gerekir.

> [!IMPORTANT]
> Şu anda Azure AD, Azure AD PowerShell kullanarak doğrudan olacak şekilde yapılandırılmış PreferredDataLocation özniteliği eşitlenmiş kullanıcı nesneleri hem bulut kullanıcı nesneleri sağlar. PreferredDataLocation öznitelik eşitlemesi etkinleştirildiğinde, öznitelik yapılandırmak için Azure AD PowerShell kullanarak durdurmalısınız **kullanıcı nesneleri eşitlenen** Azure AD Connect bunları şirket içi Active Directory'de kaynak öznitelik değerleri temel alarak geçersiz kılar.

> [!IMPORTANT]
> 1 Eylül 2017 üzerinde Azure AD artık PreferredDataLocation özniteliği üzerinde izin **kullanıcı nesneleri eşitlenen** doğrudan Azure AD PowerShell kullanarak yapılandırılmalıdır. Eşitlenen kullanıcı nesnelerindeki PreferredLocation özniteliği yapılandırmak için yalnızca Azure AD Connect kullanmanız gerekir.

Eşitleme PreferredDataLocation özniteliğinin etkinleştirmeden önce aşağıdakileri yapmalısınız:

 * İlk olarak, kaynak özniteliği olarak kullanılmak üzere hangi şirket içi Active Directory öznitelik karar verin. Türünde olmalı **dize** ve **tek değerli**.

 * Daha önce PreferredDataLocation özniteliği üzerinde yapılandırdıysanız Azure AD PowerShell kullanarak Azure AD içinde eşzamanlı kullanıcı nesneleri varolan, şunları yapmalısınız **backport** karşılık gelen kullanıcı nesnelerine şirket içi Active Directory'deki öznitelik değerleri.
 
    > [!IMPORTANT]
    > Şirket içi Active Directory'de karşılık gelen kullanıcı nesneleri için öznitelik değerlerini değil backport bunu yaparsanız, Azure AD Connect eşitleme PreferredDataLocation özniteliği için etkinleştirildiğinde Azure AD'de mevcut öznitelik değerlerini kaldırın.

 * Kaynak özniteliği yapılandırmanız önerilir daha sonra doğrulama için kullanılabilecek artık, şirket içi en az bir birkaç AD kullanıcı nesneleri.
 
Eşitleme PreferredDataLocation özniteliğinin etkinleştirme adımları olarak özetlenebilir:

1. Eşitleme Zamanlayıcı'yı devre dışı bırakın ve devam eden bir eşitleme doğrulayın

2. Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şeması

3. Azure AD Bağlayıcısı şemaya PreferredDataLocation Ekle

4. Şirket içi Active Directory'den öznitelik değeri akışı için bir gelen eşitleme kuralı oluşturma

5. Azure AD öznitelik değerini akışı için bir giden eşitleme kuralı oluşturma

6. Tam eşitleme döngüsü çalıştırın

7. Eşitleme Zamanlayıcı etkinleştir

> [!NOTE]
> Bu bölümde rest Ayrıntıları'nda aşağıdaki adımları kapsar. Özel eşitleme kuralları olmadan tek orman topolojisi ile bir Azure AD dağıtımı bağlamında açıklanmıştır. Çoklu orman topolojisini varsa, özel eşitleme kuralları yapılandırılmış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız gerekir.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
Azure AD dışarı aktarılan istenmeyen değişiklikleri önlemek için eşitleme kuralları güncelleştiriliyor ortasında durumdayken eşitleme gerçekleşir emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

 1. Azure AD Connect sunucusunda PowerShell oturumu başlatın.

 2. Zamanlanan eşitleme cmdlet'ini çalıştırarak devre dışı bırakın:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Başlat **Eşitleme Hizmeti Yöneticisi'ni** Başlangıç → eşitleme hizmeti giderek.
 
 4. Git **Operations** sekmesinde ve durumu olan işlem yok onaylayın *"sürüyor."*

![Eşitleme Hizmeti Yöneticisi - devam eden hiçbir işlemleri denetleyin](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>2. adım: şirket içi kaynak özniteliği ekleme AD Bağlayıcısı şeması
Tüm AD öznitelikleri şirket alınır AD bağlayıcı alanı. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

 1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
 
 2. Sağ **şirket içi AD Bağlayıcısı** seçip **özellikleri**.
 
 3. Açılan iletişim kutusunda, Git **öznitelikleri Seç** sekmesi.
 
 4. Kaynak öznitelikte öznitelik listesinde işaretli olduğundan emin olun.
 
 5. Tıklatın **Tamam** kaydetmek için.

![Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şeması](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>3. adım: Azure AD Bağlayıcısı şemaya PreferredDataLocation ekleme
Varsayılan olarak, Azure AD Connect alanına PreferredDataLocation özniteliği alınmaz. İçeri aktarılan öznitelikleri listesi PreferredDataLocation özniteliği eklemek için:

 1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.

 2. Sağ **Azure AD Bağlayıcısı** seçip **özellikleri**.

 3. Açılan iletişim kutusunda, Git **öznitelikleri Seç** sekmesi.

 4. PreferredDataLocation özniteliği öznitelik listesinde işaretli olduğundan emin olun.

 5. Tıklatın **Tamam** kaydetmek için.

![Azure AD Bağlayıcısı şemaya kaynak öznitelik Ekle](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>4. adım: şirket içi Active Directory'den öznitelik değeri akışı için bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı öznitelik değerini meta veri deposu için şirket içi Active Directory'den kaynak özniteliğinden akış verir:

1. Başlat **eşitleme kuralları Düzenleyicisi** Başlangıç → eşitleme kuralları Düzenleyicisi giderek.

2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.

3. Tıklatın **Yeni Kural Ekle** düğmesi yeni gelen kuralı oluşturun.

4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:
 
    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *"içinde AD'den – kullanıcı PreferredDataLocation"* |
    | Açıklama | *Bir açıklama belirtin* |  |
    | Bağlı sistem | *Şirket içi çekme AD Bağlayıcısı* |  |
    | Bağlı sistem nesne türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Kişi** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesinde ve ekleme bir **tek bir kapsam filtresi grubunu aşağıdaki yan tümcesiyle**:
 
    | Öznitelik | işleci | Değer |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Kullanıcı\_ | 
 
    Kapsam filtresi, bu gelen eşitleme kuralını uygulandığı AD nesnelerini şirket içi belirler. Bu örnekte, kullanırız olarak kullanılan aynı kapsam filtresi *"içinde AD'den – kullanıcı ortak"* Azure AD kullanıcı geri yazma özelliği kullanılarak oluşturulan kullanıcı nesnelerine uygulanan eşitleme kuralı engeller OOB eşitleme kuralı. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.

6. Git **dönüştürme sekmesi** ve aşağıdaki dönüştürme kuralı uygular:
 
    | Akış türü | Target özniteliği | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | PreferredDataLocation | Kaynak özniteliği seçin | İşaretli | Güncelleştirme |

7. Tıklatın **Ekle** gelen kuralı oluşturmak için.

![Gelen eşitleme kuralı oluşturma](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>5. adım: Azure AD öznitelik değerini akışı için bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı öznitelik değerini meta veri deposu Azure AD'de PreferredDataLocation öznitelik akış verir:

1. Git **eşitleme kuralları** Düzenleyici.

2. Arama filtresi ayarlamak **yönü** olmasını **giden**.

3. Tıklatın **Yeni Kural Ekle** düğmesi.

4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, "çıkışı için AAD – kullanıcı PreferredDataLocation" |
    | Açıklama | *Bir açıklama belirtin* |
    | Bağlı sistem | *AAD bağlayıcı seçin* |
    | Bağlı sistem nesne türü | Kullanıcı ||
    | Meta veri deposu nesne türü | **Kişi** ||
    | Bağlantı türü | **Birleştir** ||
    | Önceliği | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. YDo olmayan başka bir eşitleme kuralı tarafından kullanılan bir değer seçin. |

5. Git **Scoping filtre** sekmesinde ve ekleme bir **iki maddeleri tek kapsam filtresi grubu**:
 
    | Öznitelik | işleci | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam Filtresi hangi Azure AD bu giden eşitleme kuralının uygulandığı nesneleri belirler. Bu örnekte, "Dışı" AD – kullanıcı kimliği için aynı kapsam filtresinden kullanırız OOB eşitleme kuralı. Eşitleme kuralı şirket içi Active Directory'den eşitlenmez kullanıcı nesnelerine uygulanan engeller. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.
    
6. Git **dönüştürme** sekmesinde ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Target özniteliği | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | PreferredDataLocation | PreferredDataLocation | İşaretli | Güncelleştirme |

7. Kapat **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı oluştur](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>6. adım: Çalıştır tam eşitleme döngüsü
AD için yeni öznitelikler ekledik genel olarak, tam eşitleme döngüsü gerekli olduğu ve Azure AD Bağlayıcısı şema ve sunulan özel eşitleme kuralları. Azure AD dışarı aktarmadan önce değişiklikleri doğrulamak önerilir. Bir tam eşitleme döngüsü yapmak adımları el ile çalışırken değişiklikleri doğrulamak için aşağıdaki adımları kullanın. 

1. Çalıştırma **tam alma** üzerinde Adım **şirket içi AD Bağlayıcısı**:

   1. Git **Operations** sekme Eşitleme Hizmeti Yöneticisi'nde.

   2. Sağ **şirket içi AD Bağlayıcısı** seçip **Çalıştır...**

   3. Açılan iletişim kutusunda seçin **tam içeri aktarma** tıklatıp **Tamam**.
    
   4. İşlemin tamamlanmasını bekleyin.

    > [!NOTE]
    > Tam içeri aktarma atlayabilirsiniz şirket içi AD kaynak özniteliği zaten listesinde yer alıyorsa Bağlayıcısı öznitelikleri alındı. Diğer bir deyişle, sırasında herhangi bir değişiklik yapmak zorunda kalmadığımıza [2. adım: şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Çalıştırma **tam alma** üzerinde Adım **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **Çalıştır...**

   2. Açılan iletişim kutusunda seçin **tam içeri aktarma** tıklatıp **Tamam**.
   
   3. İşlemin tamamlanmasını bekleyin.

3. Eşitleme kuralı değişiklikleri var olan bir kullanıcı nesnesi üzerinde doğrulayın:

Kaynak özniteliği Active Directory ve Azure AD'den PreferredDataLocation ilgili bağlayıcıyı alanına içeri aktarıldığını şirket içi. Tam eşitleme adımla devam etmeden önce bunu yapmanız önerilir bir **Önizleme** üzerinde var olan bir kullanıcı nesnesi şirket içi AD bağlayıcı alanı. Seçtiğiniz nesne doldurulmuş kaynak özniteliği olmalıdır. Başarılı bir **Önizleme** eşitleme yapılandırdığınız iyi bir gösterge kuralları doğru meta veri deposunda doldurulmuş PreferredDataLocation olduğu. Nasıl yapılacağı hakkında bilgi için bir **Önizleme**, bölümüne bakın [değişikliği doğrulayın](#verify-the-change).

4. Çalıştırma **tam eşitleme** üzerinde Adım **şirket içi AD Bağlayıcısı**:

   1. Sağ **şirket içi AD Bağlayıcısı** seçip **Çalıştır...**
  
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
      
   1. Sağ **Azure AD Bağlayıcısı** seçip **Çalıştır...**
   
   2. Bağlayıcı çalıştırmak açılan iletişim kutusunda, seçin **verme** tıklatıp **Tamam**.
   
   3. Tamamlamak için Azure ad dışarı aktarma bekleyin.

> [!NOTE]
> Adımları tam eşitleme adımı ve Azure AD Bağlayıcısı üzerinde dışarı aktarma adımı içermez fark edebilirsiniz. Adımları öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akmaktadır gerekli değildir.

### <a name="step-7-re-enable-sync-scheduler"></a>7. adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. PowerShell oturumu başlatın.

2. Zamanlanan eşitleme cmdlet'ini çalıştırarak yeniden etkinleştirin:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
