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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: b7b5f92c0093faa96a367fc95d459b1babd99789
ms.lasthandoff: 04/28/2017


---
# <a name="enable-password-synchronization-with-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme
Önceki görevlerde Azure Active Directory (Azure AD) kiracınız için Azure Active Directory Domain Services (AD DS) hizmetini etkinleştirdiniz. Sıradaki görev, NT LAN Manager (NTLM) ve Kerberos kimlik doğrulamasını Azure Active Directory Domain Services ile eşitlemek için gereken kimlik bilgisi karmalarını etkinleştirmektir. Kimlik bilgisi eşitlemesini ayarladıktan sonra kullanıcılar, şirket kimlik bilgileri ile yönetilen etki alanında oturum açabilir.

Yordamlar, kuruluşunuzun yalnızca bulut Azure AD kiracısı olmasına veya Azure AD Connect kullanarak şirket içi dizininizle eşitlenecek şekilde ayarlanmasına bağlı olarak farklılık gösterir.

> [!div class="op_single_selector"]
> * [Yalnızca bulutta yer alan Azure AD kiracısı](active-directory-ds-getting-started-password-sync.md)
> * [Eşitlenmiş Azure AD kiracısı](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

## <a name="task-5-enable-password-synchronization-with-azure-active-directory-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Görev 5: Sadece bulutta yer alan Azure AD kiracısı için Azure Active Directory Domain Services'e parola eşitlemeyi etkinleştirme
Yönetilen etki alanındaki kullanıcıların kimliklerini doğrulamak için, Azure Active Directory Domain Services’ın NTLM ve Kerberos kimlik doğrulamasına uygun biçimde kimlik bilgisi karmaları olmalıdır. Azure Active Directory Domain Services’ı kiracınız için etkinleştirmediğiniz sürece Azure AD, NTLM veya Kerberos kimlik doğrulaması için gereken biçimde kimlik bilgisi karmaları oluşturmaz veya depolamaz. Güvenliğe dayalı bariz nedenlerle, Azure AD düz metin biçiminde de hiçbir kimlik bilgisi depolamaz. Bu nedenle, Azure AD’nin kullanıcıların mevcut kimlik bilgilerine dayalı olarak bu NTLM veya Kerberos kimlik bilgisi karmalarını oluşturabileceği bir yol yoktur.

> [!NOTE]
> Kuruluşunuz yalnızca bulutta yer alan bir Azure AD kiracısına sahipse Azure Active Directory Domain Services'i kullanması gereken kullanıcıların parolalarını değiştirmesi gerekir.
>
>

Bu parola değişikliği işlemi, Kerberos ve NTLM kimlik doğrulaması için Azure Active Directory Domain Services'in gerektirdiği kimlik bilgisi karmalarının Azure AD'de oluşturulmasına neden olur. Kiracıdaki Azure Active Directory Domain Services'i kullanması gereken tüm kullanıcıların parolalarının süresinin dolmasını sağlayabilir veya bu kullanıcılardan parolalarını değiştirmelerini isteyebilirsiniz.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Yalnızca bulutta yer alan Azure AD kiracısı için NTLM ve Kerberos kimlik bilgisi karması oluşturmayı etkinleştirme
Kullanıcılara parolalarını değiştirebilmeleri için sağlamanız gereken yönergeler şunlardır:

1. Kuruluşunuzun [Azure AD Erişim Paneli](http://myapps.microsoft.com) sayfasına gidin.
2. Erişim Paneli penceresinde **profil** sekmesini seçin.
3. **Parola değiştir** kutucuğuna tıklayın.

    ![Azure AD Erişim Paneli "Parola değiştir" kutucuğu](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Erişim Paneli penceresinde **Parola değiştir** seçeneği gösterilmiyorsa, kuruluşunuzun [Azure AD'de parola yönetimini](../active-directory/active-directory-passwords-getting-started.md) yapılandırdığından emin olun.
   >
   >
4. **Parola değiştir** sayfasında mevcut (eski) parolanızı yazın ve ardından yeni bir parola yazıp onaylayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. **Gönder**’e tıklayın.

Parolanızı değiştirdikten birkaç dakika sonra, yeni parola Azure Active Directory Domain Services ile kullanılabilir. Birkaç dakika daha geçtikten sonra (genellikle 20 dakika civarı), yönetilen etki alanına katılmış bilgisayarlarda yeni değiştirilen parolayı kullanarak oturum açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Kendi parolanızı güncelleştirme](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Azure AD’de Parola Yönetimi kullanmaya başlama](../active-directory/active-directory-passwords-getting-started.md)
* [Eşitlenmiş Azure AD kiracısı için Azure Active Directory Domain Services ile parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Azure Active Directory Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Windows sanal makinesini Azure Active Directory Domain Services tarafından yönetilen bir etki alanına ekleme](active-directory-ds-admin-guide-join-windows-vm.md)
* [Red Hat Enterprise Linux sanal makinesini Azure Active Directory Domain Services tarafından yönetilen bir etki alanına ekleme](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

