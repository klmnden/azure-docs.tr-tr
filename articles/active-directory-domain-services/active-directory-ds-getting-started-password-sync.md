---
title: "Azure AD Etki Alanı Hizmetleri: Parola eşitlemeyi etkinleştirme | Microsoft Belgeleri"
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
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: d75f6a9db55595ab6b40052b8609709eacf30d4e
ms.lasthandoff: 03/07/2017


---
# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure AD Domain Services için parola eşitlemeyi etkinleştirme
Önceki görevlerde Azure AD kiracınız için Azure AD Etki Alanı Hizmetleri’ni etkinleştirdiniz. Sıradaki görev, NTLM ve Kerberos kimlik doğrulamasını Azure AD Etki Alanı Hizmetleri ile eşitlemek için gereken kimlik bilgisi karmalarını etkinleştirmektir. Kimlik bilgisi eşitlemesi ayarlandıktan sonra kullanıcılar, şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilir.

Uygulanan adımlar, kuruluşunuzun yalnızca bulutta yer alan bir Azure AD kiracısına sahip olmasına veya Azure AD Connect yoluyla şirket içi dizininizle eşitlenmek üzere ayarlanmış olmasına göre değişiklik gösterir.

<br>

> [!div class="op_single_selector"]
> * [Yalnızca bulutta yer alan Azure AD kiracısı](active-directory-ds-getting-started-password-sync.md)
> * [Eşitlenmiş Azure AD kiracısı](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Görev 5: Sadece bulutta yer alan Azure AD kiracısı için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme
Azure AD Etki Alanı Hizmetleri, yönetilen etki alanında kullanıcıların kimliklerini doğrulamak için NTLM ve Kerberos kimlik doğrulamasına uygun bir biçime sahip kimlik bilgisi karmalarına gerek duyar. AAD Etki Alanı Hizmetleri’ni kiracınız için etkinleştirmediğiniz sürece Azure AD, NTLM veya Kerberos kimlik doğrulaması için gereken biçimde kimlik bilgisi karmaları oluşturmaz veya depolamaz. Güvenliğe dayalı bariz nedenlerle, Azure AD düz metin biçiminde de hiçbir kimlik bilgisi depolamaz. Bu nedenle, Azure AD’nin kullanıcıların mevcut kimlik bilgilerine dayalı olarak bu NTLM veya Kerberos kimlik bilgisi karmalarını oluşturabileceği bir yol yoktur.

> [!NOTE]
> Kuruluşunuz yalnızca bulutta yer alan bir Azure AD kiracısına sahipse Azure AD Etki Alanı Hizmetleri'ni kullanması gereken kullanıcıların parolalarını değiştirmesi gerekir.
>
>

Bu parola değişikliği işlemi, Kerberos ve NTLM kimlik doğrulaması için Azure AD Etki Alanı Hizmetleri'nin gerektirdiği kimlik bilgisi karmalarının Azure AD'de oluşturulmasına neden olur. Kiracıdaki Azure AD Etki Alanı Hizmetleri'ni kullanması gereken tüm kullanıcıların parolalarının süresinin dolmasını sağlayabilir veya bu kullanıcılardan parolalarını değiştirmelerini isteyebilirsiniz.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Yalnızca bulutta yer alan Azure AD kiracısı için NTLM ve Kerberos kimlik bilgisi karması oluşturmayı etkinleştirme
Son kullanıcılara parolalarını değiştirebilmeleri için sağlamanız gereken yönergeler şunlardır:

1. Kuruluşunuzun [http://myapps.microsoft.com](http://myapps.microsoft.com) adresindeki Azure AD Erişim Paneli sayfasına gidin.
2. Sayfadaki **profil** sekmesini seçin.
3. Bu sayfada **Parola değiştir** kutucuğuna tıklayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Erişim Paneli sayfasında **Parola değiştir** seçeneğini görmüyorsanız, kuruluşunuzun [Azure AD'de parola yönetimini](../active-directory/active-directory-passwords-getting-started.md) yapılandırdığından emin olun.
   >
   >
4. **Parola değiştir** sayfasında mevcut (eski) parolanızı yazın ve ardından yeni bir parola yazıp onaylayın. **Gönder**’e tıklayın.

    ![Azure AD Etki Alanı Hizmetleri için bir sanal ağ oluşturun.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Parolanızı değiştirdikten sonra, yeni parola kısa süre içinde Azure AD Etki Alanı Hizmetleri'nde kullanılabilir hale gelir. Birkaç dakika sonra (genellikle 20 dakika civarı), yönetilen etki alanına katılmış bilgisayarlarda yeni değiştirilen parolayı kullanarak oturum açabilirsiniz.

<br>

## <a name="related-content"></a>İlgili İçerik
* [Kendi parolanızı güncelleştirme](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Azure AD’de Parola Yönetimine başlarken](../active-directory/active-directory-passwords-getting-started.md).
* [Eşitlenmiş Azure AD kiracısı için AAD Etki Alanı Hizmetleri'ne parola eşitlemeyi etkinleştirme](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [Windows sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Red Hat Enterprise Linux sanal makinesini Azure AD Etki Alanı Hizmetleri tarafından yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

