---
title: Oturum açma Azure için e-posta kullanın Konuk kullanıcılar kullanıcı hesaplarını eşitleme | Microsoft Docs
description: Bu makalede, uygulamalara oturum açmak için alternatif kimlik kullanmak Konuk kullanıcı hesaplarını eşitlemek açıklanmaktadır.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2018
ms.author: billmath
ms.openlocfilehash: 4e6cf3bb4a691380a5fe22f5afdf749b40f15ef3
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="synchronizing-guest-user-accounts-that-use-email-for-sign-in-preview"></a>Oturum açma için (Önizleme) e-posta kullanın Konuk kullanıcı hesaplarını eşitleme

>[!NOTE]
> Bu özellik şu anda genel önizlemede değil.

Aşağıdaki senaryoyu ele olduğu dış kullanıcılar, şirket içi durum alternatif bir oturum açma yöntemi kullanan iş ortakları gibi AD ortamı.

Önceki örnekte, Fabrikam Nina Morin çalışır ve aşağıdaki e-posta adresi olan: nmorin@fabrikam.com. Nina ortak Contoso ile ve Contoso sahip belirli uygulamalara erişimi mi gerekiyor. Contoso bir hesap için Nina oluşturdu ve uygulamalara oturum açmak için kendi e-posta adresi kullanmak için Nina yönlendirilmiş.

Kendi şirket içi uygulamalar için bu senaryo harika çalışmaktadır. Ancak artık Contoso bu uygulamaları buluta taşıma ve kendi iş ortakları için aynı deneyimine sahip olmasını istiyor.

Bu senaryoda bu durum giderir.


## <a name="pre-requisites-and-assumptions"></a>Ön koşullar ve varsayımlar
Bu bölüm, ön koşullar ve bu senaryonun kurulumunu yapmak denemeden önce bilincinde olmanız gereken varsayımlar listesini içerir.

### <a name="pre-requisites"></a>Ön koşullar
- Azure AD Connect yapılandırmak için genel yönetici kimlik bilgileriniz etki alanlarını doğrulamak ve etki alanı Federasyon ayarlarını yapılandırın
- Azure AD Connect sürüm 1.1.524.0 veya üzeri
- Dış kullanıcıların UPN bulut ayarlamak için doğrulanmış etki alanı (örnek: bmcontoso.com).
- Dış kullanıcıların kimliklerini doğrulamak için Federasyon Hizmeti. AD FS kullanıyorsanız, 2012 R2 olmalıdır veya üzeri
- MSOL PowerShell v1.1 Federasyon ayarlarını doğrulamak için bir makineye yüklenir. Daha fazla bilgi için bkz: [Azure ActiveDirectory (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).


### <a name="assumptions"></a>Varsayımlar 
- Azure AD Connect zaten olduğundan ayarlamak ve başarıyla yüklendi. Azure AD Connect'i yükleme hakkında daha fazla bilgi için bkz: [Azure AD Connect ve Federasyon](active-directory-aadconnectfed-whatis.md).
Bu belge aşağıdaki varsayımlar yapar:
- ayarlanmış bir Federasyon Hizmeti ve onun sahip başarıyla kullanıcıların kimlik doğrulaması.
- Dış kullanıcılar kendi dış e-posta adresini kullanarak doğrulayabilir.
- - Oturum açma için alternatif kimlik kullanımı ayarlamak ve yapılandırıldı. Kullanıcılar kendi alternatif kimliğini kullanarak kimlik doğrulaması yapabilir AD FS ile alternatif kimlik ayarlama hakkında ek bilgi için bkz: [alternatif oturum açma Kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id).

## <a name="task-1--prepare-the-environment"></a>Görev 1: ortamı hazırlama
Bunlar alternatif kullanarak oturum açın böylece, dış hesaplarınızı eşitlemeye başlamak için hazır olduğunuzda aşağıdaki görevi bir bilgilendirme birkaçını, böylelikle i posta özniteliğinin gibi.

İkinci görevi geçmeden önce aşağıdaki tabloda öğeleri tanımlar.

![Mimari](media/active-directory-aadconnect-guest-sync/guest2.png)

|Ortam boyutu|Ne kullanılır?|Ortamınızdaki uygulama|
|-----|-----|-----|
|Bulut UPN özniteliği|Bulutta dış kullanıcı nesnelerinin UPN doldurur özniteliği. UPN soneki olan dış hesapların önkoşulların içinde tanımlanmış olanla olması gerekir. Bu doğrulanmış etki alanıdır.|* Örnek: UserPrincipalName (nmorin@bmcontoso.com)|
|Oturum açma adresi|Oturum açarken dış kullanıcılar yazacağınızı özniteliği. Bu öznitelik bir e-posta adresi biçiminde olmalıdır ve çoğu durumda, dış kullanıcı gerçek e-posta adresi ile örtüşür.|* Örnek: posta (nmorin@fabrikam.com)|
|Azure AD Connect kapsamlı filtresi|Bu kılavuzda tanımlanan eşitleme kuralların kapsamını belirlemek için dış kimlikler hedefleme veren Filtresi. Kapsam genel yöntemleri içerir: kuruluş, belirli bir adlandırma kuralı, belirli bir etki alanı, vb. önceden tanımlanmış bir OU'da.|* Örnek: OU Externals içerir.|
|Azure AD kiracısı|Azure AD Connect'e olarak Azure AD Kiracı adı görüntülenir.|Örnek: contoso.onmicrosoft.com|

Aşağıdaki ekran görüntüsünde gösterilen üç kutuları sahiptir.
- **Externals** Azure AD Connect kapsamlı filtrede kullanılan ve dış kullanıcılar konumudur OU.
- **Posta** dış kullanıcı tarafından oturum açmak için kullanılan öznitelik.
- **UserPrincipalName** içi ortamınız ile Federasyon doğrulanmış etki alanı özniteliği.

![Kullanıcı](media/active-directory-aadconnect-guest-sync/guest1.png)

## <a name="task-2--configure-azure-ad-connect"></a>Görev 2: Azure AD'yi yapılandırma bağlan
Tanımlanan Yukarıdaki bilgilere sahip olduktan sonra size Azure AD Connect eşitleme kuralları ayarını geçebilirsiniz. Kuralları ayarlamak için Azure AD Connect eşitleme kuralları Düzenleyicisi'ni kullanın. Düzenleyici hakkında daha fazla bilgi için bkz: [akımının bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

### <a name="how-to-configure-the-synchronization-rule"></a>Eşitleme kuralı yapılandırma
Azure AD Connect yapılandırmak için aşağıdaki yordamı kullanın.

1. Azure AD Connect eşitleme kuralı giderek Düzenleyicisi **Start - Azure AD Connect - eşitleme kuralları Düzenleyicisi**.
2. Üzerinde **eşitleme kuralları Düzenleyicisi** ekranında, yönü olduğundan emin olun **gelen** sağ tarafta tıklatıp **Yeni Kural Ekle**.
3. Üzerinde **açıklama** sayfasında aşağıdakileri yapılandırın ve tıklayın **sonraki**.
    - **Ad** -kural için bir ad girin 
    - **Bağlı sistem:** -şirket içi AD ortamı
    - **Bağlı sistem nesne türü:** -kullanıcı
    - **Meta veri deposu nesne türü:** -kişi
    - **Bağlantı türü:** -katılma
    - **Öncelik:** - 90
    - 
![](media/active-directory-aadconnect-guest-sync/guest3.png)

4. Üzerinde **kapsam filtresi** ekranında **Grup Ekle**.
5. Filtre yapılandırmak için açılır listeleri kullanın. ' A tıklayın ve aşağıdakileri girin **sonraki**. Bu eylem yalnızca dış OU'da bulunan nesnelere uygulanan bir filtre oluşturur.
    - **Öznitelik** -dn
    - **İşleç** -içerir
    - **Değer** -Externals
 
 ![Filtre](media/active-directory-aadconnect-guest-sync/guest4.png)

6. Üzerinde **katılma kuralları** ekranında **sonraki**.
7. Üzerinde **dönüşümleri** ekranında **ekleme dönüşümünü**. Aşağıdaki bilgileri girin:
    - **FlowType** - sabit
    - **Hedef öznitelik** -userType
    - **Kaynak** - Konuk
8. Üzerinde **dönüşümleri** ekranında **ekleme dönüşümünü**. Aşağıdaki bilgileri girin:
    - **FlowType** - doğrudan
    - **Hedef öznitelik** -onPremisesUserPrincipalName
    - **Kaynak** -posta
9. Üzerinde **dönüşümleri** ekranında **ekleme dönüşümünü**. Aşağıdaki bilgileri girin:
    - **FlowType** - doğrudan
    - **Hedef öznitelik** -userPrincipalName
    - **Kaynak** -userPrincipalName ![dönüştürme](media/active-directory-aadconnect-guest-sync/guest5.png)
10. **Ekle**'ye tıklayın. 
11. Üzerinde **eşitleme kuralları Düzenleyicisi** ekranında, yönü olduğundan emin olun **giden** sağ tarafta tıklatıp **Yeni Kural Ekle**.
12. Üzerinde **açıklama** sayfasında aşağıdakileri yapılandırın ve tıklayın **sonraki**.
    - **Ad** -kural için bir ad girin 
    - **Bağlı sistem:** -Azure AD kiracısı
    - **Bağlı sistem nesne türü:** -kullanıcı
    - **Meta veri deposu nesne türü:** -kişi
    - **Bağlantı türü** -katılma
    - **Öncelik:** - 90
    - 
![Kural](media/active-directory-aadconnect-guest-sync/guest6.png)

13. Üzerinde **Scoping filtre** ekranında **sonraki**.
14. Üzerinde **katılma kuralları** ekranında **sonraki**.
15. Üzerinde **dönüşümleri** ekranında **ekleme dönüşümünü**.  Aşağıdaki bilgileri girin:
    - **FlowType** - doğrudan
    - **Hedef öznitelik** -userType
    - **Kaynak** -userType
9. Üzerinde **dönüşümleri** ekranında **ekleme dönüşümünü**. Aşağıdaki bilgileri girin:
    - **FlowType** - doğrudan
    - **Hedef öznitelik** -onPremisesUserPrincipalName
    - **Kaynak** -onPremisesUserPrincipalName ![dönüştürme](media/active-directory-aadconnect-guest-sync/guest7.png)
10. **Ekle**'ye tıklayın. 
11. Bu kurallar yapılandırdıktan sonra tam eşitleme çalıştırmanız gerekir. Tam eşitleme başlatmak için PowerShell kullanın. Eşitleme tamamlandıktan sonra sonraki adıma geçebilirsiniz.

``` powershell
    Start-ADSyncSyncCycle -PolicyType Initial
```

## <a name="task-3--federation"></a>Görev 3: Federasyon
Aşağıdaki görev iş senaryosu için yerinde olması gereken birkaç şey üzerinde bir bilgilendirme amaçlıdır.

Azure AD PowerShell kullanarak Azure ile Federasyon ayarlarınızı doğrulayabilirsiniz. Bu belge MSOL PowerShell v1.1 kullanır. Bu sürümü yükleyebilirsiniz [burada](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).

### <a name="verify-the-federation-settings"></a>Federasyon ayarlarını doğrulayın
Federasyon ayarlarını doğrulamak için aşağıdaki yordamı kullanın.
1. Windows PowerShell'i açın ve aşağıdaki komutu kullanarak Azure AD connect:
``` powershell
      Connect-MSOLservice
```
2.  Genel yönetici kimlik bilgilerini girin
3.  Şimdi aşağıdaki komutu girin:
  ``` powershell
      Get-MSOLDomainFederationSettings
  ```
4.   Federasyon bilgi döndürülmelidir dikkat edin. Not **ActiveLogonUri** federasyon sunucusunun URL'si.

  ![Federasyon](media/active-directory-aadconnect-guest-sync/guest8.png)

### <a name="verify-alternate-login-id"></a>Alternatif oturum açma Kimliğini doğrulayın
Bu belge AD FS kimlik sağlayıcıyı (IDP) kullanır. Farklı bir IDP kullanıyorsanız, aşağıdaki adımları çok olabilir.

1. Windows PowerShell'i açın ve aşağıdaki komutu girin:
   ```powershell
    Get-ADFSClaimsProviderTrust
   ```
2. AD FS bilgileri görmeniz gerekir.  Not **AlternateLoginID** ve **LookupForests**.

![ADFS](media/active-directory-aadconnect-guest-sync/guest9.png)

## <a name="task-4--testing"></a>Görev 4: test etme
Bu düzgün çalıştığını doğrulamak için IDP kullanarak kimlik doğrulaması için yapılandırılmış bir uç nokta oturum açması gerekir. Bunu test etmek için bir Web sitesi dağıtılan ve aşağıdaki URL'yi kullanarak: contososampapp.azurewebsites.net

### <a name="verify-that-you-can-sign-in-with-the-alternate-id"></a>Oturum alternatif Kimliğiyle açma emin doğrulayın
1. Oturum uç noktasına açın.</br>
![uç noktası](media/active-directory-aadconnect-guest-sync/guest10.png)
1. Kullanıcı adınızı girin ve bunu yeniden Federasyon oturum açma sayfasına yönlendirilirsiniz.
![Federasyon](media/active-directory-aadconnect-guest-sync/guest11.png)
1. Kimlik bilgilerinizi girin.
2. Artık başarıyla oturum açmanız.
![Başarılı](media/active-directory-aadconnect-guest-sync/guest12.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure Active Directory B2B işbirliği kullanıcının özelliklerini](../../active-directory/b2b/user-properties.md#key-properties-of-the-azure-ad-b2b-collaboration-user)
- [Alternatif oturum açma Kimliğini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)
- [Azure AD Connect: Sürüm yayınlama geçmişi](active-directory-aadconnect-version-history.md)
