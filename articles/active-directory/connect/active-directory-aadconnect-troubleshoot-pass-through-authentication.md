---
title: "Azure AD Connect: Doğrudan kimlik doğrulama sorunlarını giderme | Microsoft Docs"
description: "Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulama sorunlarını giderme açıklar."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama sorunlarını giderme, Active Directory, Azure AD, SSO için gerekli bileşenleri yüklemek çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: billmath
ms.openlocfilehash: b842791be74094c87643528c0b4d3a65be6b3cb1
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Azure Active Directory doğrudan kimlik doğrulama sorunlarını giderme

Bu makale size yardımcı olacak bilgileri Azure AD geçişli kimlik doğrulaması ile ilgili genel sorunları hakkında sorun giderme bulma.

>[!IMPORTANT]
>Kullanıcı oturum açma sorunları geçişli kimlik doğrulaması ile karşılıklı verme özelliği devre dışı veya geri dönmesi için bir yalnızca bulut genel yönetici hesabını gerek kalmadan doğrudan kimlik doğrulama aracıları kaldırın. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md). Bu adımı uygulamadan önemlidir ve, kiracınızın dışında erişebilmenizin sağlar.

## <a name="general-issues"></a>Genel sorunlar

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Özellik ve kimlik doğrulama aracıların durumunu denetleme

Doğrudan kimlik doğrulama özelliği hala olduğundan emin olun **etkin** Kiracı ve kimlik doğrulama aracıların durumunu gösteren **etkin**ve **devre dışı**. Giderek durumunu denetleyebilirsiniz **Azure AD Connect** dikey penceresinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - Azure AD Connect dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi - doğrudan kimlik doğrulama dikey penceresi](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Oturum açma kullanıcı dönük hata iletileri

Kullanıcının doğrudan kimlik doğrulaması kullanarak oturum varsa aşağıdaki kullanıcı dönük hatalardan biri Azure AD oturum açma ekranında görebilirsiniz: 

|Hata|Açıklama|Çözüm
| --- | --- | ---
|AADSTS80001|Active Directory'ye bağlanılamıyor|Aracı sunucularının parolaları doğrulanması gereken kullanıcılar aynı AD ormana üyeleri olduğundan emin olun ve Active Directory'ye bağlanamıyor.  
|AADSTS8002|Active Directory bağlanırken zaman aşımı oluştu|Active Directory kullanılabilir ve aracılardan isteklerine yanıt emin olun.
|AADSTS80004|Aracıya geçirilen kullanıcı adı geçerli değil.|Kullanıcı doğru kullanıcı adıyla oturum çalışılıyor emin olun.
|AADSTS80005|Doğrulama öngörülemeyen WebException karşılaştı|Geçici bir hata oluştu. İsteği yeniden deneyin. Başarısız olmaya devam ederse, Microsoft Destek'e başvurun.
|AADSTS80007|Active Directory ile iletişim kuran bir hata oluştu|Daha fazla bilgi için aracı günlüklerine bakın ve Active Directory beklendiği gibi çalıştığını doğrulayın.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Oturum açma hatası nedeniyle Azure Active Directory Yönetim Merkezi (Premium lisansı gerekir)

Kiracı kendisiyle ilişkili bir Azure AD Premium lisansı varsa, aynı zamanda bakabilirsiniz [oturum açma etkinliği raporu](../active-directory-reporting-activity-sign-ins.md) üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - oturum açma işlemleri raporu](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Gidin **Azure Active Directory** -> **oturum açma işlemleri** üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) ve belirli bir kullanıcının oturum açma etkinliği tıklatın. Ara **oturum açmayı hata kodu** alan. Bu alanın değeri bir başarısızlık nedeniyle ve aşağıdaki tabloda kullanılarak çözümleme eşleyin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 50144 | Kullanıcının Active Directory parolasının süresi doldu. | Şirket içi Active Directory'de kullanıcının parolasını sıfırlayın.
| 80001 | Kullanılabilir Kimlik Doğrulama Aracısı yok. | Yükleyin ve bir kimlik doğrulama Aracısı kaydedin.
| 80002 | Kimlik Doğrulama Aracısı'nın parola doğrulama isteği zaman aşımına uğradı. | Active Directory kimlik doğrulama Aracısı'ndan erişilebilir olup olmadığını denetleyin.
| 80003 | Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. | Sorun birden çok kullanıcı arasında tutarlı bir şekilde yineleniyorsa, Active Directory yapılandırmanızı denetleyin.
| 80004 | Oturum açma isteğinde yanlış Kullanıcı Asıl Adı (UPN) kullanıldı. | Doğru kullanıcı adıyla oturum kullanıcıya isteyin.
| 80005 | Kimlik Doğrulama Aracısı: Hata oluştu. | Geçici hata oluştu. Daha sonra tekrar deneyin.
| 80007 | Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. | Active Directory kimlik doğrulama Aracısı'ndan erişilebilir olup olmadığını denetleyin.
| 80010 | Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. | Sorun tutarlı bir şekilde yineleniyorsa yükleyin ve yeni bir kimlik doğrulama Aracısı kaydedin. Ve geçerli bir kaldırın. 
| 80011 | Kimlik Doğrulama Aracısı şifre çözme anahtarını alamıyor. | Sorun tutarlı bir şekilde yineleniyorsa yükleyin ve yeni bir kimlik doğrulama Aracısı kaydedin. Ve geçerli bir kaldırın.

## <a name="authentication-agent-installation-issues"></a>Kimlik Doğrulama Aracısı yükleme sorunları

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Aracı günlükleri toplamak](#collecting-pass-through-authentication-agent-logs) sunucusuna gelen ve sorunu ile Microsoft Support başvurun.

## <a name="authentication-agent-registration-issues"></a>Kimlik Doğrulama Aracısı kayıt sorunları

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Kimlik Doğrulama Aracısı kaydı Engellenen bağlantı noktaları nedeniyle başarısız oldu

Kimlik Doğrulama Aracısı yüklenmiş sunucu URL'lerin ve bağlantı noktalarının listelenen bizim hizmeti ile iletişim kurabildiğinden emin olun [burada](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Kimlik Doğrulama Aracısı kaydı belirteç veya hesap yetkilendirme hataları nedeniyle başarısız oldu

Tüm Azure AD Connect veya tek başına kimlik doğrulama Aracısı yükleme ve kayıt işlemleri için bir yalnızca bulut genel yönetici hesabı kullandığınızdan emin olun. MFA etkin genel yönetici hesapları ile ilgili bilinen bir sorun yoktur; (yalnızca işlemleri tamamlamak için) MFA devre dışı geçici olarak geçici bir çözüm olarak etkinleştirin.

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Aracı günlükleri toplamak](#collecting-pass-through-authentication-agent-logs) sunucusuna gelen ve sorunu ile Microsoft Support başvurun.

## <a name="authentication-agent-uninstallation-issues"></a>Kimlik Doğrulama Aracısı kaldırma konuları

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Azure AD Connect kaldırırken uyarı iletisi

Doğrudan kiracınız üzerinde etkin kimlik doğrulama varsa ve Azure AD Connect kaldırmaya çalıştığınızda, bu, aşağıdaki uyarı iletisi görüntüler: "kullanıcılar edemeyecek oturum Azure AD ile diğer doğrudan kimlik doğrulama aracıları yüklü olmadığı sürece açmak diğer sunucuları"

Kurulumunuzu olduğundan emin olun [yüksek kullanılabilir](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) kullanıcı oturum açma çiğnemekten önlemek için Azure AD Connect kaldırmadan önce.

## <a name="issues-with-enabling-the-feature"></a>Özelliğin etkinleştirilmesi sorunlar

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Kullanılabilir herhangi bir kimlik doğrulama aracı olduğundan başarısız oldu özelliğini etkinleştirme

En az bir etkin kimlik doğrulama, Kiracı'geçişli kimlik doğrulamasını etkinleştirmek için aracı olması gerekir. Azure AD Connect yükleme ya da tek başına bir kimlik doğrulama aracısı tarafından bir kimlik doğrulama Aracısı yükleyebilirsiniz.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Özellik etkinleştirme Engellenen bağlantı noktaları nedeniyle başarısız oldu

Azure AD Connect yüklendiği sunucunun URL'lerin ve bağlantı noktalarının listelenen bizim hizmeti ile iletişim kurabildiğinden emin olun [burada](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Belirteç veya hesap yetkilendirme hataları nedeniyle başarısız özelliğini etkinleştirme

Özellik etkinleştirilirken bir yalnızca bulut genel yönetici hesabı kullandığınızdan emin olun. Çok faktörlü kimlik doğrulaması (MFA) ile ilgili bilinen bir sorun yoktur-genel yönetici hesapları; etkin (yalnızca işlemi tamamlamak için) MFA devre dışı geçici olarak geçici bir çözüm olarak etkinleştirin.

## <a name="exchange-activesync-configuration-issues"></a>Exchange ActiveSync yapılandırma sorunları

Bunlar, geçişli kimlik doğrulaması için Exchange ActiveSync desteği yapılandırdığınızda yaygın sorunlardır.

### <a name="exchange-powershell-issue"></a>Exchange PowerShell sorunu

Görürseniz "**parametre adı 'PerTenantSwitchToESTSEnabled' ile eşleşen bir parametre bulunamıyor\.**" çalıştırdığınızda hata `Set-OrganizationConfig` Exchange PowerShell komutu, Microsoft Support başvurun.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync çalışmıyor

Yapılandırmanın etkili olması için biraz zaman alır - süre ortamınıza bağlıdır. Durum uzun bir süre devam ederse, Microsoft Support başvurun.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Toplama doğrudan kimlik doğrulama Aracısı günlükleri

Olabilirsiniz sorunu türüne bağlı olarak, doğrudan kimlik doğrulama Aracısı günlükleri için farklı yerlerde Ara gerekir.

### <a name="azure-ad-connect-logs"></a>Azure AD Connect günlükleri

Yüklemeyle ilgili hatalar için Azure AD Connect günlüklerine **%ProgramData%\AADConnect\trace-\*.log**.

### <a name="authentication-agent-event-logs"></a>Kimlik Doğrulama Aracısı olay günlükleri

Kimlik Doğrulama Aracısı ile ilgili hataları, sunucuda Olay Görüntüleyicisi uygulamasının açın ve altında denetleyin **uygulama ve hizmet Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Ayrıntılı analizler için "oturum" etkinleştirin. Kimlik Doğrulama Aracısı, bu günlüğü normal işlemler sırasında etkin ile çalıştırma; yalnızca sorun giderme için kullanın. Günlüğü yeniden devre dışı bırakıldıktan sonra günlük içeriği yalnızca görünür.

### <a name="detailed-trace-logs"></a>Ayrıntılı izleme günlükleri

Kullanıcı oturum açma hatalarını giderme için izleme günlüklerine bakın **%ProgramData%\Microsoft\Azure AD Connect kimlik doğrulama Agent\Trace\\**. Bu günlükler geçişli kimlik doğrulaması özelliğini kullanarak başarısız neden belirli bir kullanıcı oturum açma nedenleri. Bu hatalar da önceki gösterilen oturum açma hatası nedeniyle eşlendiği [tablo](#sign-in-failure-reasons-on-the-Azure-portal). Günlük girişi örneğidir aşağıdadır:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

İçin komut istemini açın ve aşağıdaki komutu çalıştırarak (önceki örnekte ' 1328') hata açıklayıcı ayrıntılarını alabilirsiniz (Not: günlüklerinizi içinde gördüğünüz gerçek hata numarası '1328' yerine):

`Net helpmsg 1328`

![Doğrudan Kimlik Doğrulama](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi

Denetim günlüğü etkinleştirilirse, etki alanı denetleyicilerinin güvenlik günlüklerini ek bilgiler bulunabilir. Oturum açma isteklerini doğrudan kimlik doğrulama aracı tarafından gönderilen sorgulamak için basit bir yol şu şekildedir:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Performans İzleyicisi sayaçları

Kimlik doğrulama aracılarını izlemek için başka bir kimlik doğrulama Aracısı yüklendiği her sunucuda özel Performans İzleyicisi sayaçları izlemek için yoludur. Aşağıdaki genel sayaçları kullanın (**# PTA kimlik doğrulamaları**, **#PTA kimlik doğrulamaları başarısız** ve **#PTA başarılı kimlik doğrulamalarını**) ve hata sayaçları (**# PTA kimlik doğrulama hataları**):

![Doğrudan kimlik doğrulama Performans İzleyicisi sayaçları](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Doğrudan kimlik doğrulama birden çok kimlik doğrulama aracı kullanarak yüksek kullanılabilirlik sağlar ve _değil_ Yük Dengeleme. Yapılandırmanıza bağlı olarak _değil_ tüm kimlik doğrulama Aracısı kabaca alma _eşit_ isteklerinin sayısı. Belirli bir kimlik doğrulama Aracısı hiçbir trafik hiç alır mümkündür.
