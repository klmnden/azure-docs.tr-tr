---
title: Kurumsal güvenlik paketi ile Azure HDInsight mimarisi
description: Kurumsal güvenlik paketi ile HDInsight güvenlik planlama hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: 8b8c200979b70e145fca64746547b37dee558848
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720441"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>HDInsight Kurumsal güvenlik paketi kullanma

Standart Azure HDInsight küme tek kullanıcılı bir kümedir. Büyük veri iş yüklerini oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Her kullanıcının isteğe bağlı olarak ayrılmış bir küme oluşturabilir ve artık gerekli olmadığında, yok. 

Çoğu kurum, BT ekipleri kümelerini yönetme çalıştığı bir modele geçiş taşıdık ve birden çok uygulama ekibinin ortak kümelerde takımlar. Bu büyük kuruluşlar, her bir Azure HDInsight kümesinde çok kullanıcılı erişiminin olması gerekir.

HDInsight, yönetilen bir şekilde Active Directory--bir popüler kimlik sağlayıcısını kullanır. HDInsight ile tümleştirerek [Azure Active Directory etki alanı Hizmetleri (Azure AD DS)](../../active-directory-domain-services/overview.md), kümeleri, etki alanı kimlik bilgilerinizi kullanarak erişebilir. 

Sağlanan, etki alanına katılmış HDInsight sanal makinelerin (VM'ler) var. Bu nedenle, HDInsight (Apache Ambari, Apache Hive sunucusu, Apache Ranger, Apache Spark thrift sunucusu ve diğerleri) üzerinde çalışan tüm hizmetler, kimliği doğrulanmış kullanıcı için sorunsuz bir şekilde çalışır. Yöneticiler, daha sonra kümedeki kaynaklar için rol tabanlı erişim denetimi sağlamak için Apache Ranger'ı kullanarak güçlü yetkilendirme ilkeleri oluşturabilirsiniz.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Açık kaynaklı Apache Hadoop kimlik doğrulama ve güvenlik için Kerberos protokolünü kullanır. Bu nedenle, HDInsight küme düğümleri Kurumsal güvenlik paketi (ESP) ile Azure AD DS tarafından yönetilen bir etki alanına katılır. Kerberos güvenlik küme üzerindeki Hadoop bileşenleri için yapılandırılır. 

Aşağıdaki öğeleri otomatik olarak oluşturulur:

- Her bir Hadoop bileşeni için bir hizmet sorumlusu
- etki alanına katılan her makine için bir makine sorumlusu
- bir kuruluş birimi (Bu hizmet ve makine ilkelerini depolamak için OU) her küme için

Özetlemek gerekirse, bir ortam ile ayarlamanız gerekir:

- Bir Active Directory (Azure AD DS tarafından yönetilen) etki alanı. **Etki alanı adı 39 karakteri olmalıdır ya da Azure HDInsight ile çalışmak için daha az.**
- Azure AD DS'de etkinleştirilmiş LDAP (LDAPS) güvenli hale getirin.
- Ayrı sanal ağlar tercih ederseniz Azure AD DS sanal ağ için uygun ağ bağlantısı HDInsight sanal ağdan. HDInsight'ın sanal ağ içindeki bir VM görebilmesi için sanal ağ eşlemesi üzerinden Azure AD DS olması gerekir. Aynı sanal ağda HDInsight ve Azure AD DS dağıtılmışsa, bağlantı otomatik olarak sağlanır ve başka bir eylem gerekmez.

## <a name="set-up-different-domain-controllers"></a>Farklı bir etki alanı denetleyicileri kurun
HDInsight, küme Kerberos iletişimi için kullandığı ana etki alanı denetleyicisi şu anda yalnızca Azure AD DS destekler. Ancak böyle bir kurulum HDInsight erişim için Azure AD DS'yi etkinleştirmek için müşteri adayları sürece diğer karmaşık Active Directory ayarları mümkündür.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/overview.md) Windows Server Active Directory ile tamamen uyumlu olan yönetilen bir etki alanı sağlar. Microsoft yönetme, düzeltme eki uygulama ve etki alanında bir yüksek oranda kullanılabilir (HA) Kurulum izleme üstlenir. Etki alanı denetleyicilerinin bakımını hakkında endişelenmeden, kümeye dağıtabilirsiniz. 

Kullanıcılar, gruplar ve parolaları Azure AD'den eşitlenir. Azure AD DS için Azure AD Örneğinize tek yönlü eşitleme kümeye şirket kimlik bilgilerini kullanarak oturum açmalarını sağlar. 

Daha fazla bilgi için [yapılandırma HDInsight kümeleri ile Azure AD DS kullanarak ESP](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Şirket içi Active Directory veya Iaas Vm'leri üzerinde Active Directory

Etki alanınız için bir şirket içi Active Directory örneğine veya daha karmaşık Active Directory ayarları varsa, Azure AD Connect kullanarak kimliklerle Azure AD'ye eşitleyebilirsiniz. Ardından, Azure AD DS, Active Directory kiracısı üzerinde etkinleştirebilirsiniz. 

Kerberos Parola karmalarının üzerinde kullandığından, şunları yapmalısınız [Azure AD DS parola karması eşitlemeyi etkinleştir](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). 

Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon kullanıyorsanız, parola karma eşitlemesini etkinleştirmeniz gerekir. (Önerilen bir kurulum için bkz: [bu videoyu](https://youtu.be/qQruArbu2Ew).) Parola karma eşitlemesi ile olağanüstü durum kurtarma olasılığına AD FS altyapınızı başarısız olur ve sızmasına kimlik bilgisi koruma sağlamak da yardımcı olur yardımcı olur. Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karma eşitlemesini etkinleştirme](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Kullanarak Active Directory veya Active Directory tek başına, Iaas vm'lerinde Azure AD ile Azure AD DS olmayan olmadan ESP ile HDInsight kümeleri için desteklenen bir yapılandırma şirket içi.

Federasyon kullanılır ve parola karmalarının eşitlenmesini doğru ancak kimlik doğrulama hataları alıyorsanız, PowerShell hizmet sorumlusu için bulut parola kimlik doğrulamasının etkin olup olmadığını denetleyin. Değilse, ayarlamanız gerekir, bir [giriş bölgesi bulma (HRD) İlkesi](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) Azure AD kiracınız için. Denetleyin ve HRD İlkesi ayarlamak için:

1. Önizleme yükleme [Azure AD PowerShell modülünün](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

   ```powershell
   Install-Module AzureAD
   ```

2. Genel yönetici (Kiracı Yöneticisi) kimlik bilgilerini kullanarak bağlanın.
   
   ```powershell
   Connect-AzureAD
   ```

3. Microsoft Azure PowerShell hizmet sorumlusu zaten oluşturulmuş olmadığını kontrol edin.

   ```powershell
   Get-AzureADServicePrincipal -SearchString "Microsoft Azure Powershell"
   ```

4. Yoksa, daha sonra hizmet sorumlusu oluşturun.

   ```powershell
   $powershellSPN = New-AzureADServicePrincipal -AppId 1950a258-227b-4e31-a9cf-717495945fc2
   ```

5. Oluşturma ve bu hizmet sorumlusuna ilke ekleme.

   ```powershell
    # Determine whether policy exists
    Get-AzureADPolicy | Where {$_.DisplayName -eq "EnableDirectAuth"}

    # Create if not exists
    $policy = New-AzureADPolicy `
        -Definition @('{"HomeRealmDiscoveryPolicy":{"AllowCloudPasswordValidation":true}}') `
        -DisplayName "EnableDirectAuth" `
        -Type "HomeRealmDiscoveryPolicy"

    # Determine whether a policy for the service principal exist
    Get-AzureADServicePrincipalPolicy `
        -Id $powershellSPN.ObjectId
    
    # Add a service principal policy if not exist
    Add-AzureADServicePrincipalPolicy `
        -Id $powershellSPN.ObjectId `
        -refObjectID $policy.ID
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [ESP ile HDInsight kümelerini yapılandırma](apache-domain-joined-configure-using-azure-adds.md)
* [ESP ile HDInsight kümeleri için Apache Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md)
* [ESP ile HDInsight kümelerini yönetme](apache-domain-joined-manage.md) 
