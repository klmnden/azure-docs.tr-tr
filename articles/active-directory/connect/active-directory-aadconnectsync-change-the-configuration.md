---
title: "Azure AD Connect eşitleme: bir yapılandırma değişikliği Azure AD Connect eşitleme yaptığınızda | Microsoft Docs"
description: "Azure AD Connect eşitleme yapılandırmasında değişiklik konusunda size yol göstermektedir."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2018
ms.author: billmath
ms.openlocfilehash: e97d3e3e35ee87864c5d38e75e08e62088e25fdb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect eşitleme: varsayılan yapılandırmayı değişiklik
Bu makalenin amacı, Azure Active Directory (Azure AD) Connect eşitleme varsayılan yapılandırmasında değişiklik konusunda size yol sağlamaktır. Bu, bazı ortak senaryolar için adımları sağlar. Bu bilgiyle, kendi iş kurallarına göre kendi yapılandırma basit değişiklik yapabiliyor olmanız gerekir.

## <a name="synchronization-rules-editor"></a>Eşitleme kuralları Düzenleyicisi
Eşitleme kuralları Düzenleyicisi görmek ve varsayılan yapılandırmasını değiştirmek için kullanılır. Üzerinde Bul **Başlat** menüsünün altında **Azure AD Connect** grubu.  
![Başlat menüsü ile eşitleme kural Düzenleyicisi](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Düzenleyicisi'ni açmak için varsayılan out-of-box kuralları görürsünüz.

![Eşitleme kuralı Düzenleyicisi](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Düzenleyicide gezinme
Aşağı açılan listeler Düzenleyicisi üstündeki kullanarak, belirli bir kuralın hızlı bir şekilde bulabilirsiniz. Öznitelik proxyAddresses dahil olduğu kuralları görmek istiyorsanız, örneğin, aşağı açılan listeler aşağıdaki değiştirebilirsiniz:  
![SRE filtreleme](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Filtreleme sıfırlamak ve yeni yapılandırmayı klavyede F5'e basın.

Üst sağ tarafta olan **Yeni Kural Ekle** düğmesi. Kendi özel bir kural oluşturmak için bu düğmeyi kullanın.

Sayfanın alt tarafında bir seçili eşitleme kuralı hareket için düğmeler bulunur. **Düzen** ve **silmek** onlara beklediğiniz yapın. **Dışarı aktarma** yeniden eşitleme kuralı oluşturmak için bir PowerShell Betiği oluşturur. Bu yordam ile bir eşitleme kuralı bir sunucudan diğerine taşıyabilirsiniz.

## <a name="create-your-first-custom-rule"></a>İlk, özel bir kural oluşturun
Öznitelik akışları için en yaygın değişir. Veri kaynağı dizininizde Azure AD ile aynı olmayabilir. Bu bölümdeki örnekte, bir kullanıcının verilen adı her zaman içinde olduğundan emin olun *uygun durumda*.

### <a name="disable-the-scheduler"></a>Zamanlayıcı'yı devre dışı bırak
[Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) varsayılan olarak 30 dakikada bir çalışır. Değişiklik yapmadan ve yeni kurallarınızı sorun giderme sırasında başlangıç değil emin olun. Geçici olarak Zamanlayıcı'yı devre dışı bırakmak için PowerShell'i başlatın ve çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Zamanlayıcı'yı devre dışı bırak](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Kural oluşturma
1. Tıklatın **Yeni Kural Ekle**.
2. Üzerinde **açıklama** sayfasında, aşağıdakileri girin:  
   ![Filtreleme kuralını gelen](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * **Ad**: kuralına açıklayıcı bir ad verin.
   * **Açıklama**: birisi kural içindir anlamaları için bazı açıklama verin.
   * **Sisteme bağlı**: Bu, nesne bulunabilir sistemidir. Bu durumda, seçin **Active Directory Bağlayıcısı**.
   * **Sistem/meta veri deposu nesne türü bağlı**: seçin **kullanıcı** ve **kişi**sırasıyla.
   * **Bağlantı türü**: Bu değer ile değiştirmek **katılma**.
   * **Öncelik**: sistemdeki benzersiz olan bir değer sağlayın. Daha yüksek önceliği daha düşük bir sayısal değer belirtir.
   * **Etiket**: Bu alanı boş bırakın. Yalnızca out-of-box kuralları Microsoft'tan bir değerle doldurulmuş bu kutu olması gerekir.
3. Üzerinde **Scoping filtre** want **givenName ISNOTNULL**.  
   ![Gelen kuralı kapsamı filtresi](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Bu bölümde, hangi nesnelerin kuralın geçerli olması gereken tanımlamak için kullanılır. Boş bırakılırsa kuralı tüm kullanıcı nesneleri için geçerli olur. Ancak, konferans odaları, hizmet hesapları ve diğer kişiler olmayan kullanıcı nesneleri içerir.
4. Üzerinde **katılma kuralları** sayfasında, bu alanı boş bırakın.
5. Üzerinde **dönüşümleri** sayfasında, değişiklik **FlowType** için **ifade**. İçin **Target özniteliği**seçin **givenName**. Ve **kaynak**, girin **PCase([givenName])**.
   ![Gelen kuralı dönüşümleri](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Eşitleme altyapısı işlev adı ve öznitelik adı için duyarlıdır. Bir sorun yazarsanız, kural eklediğinizde, bir uyarı görürsünüz. Kaydedin ve devam, ancak yeniden açın ve hatayı düzeltmek gerekir.
6. Tıklatın **Ekle** kuralını kaydetmek için.

Yeni özel kuralın sistemdeki diğer eşitleme kuralları görünür olması gerekir.

### <a name="verify-the-change"></a>Değişikliği doğrulayın
Bu yeni değişiklikle hataları atma değil ve beklendiği gibi çalıştığından emin olmak istiyorsunuz. Sahip olduğunuz nesneleri sayısına bağlı olarak, bu adımı gerçekleştirmenin iki yolu vardır:

- Tüm nesneler üzerinde tam bir eşitleme çalıştırın.
- Önizleme ve tam eşitleme tek bir nesne üzerinde çalıştırın.

Açık **eşitleme hizmeti** gelen **Başlat** menüsü. Bu bölümde bu araç tüm adımlardır.

**Tüm nesneler üzerinde tam eşitleme**  

   1. Seçin **Bağlayıcılar** üstünde. (Bu durumda, Active Directory etki alanı Hizmetleri) önceki bölümdeki değiştirdiğiniz bağlayıcıyı tanımlamak ve seçin. 
   2. İçin **Eylemler**seçin **çalıştırmak**.
   3. Seçin **tam eşitleme**ve ardından **Tamam**.
   ![Tam eşitleme](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Nesneler meta veri deposunda şimdi güncelleştirilir. Meta veri nesnesinde bakarak değişikliklerinizi doğrulayın.

**Önizleme ve tek bir nesne üzerinde tam eşitleme**  

   1. Seçin **Bağlayıcılar** üstünde. (Bu durumda, Active Directory etki alanı Hizmetleri) önceki bölümdeki değiştirdiğiniz bağlayıcıyı tanımlamak ve seçin.
   2. Seçin **bağlayıcı alanı arama**. 
   3. Kullanmak **kapsam** değişiklik test etmek için kullanmak istediğiniz bir nesne bulunamadı. Nesne seçin ve tıklatın **Önizleme**. 
   4. Yeni ekranda seçin **Commit Önizleme**.  
   ![Önizleme Yürüt](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Değişiklik için meta veri deposu artık kararlıdır.

**Meta veri deposunda nesneyi görüntüleme**  

1. Değer beklenen davranıştır ve kuralın uygulanacağı emin olmak için birkaç örnek nesneleri seçin. 
2. Seçin **meta veri deposu arama** üstten. İlgili nesneleri bulmak için gereken herhangi bir filtre ekleyin. 
3. Arama sonuçlarından nesneyi açın. Ara öznitelik değerleri ve ayrıca doğrulayın **eşitleme kuralları** beklendiği gibi kuralın uygulandığı sütun.  
![Meta veri deposu arama](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Zamanlayıcısını Etkinleştir
Her şeyin beklendiği gibi ise, Zamanlayıcı yeniden etkinleştirebilirsiniz. PowerShell çalıştırmak `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Diğer ortak öznitelik akışı değişiklikleri
Önceki bölümde bir öznitelik akışı değişiklik açıklanmıştır. Bu bölümde, bazı ek örnekler verilmiştir. Eşitleme kuralı oluşturmak adımlar kısaltılmış, ancak tam adımlar önceki bölümde bulabilirsiniz.

### <a name="use-an-attribute-other-than-the-default"></a>Özniteliğin varsayılan dışındaki kullanın
Bu Fabrikam senaryoda yerel alfabe verilen ad, Soyadı ve görünen ad için kullanıldığı bir orman yoktur. Bu öznitelikler Latin karakterlerini gösterimini uzantı öznitelikleri bulunabilir. Genel bir adres oluşturmak için Azure AD içinde listelemek ve Office 365, kuruluşun bu öznitelikler yerine kullanmak istiyor.

Varsayılan yapılandırma ile yerel orman nesneden şöyle görünür:  
![Öznitelik akışı 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Diğer öznitelik akışları ile bir kural oluşturmak için aşağıdakileri yapın:

1. Açık **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
2. İle **gelen** sola seçiliyken, tıklatın **Yeni Kural Ekle** düğmesi.
3. Kural, bir ad ve açıklama verin. Şirket içi Active Directory örneğiyle ve ilgili nesne türlerini seçin. İçinde **bağlantı türü**seçin **katılma**. İçin **öncelik**, başka bir kural tarafından kullanılmayan bir sayı seçin. 50 değeri bu örnekte, böylece out-of-box kuralları 100 ile başlatın.
  ![Öznitelik akışı 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
4. Bırakın **Scoping filtre** boş. (Diğer bir deyişle, ormandaki tüm kullanıcı nesneleri geçerli.)
5. Bırakın **katılma kuralları** boş. (Diğer bir deyişle, birleşimlerin işlemek out-of-box kural sağlar.)
6. İçinde **dönüşümleri**, aşağıdaki akışları oluşturun:  
  ![Öznitelik akışı 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
7. Tıklatın **Ekle** kuralını kaydetmek için.
8. Git **Eşitleme Hizmeti Yöneticisi'ni**. Üzerinde **Bağlayıcılar**, kural eklediğiniz Bağlayıcısı'nı seçin. Seçin **çalıştırmak**ve ardından **tam eşitleme**. Tam eşitleme, geçerli kurallarını kullanarak tüm nesneleri yeniden hesaplar.

Bu bu özel kuralın ile aynı nesne için kaynaklanır:  
![Öznitelik akışı 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Öznitelikleri uzunluğu
Dize öznitelikleri varsayılan olarak türünden dizin ve uzunluk en fazla 448 karakter olmalıdır. Daha fazla içerebilir dizesi öznitelikleri ile çalışıyorsanız, aşağıdaki öznitelik akışı eklediğinizden emin olun:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>UserPrincipalSuffix değiştirme
UserPrincipalName özniteliği Active Directory'de her zaman kullanıcılar tarafından bilinmiyor ve oturum açma kimliği olarak uygun olmayabilir. Azure AD Connect eşitleme Yükleme Sihirbazı'yla farklı bir öznitelik--Örneğin, seçebileceğiniz *posta*. Ancak bazı durumlarda, öznitelik hesaplanması gerekir.

Örneğin, Contoso şirket, iki Azure AD dizini, bir üretim için ve test etmek için bir sahiptir. Oturum açma kimliği başka bir sonek kullanmak üzere kendi test Kiracı kullanıcılar istedikleri:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

Bu ifadede her şeyi ele ilk sol @-sign (Word) ve sabit bir dize ile Birleştir.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Birden çok değerli özniteliği tek değerine dönüştürür
Active Directory Kullanıcıları ve Bilgisayarları'nda tek değerli göründükleri olsa bile Active Directory'de bazı öznitelikler şema, birden çok değerli. Açıklama özniteliği buna bir örnektir:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Öznitelik değeri varsa, Bu deyimde ilk öğe alın (*öğesi*) özniteliğinde öndeki ve sondaki boşlukları Kaldır (*kırpma*) ve ardından ilk 448 karakter tutun (*sol* ) dizesi içinde.

### <a name="do-not-flow-an-attribute"></a>Bir öznitelik akışı değil
Bu bölümde senaryo arka planda için bkz: [öznitelik akışı işlemini denetleyen](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Bir öznitelik akışı olmayan iki yolu vardır. İlk Yükleme Sihirbazı'na kullanmaktır [seçili öznitelikleri kaldırmak](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Bu seçenek, hiçbir zaman önce öznitelik eşitlenmiş durumunda çalışır. Ancak, bu öznitelik eşitlemek ve daha sonra bu özellik ile kaldırmak başlattıysanız, öznitelik ve var olan değerleri yönetme eşitleme altyapısı durakları Azure AD'de bırakılır.

Özniteliğin değerini kaldırın ve gelecekte akmaz emin olmak istiyorsanız, özel bir kural oluşturmak.

Bu Fabrikam senaryoda biz bazı biz buluta eşitleme öznitelikleri var olmamalıdır, gerçekleştirilmiş. Bu öznitelikler Azure AD'den kaldırılır emin olmak istiyoruz.  
![Hatalı uzantı öznitelikleri](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

1. Yeni bir gelen eşitleme kuralını oluşturmak ve bir açıklama doldurabilirsiniz.
  ![Açıklamaları](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
2. Öznitelik akışları ile oluşturma **ifade** için **FlowType** ile **AuthoritativeNull** için **kaynak**. Sabit **AuthoritativeNull** değerini doldurmak bir alt öncelik eşitleme kuralı çalıştığında olsa bile değerin meta veri deposunda boş olması gerektiğini gösterir.
  ![Uzantı öznitelikleri dönüşümü](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
3. Eşitleme kuralı kaydedin. Başlat **eşitleme hizmeti**, bağlayıcı bulmak, seçmek **çalıştırmak**ve ardından **tam eşitleme**. Bu adım, tüm öznitelik akışları yeniden hesaplar.
4. Hedeflenen değişiklikleri bağlayıcı alanı arama yaparak dışarı aktarılacak olduğunu doğrulayın.
  ![Hazırlanan Sil](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>PowerShell ile kuralları oluşturma
Yalnızca yapmak için birkaç değişiklik olduğunda eşitleme kuralı Düzenleyicisi'ni kullanarak düzgün çalışır. Birçok değişiklik yapmanız gerekirse, PowerShell daha iyi bir seçenek olabilir. Gelişmiş özellikler yalnızca PowerShell ile kullanılabilir.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>PowerShell komut dosyası için bir out-of-box kuralını Al
Bir out-of-box kuralı oluşturulan komut dosyası, eşitleme kuralları Düzenleyicisi'nde kuralı seçin ve tıklatın PowerShell görmek için **verme**. Bu eylem, kural oluşturulan PowerShell komut dosyası sağlar.

### <a name="advanced-precedence"></a>Gelişmiş önceliği
Out-of-box eşitleme kuralları öncelik değeri 100 ile başlatın. Daha sonra birçok ormanlar varsa ve birçok özel değişiklikler yapmanız gerekir, 99 eşitleme kuralları yeterli olmayabilir.

Out-of-box kurallardan önce eklenen ek kurallar istediğiniz eşitleme altyapısı söyleyebilirsiniz. Bu davranışı elde etmek üzere aşağıdaki adımları izleyin:

1. İlk out-of-box eşitleme kuralı işaretlemek (**içinde AD kullanıcı katılma gelen**) eşitleme kuralları Düzenleyicisi'ni seçip **verme**. SR tanımlayıcı değeri kopyalayın.  
![PowerShell değişikliği öncesinde](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Yeni eşitleme kuralı oluşturun. Eşitleme kuralları Düzenleyicisi'ni oluşturmak için kullanabilirsiniz. Kural için bir PowerShell betiğini dışarı aktarın.
3. Özelliğindeki **PrecedenceBefore**, out-of-box kuraldan tanımlayıcı değeri ekleyin. Ayarlama **öncelik** için **0**. Tanımlayıcı özniteliği benzersiz ve başka bir kural adresinden bir GUID yeniden değil olduğundan emin olun. Da emin olmanız **ImmutableTag** özelliği ayarlı değil. Bu özellik yalnızca bir out-of-box kuralı için ayarlamanız gerekir.
4. PowerShell komut dosyasını kaydedin ve çalıştırın. Özel kural öncelik değeri 100 atanır ve diğer tüm out-of-box kurallar artırılır sonucudur.  
![Değişiklikten sonra PowerShell](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Aynı kullanarak birçok özel eşitleme kuralları olabilir **PrecedenceBefore** değer gerektiğinde.

## <a name="enable-synchronization-of-usertype"></a>UserType eşitlemeyi etkinleştir
Azure AD Connect eşitleme destekleyen **UserType** için öznitelik **kullanıcı** sürüm 1.1.524.0 nesneleri ve daha sonra. Daha açık belirtmek gerekirse, aşağıdaki değişiklikleri sunulmuştur:

- Nesne türü şeması **kullanıcı** Azure AD bağlayıcısında dize türünde ve tek değerli UserType özniteliği kapsayacak şekilde genişletilir.
- Nesne türü şeması **kişi** meta veri deposunda dize türünde ve tek değerli UserType özniteliği kapsayacak şekilde genişletilir.

Şirket içi Active Directory içinde karşılık gelen hiçbir UserType özniteliği olduğundan varsayılan olarak, eşitleme için temel alınan UserType özniteliği etkin değil. Eşitleme el ile etkinleştirmeniz gerekir. Bunu yapmadan önce Azure AD tarafından zorlanan aşağıdaki davranış Not yapmanız gerekir:

- Azure AD yalnızca temel alınan UserType özniteliği için iki değeri kabul eder: **üye** ve **Konuk**.
- Temel alınan UserType özniteliği Azure AD Connect eşitleme için etkin değilse, dizin eşitleme ile oluşturulan Azure AD kullanıcıların ayarlamak temel alınan UserType özniteliği olurdu **üye**.
- Azure AD temel alınan UserType özniteliği var olan Azure AD kullanıcıları Azure AD Connect tarafından değiştirilmesine izin vermez. Yalnızca Azure AD kullanıcıları oluşturma sırasında ayarlanabilir.

Temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce ilk öznitelik şirket içi Active Directory'den nasıl türetilmiş karar vermeniz gerekir. En yaygın yaklaşımlar şunlardır:

- Bir kullanılmayan atayın (örneğin, extensionAttribute1) AD özniteliği kaynak özniteliği olarak kullanılacak şirket. Belirtilen şirket içi AD özniteliği, türü olmalıdır **dize**tek değerli ve değeri içeren **üye** veya **Konuk**. 

    Bu yaklaşım seçerseniz, belirtilen öznitelik UserType özniteliği eşitlenmesi etkinleştirmeden önce Azure AD'ye eşitlenen tüm var olan kullanıcı şirket içi Active Directory içindeki nesneleri için geçerli bir değer ile doldurulur sağlamanız gerekir. .

- Alternatif olarak, diğer özelliklerinden UserType özniteliği için değer türetilemeyeceğini. Örneğin, tüm kullanıcılar olarak eşitlemek için istediğiniz **Konuk** , şirket içi AD userPrincipalName özniteliğinin etki alanı bölümü ile biten  *@partners.fabrikam123.org* . 

    Daha önce belirtildiği gibi Azure AD Connect UserType özniteliği var olan Azure AD kullanıcıları Azure AD Connect tarafından değiştirilmesine izin vermez. Bu nedenle, nasıl UserType özniteliği zaten kiracınızda tüm mevcut Azure AD kullanıcıları için yapılandırılmış ile tutarlıdır karar verdiniz mantığı emin olmalısınız.

Temel alınan UserType özniteliği ile eşitlenmesine olanak tanımak için adımları olarak özetlenebilir:

1.  Eşitleme Zamanlayıcı'yı devre dışı bırakın ve devam eden bir eşitleme doğrulayın.
2.  Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema.
3.  Azure AD Bağlayıcısı şemaya UserType ekleyin.
4.  Şirket içi Active Directory'den öznitelik değeri akış için bir gelen eşitleme kuralı oluşturun.
5.  Azure AD öznitelik değerini akış için giden eşitleme kuralı oluşturun.
6.  Bir tam eşitleme döngüsü çalıştırın.
7.  Eşitleme Zamanlayıcı'yı etkinleştirin.

>[!NOTE]
> Bu bölümde kalan adımları kapsar. Özel eşitleme kuralları olmadan tek orman topolojisi ile bir Azure AD dağıtımı bağlamında açıklanmıştır. Çoklu orman topolojisini varsa, özel eşitleme kuralları yapılandırılmış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız gerekir.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden bir eşitleme doğrulayın
İstenmeyen değişiklikleri için Azure AD dışarı aktarma önlemek için eşitleme kuralları güncelleştiriliyor ortasında durumdayken eşitleme yer alan emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

 1. Bir PowerShell oturumunda Azure AD Connect bir sunucuda başlatın.
 2. Cmdlet'ini çalıştırarak zamanlanmış eşitleme devre dışı `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Giderek Eşitleme Hizmeti Yöneticisi'ni açmak **Başlat** > **eşitleme hizmeti**.
 4. Git **Operations** sekmesinde ve durumuna sahip bir işlem yok onaylayın *devam eden*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>2. adım: şirket içi kaynak özniteliği ekleme AD Bağlayıcısı şeması
Tüm Azure AD öznitelikleri şirket alınır AD bağlayıcı alanı. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

 1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
 2. Sağ şirket içi AD Bağlayıcısı ve select **özellikleri**.
 3. Açılan iletişim kutusunda Git **öznitelikleri Seç** sekmesi.
 4. Kaynak öznitelikte öznitelik listesinde işaretli olduğundan emin olun.
 5. Tıklatın **Tamam** kaydetmek için.
![Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şeması](./media/active-directory-aadconnectsync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>3. adım: Azure AD Bağlayıcısı şemaya UserType ekleme
Varsayılan olarak, temel alınan UserType özniteliği Azure AD Connect alanına içe aktarılmaz. Temel alınan UserType özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

 1. Git **Bağlayıcılar** sekme Eşitleme Hizmeti Yöneticisi'nde.
 2. Sağ **Azure AD Bağlayıcısı** seçip **özellikleri**.
 3. Açılan iletişim kutusunda Git **öznitelikleri Seç** sekmesi.
 4. PreferredDataLocation özniteliği öznitelik listesinde işaretli olduğundan emin olun.
 5. Tıklatın **Tamam** kaydetmek için.

![Azure AD Bağlayıcısı şemaya kaynak öznitelik Ekle](./media/active-directory-aadconnectsync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>4. adım: şirket içi Active Directory'den öznitelik değeri akışı için bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı öznitelik değerini meta veri deposu için şirket içi Active Directory'den kaynak özniteliğinden akış verir:

1. Eşitleme kuralları Düzenleyicisi'ni giderek açmak **Başlat** > **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.
3. Tıklatın **Yeni Kural Ekle** düğmesi yeni gelen kuralı oluşturun.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *içinde AD'den – kullanıcı UserType* |
    | Açıklama | *Bir açıklama belirtin* |  |
    | Bağlı sistem | *Şirket içi çekme AD Bağlayıcısı* |  |
    | Bağlı sistem nesne türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Person** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesinde ve ekleme bir **tek bir kapsam filtresi grubunu** aşağıdaki yan tümcesi ile:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Kullanıcı\_ |

    Kapsam filtresi, hangi şirket içi AD bu gelen eşitleme kuralı uygulanmadan nesneleri belirler. Bu örnekte kullanılan aynı kapsam filtresi kullanırız *içinde AD'den – kullanıcı ortak* Azure AD kullanıcı oluşturulan kullanıcı nesnelerine uygulanan eşitleme kuralı engeller out-of-box eşitleme kuralı geri yazma özelliği. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.

6. Git **dönüştürme** sekmesinde ve istenen dönüştürme kuralı uygulayın. Örneğin, bir kullanılmayan belirlediyseniz şirket içi AD özniteliği (örneğin, extensionAttribute1) UserType kaynak özniteliği doğrudan öznitelik akışı uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | UserType | extensionAttribute1 | İşaretli | Güncelleştirme |

    Başka bir örnekte, temel alınan UserType özniteliği değeri diğer özelliklerinden türetilen istiyorsunuz. Örneğin, tüm kullanıcılar, konuk olarak eşitlemek için istediğiniz şirket içi AD userPrincipalName özniteliğinin etki alanı bölümü ile biten  *@partners.fabrikam123.org* . Böyle bir ifade uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | UserType | IIf(IsPresent([userPrincipalName]),IIf(CBool(InStr(LCase([userPrincipalName]),"@partners.fabrikam123.org")=0), "Üye", "Konuk"), hata ("UserPrincipalName UserType belirlemek için mevcut değil")) | İşaretli | Güncelleştirme |

7. Tıklatın **Ekle** gelen kuralı oluşturmak için.

![Gelen eşitleme kuralı oluşturma](./media/active-directory-aadconnectsync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>5. adım: Azure AD öznitelik değerini akışı için bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı öznitelik değerini meta veri deposu Azure AD'de PreferredDataLocation öznitelik akış verir:

1. Eşitleme kuralları Düzenleyici'ye gidin.
2. Arama filtresi ayarlamak **yönü** olmasını **giden**.
3. Tıklatın **Yeni Kural Ekle** düğmesi.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırma sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *Out AAD – kullanıcı UserType* |
    | Açıklama | *Bir açıklama belirtin* ||
    | Bağlı sistem | *AAD bağlayıcı seçin* ||
    | Bağlı sistem nesne türü | **Kullanıcı** ||
    | Meta veri deposu nesne türü | **Person** ||
    | Bağlantı türü | **Birleştir** ||
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekmesinde ve ekleme bir **tek bir kapsam filtresi grubunu** iki yan tümceleri içeren:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam filtresi, bu giden eşitleme kuralı uygulanmadan olduğu Azure AD nesneleri belirler. Bu örnekte aynı kapsam filtresinden kullanırız *AD – kullanıcı kimliği için çıkış* out-of-box eşitleme kuralı. Eşitleme kuralı şirket içi Active Directory'den eşitlenmez kullanıcı nesnelerine uygulanan engeller. Azure AD Connect dağıtımınızı göre kapsam filtresi ince ayar gerekebilir.

6. Git **dönüştürme** sekmesinde ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Hedef öznitelik | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | UserType | UserType | İşaretli | Güncelleştirme |

7. Tıklatın **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı oluştur](./media/active-directory-aadconnectsync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>6. adım: bir tam eşitleme döngüsü çalıştırın
Genel olarak, biz yeni öznitelikler için Active Directory ve Azure AD Bağlayıcısı şemaları eklendi ve özel eşitleme kuralları sunulan çünkü bir tam eşitleme döngüsü gereklidir. Azure AD dışarı aktarmadan önce değişiklikleri doğrulamak istiyorsunuz. 

Bir tam eşitleme döngüsü yapmak adımları el ile çalışırken değişiklikleri doğrulamak için aşağıdaki adımları kullanın.

1. Çalıştıran bir **tam alma** üzerinde **şirket içi AD Bağlayıcısı**:

   1. Git **Operations** sekme Eşitleme Hizmeti Yöneticisi'nde.
   2. Sağ **şirket içi AD Bağlayıcısı** seçip **çalıştırmak**.
   3. Açılan iletişim kutusunda **tam içeri aktarma** ve ardından **Tamam**.
   4. İşlemin tamamlanmasını bekleyin.

    > [!NOTE]
    > Tam içeri aktarma atlayabilirsiniz şirket içi AD kaynak özniteliği zaten listesinde yer alıyorsa Bağlayıcısı öznitelikleri alındı. Diğer bir deyişle, değişiklikleri sırasında yapmak zorunda kalmadığımıza [2. adım: şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Çalıştıran bir **tam alma** üzerinde **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **çalıştırmak**.
   2. Açılan iletişim kutusunda **tam içeri aktarma** ve ardından **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

3. Eşitleme kuralı değişiklikleri var olan bir kullanıcı nesnesi üzerinde doğrulayın:

    Kaynak özniteliği Active Directory ve Azure AD'den UserType Connector'ın ilgili alanları içeri aktarıldığını şirket içi. Tam eşitleme işlemine devam etmeden önce yapmak bir **Önizleme** üzerinde var olan bir kullanıcı nesnesi şirket içi AD bağlayıcı alanı. Seçtiğiniz nesne doldurulmuş kaynak özniteliği olmalıdır.
    
    Başarılı bir **Önizleme** eşitleme yapılandırdığınız iyi bir gösterge kuralları doğru meta veri deposunda doldurulmuş UserType olduğu. Nasıl yapılacağı hakkında bilgi için bir **Önizleme**, bölümüne bakın [değişikliği doğrulayın](#verify-the-change).

4. Çalıştıran bir **tam eşitleme** üzerinde **şirket içi AD Bağlayıcısı**:

   1. Sağ **şirket içi AD Bağlayıcısı** seçip **çalıştırmak**.
   2. Açılan iletişim kutusunda **tam eşitleme** ve ardından **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

5. Doğrulama **bekleyen dışarı aktarmaların** Azure ad:

   1. Sağ **Azure AD Bağlayıcısı** seçip **arama bağlayıcı alanı**.
   2. İçinde **arama bağlayıcı alanı** açılan iletişim kutusunda:

      - Ayarlama **kapsam** için **dışa aktarma bekleyen**.
      - Üç onay kutusunu seçin: **Ekle**, **Değiştir**, ve **silmek**.
      - Tıklatın **arama** dışarı değişikliklerle nesneleri listesini almak için düğmesi. Belirli nesne değişiklikleri incelemek için nesne çift tıklayın.
      - Değişiklikleri beklenir doğrulayın.

6. Çalıştırma **verme** üzerinde **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **çalıştırmak**.
   2. İçinde **çalıştırmak bağlayıcı** açılan iletişim kutusunda **verme** ve ardından **Tamam**.
   3. Dışarı bitirmek için Azure ad bekleyin.

> [!NOTE]
> Bu adımları değil tam eşitleme ve Azure AD Bağlayıcısı üzerinde adımları dışa aktarma. Öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akmaktadır çünkü bu adımları gerekli değildir.

### <a name="step-7-re-enable-the-sync-scheduler"></a>7. adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. Bir PowerShell oturumu başlatın.
2. Zamanlanan eşitleme cmdlet'i çalıştırarak yeniden etkinleştirme `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* İfade dili hakkında daha fazla bilgiyi [anlama bildirim temelli hazırlama ifadelerini](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
