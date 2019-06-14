---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması sorunlarını giderme | Microsoft Docs'
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması sorunlarını gidermek açıklar.
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması sorunlarını giderme, Active Directory, Azure AD SSO için gerekli bileşenleri yükleme çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae83cea866367fa6a6596caa683d0287bea96c29
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60456173"
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Azure Active Directory geçişli kimlik doğrulaması sorunlarını giderme

Bu makale size yardımcı olur. Azure AD geçişli kimlik doğrulaması ile ilgili yaygın sorunları ile ilgili sorun giderme bilgileri bulun.

>[!IMPORTANT]
>Geçişli kimlik doğrulaması ile kullanıcı oturum açma sorunları yaşıyorsanız yok özelliğini devre dışı bırakma veya geri dönüş için bir yalnızca bulut genel yönetici hesabı zorunda kalmadan doğrudan kimlik doğrulama aracılarının kaldırın. Hakkında bilgi edinin [bir yalnızca bulut genel yönetici hesabı ekleyerek](../active-directory-users-create-azure-portal.md). Bu adımı uygulamadan kritik öneme sahiptir ve bu ayar, kiracınızın dışında kilitli kalmamanızı sağlar.

## <a name="general-issues"></a>Genel sorunlar

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Özellik ve kimlik doğrulama aracılarının durumunu denetleyin

Geçişli kimlik doğrulaması özelliği hala olduğundan emin olun **etkin** Kiracı ve kimlik doğrulama aracılarının durumunu gösteren **etkin**ve **devre dışı**. Durumu giderek denetleyebilirsiniz **Azure AD Connect** dikey penceresinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - Azure AD Connect dikey penceresi](./media/tshoot-connect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi - geçişli kimlik doğrulaması dikey penceresi](./media/tshoot-connect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Kullanıcıya yönelik oturum açma hata iletileri

Geçişli kimlik doğrulaması kullanarak oturum açmak kullanıcının silemiyor, kullanıcıya yönelik aşağıdaki hatalardan birini Azure AD oturum açma ekranında görebilirsiniz: 

|Hata|Açıklama|Çözüm
| --- | --- | ---
|AADSTS80001|Active Directory'ye bağlanılamıyor|Aracı sunucularının parolaları doğrulanması gereken kullanıcılar aynı AD ormanında üyeleri olduğundan emin olun ve bunlar Active Directory'ye bağlanamıyor.  
|AADSTS8002|Active Directory ile bağlantı kurulurken bir zaman aşımı oluştu|Active Directory kullanılabilir ve isteklere yanıt aracılardan emin olun.
|AADSTS80004|Aracıya geçirilen kullanıcı adı geçerli değil|Kullanıcı doğru kullanıcı adıyla oturum çalışıyor olun.
|AADSTS80005|Öngörülemeyen WebException doğrulama karşılaşıldı|Geçici bir hata oluştu. İsteği yeniden deneyin. Başarısız olmaya devam ederse Microsoft desteğine başvurun.
|AADSTS80007|Active Directory ile iletişimde bir hata oluştu|Daha fazla bilgi için aracı günlüklerini denetleyin ve Active Directory beklendiği gibi çalıştığını doğrulayın.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Azure Active Directory Yönetim Merkezi'nde oturum açma hatası nedeniyle (Premium lisansı gerekir)

Kiracınızın ilişkili bir Azure AD Premium lisansınız varsa, ayrıca bakabilirsiniz [oturum açma etkinliği raporunu](../reports-monitoring/concept-sign-ins.md) üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/).

![Azure Active Directory Yönetim Merkezi - oturum açma işlemleri raporu](./media/tshoot-connect-pass-through-authentication/pta4.png)

Gidin **Azure Active Directory** -> **oturum açma işlemleri** üzerinde [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) ve belirli bir kullanıcının oturum açma etkinliği tıklatın. Aranacak **oturum açma hata kodu** alan. Bu alanın değeri bir hata nedeni ve çözümü aşağıdaki tabloyu kullanarak eşleme:

|Oturum açma hata kodu|Oturum açma hata nedeni|Çözüm
| --- | --- | ---
| 50144 | Kullanıcının Active Directory parolasının süresi doldu. | Şirket içi Active directory'nizde kullanıcının parolasını sıfırlayın.
| 80001 | Kullanılabilir Kimlik Doğrulama Aracısı yok. | Yükleyin ve bir kimlik doğrulama Aracısı kaydedin.
| 80002 | Kimlik Doğrulama Aracısı'nın parola doğrulama isteği zaman aşımına uğradı. | Active Directory kimlik doğrulaması Aracısı'ndan erişilebilir olup olmadığını denetleyin.
| 80003 | Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. | Sorun birden çok kullanıcı arasında tutarlı bir şekilde yeniden üretilebilen ise, Active Directory yapılandırmanızı denetleyin.
| 80004 | Oturum açma isteğinde yanlış Kullanıcı Asıl Adı (UPN) kullanıldı. | Doğru kullanıcı adıyla oturum açmasını isteyin.
| 80005 | Kimlik Doğrulama Aracısı: Hata oluştu. | Geçici hata oluştu. Daha sonra tekrar deneyin.
| 80007 | Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. | Active Directory kimlik doğrulaması Aracısı'ndan erişilebilir olup olmadığını denetleyin.
| 80010 | Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. | Sorun tutarlı bir şekilde yineleniyorsa yükleyin ve yeni bir kimlik doğrulama Aracısı kaydedin. Ve geçerli kaldırın. 
| 80011 | Kimlik Doğrulama Aracısı şifre çözme anahtarını alamıyor. | Sorun tutarlı bir şekilde yineleniyorsa yükleyin ve yeni bir kimlik doğrulama Aracısı kaydedin. Ve geçerli kaldırın.

>[!IMPORTANT]
>Doğrudan kimlik doğrulama aracılarının kimliğini Azure AD kullanıcılarının, kullanıcı adları ve parolaları Active Directory karşı çağırarak doğrulayarak [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx). Sonuç olarak, iş istasyonu oturum açma erişimi sınırlandırmak için Active Directory'de "Oturum açma için" ayarı ayarladıysanız, geçişli kimlik doğrulama aracılarının "Oturum açma için" de sunucular listesine barındırma sunucuları eklemeniz gerekir. Bunu yapmak başarısız olan, kullanıcılarınızın Azure AD açmasını engeller.

## <a name="authentication-agent-installation-issues"></a>Kimlik Doğrulama Aracısı yükleme sorunları

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Aracı günlüklerini toplamak](#collecting-pass-through-authentication-agent-logs) sunucusuna gelen ve sorununuzla Support Microsoft ile iletişime geçin.

## <a name="authentication-agent-registration-issues"></a>Kimlik Doğrulama Aracısı kayıt sorunları

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Engellenen bağlantı noktaları nedeniyle kimlik doğrulama Aracısı kaydı başarısız oldu

Kimlik Doğrulama Aracısı yüklendi sunucusu URL'lerini ve bağlantı noktalarını listelenen hizmetimiz ile iletişim kurabildiğinden emin olun [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites).

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Kimlik Doğrulama Aracısı kaydı belirteç veya hesap yetkilendirme hataları nedeniyle başarısız oldu

Tüm Azure AD Connect veya tek başına kimlik doğrulama Aracısı yükleme ve kayıt işlemleri için yalnızca bulutta yer alan bir genel yönetici hesabını kullandığınızdan emin olun. MFA etkin genel yönetici hesapları ile ilgili bilinen bir sorun yoktur; (yalnızca işlemleri tamamlamak için) mfa'yı devre dışı geçici olarak geçici bir çözüm olarak açın.

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Aracı günlüklerini toplamak](#collecting-pass-through-authentication-agent-logs) sunucusuna gelen ve sorununuzla Support Microsoft ile iletişime geçin.

## <a name="authentication-agent-uninstallation-issues"></a>Kimlik Doğrulama Aracısı kaldırma sorunları

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Azure AD Connect kaldırırken uyarı iletisi

Doğrudan kiracınızda etkin kimlik doğrulama varsa ve Azure AD Connect kaldırmayı deneyin, aşağıdaki uyarı iletisini gösterir: "Kullanıcıları Azure AD'ye diğer sunucularda yüklü diğer doğrudan kimlik doğrulama aracılarının yoksa oturum açmanız mümkün olmayacaktır."

Kurulumunuzu olduğundan emin olun [yüksek oranda kullanılabilir](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability) Azure AD Connect kullanıcı oturumu açma bozmayı önlemek için kaldırmadan önce.

## <a name="issues-with-enabling-the-feature"></a>Özellik etkinleştirilmesiyle ilgili sorunlar

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Kullanılabilir hiçbir kimlik doğrulama aracılarının olduğundan başarısız oldu özelliğin etkinleştirilmesi

En az bir etkin kimlik doğrulama kiracınızda geçişli kimlik doğrulamasını etkinleştirmek için aracı olması gerekir. Azure AD Connect'i yükleme veya tek başına bir kimlik doğrulama aracısı tarafından bir kimlik doğrulama Aracısı yükleyebilirsiniz.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Özelliğin etkinleştirilmesi nedeniyle engellenen bağlantı başarısız oldu

Azure AD Connect'in yüklü olduğu sunucunun URL'lerini ve bağlantı noktalarını listelenen hizmetimiz ile iletişim kurabildiğinden emin olun [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites).

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Özellik etkinleştirme belirteci veya hesap yetkilendirme hatalar nedeniyle başarısız oldu

Bu özellik etkinleştirilirken bir yalnızca bulut genel yönetici hesabı kullandığınızdan emin olun. Multi-Factor authentication (MFA) ile bilinen bir sorun-genel yönetici hesapları; etkin (yalnızca işlemi tamamlamak için) mfa'yı devre dışı geçici olarak geçici bir çözüm olarak açın.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Geçişli kimlik doğrulaması Aracısı günlükleri toplama

Bilgisayarınızda yüklü olmayabilir sorun türüne bağlı olarak, geçişli kimlik doğrulaması Aracısı günlükleri için farklı yerlerde bakmanız gerekir.

### <a name="azure-ad-connect-logs"></a>Azure AD Connect günlükleri

Yüklemeyle ilgili hataları denetlemek için Azure AD Connect günlüklerine **%ProgramData%\AADConnect\trace-\*.log**.

### <a name="authentication-agent-event-logs"></a>Kimlik Doğrulama Aracısı olay günlükleri

Kimlik Doğrulama Aracısı ile ilgili hatalara sunucusundaki Olay Görüntüleyici uygulamasını açın ve altında denetleyin **uygulama ve hizmet Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Ayrıntılı analizler için "" oturum (Bu seçenek bulmak için Olay Görüntüleyicisi'ni uygulama içinde sağ tıklama) etkinleştirin. Kimlik doğrulaması Aracısı, bu günlük, normal işlemler sırasında etkin çalışmıyor; yalnızca sorun giderme için kullanın. Günlük içeriklerini yalnızca günlük yeniden devre dışı bırakıldıktan sonra görülebilir.

### <a name="detailed-trace-logs"></a>Ayrıntılı izleme günlükleri

Kullanıcı oturum açma sorunlarını giderme için izleme günlüklerine bakın **%ProgramData%\Microsoft\Azure AD Connect kimlik doğrulaması Agent\Trace\\** . Bu günlükler geçişli kimlik doğrulaması özelliğini kullanarak başarısız neden belirli bir kullanıcı oturum açma nedenleri. Bu hatalar da önceki oturum açma hatası nedeniyle tabloda gösterilen oturum açma hatası nedeniyle eşlenir. Bir örnek günlük girişi aşağıda verilmiştir:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Tanımlayıcı (yukarıdaki örnekte ' 1328') hata ayrıntılarını için komut istemini açın ve aşağıdaki komutu çalıştırarak alabilirsiniz (Not: Günlükleri görmek gerçek hata numarası '1328' değiştirin):

`Net helpmsg 1328`

![Doğrudan Kimlik Doğrulama](./media/tshoot-connect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri

Denetim günlüğü etkinleştirilirse, etki alanı denetleyicilerinizin Güvenlik günlüklerine ek bilgiler bulunabilir. Doğrudan kimlik doğrulama aracılarının tarafından gönderilen oturum açma istekleri sorgulamak için basit bir yol aşağıdaki gibidir:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Performans İzleyicisi sayaçları

Kimlik doğrulama aracılarının izlemek için başka bir kimlik doğrulama Aracısı yüklendiği her sunucuda belirli Performans İzleyicisi sayaçları izlemek için yoludur. Aşağıdaki genel sayaçları kullanın ( **# PTA kimlik doğrulamaları**, **#PTA kimlik doğrulamaları başarısız** ve **#PTA başarılı kimlik doğrulamalarını**) ve hata sayaçları ( **# PTA kimlik doğrulama hataları**):

![Geçişli kimlik doğrulaması Performans İzleyicisi sayaçları](./media/tshoot-connect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Geçişli kimlik doğrulaması kullanarak birden çok kimlik doğrulama aracılarının, yüksek kullanılabilirlik sağlar ve _değil_ Yük Dengeleme. Yapılandırmanıza bağlı olarak _değil_ tüm kimlik doğrulama aracılarının kabaca alma _eşit_ isteklerinin sayısı. Belirli bir kimlik doğrulama Aracısı tüm trafik alır mümkündür.
