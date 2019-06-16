---
title: SSS - Azure Active Directory etki alanı Hizmetleri | Microsoft Docs
description: Azure Active Directory Domain Services hakkında sık sorulan sorular
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mstephen
ms.openlocfilehash: 89930bcc4f49a406586c2770f65cc65f81387302
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246112"
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory etki alanı Hizmetleri: Sık Sorulan Sorular (SSS)
Bu sayfa, Azure Active Directory Domain Services hakkında sık sorulan sorular yanıtlanmaktadır. Geri güncelleştirmeleri kontrol etmeyi unutmayın.

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu
Başvurmak [sorun giderme kılavuzu](troubleshoot.md) yapılandırma veya Azure AD Domain Services'ı yönetme ile sık karşılaşılan sorunlara çözümler için.

## <a name="configuration"></a>Yapılandırma
### <a name="can-i-create-multiple-managed-domains-for-a-single-azure-ad-directory"></a>Tek bir yönetilen birden çok etki alanı oluşturmak Azure AD directory?
Hayır. Tek bir Azure AD Domain Services tarafından hizmet verilen tek bir yönetilen etki alanı oluşturabilmeniz için Azure AD dizini.  

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Azure AD Domain Services, Azure Resource Manager sanal ağında etkinleştirebilirim?
Evet. Azure AD etki alanı Hizmetleri, bir Azure Resource Manager sanal ağında etkinleştirilebilir. Klasik Azure sanal ağları yeni yönetilen etki alanları oluşturmak için artık desteklenmemektedir.

### <a name="can-i-migrate-my-existing-managed-domain-from-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Mevcut yönetilen etki alanım klasik bir sanal ağdan bir Resource Manager sanal ağına geçişini sağlayabilir miyim?
Şu anda değil. Microsoft bir Resource Manager sanal ağı klasik bir sanal ağdan mevcut yönetilen etki alanınızı gelecekte geçirmek için bir mekanizma sunar.

### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-csp-cloud-solution-provider-subscription"></a>Bir Azure CSP (bulut çözümü sağlayıcısı) aboneliği Azure AD Etki Alanı Hizmetleri'nde etkinleştirebilirim?
Evet. Nasıl imkan tanıdığını öğrenin [Azure CSP aboneliklerinde Azure AD Etki Alanı Hizmetleri'nde](csp.md).

### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-do-not-synchronize-password-hashes-to-azure-ad-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Azure AD Domain Services Federasyon azure'da etkinleştirebilirim AD directory? Parola karmalarının Azure AD'ye eşitlenmez. Bu dizin için Azure AD Domain Services etkinleştirebilirim?
Hayır. Azure AD etki alanı Hizmetleri üzerinden NTLM veya Kerberos kullanıcı kimlik doğrulaması için kullanıcı hesaplarının, parola karmaları erişmesi gerekir. Bir Federasyon dizinde parola karmalarının Azure AD dizininde depolanmaz. Bu nedenle, Azure AD Domain Services çalışmıyor gibi Azure AD dizin.

### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Azure AD Domain Services birden çok sanal ağlarda kullanılabilir Aboneliğim dahilindeki oluşturabilir miyim?
Hizmet, bu senaryo doğrudan desteklemez. Yönetilen etki alanınızla aynı anda yalnızca bir sanal ağda kullanılabilir. Ancak, diğer sanal ağlara Azure AD Domain Services'ı göstermek için birden çok sanal ağlar arasında bağlantı yapılandırabilirsiniz. Öğrenmek [azure'da sanal ağları birbirine bağlama](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>PowerShell kullanarak Azure AD Domain Services etkinleştirebilirim?
Evet. Bkz: [PowerShell kullanarak Azure AD etki alanı hizmetleri nasıl](powershell-create-instance.md).

### <a name="can-i-enable-azure-ad-domain-services-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak Azure AD Domain Services etkinleştirebilirim?
Hayır, bir şablon kullanarak Azure AD Domain Services'ı etkinleştirmek şu anda mümkün değildir. Bunun yerine PowerShell kullanmak, bkz: [PowerShell kullanarak Azure AD etki alanı hizmetleri nasıl](powershell-create-instance.md).

### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD Domain Services yönetilen etki alanına etki alanı denetleyicileri ekleyebilir miyim?
Hayır. Azure AD Domain Services tarafından sağlanan etki alanı, yönetilen bir etki alanıdır. Bu yönetim etkinlikleri, sağlama, yapılandırma veya etki alanı denetleyicileri bu etki alanı - yönetmek gerekmez Microsoft tarafından bir hizmet olarak sağlanır. Bu nedenle, yönetilen etki alanınız için ek etki alanı denetleyicileri (salt okunur veya salt okunur) ekleyemezsiniz.

### <a name="can-guest-users-invited-to-my-directory-use-azure-ad-domain-services"></a>Konuk kullanıcıları davet için Dizinimi Azure AD Domain Services ile kullanabilir miyim?
Hayır. Konuk kullanıcıları davet kullanarak, Azure AD directory [Azure AD B2B](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) davet işlemi, Azure AD Domain Services yönetilen etki alanına eşitlenir. Ancak, bu kullanıcıların parolalarını Azure AD dizininizde depolanmaz. Bu nedenle, Azure AD etki alanı Hizmetleri NTLM eşitlemeye hiçbir yolu yoktur ve Kerberos, yönetilen etki alanına bu kullanıcılar için karma hale getirir. Sonuç olarak, bu kullanıcılar yönetilen etki alanına oturum açın veya bilgisayarların yönetilen etki alanına katılmasını sağlamak.

## <a name="administration-and-operations"></a>Yönetim ve işlemler
### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Uzak Masaüstü kullanarak yönetilen etki alanım için etki alanı denetleyicisine bağlanabilir miyim?
Hayır. Uzak Masaüstü aracılığıyla yönetilen etki alanı için etki alanı denetleyicisine bağlanmak için gerekli izinlere sahip değil. 'AAD DC Administrators' grubunun üyeleri, yönetilen etki alanı AD yönetim araçları gibi Active Directory Yönetim Merkezi (ADAC) veya AD PowerShell kullanarak yönetebilirsiniz. Bu araçlar, yönetilen etki alanına katılmış bir Windows server üzerinde 'Uzak sunucu Yönetim Araçları' özelliği kullanılarak yüklenir.

### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ben Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiniz. Bu etki alanı için etki alanı katılma makinelere hangi kullanıcı hesabı kullanabilir?
Yönetim 'AAD DC Administrators' grubunun üyeleri, etki alanına katılım makineler olabilir. Ayrıca, bu grubun üyeleri, etki alanına katılmış makinelerde Uzak Masaüstü erişimi verilir.

### <a name="do-i-have-domain-administrator-privileges-for-the-managed-domain-provided-by-azure-ad-domain-services"></a>Azure AD Domain Services tarafından sağlanan yönetilen etki alanı için etki alanı yöneticisi ayrıcalıkları var mı?
Hayır. Yönetilen etki alanı üzerinde yönetim ayrıcalıkları verilmez. Hem ' etki alanı Yöneticisi ' ve ' Kurumsal Yönetici ' ayrıcalıkları, etki alanı içinde kullanmanız için kullanılamaz. Etki alanı yönetici veya şirket içi Active directory'nizde Kurumsal Yönetici gruplarından üyeler de yönetilen etki alanındaki etki alanı/Kurumsal Yönetici ayrıcalıkları verilmez.

### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Yönetilen etki alanlarında LDAP veya AD yönetim araçlarını kullanarak grup üyeliklerini değiştirebilir miyim?
Hayır. Azure AD Domain Services tarafından hizmet verilen etki alanlarında grup üyeliği değiştirilemez. Aynı kullanıcı öznitelikleri için geçerlidir. Azure AD'de ya da şirket içi etki alanınızdaki grup üyelikleri veya kullanıcı öznitelikleri ancak değişebilir. Bu tür değişiklikler Azure AD Domain Services için otomatik olarak eşitlenir.

### <a name="how-long-does-it-take-for-changes-i-make-to-my-azure-ad-directory-to-be-visible-in-my-managed-domain"></a>Ne kadar süreyle sürer değişiklikleri miyim yönetilen etki alanım görünür olması için Azure AD directory dizinimde olun?
Azure AD kullanıcı Arabirimi veya PowerShell kullanarak Azure AD dizininizde yapılan değişiklikler, yönetilen Etki Alanınızla eşitlenir. Bu eşitleme işlemi arka planda çalışır. İlk eşitleme tamamlandıktan sonra genellikle yönetilen etki alanınızda yansıtılması için Azure AD'de yapılan değişiklikler için yaklaşık 20 dakika sürer.

### <a name="can-i-extend-the-schema-of-the-managed-domain-provided-by-azure-ad-domain-services"></a>Ben Azure AD Domain Services tarafından sağlanan yönetilen etki alanında şemasını uzatabilir miyim?
Hayır. Şema, yönetilen etki alanı için Microsoft tarafından yönetilir. Şema uzantıları, Azure AD Domain Services tarafından desteklenmez.

### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Ben değiştirebilir veya my yönetilen etki alanında DNS kayıtları eklemek?
Evet. 'AAD DC Administrators' grubunun üyeleri, yönetilen etki alanında DNS kayıtları değiştirmek için ' DNS Yöneticisi ' ayrıcalıklarını verilir. DNS Yöneticisi konsolunu çalıştıran Windows Server'ın yönetilen etki alanına katılmış bir makinede DNS yönetmek için kullanabilirler. DNS Yöneticisi konsolunu kullanmak için 'DNS sunucusu sunucuda 'Uzak sunucu Yönetim Araçları' isteğe bağlı özelliği parçası olan Araçlar','i yükleyin. Daha fazla bilgi [yönetme, izleme ve DNS sorun giderme için yardımcı programlar](https://technet.microsoft.com/library/cc753579.aspx) TechNet üzerindeki kullanılabilir.

### <a name="what-is-the-password-lifetime-policy-on-a-managed-domain"></a>Yönetilen etki alanında Parola ömrü ilkesi nedir?
Azure AD etki alanı üzerinde varsayılan Parola ömrü Services yönetilen etki alanında 90 gündür. Bu parola yaşam süresi Azure AD'de yapılandırılmış Parola ömrü ile eşitlenmemiş. Bu nedenle, burada kullanıcıların parolalarını yönetilen etki alanınızda süresi dolacak, ancak Azure AD'de hala geçerli bir durum olabilir. Böyle senaryolarda, kullanıcıların Azure AD'de parola değiştirmesi gerekmez ve yeni parolayı yönetilen Etki Alanınızla eşitleme yapar. Ayrıca, 'parola-mu-not-zaman aşımı' ve 'user-must-change-password-at-next-logon' öznitelikleri kullanıcı hesapları için yönetilen Etki Alanınızla eşitlenmez.

### <a name="does-azure-ad-domain-services-provide-ad-account-lockout-protection"></a>Azure AD Domain Services AD hesap kilitleme korumasına sağlar?
Evet. Yönetilen etki alanındaki beş geçersiz parola denemelerinin 2 dakika içinde bir kullanıcı hesabı, 30 dakika boyunca kilitlenmesine neden. 30 dakika sonra kullanıcı hesabının kilidi otomatik olarak. Yönetilen etki alanındaki geçersiz parola denemelerinin Azure AD'de kullanıcı hesabının kilitleneceği değil. Kullanıcı hesabı yalnızca Azure AD Domain Services yönetilen etki alanı içinde kilitli.

## <a name="billing-and-availability"></a>Faturalandırma ve kullanılabilirliği
### <a name="is-azure-ad-domain-services-a-paid-service"></a>Azure AD etki alanı Hizmetleri Ücretli bir hizmet mi?
Evet. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory-ds/).

### <a name="is-there-a-free-trial-for-the-service"></a>Hizmeti için ücretsiz deneme sürümü var mı?
Bu hizmet, Azure için ücretsiz deneme bulunmaktadır. Oturum açabileceğiniz bir [ücretsiz bir aylık deneme sürümü Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="can-i-pause-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen etki alanı duraklatarak miyim? 
Hayır. Bir Azure AD Domain Services yönetilen etki alanı etkinleştirildikten sonra devre dışı bırak/Sil yönetilen etki alanı kadar hizmet seçilen sanal ağda kullanılabilir. Hizmeti duraklatma hiçbir yolu yoktur. Yönetilen etki alanı silene kadar faturalama, saatlik olarak devam eder.

### <a name="can-i-failover-azure-ad-domain-services-to-another-region-for-a-dr-event"></a>Alabilirim DR olay için Azure AD Domain Services için başka bir bölgeye yük devretme?
Hayır.  Azure AD etki alanı Hizmetleri şu anda sağlamaz coğrafi olarak yedekli dağıtım modeli. Bir Azure bölgesi içinde tek bir sanal ağ için sınırlıdır. Birden fazla Azure bölgesini kullanmak istiyorsanız, Active Directory etki alanı denetleyicilerinizi Azure Iaas Vm'lerinde çalıştırmak gerekir.  Mimari Kılavuzu bulunabilir [burada](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-extend-domain).

### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Enterprise Mobility Suite (EMS) bir parçası olarak Azure AD Domain Services alabilir miyim? Azure AD Domain Services'ı kullanmak için Azure AD Premium ihtiyacım var?
Hayır. Azure AD etki alanı Hizmetleri, bir Kullandıkça Öde Azure hizmeti olan ve EMS'nin parçası değildir. Azure AD etki alanı Hizmetleri ile Azure AD'nin tüm sürümleri kullanılabilir (ücretsiz, temel ve, Premium). Kullanımına bağlı olarak bir saatlik olarak faturalandırılırsınız.

### <a name="what-azure-regions-is-the-service-available-in"></a>Hangi Azure bölgeleri kullanılabilir hizmet?
Başvurmak [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services/) Azure AD Domain Services'in kullanılabildiği Azure bölgelerini listesini görmek için sayfayı.
