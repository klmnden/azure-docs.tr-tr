<properties
    pageTitle="Azure AD Etki Alanı Hizmetleri: Parola eşitlemeyi etkinleştirme | Microsoft Azure"
    description="Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Azure AD Etki Alanı Hizmetleri *(Önizleme)* - Azure AD Etki Alanı Hizmetleri için parola eşitlemeyi etkinleştirme

## Görev 5: Sadece bulutta yer alan Azure AD dizini için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme
Azure AD kiracınız için Azure AD Etki Alanı Hizmetleri'ni etkinleştirdikten sonra, sıradaki görev Azure AD Etki Alanı Hizmetleri'ne kimlik bilgilerinin eşitlenmesini etkinleştirmektir. Bu işlem, kullanıcıların yönetilen etki alanında kurumsal kimlik bilgilerini kullanarak oturum açmalarını sağlar.

Uygulanan adımlar, kuruluşunuzun yalnızca bulutta yer alan bir Azure AD dizinine sahip olmasına veya Azure AD Connect yoluyla şirket içi dizininizle eşitlenmek üzere ayarlanmış olmasına göre değişiklik gösterir.

<br>

> [AZURE.SELECTOR]
- [Yalnızca bulutta yer alan Azure AD dizini](active-directory-ds-getting-started-password-sync.md)
- [Eşitlenmiş Azure AD dizini](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Yalnızca bulutta yer alan Azure AD dizini için NTLM ve Kerberos kimlik bilgisi karması oluşturmayı etkinleştirme
Kuruluşunuz yalnızca bulutta yer alan bir Azure AD dizinine sahipse Azure AD Etki Alanı Hizmetleri'ni kullanması gereken kullanıcıların parolalarını değiştirmesi gerekir. Bu parola değişikliği işlemi, Kerberos ve NTLM kimlik doğrulaması için Azure AD Etki Alanı Hizmetleri'nin gerektirdiği kimlik bilgisi karmalarının Azure AD'de oluşturulmasına neden olur. Kiracıdaki Azure AD Etki Alanı Hizmetleri'ni kullanması gereken tüm kullanıcıların parolalarının süresinin dolmasını sağlayabilir veya bu kullanıcılardan parolalarını değiştirmelerini isteyebilirsiniz.

Son kullanıcılara parolalarını değiştirmeleri için sağlamanız gereken talimatlar şunlardır:

1. Kuruluşunuzun Azure AD Erişim Paneli sayfasına gidin. Bu sayfaya genellikle şu adresten erişilebilir: [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Sayfadaki **profil** sekmesini seçin.

3. Bir parola değişikliğini başlatmak için bu sayfadaki **Parola değiştir** kutucuğuna tıklayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password.png)

4. Bu, **parola değiştir** sayfasını getirir. Kullanıcılar var olan (eski) parolalarını girip parolalarını değiştirmeye geçebilirler.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Kullanıcılar parolalarını değiştirdikten sonra, yeni parola kısa süre içinde Azure AD Etki Alanı Hizmetleri'nde kullanılabilir olur. Birkaç dakika sonra, kullanıcılar yönetilen etki alanına katılan bilgisayarlarında yeni değiştirilen parolalarını kullanarak oturum açabilir.


<br>

## İlgili İçerik

- [Eşitlenmiş Azure AD dizini için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)

- [Windows sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)

- [Red Hat Enterprise Linux sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!--HONumber=Jun16_HO2-->


