---
title: Azure dosyaları (Önizleme) - Azure depolama için SMB üzerinden Azure Active Directory kimlik genel bakış
description: Azure dosyaları, Azure Active Directory (Azure AD) etki alanı Hizmetleri'nden (sunucu ileti bloğu) (Önizleme) SMB üzerinden kimlik tabanlı kimlik doğrulamasını destekler. Etki alanına katılmış Windows sanal makinelerinizi (VM), ardından Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarını erişebilirsiniz.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/18/2019
ms.author: rogarana
ms.openlocfilehash: 21087424be1a7a3edfe2dddcbec830bd74559b23
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269372"
---
# <a name="overview-of-azure-files-azure-active-directory-domain-service-aad-ds-authentication-support-for-smb-access-preview"></a>Azure dosyaları Azure Active Directory etki alanı hizmeti (DS AAD) kimlik doğrulama desteği için SMB erişimi (Önizleme) genel bakış
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosyaları için DS AAD kimlik doğrulamasını etkinleştirme hakkında bilgi için bkz: [etkinleştirme Azure Active Directory etki alanı hizmeti kimlik doğrulaması (Önizleme) Azure dosyaları için SMB üzerinden](storage-files-active-directory-enable.md).

## <a name="glossary"></a>Sözlük 
Azure AD etki alanı hizmeti kimlik doğrulama için SMB üzerinden Azure dosyaları için ilgili bazı önemli terimler anlamak yararlıdır:

-   **Azure Active Directory (Azure AD)**  
    Azure Active Directory (Azure AD), Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD temel Dizin Hizmetleri, uygulama erişim yönetimi ve kimlik korumasını tek bir çözüm birleştirir. Daha fazla bilgi için [Azure Active Directory nedir?](../../active-directory/fundamentals/active-directory-whatis.md)

-   **Azure AD etki alanı Hizmetleri**  
    Azure AD etki alanı Hizmetleri etki alanına katılım, Grup İlkesi, LDAP ve Kerberos/NTLM gibi yönetilen etki alanı hizmetleri sağlayan kimlik doğrulaması. Bu hizmetler, Windows Server Active Directory ile tamamen uyumludur. Daha fazla bilgi için [Azure Active Directory (AD) etki alanı Hizmetleri](../../active-directory-domain-services/overview.md).

-   **Azure rol tabanlı erişim denetimi (RBAC)**  
    Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcıların işlerini yapmak için gereken en az izinleri vererek kaynaklarına erişimi yönetebilir. RBAC hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md)

-   **Kerberos kimlik doğrulaması**

    Kerberos, bir kullanıcının veya ana bilgisayarın kimliğini doğrulamak için kullanılan bir kimlik doğrulama protokolüdür. Kerberos hakkında daha fazla bilgi için bkz. [Kerberos kimlik doğrulamasına genel bakış](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview).

-  **Sunucu İleti Bloğu (SMB) Protokolü**  
    SMB bir endüstri standardı ağ dosyası paylaşım protokolüdür. SMB, ortak Internet dosya sistemi veya CIFS olarak da bilinir. SMB hakkında daha fazla bilgi için bkz. [Microsoft SMB protokolü ve CIFS protokolüne genel bakış](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview).

## <a name="advantages-of-azure-ad-domain-service-authentication"></a>Azure AD etki alanı hizmeti kimlik doğrulama avantajları
Azure AD etki alanı hizmeti kimlik doğrulaması Azure dosyaları için paylaşılan anahtar kimlik doğrulamasını kullanmaya göre çeşitli avantajlar sunar:

-   **Azure AD ile Azure AD ile bulut kimlik tabanlı geleneksel bir dosya paylaşımına erişim deneyimini genişleten etki alanı hizmeti**  
    Uygulamanızı Azure AD kimlik doğrulaması için uygulamanızı isteyebilirsiniz, Azure dosyaları ile geleneksel dosya sunucuları değiştirme buluta "kaldırma ve kaydırma" düşünüyorsanız, dosya verilerine erişmek için kimlik bilgileri. AAD DS SMB üzerinden Azure dosyalarına erişmek için Azure AD kimlik bilgilerini kullanarak azure dosyaları destekleyen etki alanı ile Windows Vm'leri birleşik. Tüm şirket içi Active Directory nesnelerinizi kullanıcı adları, parolalar ve diğer Grup atamalarını korumak için Azure AD'ye eşitlemeyi seçebilirsiniz.

-   **Azure dosya paylaşımları üzerinde ayrıntılı erişim denetimi uygula**  
    Belirli bir kimlik paylaşım, dizin veya dosya düzeyinde izinler verebilirsiniz. Örneğin, birden fazla takım projesi işbirliği için tek bir Azure dosya paylaşımı kullanarak olduğunu varsayın. Yalnızca finans ekibi için hassas finansal verileri içeren dizinler erişimi sınırlandırırken hassas olmayan dizinleri, tüm takımlar erişim izni verebilirsiniz. 

-   **ACL'ler yanı sıra verilerinizi yedekleme**  
    Mevcut şirket içi dosya paylaşımlarını yedeklemek için Azure dosyaları'nı kullanabilirsiniz. Azure dosyaları, dosya yedekleme Azure dosyaları'na SMB üzerinden paylaştığınızda, ACL'ler birlikte verilerinizi korur.

## <a name="how-it-works"></a>Nasıl çalışır?
Azure dosyaları, etki alanına katılmış vm'lerden Azure AD kimlik bilgileriyle Kerberos kimlik doğrulamasını desteklemek için Azure AD Domain Services kullanır. Azure AD ile Azure dosyaları kullanmadan önce öncelikle Azure AD Domain Services'ı etkinleştir ve dosya verilerine erişmek planlama vm'lerden etki alanına katılın. Aynı sanal ağ (VNET) Azure AD Domain Services etki alanına katılmış sanal makinenizin bulunmalıdır. 

Bir sanal makine üzerinde çalışan bir uygulama ile ilişkilendirilmiş bir kimliği Azure dosyaları'nda verilere erişmeye çalıştığında istek kimliğini doğrulamak için Azure AD Domain Services için gönderilir. Kimlik doğrulaması başarılı olursa, Azure AD Domain Services Kerberos belirteci döndürür. Uygulama Kerberos belirteci içeren bir istek gönderir ve Azure dosyaları isteği yetkilendirmek için bu belirteci kullanır. Azure dosyaları, yalnızca belirteci alır ve Azure AD kimlik devam etmez.

![SMB üzerinden Azure AD kimlik doğrulamasının ekran gösteren diyagram](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-azure-ad-domain-service-authentication-for-smb-access"></a>Azure AD etki alanı hizmeti kimlik doğrulama için SMB erişimini etkinleştirin
24 Eylül 2018'den sonra oluşturulan yeni ve var olan depolama hesaplarında Azure dosyaları için Azure AD etki alanı hizmeti kimlik doğrulamasını etkinleştirebilirsiniz. 

Bu özelliği etkinleştirmeden önce Azure AD Domain Services için birincil dağıtıldığını doğrulayın, depolama hesabınız olduğu ilişkili Azure AD kiracısı. Azure AD Domain Services ' henüz ayarlamadıysanız, sağlanan adım adım kılavuzu izleyin [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/create-instance.md).

Azure AD etki alanı Hizmetleri dağıtım genellikle 10-15 dakika sürer. Azure AD Domain Services dağıtılan sonra Azure dosyaları için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirebilirsiniz. Daha fazla bilgi için [SMB üzerinden Azure Active Directory etki alanı hizmetini etkinleştir kimlik doğrulaması için Azure dosyaları (Önizleme)](storage-files-active-directory-enable.md). 

### <a name="configure-share-level-permissions-for-azure-files"></a>Azure dosyaları paylaşım düzeyi izinlerini yapılandırma
Azure AD etki alanı hizmeti kimlik doğrulama etkinleştirildikten sonra Azure AD kimlikleri için özel bir RBAC rolleri yapılandırabilir ve depolama hesabındaki tüm dosya paylaşımları için erişim hakları atayın.

Bir Azure dosya paylaşımını veya bir dizin veya dosya erişmek etki alanına katılmış bir VM'de çalışan bir uygulama çalıştığında, uygun paylaşım düzeyi izinleri ve NTFS izinleri emin olmak için uygulamanın Azure AD Kimlik doğrulandı. Paylaşım düzeyi izinlerini yapılandırma hakkında daha fazla bilgi için bkz: [SMB (Önizleme) üzerinden etkinleştirme Azure Active Directory etki alanı hizmeti kimlik doğrulaması](storage-files-active-directory-enable.md).

### <a name="configure-directory--or-file-level-permissions-for-azure-files"></a>Azure dosyaları için dizin veya dosya düzeyinde izinleri yapılandırma 
Azure dosyaları, standart NTFS dosya izinleri kök dizininde dahil olmak üzere dizin ve dosya düzeyinde zorlar. Dizin veya dosya düzeyinde izinlerin yapılandırmasını yalnızca SMB üzerinden desteklenir. Hedef dosya paylaşımı, VM'den bağlayabilir ve Windows kullanarak izinlerini yapılandırma [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) veya [kümesi ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) komutu. 

> [!NOTE]
> Windows dosya Gezgini aracılığıyla NTFS izinleri yapılandırma Önizleme sürümünde desteklenmiyor.

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Depolama hesabı anahtarını süper kullanıcı izinlerini kullanın. 
Depolama hesabı anahtarını işlediği bir kullanıcı, süper kullanıcı izinlerine sahip Azure dosyaları erişebilir. Süper kullanıcı izinlerine RBAC ile paylaşım düzeyinde yapılandırılır ve Azure AD tarafından zorlanan tüm erişim denetimi kısıtlamalarını aşan. Bir Azure dosya paylaşımını bağlayabilmeniz için süper kullanıcı izinleri gereklidir. 

> [!IMPORTANT]
> Güvenlik için en iyi bir parçası olarak depolama hesap anahtarlarınızı paylaşmaktan kaçınmanızı ve Azure AD izinleri mümkün olduğunca yararlanın.

### <a name="preserve-directory-and-file-acls-for-data-import-to-azure-file-shares"></a>Azure dosya paylaşımları için veri alma için dizin ve dosya ACL'leri koru
Azure dosyaları destekleyen Azure dosya paylaşımlarını veri kopyaladığınızda, dizin veya dosya ACL'leri koruma. Azure dosyaları için ACL'leri bir dizin veya dosya çubuğunda kopyalayabilirsiniz. Örneğin, kullanabileceğiniz [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) bayrağıyla `/copy:s` hem verileri hem de ACL'ler için bir Azure dosya paylaşımı kopyalamak için. ACL korunması varsayılan olarak açıktır ve depolama hesabınızda Azure AD etki alanı hizmeti kimlik doğrulama özelliğini açıkça etkinleştirmeniz gerekmez. 

## <a name="pricing"></a>Fiyatlandırma
Azure AD kimlik doğrulaması üzerinden SMB depolama hesabınızı etkinleştirmek için ek bir hizmet ücret yoktur. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure dosyaları fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/files/) ve [Azure AD Domain Services fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-ds/) sayfaları.

## <a name="next-steps"></a>Sonraki Adımlar
SMB üzerinden Azure dosyaları ve Azure AD kimlik doğrulaması hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure dosyaları'na giriş](storage-files-introduction.md)
- [Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme) için etkinleştirin.](storage-files-active-directory-enable.md)
- [SSS](storage-files-faq.md)
