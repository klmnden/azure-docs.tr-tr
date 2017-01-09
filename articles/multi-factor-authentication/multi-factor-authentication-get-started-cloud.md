---
title: "Bulutta Azure MFA’yı kullanmaya başlama | Microsoft Belgeleri"
description: "Bu, bulutta nasıl Microsoft Azure MFA kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: c6fe00b72d95a3eb40d91f6f7989b7163518c46f


---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Bulutta Azure Multi-Factor Authentication kullanmaya başlama
Bu makalede bulutta Azure Multi-Factor Authentication kullanmaya nasıl başlayacağınız gösterilmektedir.

> [!NOTE]
> Aşağıdaki belgeler kullanıcıların **Klasik Azure Portalı** kullanarak nasıl etkinleştirileceğine ilişkin bilgi sağlar. O365 kullanıcıları için Azure Multi-Factor Authentication kurulumuna ilişkin bilgileri arıyorsanız, bkz. [Office 365 için multi-factor authentication kurulumu.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![Bulutta MFA](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki ön koşullar, kullanıcılarınız için Azure Multi-Factor Authentication’ı etkinleştirebilmeniz için gereklidir.

1. [Azure aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/) - Zaten bir Azure aboneliğiniz yoksa, birisi için kaydolmalısınız. Yeni başlıyor ve Azure MFA’yı henüz kullanmaya başlıyorsanız, deneme aboneliğini kullanabilirsiniz.
2. [Multi-Factor Auth Sağlayıcısı oluşturun](multi-factor-authentication-get-started-auth-provider.md) ve dizininize atayın veya [kullanıcılara lisans atayın](multi-factor-authentication-get-started-assign-licenses.md).

> [!NOTE]
> Lisanlar, Azure MFA, Azure AD Premium ya da Enterprise Mobility Suite (EMS) sahibi kullanıcıların kullanımına sunulmuştur.  MFA, Azure AD Premium ve EMS’de yer almaktadır. Yeterli lisansa sahipseniz, Kimlik Doğrulaması Sağlayıcısı oluşturmanız gerekmez.

## <a name="turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı açma
Bir kullanıcıdan iki aşamalı doğrulama istemeye başlamak için kullanıcının devre dışı olan durumunu etkin olarak değiştirin.  Kullanıcı durumları hakkında daha fazla bilgi için bkz. [ Azure Multi-Factor Authentication’da kullanıcı durumları](multi-factor-authentication-get-started-user-states.md)

Kullanıcılarınız için MFA'yı etkinleştirmek üzere aşağıdaki yordamı kullanın.

### <a name="to-turn-on-multi-factor-authentication"></a>Multi-factor authentication’ı açmak için
1. [Klasik Azure portalında](https://manage.windowsazure.com) yönetici olarak oturum açın.
2. Solda, **Active Directory**'ye tıklayın.
3. Dizin altında etkinleştirmek istediğiniz kullanıcının dizinini seçin.
   ![Dizin’e tıklayın](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Üst kısımda, **Kullanıcılar**’a tıklayın.
5. Sayfanın altında, **Multi-Factor Auth’ı Yönet**’e tıklayın. Yeni bir tarayıcı sekmesi açılır.
   ![Dizin’e tıklayın](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. İki aşamalı doğrulamayı etkinleştirmek istediğiniz kullanıcıyı bulun. Üst kısımdaki görünümü değiştirmeniz gerekebilir. Durumun **devre dışı** olduğundan emin olun.
   ![Kullanıcıyı etkinleştirme](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. Kutuda, adının yanına bir **onay işareti** koyun.
8. Sağda, **Etkinleştir**’e tıklayın.
   ![Kullanıcıyı etkinleştirme](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. **Multi-Factor Auth'u etkinleştir** seçeneğine tıklayın.
   ![Kullanıcıyı etkinleştirme](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Kullanıcı durumunun **devre dışı** yerine **etkin** olduğuna dikkat edin.
    ![Kullanıcıları Etkinleştirme](./media/multi-factor-authentication-get-started-cloud/user.png)

Kullanıcılarınızı etkinleştirdikten sonra, e-posta ile bildirimde bulunmanız gerekir. Kullanıcılardan sonraki oturum açma girişimleri sırasında iki aşamalı doğrulamaya kaydolmaları istenecektir. İki aşamalı doğrulamayı kullanmaya başlayan kullanıcıların tarayıcı harici uygulamaların devre dışı kalmaması için uygulama parolaları oluşturmaları da gerekecektir.

## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>İki aşamalı doğrulamayı açmak için PowerShell kullanma
[Azure AD PowerShell](/powershell/azureps-cmdlets-docs) kullanarak [durumu](multi-factor-authentication-whats-next.md) değiştirmek için aşağıdakileri kullanabilirsiniz.  Aşağıdaki durumlardan birine eşit olacak şekilde `$st.State` öğesini değiştirebilirsiniz:

* Etkin
* Uygulandı
* Devre dışı  

> [!IMPORTANT]
> Kullanıcıların Devre dışı durumunun doğrudan Zorlanmış olarak değiştirilmesini önermiyoruz. Kullanıcı MFA kaydı gerçekleştirmediğinden ve [uygulama parolası](multi-factor-authentication-whats-next.md#app-passwords) edinmediğinden, tarayıcı tabanlı olmayan uygulamalar devre dışı kalacaktır. Tarayıcı tabanlı olmayan uygulamalarını varsa ve uygulama parolaları istiyorlarsa, Devre dışı durumundan Etkin duruma geçmeniz önerilir. Bu, kullanıcıların kaydolmalarına ve uygulama parolalarını almalarına izin verir. Kullanıcıları bu adımdan sonra Zorlanmış duruma geçirebilirsiniz.

PowerShell kullanmak toplu etkinleştirme kullanıcıları için bir seçenek olabilir. Şu anda Azure portalda toplu etkinleştirme özelliği yoktur ve her kullanıcıyı ayrı ayrı seçmeniz gerekir. Çok sayıda kullanıcınız varsa bu yorucu bir görev olabilir. Aşağıdakilerle bir PowerShell betiği oluşturarak, bir kullanıcı listesinde sayım yaparak bunları etkinleştirebilirsiniz.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Örnek aşağıda verilmiştir:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication’da kullanıcı durumları](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Sonraki Adımlar
Bulutta Azure Multi-Factor Authentication özelliğini ayarladığınıza göre, şimdi dağıtımınızı yapılandırıp ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication’ı yapılandırma](multi-factor-authentication-whats-next.md).




<!--HONumber=Dec16_HO1-->


