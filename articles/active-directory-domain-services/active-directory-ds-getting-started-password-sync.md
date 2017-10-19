---
title: "Azure Active Directory Domain Services: Parola eşitlemeyi etkinleştirme | Microsoft Docs"
description: "Azure Active Directory Etki Alanı Hizmetleri ile çalışmaya başlama"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: maheshu
ms.openlocfilehash: c0cd24e03c24655adfe851bc85b721c0b617efcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme
Önceki görevlerde Azure Active Directory (Azure AD) kiracınız için Azure Active Directory Domain Services hizmetini etkinleştirdiniz. Sıradaki görev, NT LAN Manager (NTLM) ve Kerberos kimlik doğrulaması için gereken kimlik bilgisi karmalarının Azure AD Domain Services ile eşitlemesini etkinleştirmektir. Kimlik bilgisi eşitlemesini ayarladıktan sonra kullanıcılar, şirket kimlik bilgileri ile yönetilen etki alanında oturum açabilir.

İşlemin adımları, yalnızca bulutta yer alan kullanıcı hesapları ile Azure AD Connect kullanılarak şirket içi dizininizden eşitlenmiş hesaplar arasında farklılık gösterir. 

<br>
| **Kullanıcı hesabı türü** | **Gerçekleştirilecek adımlar** |
| --- |---|
| **Azure AD'de oluşturulan bulut kullanıcı hesapları** |**&#x2713;** [Bu makaledeki talimatları izleyin](active-directory-ds-getting-started-password-sync.md#task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts) |
| **Şirket içi dizinden eşitlenen kullanıcı hesapları** |**&#x2713;** [Yönetilen etki alanınıza şirket içi AD’nizden eşitlenmiş kullanıcı hesaplarının parolalarını eşitleyin](active-directory-ds-getting-started-password-sync-synced-tenant.md) | 

<br>

> [!TIP]
> **İki kümedeki adımları da gerçekleştirmeniz gerekir.**
> Azure AD kiracınızda yalnızca bulut kullanıcılarıyla ve şirket içi AD kullanıcılarının bir bileşimi varsa her iki işlemin de adımlarını takip etmeniz gerekir.
>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts"></a>5. Görev: Yönetilen etki alanınıza yalnızca bulutta yer alan kullanıcı hesaplarının parolalarını eşitlemeyi etkinleştirme
Yönetilen etki alanındaki kullanıcıların kimliklerini doğrulamak için, Azure Active Directory Domain Services’ın NTLM ve Kerberos kimlik doğrulamasına uygun biçimde kimlik bilgisi karmaları olmalıdır. Kiracınız için Azure Active Directory Domain Services’ı etkinleştirene kadar, Azure AD, NTLM veya Kerberos kimlik doğrulaması için gereken biçimde kimlik bilgisi karmaları oluşturmaz veya depolamaz. Güvenliğe dayalı bariz nedenlerle, Azure AD düz metin biçiminde de hiçbir parola kimlik bilgisi depolamaz. Bu nedenle, Azure AD’nin kullanıcıların mevcut kimlik bilgileri temelinde otomatik olarak bu NTLM veya Kerberos kimlik bilgisi karmalarını oluşturabileceği bir yol yoktur.

> [!NOTE]
> **Kuruluşunuz yalnızca bulutta yer alan bir kullanıcı hesabına sahipse Azure Active Directory Domain Services'i kullanması gereken tüm kullanıcıların parolalarını değiştirmesi gerekir.** Yalnızca bulutta yer alan bir kullanıcı hesabı, Azure portal veya Azure AD PowerShell cmdlet’leri kullanılarak Azure AD dizininizde oluşturulmuş bir hesaptır. Bu tür kullanıcı hesapları şirket içi dizinden eşitlenmez.
>
>

Bu parola değişikliği işlemi, Kerberos ve NTLM kimlik doğrulaması için Azure Active Directory Domain Services'in gerektirdiği kimlik bilgisi karmalarının Azure AD'de oluşturulmasına neden olur. Kiracıdaki Azure Active Directory Domain Services'i kullanması gereken tüm kullanıcıların parolalarının süresinin dolmasını sağlayabilir veya bu kullanıcılardan parolalarını değiştirmelerini isteyebilirsiniz.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Yalnızca bulutta yer alan kullanıcı hesabı için NTLM ve Kerberos kimlik bilgileri karması oluşturmayı etkinleştirme
Kullanıcılara parolalarını değiştirebilmeleri için sağlamanız gereken yönergeler şunlardır:

1. Kuruluşunuzun [Azure AD Erişim Paneli](http://myapps.microsoft.com) sayfasına gidin.

    ![Azure AD erişim panelini başlatma](./media/active-directory-domain-services-getting-started/access-panel.png)

2. Sağ üst köşede adınıza tıklayın ve menüden **Profil**’i seçin.

    ![Profil seçme](./media/active-directory-domain-services-getting-started/select-profile.png)

3. **Profil** sayfasında **Parola değiştir**’e tıklayın.

    ![“Parola değiştir”e tıklayın](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!TIP]
   > Erişim Paneli penceresinde **Parola değiştir** seçeneği gösterilmiyorsa, kuruluşunuzun [Azure AD'de parola yönetimini](../active-directory/active-directory-passwords-getting-started.md) yapılandırdığından emin olun.
   >
   >
4. **Parola değiştir** sayfasında mevcut (eski) parolanızı yazın ve ardından yeni bir parola yazıp onaylayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. **Gönder**’e tıklayın.

Parolanızı değiştirdikten birkaç dakika sonra, yeni parola Azure Active Directory Domain Services ile kullanılabilir. Yaklaşık 20 dakika sonra, yönetilen etki alanına katılmış bilgisayarlarda yeni değiştirilen parolayı kullanarak oturum açabilirsiniz.

## <a name="related-content"></a>İlgili İçerik
* [Kendi parolanızı güncelleştirme](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Azure AD’de Parola Yönetimi kullanmaya başlama](../active-directory/active-directory-passwords-getting-started.md)
* [Eşitlenmiş Azure AD kiracısı için Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Azure Active Directory Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Windows sanal makinesini Azure Active Directory Domain Services tarafından yönetilen bir etki alanına ekleme](active-directory-ds-admin-guide-join-windows-vm.md)
* [Red Hat Enterprise Linux sanal makinesini Azure Active Directory Domain Services tarafından yönetilen bir etki alanına ekleme](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
