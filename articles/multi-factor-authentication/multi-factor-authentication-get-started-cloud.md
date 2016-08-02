<properties 
    pageTitle="Bulutta Microsoft Azure Multi-Factor Authentication kullanmaya başlama" 
    description="Bu, bulutta nasıl Microsoft Azure MFA kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Bulutta Azure Multi-Factor Authentication kullanmaya başlama
Aşağıdaki makalede bulutta Azure Multi-Factor Authentication kullanmaya nasıl başlayacağınızı öğrenirsiniz.

> [AZURE.NOTE]  Aşağıdaki belgeler kullanıcıların **Klasik Azure Portalı** kullanarak nasıl etkinleştirileceğine ilişkin bilgi sağlar. O365 kullanıcıları için Azure Multi-Factor Authentication kurulumuna ilişkin bilgileri arıyorsanız, bkz. [Office 365 için multi-factor authentication kurulumu.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![Bulutta MFA](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## Ön koşullar
Aşağıdaki ön koşullar, kullanıcılarınız için Azure Multi-Factor Authentication’ı etkinleştirebilmeniz için gereklidir. 




- [Azure aboneliği için kaydolun](https://azure.microsoft.com/pricing/free-trial/) - Zaten bir Azure aboneliğiniz yoksa, birisi için kaydolmalısınız. Yeni başlıyor ve Azure MFA’yı henüz kullanmaya başlıyorsanız, deneme aboneliğini kullanabilirsiniz.
2. [Multi-Factor Auth Sağlayıcısı oluşturun](multi-factor-authentication-get-started-auth-provider.md) ve dizininize atayın veya [kullanıcılara lisans atayın](multi-factor-authentication-get-started-assign-licenses.md). 

> [AZURE.NOTE]  Lisanlar, Azure MFA, Azure AD Premium ya da Enterprise Mobility Suite (EMS) sahibi kullanıcıların kullanımına sunulmuştur.  MFA, Azure AD Premium ve EMS’de yer almaktadır. Yeterli lisansa sahipseniz, Kimlik Doğrulaması Sağlayıcısı oluşturmanız gerekmez. 
        

## Kullanıcılar için multi-factor authentication’ı açma
Bir kullanıcı için multi-factor authentication’ı açmak için, kullanıcının durumunu devre dışı iken etkin olarak değiştirmeniz yeterlidir.  Kullanıcı durumları hakkında daha fazla bilgi için bkz. [ Azure Multi-Factor Authentication’da kullanıcı durumları](multi-factor-authentication-get-started-user-states.md)

Kullanıcılarınız için MFA'yı etkinleştirmek üzere aşağıdaki yordamı kullanın.

### Multi-factor authentication’ı açmak için
--------------------------------------------------------------------------------
1.  **Klasik Azure portalında** Yönetici olarak oturum açın.
2.  Solda, **Active Directory**'ye tıklayın.
3.  **Dizin** altında etkinleştirmek istediğiniz kullanıcının dizinine tıklayın.
![Dizin'e tıklayın](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Üst kısımda, **Kullanıcılar**’a tıklayın.
5.  Sayfanın altında, **Multi-Factor Auth’ı Yönet**’e tıklayın.
![Dizin'e tıklayın](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Bu yeni bir tarayıcı sekmesi açar.  multi-factor authentication için etkinleştirmek istediğiniz kullanıcıyı bulun. Üst kısımdaki görünümü değiştirmeniz gerekebilir. Durumun **devre dışı** olduğundan emin olun.
![Kullanıcı etkinleştir](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Kutuda, adının yanına bir **onay işareti** koyun.
7.  Sağda, **Etkinleştir**’e tıklayın. 
![Kullanıcı etkinleştir](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  **Multi-Factor Auth'u etkinleştir** seçeneğine tıklayın.
![Kullanıcı etkinleştir](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Kullanıcı durumunun **devre dışı** iken **etkin** olarak değiştiğini fark etmelisiniz.
![Kullanıcıları Etkinleştirme](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Kullanıcılarınızı etkinleştirdikten sonra, bunlara e-posta ile bildirimde bulunmanız önerilir.  Bu e-posta ayrıca, kilitlenmelerini önlemek için tarayıcı olmayan uygulamalarını nasıl kullanabileceklerini açıklamalıdır.


## PowerShell kullanarak multi-factor authentication’ı açmayı otomatik hale getirme

[Azure AD PowerShell](powershell-install-configuremd) kullanarak [durumu](multi-factor-authentication-whats-next.md) değiştirmek için, aşağıdakileri kullanabilirsiniz.  Aşağıdaki durumlardan birine eşit olacak şekilde `$st.State` öğesini değiştirebilirsiniz:


- Etkin
- Uygulandı
- Devre dışı  

> [AZURE.IMPORTANT]  Lütfen, kullanıcı MFA kaydından geçmediği ve bir [uygulama parolası](multi-factor-authentication-whats-next.md#app-passwords) almadığından, Devre dışı durumundan Uygulandı durumuna doğrudan giderseniz, modern olmayan kimlik doğrulaması istemcilerinin çalışmayı bırakacağını unutmayın.  Modern olmayan kimlik doğrulaması istemcileriniz varsa ve uygulama parolaları istiyorlarsa, Devre dışı durumundan Etkin duruma getirmeniz önerilir.  Bu, kullanıcıların uygulama parolalarını kaydetmelerine ve almalarına izin verir.   
        
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

PowerShell kullanmak toplu etkinleştirme kullanıcıları için bir seçenek olabilir.  Şu anda Azure portalda toplu etkinleştirme özelliği yoktur ve her kullanıcıyı ayrı ayrı seçmeniz gerekir.  Birçok kullanıcınız varsa bu yorucu bir görev olabilir.  Yukarıdakini kullanarak bir PowerShell betiği oluşturarak, bir kullanıcı listesinde sayım yaparak bunları etkinleştirebilirsiniz.  Örnek aşağıda verilmiştir:
    
    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Kullanıcı durumları hakkında daha fazla bilgi için bkz. [ Azure Multi-Factor Authentication’da kullanıcı durumları](multi-factor-authentication-get-started-user-states.md)

## Sonraki Adımlar
Bulutta multi-factor authentication özelliğini ayarladığınıza göre, şimdi dağıtımınızı yapılandırıp ayarlayabilirsiniz.  Bkz. [Azure Multi-Factor Authentication.]



<!---HONumber=Jun16_HO2-->


