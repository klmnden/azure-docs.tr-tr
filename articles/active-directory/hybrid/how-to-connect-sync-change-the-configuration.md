---
title: 'Azure AD Connect eşitleme: Bir yapılandırma değişikliği Azure AD Connect eşitleme yaptığınızda | Microsoft Docs'
description: Azure AD Connect eşitleme yapılandırmasında bir değişiklik yapmak nasıl gösterilmektedir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31fe3877fd6098b18686b9d99a012cbfbef7c300
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60244011"
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect eşitleme: Bir varsayılan yapılandırmaya bir değişiklik yapın
Bu makalenin amacı, Azure Active Directory (Azure AD) Connect eşitleme Varsayılan yapılandırmada değişiklik yapmak nasıl rehberlik etmektir. Bu, bazı yaygın senaryolar için adımları sağlar. Bu bilgiyle, kendi iş kurallarına göre kendi yapılandırmasına basit değişiklikler yapabilir olmalıdır.

> [!WARNING]
> Varsayılan eşitleme kuralları için değişiklik yaparsanız bu değişiklikler Azure AD Connect, eşitleme beklenmeyen ve olası istenmeyen sonuçları elde edilen sonraki güncelleştirildiğinde yazılır.
>
> Kullanıma hazır eşitleme kuralları bir parmak izi var. Bu kural için bir değişiklik yaparsanız, parmak izini artık eşleşen. Gelecekte yeni Azure AD Connect sürümü uygulamaya çalıştığınızda sorunlar yaşayabilirsiniz. Yalnızca bu anlatılan şekilde bu makalede değişiklik.

## <a name="synchronization-rules-editor"></a>Eşitleme kuralları Düzenleyicisi
Eşitleme kuralları Düzenleyicisi görmek ve varsayılan yapılandırmasını değiştirmek için kullanılır. Üzerinde bulabilirsiniz **Başlat** menüsünün altında **Azure AD Connect** grubu.  
![Eşitleme kuralı Düzenleyicisi ile Başlat menüsü](./media/how-to-connect-sync-change-the-configuration/startmenu2.png)

Düzenleyici açıldığında, varsayılan kullanıma hazır kuralları bakın.

![Eşitleme kuralı Düzenleyicisi](./media/how-to-connect-sync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Düzenleyici içinde gezinme
Düzenleyicisinin en üstünde açılan listeler kullanarak belirli bir kural hızlıca bulabilirsiniz. Öznitelik proxyAddresses dahil olduğu kuralları görmek istiyorsanız, örneğin, açılır listeleri aşağıdaki değiştirebilirsiniz:  
![SRE filtreleme](./media/how-to-connect-sync-change-the-configuration/filtering.png)  
Filtreleme sıfırlamak ve yeni yapılandırmayı klavyede F5 tuşuna basın.

Sağ üst köşedeki olan **Yeni Kural Ekle** düğmesi. Kendi özel bir kural oluşturmak için bu düğmeyi kullanın.

Altta bir seçili eşitleme kuralının almalarını için düğmeler bulunur. **Düzen** ve **Sil** kendisine beklediğiniz şeyi. **Dışarı aktarma** yeniden eşitleme kuralı oluşturmak için bir PowerShell Betiği oluşturur. Bu yordam ile bir eşitleme kuralı bir sunucudan diğerine taşıyabilirsiniz.

## <a name="create-your-first-custom-rule"></a>İlk uygulamanızı özel bir kural oluşturun
Öznitelik akışları için en yaygın değişiklikler. Veri kaynak dizininizde Azure AD ile aynı olmayabilir. Bu bölümdeki örnekte, bir kullanıcının verilen adı her zaman içinde olduğundan emin olun *harfe*.

### <a name="disable-the-scheduler"></a>Zamanlayıcı'yı devre dışı bırak
[Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) varsayılan olarak 30 dakikada bir çalışır. Değişiklikler yapmak ve yeni kurallarınızı sorun giderme sırasında başlatıyor değil emin olun. Zamanlayıcı geçici olarak devre dışı bırakmak, PowerShell'i başlatın ve çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Zamanlayıcı'yı devre dışı bırak](./media/how-to-connect-sync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Kural oluşturma
1. Tıklayın **Yeni Kural Ekle**.
2. Üzerinde **açıklama** sayfasında, aşağıdakileri girin:  
   ![Filtreleme kuralını gelen](./media/how-to-connect-sync-change-the-configuration/description2.png)  
   * **Ad**: Kural, açıklayıcı bir ad verin.
   * **Açıklama**: Başka birisi kural ne olduğunu anlayabilmeniz için bazı açıklama verin.
   * **Sistem bağlı**: Bu, nesnenin bulunabilir sistemidir. Bu örnekte **Active Directory Bağlayıcısı**.
   * **Sistem/meta veri deposu nesne türü bağlı**: Seçin **kullanıcı** ve **kişi**sırasıyla.
   * **Bağlantı türünü**: Bu değeri değiştirmek **katılın**.
   * **Öncelik**: Sisteminde benzersiz bir değer sağlayın. Daha yüksek önceliği daha düşük bir sayısal değer belirtir.
   * **Etiket**: Bu alanı boş bırakın. Yalnızca Microsoft gelen kuralları kullanıma hazır bir değerle doldurulur bu kutuyu olması gerekir.
3. Üzerinde **Scoping filtre** want **givenName ISNOTNULL**.  
   ![Gelen kapsam belirleme filtresi kuralı](./media/how-to-connect-sync-change-the-configuration/scopingfilter.png)  
   Bu bölümde, hangi nesnelerin kuralın geçerli olacağı tanımlamak için kullanılır. Kural, boş bırakılması durumunda tüm kullanıcı nesnelerinin geçerli. Ancak, Konferans salonu, hizmet hesapları ve diğer kişiler olmayan kullanıcı nesnelerini içerir.
4. Üzerinde **birleştirme kuralları** sayfasında, bu alanı boş bırakın.
5. Üzerinde **dönüşümleri** sayfasında, değişiklik **FlowType** için **ifade**. İçin **Target özniteliği**seçin **givenName**. Ve **kaynak**, girin **PCase([givenName])** .
   ![Gelen kuralı dönüşümleri](./media/how-to-connect-sync-change-the-configuration/transformations.png)  
   Eşitleme altyapısı, işlev adı ve öznitelik adı için duyarlıdır. Bir sorun yazarsanız, kural eklediğinizde, bir uyarı görürsünüz. Kaydet ve devam et, ancak yeniden açın ve hatayı düzeltmek için ihtiyaç duyduğunuz.
6. Tıklayın **Ekle** kuralını kaydetmek için.

Yeni bir özel kural sistemdeki diğer eşitleme kuralları görünür olmalıdır.

### <a name="verify-the-change"></a>Değişikliği doğrulayın
Bu yeni değişiklik, beklendiği gibi çalıştığından ve hataları oluşturmama emin olmak istersiniz. Sahip nesne sayısına bağlı olarak, bu adımı gerçekleştirmenin iki yolu vardır:

- Tüm nesneler üzerinde tam bir eşitleme çalıştırın.
- Önizleme ve tam eşitleme, tek bir nesne üzerinde çalıştırın.

Açık **eşitleme hizmeti** gelen **Başlat** menüsü. Bu bölümde bu araç tüm adımlardır.

**Tüm nesneler üzerinde tam eşitleme**  

   1. Seçin **Bağlayıcılar** en üstünde. (Bu durumda, Active Directory etki alanı Hizmetleri) bir önceki bölümdeki değiştirdiğiniz bağlayıcıyı tanımlamak ve seçin. 
   2. İçin **eylemleri**seçin **çalıştırma**.
   3. Seçin **tam eşitleme**ve ardından **Tamam**.
   ![Tam eşitleme](./media/how-to-connect-sync-change-the-configuration/fullsync.png)  
   Nesneler, artık meta veri deposunda güncelleştirilir. Meta veri deposu nesne bakarak değişikliklerinizi doğrulayın.

**Önizleme ve tek bir nesne üzerinde tam eşitleme**  

   1. Seçin **Bağlayıcılar** en üstünde. (Bu durumda, Active Directory etki alanı Hizmetleri) bir önceki bölümdeki değiştirdiğiniz bağlayıcıyı tanımlamak ve seçin.
   2. Seçin **bağlayıcı alanı arama**. 
   3. Kullanma **kapsam** değişikliğini test etmek için kullanmak istediğiniz bir nesne bulunamadı. Nesneyi seçin ve tıklayın **Önizleme**. 
   4. Yeni ekranda seçin **işleme Önizleme**.  
   ![Önizleme işleme](./media/how-to-connect-sync-change-the-configuration/commitpreview.png)  
   Değişiklik için meta veri deposu şimdi kararlıdır.

**Meta veri deposunda nesneyi görüntüle**  

1. Değeri beklenir ve kuralın uygulanmasını emin olmak için birkaç örnek nesneleri seçin. 
2. Seçin **meta veri deposu arama** üst. İlgili nesneleri bulmak için gereken herhangi bir filtre ekleyin. 
3. Arama sonuçlarından bir nesneyi açın. Öznitelik değerlerinde arayın ve ayrıca doğrulayın **eşitleme kuralları** kural beklendiği gibi geçerli bir sütun.  
![Meta veri deposu arama](./media/how-to-connect-sync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Zamanlayıcı'yı etkinleştir
Her şeyin beklendiği gibi ise, Zamanlayıcı'yı yeniden etkinleştirebilirsiniz. PowerShell üzerinden çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Diğer genel öznitelik akışı değişiklikleri
Önceki bölümde bir öznitelik akışı için değişiklik yapmak nasıl kaydedileceği açıklanır. Bu bölümde, bazı ek örnekler sağlanır. Eşitleme kuralı oluşturmak için adımları kısaltılmış, ancak tüm adımlar önceki bölümde bulabilirsiniz.

### <a name="use-an-attribute-other-than-the-default"></a>Varsayılan dışındaki bir özniteliği kullanın
Fabrikam Bu senaryoda, yerel alfabetik verilen ad, Soyadı ve görünen ad için kullanıldığı bir orman yoktur. Bu öznitelikler Latin karakter gösterimi uzantı öznitelikleri bulunabilir. Genel bir adres oluşturmak için Azure AD'de listelemek ve Office 365, kuruluşun bu öznitelikler yerine kullanmak istiyor.

Varsayılan yapılandırma ile yerel ormanında bir nesneden şöyle görünür:  
![Öznitelik akışı 1](./media/how-to-connect-sync-change-the-configuration/attributeflowjp1.png)

Diğer öznitelik akışları ile bir kural oluşturmak için aşağıdakileri yapın:

1. Açık **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
2. İle **gelen** sol tarafa seçiliyken, tıklayın **Yeni Kural Ekle** düğmesi.
3. Kural, bir ad ve açıklama ekleyin. Şirket içi Active Directory örneğine ve ilgili nesne türlerini seçin. İçinde **bağlantı türü**seçin **katılın**. İçin **öncelik**, başka bir kural tarafından kullanılmayan bir sayı seçin. Bu örnekte ' % s'değeri 50 kullanılabilmesi için kullanıma hazır kuralları 100 ile başlatın.
  ![Öznitelik akışı 2](./media/how-to-connect-sync-change-the-configuration/attributeflowjp2.png)
4. Bırakın **Scoping filtre** boş. (Diğer bir deyişle, bu ormandaki tüm kullanıcı nesnelerini uygulamalısınız.)
5. Bırakın **birleştirme kuralları** boş. (Diğer bir deyişle, kullanıma hazır kural birleşimlerin işlemek olanak tanır.)
6. İçinde **dönüşümleri**, aşağıdaki akışlar oluşturun:  
  ![Öznitelik akışı 3](./media/how-to-connect-sync-change-the-configuration/attributeflowjp3.png)
7. Tıklayın **Ekle** kuralını kaydetmek için.
8. Git **Eşitleme Hizmeti Yöneticisi**. Üzerinde **Bağlayıcılar**, kural eklediğiniz bağlayıcısını seçin. Seçin **çalıştırma**ve ardından **tam eşitleme**. Tam eşitleme, geçerli kurallarını kullanarak tüm nesneleri yeniden hesaplar.

Bu özel bu kuralla aynı nesne için sonucu.  
![Öznitelik akışı 4](./media/how-to-connect-sync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Öznitelikleri uzunluğu
Varsayılan olarak dizinlenebilir dize öznitelikleri ve uzunluğu en fazla 448 karakterdir. Daha içerebilecek dize öznitelikleri ile çalışıyorsanız, aşağıdaki öznitelik akışı eklediğinizden emin olun:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>UserPrincipalSuffix değiştirme
Active Directory'de userPrincipalName özniteliği, her zaman kullanıcılar tarafından bilinmiyor ve oturum açma kimliği olarak uygun olmayabilir Azure AD Connect eşitlemeyi Yükleme Sihirbazı'yla farklı bir öznitelik; Örneğin, seçebileceğiniz *posta*. Ancak bazı durumlarda, öznitelik hesaplanmalıdır.

Örneğin, Contoso şirketi, iki Azure AD dizini, bir üretim ve test etmek için bir vardır. Başka bir sonek oturum Kimliğinde kullanılacak test kiracısındaki kullanıcılar istedikleri:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

Bu ifadede her şey ele ilk sol @-sign (Word) ve sabit bir dize ile Birleştir.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Birden çok değerli öznitelik tek değere Dönüştür
Tek değerli Active Directory Kullanıcıları ve Bilgisayarları'nda Ara olsa bile bazı öznitelikler Active Directory Şeması'nda, birden çok değerli. Açıklama özniteliği bir örnek verilmiştir:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Özniteliğin bir değere sahipse, bu ifadede, ilk öğeyi ele (*öğesi*) özniteliği, baştaki ve sondaki boşlukları Kaldır (*Trim*) ve ardından ilk 448 karakter tutun (*sol* ) dizesi içinde.

### <a name="do-not-flow-an-attribute"></a>Bir öznitelik akışı değil
Bu bölümün senaryosunda arka plan bilgileri için bkz. [öznitelik akış işlemini denetleyen](concept-azure-ad-connect-sync-declarative-provisioning.md#control-the-attribute-flow-process).

Bir öznitelik akışı olmayan iki yolu vardır. İlk Yükleme Sihirbazı'na kullanmaktır [seçilen öznitelikler Kaldır](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering). Bu seçenek, hiçbir zaman önce öznitelik eşitlediyseniz çalışır. Ancak, bu öznitelik eşitlemek ve daha sonra bu özelliği kullanarak kaldırmak başlattıysanız, Azure AD'de öznitelik ve mevcut değerleri yönetme eşitleme altyapısı durdurur bırakılır.

Bir özniteliğin değerini kaldırın ve gelecekte geçmeyen emin olmak istiyorsanız, özel bir kural oluşturmanız gerekir.

Fabrikam Bu senaryoda, biz bazı biz buluta eşitleme öznitelikleri vardır gerektiğini gerçekleştirilmiş. Bu öznitelikler Azure AD'deki kaldırıldı emin olmak istiyoruz.  
![Hatalı bir uzantı öznitelikleri](./media/how-to-connect-sync-change-the-configuration/badextensionattribute.png)

1. Yeni bir gelen eşitleme kuralı oluşturmak ve açıklamayı doldurun.
  ![Açıklamaları](./media/how-to-connect-sync-change-the-configuration/syncruledescription.png)
2. Öznitelik akışları ile oluşturma **ifade** için **FlowType** ile **AuthoritativeNull** için **kaynak**. Değişmez değer **AuthoritativeNull** değerini doldurmak daha düşük öncelikli eşitleme kuralı çalıştığında bile değerin meta veri deposunda, boş olması gerektiğini gösterir.
  ![Uzantı öznitelikleri için dönüştürme](./media/how-to-connect-sync-change-the-configuration/syncruletransformations.png)
3. Eşitleme kuralı kaydedin. Başlangıç **eşitleme hizmeti**, bağlayıcıyı bulun, seçin **çalıştırma**ve ardından **tam eşitleme**. Bu adım, tüm öznitelik akışları yeniden hesaplar.
4. Hedeflenen değişiklikleri bağlayıcı alanında arama yaparak dışarı aktarılacak olduğunu doğrulayın.
  ![Hazırlanmış Sil](./media/how-to-connect-sync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>PowerShell ile kuralları oluşturma
Yalnızca birkaç değişiklik yapmak için varsa eşitleme kuralı Düzenleyicisi'ni kullanarak düzgün çalışır. Birçok değişiklik yapmanız gerekirse, PowerShell, daha iyi bir seçenek olabilir. Bazı gelişmiş özellikleri yalnızca PowerShell ile kullanılabilir.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>PowerShell Betiği için bir out-of-box kuralı alma
Kullanıma hazır kural oluşturulan komut dosyası, eşitleme kuralları Düzenleyicisi'nde kuralı seçin ve tıklayın PowerShell görmek için **dışarı**. Bu eylem, kural oluşturulmuş PowerShell komut dosyası sağlar.

### <a name="advanced-precedence"></a>Gelişmiş önceliği
Kullanıma hazır eşitleme kuralları öncelik değeri 100 ile başlayın. Ardından çok ormanınız ve birçok özel değişiklikler yapmanız, 99 eşitleme kuralları yeterli olmayabilir.

Kullanıma hazır kurallardan önce eklenen ek kurallar istediğiniz eşitleme altyapısı bildirebilirsiniz. Bu davranışı sağlamak için şu adımları izleyin:

1. İlk kutusu giden eşitleme kuralı işaretlemek (**içinde katılın AD kullanıcısı gelen**) eşitleme kuralları Düzenleyicisi'ni seçip **dışarı**. SR tanımlayıcı değerini kopyalayın.  
![PowerShell değişikliği](./media/how-to-connect-sync-change-the-configuration/powershell1.png)  
2. Yeni eşitleme kuralı oluşturun. Eşitleme kuralları Düzenleyicisi'ni oluşturmak için kullanabilirsiniz. Kural için bir PowerShell betiğini dışarı aktarın.
3. Özelliğinde **PrecedenceBefore**, kullanıma hazır kuralından tanımlayıcı değerini ekleyin. Ayarlama **öncelik** için **0**. Başka bir kural adresinden bir GUID yeniden değil ve benzersiz tanımlayıcı özniteliği olduğundan emin olun. Da emin olmanız **ImmutableTag** özelliği ayarlı değil. Bu özellik için yalnızca bir out-of-box kuralı ayarlamanız gerekir.
4. PowerShell komut dosyasını kaydedin ve çalıştırın. Özel kuralınızı öncelik değeri 100 atanır ve diğer tüm out-of-box kurallar artırılır sonucudur.  
![Değişiklikten sonra PowerShell](./media/how-to-connect-sync-change-the-configuration/powershell2.png)  

Aynı kullanarak birçok özel eşitleme kurallara sahip olabilirler **PrecedenceBefore** değer gerektiğinde.

## <a name="enable-synchronization-of-usertype"></a>UserType eşitlenmesini etkinleştirir
Azure AD Connect eşitleme destekler **UserType** özniteliğini **kullanıcı** nesneleri sürüm 1.1.524.0 ve üzeri. Özellikle, aşağıdaki değişiklikleri sunulmuştur:

- Nesne türünün şeması **kullanıcı** tek değerli dize türünde olduğu ve temel alınan UserType özniteliği eklemek için Azure AD bağlayıcısında genişletilir.
- Nesne türünün şeması **kişi** meta veri deposunda tek değerli dize türünde olduğu ve temel alınan UserType özniteliği içerecek şekilde genişletilir.

Şirket içi Active Directory'de karşılık gelen hiçbir temel alınan UserType özniteliği olduğundan varsayılan olarak, eşitleme için temel alınan UserType özniteliği etkin değil. Ayrıca, eşitleme el ile etkinleştirmeniz gerekir. Bunu yapmadan önce Azure AD tarafından zorlanan aşağıdaki davranış Not gerçekleştirmeniz gerekir:

- Azure AD, yalnızca temel alınan UserType özniteliği için iki değer kabul eder: **Üye** ve **Konuk**.
- Temel alınan UserType özniteliği Azure AD CONNECT'te eşitleme için etkin değilse, dizin eşitlemesi oluşturulan Azure AD kullanıcıları ayarlamak temel alınan UserType özniteliği gerekir **üye**.
- Azure AD temel alınan UserType özniteliği mevcut Azure AD kullanıcıları Azure AD Connect tarafından değiştirilmesi izin vermez. Yalnızca Azure AD kullanıcılarının oluşturma sırasında ayarlanabilir.

Temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce ilk öznitelik şirket içi Active Directory'den türetilmiş nasıl karar vermeniz gerekir. En yaygın yaklaşımlar aşağıda verilmiştir:

- Kullanılmayan bir belirleyin (örneğin, extensionAttribute1) AD özniteliğini kaynak özniteliği olarak kullanılacak şirket. Belirtilen şirket içinde AD özniteliği, türü olmalıdır **dize**tek değerli ve değeri içeren **üye** veya **Konuk**. 

    Bu yaklaşımı seçerseniz, belirtilen öznitelik için temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce Azure AD'ye eşitlenen tüm var olan kullanıcı nesneleri şirket içi Active Directory'de doğru değeri ile doldurulur emin olmalısınız .

- Alternatif olarak, diğer özelliklerden UserType özniteliği için değer türetebilirsiniz. Örneğin, tüm kullanıcılar olarak eşitlemek istediğiniz **Konuk** , şirket içi AD userPrincipalName özniteliği, etki alanı bölümü ile sona erer <em>@partners.fabrikam123.org</em>. 

    Daha önce belirtildiği gibi Azure AD Connect temel alınan UserType özniteliği mevcut Azure AD kullanıcıları Azure AD Connect tarafından değiştirilmesi izin vermez. Bu nedenle, nasıl temel alınan UserType özniteliği için tüm mevcut Azure AD kullanıcılarının kiracınızda yapılandırılmış ile tutarlı olduğuna karar verdiler mantıksal emin olmanız gerekir.

Temel alınan UserType özniteliği eşitlemesini etkinleştirme adımları olarak özetlenebilir:

1.  Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden eşitleme olduğunu doğrulayın.
2.  Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema.
3.  UserType Azure AD Bağlayıcısı şemasına ekleyin.
4.  Şirket içi Active Directory'den öznitelik değeri akış için bir gelen eşitleme kuralı oluşturun.
5.  Öznitelik değeri Azure AD'ye akışına bir giden eşitleme kuralı oluşturun.
6.  Bir tam eşitleme döngüsü çalıştırın.
7.  Eşitleme Zamanlayıcısı'nı etkinleştirin.

>[!NOTE]
> Bu bölümün geri kalanında bu adımları kapsar. Özel eşitleme kuralları olmadan tek ormanlı bir topolojiye ile bir Azure AD dağıtım bağlamında açıklanmıştır. Çok ormanlı topolojisi varsa, özel eşitleme kuralları yapılandırılmamış veya bir hazırlama sunucusunda, adımları uygun şekilde ayarlamanız gerekir.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1\. adım: Eşitleme Zamanlayıcısı'nı devre dışı bırakın ve devam eden eşitleme olduğunu doğrulayın
İstenmeden yapılmış olabilecek değişiklikleri Azure AD'ye dışarı önlemek için eşitleme kuralları güncelleniyor ortasında durumdayken eşitleme yer aldığını emin olun. Yerleşik Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için:

 1. Azure AD Connect sunucusunda bir PowerShell oturumu başlatın.
 2. Cmdlet'ini çalıştırarak zamanlanmış eşitleme devre dışı `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Eşitleme Hizmeti Yöneticisi giderek açın **Başlat** > **eşitleme hizmeti**.
 4. Git **işlemleri** sekme ve durumu ile işlemi yok onaylayın *sürüyor*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>2\. adım: Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şeması
Şirket alınan tüm Azure AD öznitelikleri AD bağlayıcı alanında. Kaynak özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

 1. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
 2. Sağ tıklama şirket içi AD Bağlayıcısı ve select **özellikleri**.
 3. Açılan iletişim kutusuna gidin **öznitelikleri Seç** sekmesi.
 4. Kaynak özniteliğinde öznitelik listesinde seçeneğinin işaretli olduğundan emin olun.
 5. Tıklayın **Tamam** kaydetmek için.
![Şirket içi kaynak öznitelik Ekle AD Bağlayıcısı şeması](./media/how-to-connect-sync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>3\. adım: Azure AD Bağlayıcısı şemasına UserType ekleyin
Varsayılan olarak, temel alınan UserType özniteliği Azure AD Connect alanına içe aktarılmaz. Temel alınan UserType özniteliği içeri aktarılan öznitelikleri listesine eklemek için:

 1. Git **Bağlayıcılar** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
 2. Sağ **Azure AD Bağlayıcısı** seçip **özellikleri**.
 3. Açılan iletişim kutusuna gidin **öznitelikleri Seç** sekmesi.
 4. Temel alınan UserType özniteliği öznitelik listesinde seçeneğinin işaretli olduğundan emin olun.
 5. Tıklayın **Tamam** kaydetmek için.

![Azure AD Bağlayıcısı şemaya kaynak öznitelik Ekle](./media/how-to-connect-sync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>4\. Adım: Şirket içi Active Directory'den öznitelik değeri akış için bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı öznitelik değerini meta veri deposu için şirket içi Active Directory'den kaynak özniteliğindeki akış izin verir:

1. Eşitleme kuralları Düzenleyicisi giderek açın **Başlat** > **eşitleme kuralları Düzenleyicisi**.
2. Arama filtresi ayarlamak **yönü** olmasını **gelen**.
3. Tıklayın **Yeni Kural Ekle** yeni bir gelen kuralı oluşturmak için.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *içinde ad – kullanıcı UserType* |
    | Açıklama | *Bir açıklama sağlayın* |  |
    | Bağlı sistem | *Şirket içi çekme AD Bağlayıcısı* |  |
    | Bağlı sistem nesnesi türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Kişi** |  |
    | Bağlantı türü | **Birleştir** |  |
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekme ve Ekle bir **tek kapsam belirleme filtre grubu** aşağıdaki yan tümcesiyle:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Kullanıcı\_ |

    Kapsam belirleme filtresi bu gelen eşitleme kuralı uygulanmadan hangi şirket içi AD nesneleri belirler. Bu örnekte kullanılan aynı kapsam belirleme filtresi kullandığımız *içinde ad – kullanıcı ortak* Azure AD kullanıcı oluşturulan kullanıcı nesnelerine uygulanan eşitleme kuralı önleyen kutusu giden eşitleme kuralı geri yazma özelliği. Azure AD Connect dağıtımınıza göre kapsam belirleme filtresi ince yapmanız gerekebilir.

6. Git **dönüştürme** sekme ve istenen dönüştürme kuralı uygulayın. Örneğin bir kullanılmayan tanımladıysanız, şirket içinde AD özniteliği (örneğin, extensionAttribute1) UserType kaynak özniteliği doğrudan öznitelik akışı uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | source | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | UserType | extensionAttribute1 | Denetlenmeyen | Güncelleştirme |

    Başka bir örnekte, diğer özelliklerden temel alınan UserType özniteliği değeri türetmek istersiniz. Örneğin, tüm kullanıcılar konuk olarak, eşitlemek istediğiniz şirket içi AD userPrincipalName özniteliği, etki alanı bölümü ile sona erer <em>@partners.fabrikam123.org</em>. Böyle bir ifade uygulayabilirsiniz:

    | Akış türü | Hedef öznitelik | source | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | İfade | UserType | IIf(IsPresent([userPrincipalName]),IIf(CBool(InStr(LCase([userPrincipalName]),"@partners.fabrikam123.org")=0), "Üye", "Konuk"), hata ("UserPrincipalName UserType belirlemek için mevcut değil")) | Denetlenmeyen | Güncelleştirme |

7. Tıklayın **Ekle** gelen kuralı oluşturmak için.

![Gelen eşitleme kuralı oluşturma](./media/how-to-connect-sync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>5\. Adım: Öznitelik değeri Azure AD'ye akışına bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı öznitelik değerini meta veri deposu için temel alınan UserType özniteliği Azure AD'de akış izin verir:

1. Eşitleme kuralları Düzenleyicisi gidin.
2. Arama filtresi ayarlamak **yönü** olmasını **giden**.
3. Tıklayın **Yeni Kural Ekle** düğmesi.
4. Altında **açıklama** sekmesinde, aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Ad | *Bir ad sağlayın* | Örneğin, *AAD – kullanıcı UserType kullanıma al* |
    | Açıklama | *Bir açıklama sağlayın* ||
    | Bağlı sistem | *AAD bağlayıcı seçin* ||
    | Bağlı sistem nesnesi türü | **Kullanıcı** ||
    | Meta veri deposu nesne türü | **Kişi** ||
    | Bağlantı türü | **Birleştir** ||
    | Öncellik | *1-99 arasında bir sayı seçin* | 1-99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmesi değil. |

5. Git **Scoping filtre** sekme ve Ekle bir **tek kapsam belirleme filtre grubu** iki yan tümceyi ile:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | EŞİTTİR | Kullanıcı |
    | cloudMastered | EŞİT DEĞİLDİR | True |

    Kapsam belirleme filtresi, bu giden eşitleme kuralı uygulanmadan olduğu Azure AD nesneleri belirler. Bu örnekte, aynı kapsam belirleme filtresi kullandığımız *AD – kullanıcı kimliği için çıkış* kutusu giden eşitleme kuralı. Eşitleme kuralı, şirket içi Active Directory'nizden eşitlenen değil kullanıcı nesneleri uygulanmasını önler. Azure AD Connect dağıtımınıza göre kapsam belirleme filtresi ince yapmanız gerekebilir.

6. Git **dönüştürme** sekme ve aşağıdaki dönüştürme kuralı uygulayın:

    | Akış türü | Hedef öznitelik | source | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Doğrudan | UserType | UserType | Denetlenmeyen | Güncelleştirme |

7. Tıklayın **Ekle** giden kuralı oluşturmak için.

![Giden eşitleme kuralı oluşturma](./media/how-to-connect-sync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>6\. Adım: Bir tam eşitleme döngüsü çalıştırın
Genel olarak, biz Active Directory ve Azure AD Bağlayıcısı şemaları için yeni özellikler eklendi ve kullanılmaya özel eşitleme kuralları için bir tam eşitleme döngüsü gereklidir. Azure AD'ye dışarı aktarmadan önce değişiklikleri doğrulamak istiyorsanız. 

El ile yaptığınız bir tam eşitleme döngüsü adımları çalıştırırken değişiklikleri doğrulamak için aşağıdaki adımları kullanabilirsiniz.

1. Çalıştıran bir **tam içeri aktarma** üzerinde **şirket içi AD Bağlayıcısı**:

   1. Git **işlemleri** Eşitleme Hizmeti Yöneticisi'nde sekmesi.
   2. Sağ **şirket içi AD Bağlayıcısı** seçip **çalıştırma**.
   3. Açılan iletişim kutusunda **tam içeri aktarma** ve ardından **Tamam**.
   4. İşlemin tamamlanmasını bekleyin.

      > [!NOTE]
      > Tam içeri aktarma atlayabilirsiniz şirket içi AD zaten kaynak öznitelik listesinde yer alıyorsa Bağlayıcısı öznitelikleri içeri aktarıldı. Diğer bir deyişle, sırasında herhangi bir değişiklik yapmak zorunda kalmadığımıza [2. adım: Şirket içi kaynak özniteliği eklemek AD Bağlayıcısı şema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Çalıştıran bir **tam içeri aktarma** üzerinde **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **çalıştırma**.
   2. Açılan iletişim kutusunda **tam içeri aktarma** ve ardından **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

3. Var olan bir kullanıcı nesnesinde eşitleme kuralı değişiklikleri doğrulayın:

    Kaynak özniteliği Active Directory ve Azure AD'den UserType kendi ilgili bağlayıcı alanları alınmış şirket içi. Tam eşitleme devam etmeden önce yapmanız bir **Önizleme** üzerinde mevcut bir kullanıcının nesnesinin şirket içi AD bağlayıcı alanında. Seçtiğiniz nesne doldurulmuş kaynak özniteliğine sahip olmamalıdır.
    
    Başarılı bir **Önizleme** eşitlemenin yapılandırdığınız iyi bir göstergesi kuralları doğru meta veri deposunda doldurulmuş UserType olduğu. Nasıl yapılacağı hakkında daha fazla bilgi için bir **Önizleme**, bölüme [değişikliği doğrulayın](#verify-the-change).

4. Çalıştıran bir **tam eşitleme** üzerinde **şirket içi AD Bağlayıcısı**:

   1. Sağ **şirket içi AD Bağlayıcısı** seçip **çalıştırma**.
   2. Açılan iletişim kutusunda **tam eşitleme** ve ardından **Tamam**.
   3. İşlemin tamamlanmasını bekleyin.

5. Doğrulama **bekleyen dışarı aktarmaların** Azure AD'ye:

   1. Sağ **Azure AD Bağlayıcısı** seçip **arama bağlayıcı alanı**.
   2. İçinde **arama bağlayıcı alanı** açılır iletişim kutusunda:

      - Ayarlama **kapsam** için **bekleyen dışa aktarım**.
      - Üç onay kutusunu seçin: **Ekleme**, **değiştirme**, ve **Sil**.
      - Tıklayın **arama** dışarı değişikliklerle nesneleri listesini almak için düğme. Belirli bir nesne için değişiklikleri incelemek için nesneyi çift tıklayın.
      - Değişiklikleri beklendiğini doğrulayın.

6. Çalıştırma **dışarı** üzerinde **Azure AD Bağlayıcısı**:

   1. Sağ **Azure AD Bağlayıcısı** seçip **çalıştırma**.
   2. İçinde **çalıştırma bağlayıcı** açılır iletişim kutusunda **dışarı** ve ardından **Tamam**.
   3. Tamamlamak için Azure AD'ye dışarı aktarma için bekleyin.

> [!NOTE]
> Bu adımları değil tam eşitleme ve Azure AD Bağlayıcısı üzerinde adımları dışa aktarma. Öznitelik değerleri yalnızca Azure AD ile şirket içi Active Directory'den akışa çünkü bu adımlar gerekli değildir.

### <a name="step-7-re-enable-the-sync-scheduler"></a>7\. Adım: Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin
Yerleşik Eşitleme Zamanlayıcısı'nı yeniden etkinleştirin:

1. Bir PowerShell oturumu başlatın.
2. Zamanlanan eşitleme cmdlet'i çalıştırarak yeniden etkinleştirme `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Sonraki adımlar
* Yapılandırma modeli hakkında daha fazla bilgiyi [anlama bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md).
* İfade dili, hakkında daha fazla bilgiyi [bildirim temelli sağlama ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
