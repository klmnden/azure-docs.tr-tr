---
title: "Azure yığın uygulama hizmeti dağıtmadan önce | Microsoft Docs"
description: "Azure yığın uygulama hizmeti dağıtmadan önce tamamlamak için adımlar"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: 17967131853d4334ae2c0ba3c0aa01089b7f3b61
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce

Azure uygulama hizmeti Azure yığında çeşitli dağıtımdan önce tamamlanması gereken önkoşul adımları vardır:

- Azure uygulama hizmeti Azure yığın yardımcı komut dosyaları indirme
- Yüksek kullanılabilirlik
- Azure uygulama hizmeti Azure yığında için gerekli sertifikalar
- Dosya sunucusu hazırlayın
- SQL Server hazırlayın
- Azure Active Directory Uygulama oluşturma
- Active Directory Federasyon Hizmetleri uygulama oluşturma

## <a name="download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts"></a>Azure uygulama hizmeti Azure yığın yükleyici ve yardımcı komut dosyaları indirme

1. Karşıdan [Azure yığın dağıtım Yardımcısı betikleri uygulama hizmeti](https://aka.ms/appsvconmashelpers).
2. Karşıdan [yükleyici Azure yığın uygulama hizmeti](https://aka.ms/appsvconmasinstaller).
3. Yardımcı komut dosyaları zip dosyasından dosyaları ayıklayın. Aşağıdaki dosyalar ve klasör yapısı görünür:
  - Common.ps1
  - Oluşturma AADIdentityApp.ps1
  - Oluşturma ADFSIdentityApp.ps1
  - Oluşturma AppServiceCerts.ps1
  - Get-AzureStackRootCert.ps1
  - Remove-AppService.ps1
  - Modüller
    - GraphAPI.psm1
    
## <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure uygulama hizmeti Azure yığında şu anda Azure yığın yalnızca bir tek hata etki alanı iş yüklerinin dağıtır için yüksek kullanılabilirlik sunar mümkün değil.

Azure uygulama hizmeti Azure yığında yüksek kullanılabilirlik için hazırlamak üzere gerekli dosya sunucusu ve SQL Server yüksek oranda kullanılabilir bir yapılandırmaya dağıtılmasını emin olun. Azure yığın birden çok hata etki alanlarını desteklediğinde, biz Azure uygulama hizmeti Azure yığında yüksek oranda kullanılabilir bir yapılandırmada etkinleştirme hakkında kılavuzluk sağlar.


## <a name="certificates-required-for-azure-app-service-on-azure-stack"></a>Azure uygulama hizmeti Azure yığında için gerekli sertifikalar

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Azure yığın Geliştirme Seti için gerekli sertifikalar

Bu ilk komut, App Service tarafından gerekli olan dört sertifikalar oluşturmak üzere Azure yığın sertifika yetkilisi ile çalışır.

| Dosya adı | Kullanım |
| --- | --- |
| _.appservice.Local.azurestack.external.pfx | Uygulama hizmeti varsayılan SSL sertifikası |
| Api.appservice.Local.azurestack.external.pfx | Uygulama hizmeti API SSL sertifikası |
| FTP.appservice.Local.azurestack.external.pfx | Uygulama hizmeti yayımcı SSL sertifikası |
| SSO.appservice.Local.azurestack.external.pfx | Uygulama hizmeti kimliği uygulama sertifika |

Azure yığın Geliştirme Seti konakta komut dosyasını çalıştırın ve PowerShell azurestack\CloudAdmin çalıştırdığınızdan emin olun.

1. Azurestack\AzureStackAdmin çalışan bir PowerShell oturumunda Yardımcısı betikleri ayıkladığınız klasöründen oluşturma AppServiceCerts.ps1 betiğini yürütün. Komut dosyası, uygulama hizmeti gereken sertifikaları komut dosyası oluşturma ile aynı klasörde dört sertifikaları oluşturur.
2. .Pfx dosyaları korumak için bir parola girin ve bunu not edin. Uygulama hizmeti Azure yığın yükleyici girmelisiniz.

#### <a name="create-appservicecertsps1-parameters"></a>Oluşturma AppServiceCerts.ps1 parametreleri

| Parametre | Gerekli/isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| PfxPassword | Gerekli | Null | Sertifikanın özel anahtarını korumak için kullanılan parola |
| domainName | Gerekli | Local.azurestack.external | Azure yığın bölge ve etki alanı soneki |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Azure uygulama hizmeti Azure yığında Üretim dağıtımı için gerekli sertifikalar

Üretim kaynak sağlayıcısındaki çalıştırmak için aşağıdaki dört sertifikaları belirtmeniz gerekir:

#### <a name="default-domain-certificate"></a>Varsayılan etki alanı sertifikası

Varsayılan etki alanı sertifikası ön uç rolüne yerleştirilir. Azure App Service'e joker veya varsayılan etki alanı istekleri için kullanıcı uygulamalar tarafından kullanılır. Sertifika Ayrıca kaynak denetimi işlemleri (KUDU) için kullanılır.

Sertifika .pfx biçiminde olmalıdır ve iki konulu bir joker sertifika olmalıdır. Bu, hem varsayılan etki alanı hem de tek bir sertifika kapsamında olmasını kaynak denetimi işlemleri için scm uç noktasının sağlar.

| Biçim | Örnek |
| --- | --- |
| \*.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | \*. appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*. appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>API sertifika

API sertifika yönetimi rolüne yerleştirilir ve kaynak sağlayıcısı tarafından API çağrıları güvenliğini sağlamak için kullanılır. Yayımlama sertifikası API DNS girişi ile eşleşen bir konu içermelidir:

| Biçim | Örnek |
| --- | --- |
| api.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Yayımlama sertifikası

İçerik yüklediklerinde yayımcı rolünün sertifikası için uygulama sahipleri FTPS trafiğinin güvenliğini sağlar.  Yayımlama sertifikası FTPS DNS girişi ile eşleşen bir konu içermesi gerekir.

| Biçim | Örnek |
| --- | --- |
| FTP.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | api.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Kimlik sertifikası

Kimlik uygulama için sertifika sağlar:
- AAD/ADFS dizin, Azure yığını ve işlem kaynak sağlayıcısı ile tümleştirmeyi desteklemek için uygulama hizmeti arasında tümleştirme
- Çoklu oturum açma senaryoları için Azure yığında Azure App Service içinde Gelişmiş geliştirici araçları.
Sertifika kimliği şu biçimde eşleşen bir konu içermelidir:

| Biçim | Örnek |
| --- | --- |
| SSO.appservice. \<bölge\>.\< DomainName\>.\< uzantısı\> | SSO.appservice.redmond.azurestack.external |

### <a name="extract-the-azure-stack-azure-resource-manager-root-certificate"></a>Azure yığın Azure Kaynak Yöneticisi kök sertifikasını ayıklayın

Azurestack\CloudAdmin çalışan bir PowerShell oturumunda Yardımcısı betikleri ayıkladığınız klasöründen Get-AzureStackRootCert.ps1 betiğini yürütün. Komut dosyası, uygulama hizmeti gereken sertifikaları komut dosyası oluşturma ile aynı klasörde dört sertifikaları oluşturur.

| Get-AzureStackRootCert.ps1 parametresi | Gerekli/isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Gerekli | AzS ERCS01 | Ayrıcalıklı uç noktası. |
| CloudAdminCredential | Gerekli | AzureStack\CloudAdmin | Azure yığın bulut yönetici etki alanı hesabı kimlik bilgileri |


## <a name="prepare-the-file-server"></a>Dosya sunucusu hazırlayın

Azure uygulama hizmeti bir dosya sunucusu kullanılmasını gerektirir. Üretim dağıtımları için dosya sunucusu yüksek oranda kullanılabilir olması için yapılandırılmış ve hatalarını işleme, özelliğine sahip olması gerekir.

Yalnızca Azure yığın Geliştirme Seti dağıtımları ile kullanım için yapılandırılmış tek düğümlü dosya sunucusu dağıtmak için bu örnek Azure Resource Manager dağıtım şablonu kullanabilirsiniz: https://aka.ms/appsvconmasdkfstemplate. Tek düğümlü dosya sunucusu bir çalışma grubunda olacaktır.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Gruplar ve hesaplar Active Directory'de sağlama


1. Aşağıdaki Active Directory genel güvenlik gruplarını oluşturun:
    - FileShareOwners
    - FileShareUsers
2. Hizmet hesapları olarak aşağıdaki Active Directory hesaplarını oluşturun:
    - FileShareOwner
    - FileShareUser

    Güvenlik en iyi uygulama, bu hesaplar için (ve tüm Web rollerinin) kullanıcıları birbirinden farklı ve güçlü kullanıcı adları ve parolalar olması gerekir.
    Parolaları aşağıdaki koşullarla ayarlayın:
     - Etkinleştirme **parola her zaman geçerli olsun**.
     - Etkinleştirme **kullanıcı parolayı değiştiremez**.
     - Devre dışı **kullanıcı bir sonraki oturumda parola değiştirmeli**.
3. Hesapları grup üyeliklerine aşağıdaki gibi ekleyin:
    - Ekleme **FileShareOwner** için **FileShareOwners** grubu.
    - Ekleme **FileShareUser** için **FileShareUsers** grubu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Gruplar ve hesaplar bir çalışma grubunda sağlama

>[!NOTE]
> Dosya sunucusu, bir yönetici komut istemi oturumunda yapılandırırken tüm aşağıdaki komutları çalıştırın.  **PowerShell kullanmayın.**

Yukarıdaki Azure Resource Manager şablonu kullanarak, kullanıcılar zaten oluşturulur.

1. FileShareOwner ve FileShareUser hesapları oluşturmak için aşağıdaki komutları çalıştırın. Değiştir <password> kendi değerlere sahip.
``` DOS
net user FileShareOwner <password> /add /expires:never /passwordchg:no
net user FileShareUser <password> /add /expires:never /passwordchg:no
```
2. Aşağıdaki WMIC aşağıdaki komutlarını çalıştırarak dolmayacak hesaplar için parolaları ayarlayın:
``` DOS
WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
```
3. FileShareUsers ve FileShareOwners yerel gruplarını oluşturun ve hesapları ilk adımı eklemelisiniz.
``` DOS
net localgroup FileShareUsers /add
net localgroup FileShareUsers FileShareUser /add
net localgroup FileShareOwners /add
net localgroup FileShareOwners FileShareOwner /add
```

### <a name="provision-the-content-share"></a>İçerik paylaşımı sağlama

İçerik paylaşımı Kiracı web sitesi içeriğini içerir. Tek dosya sunucusunda içerik paylaşımı sağlama yordamı, Active Directory'deki bir yük devretme kümesi için farklı ancak Active Directory ve çalışma grubu ortamları için aynıdır.

#### <a name="provision-the-content-share-on-a-single-file-server-ad-or-workgroup"></a>Tek dosya sunucusunda içerik paylaşımı sağlama (AD veya çalışma grubu)

Tek dosya sunucusunda, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın. Değer 'C:\WebSites için' ortamınızdaki karşılık gelen yollarla değiştirin.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="to-enable-winrm-add-the-fileshareowners-group-to-the-local-administrators-group"></a>WinRM etkinleştirmek için yerel Administrators grubuna FileShareOwners grubunu ekleyin

Sırayla düzgün çalışması Windows Uzaktan Yönetim için yerel Administrators grubuna FileShareOwners grubunu eklemeniz gerekir.

#### <a name="active-directory"></a>Active Directory

Aşağıdaki komutlar, dosya sunucusunda veya her dosya sunucusu yük devretme kümesi düğümünde yükseltilmiş bir komut isteminde çalıştırın. Değeri Değiştir <DOMAIN> kullanmak istediğiniz etki alanı adına sahip.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Çalışma Grubu

Aşağıdaki komutu yükseltilmiş bir komut isteminde dosya sunucusunda çalıştırın.

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Paylaşımlara erişim denetimini yapılandırın

Aşağıdaki komutlar, dosya sunucusunda veya geçerli küme kaynak sahibi olan dosya sunucusu yük devretme kümesi düğümünde yükseltilmiş bir komut isteminde çalıştırın. İtalik değerleri ortamınıza özgü değerlerle değiştirin.

#### <a name="active-directory"></a>Active Directory
```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Çalışma Grubu
```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server"></a>SQL Server hazırlayın

Azure uygulama hizmeti Azure yığın barındırma ve ölçüm veritabanları, Azure uygulama hizmeti veritabanları tutmak için bir SQL Server hazırlamanız gerekir.

Azure yığın Geliştirme Seti ile kullanmak için SQL Express 2014 SP2 kullanabilirsiniz veya sonraki bir sürümü.

Üretim ve yüksek düzeyde kullanılabilirlik sağlamak için SQL 2014 SP2 tam bir sürümünü kullanmanız veya daha sonra karma mod kimlik doğrulamasını etkinleştirmek ve gerekir dağıtmanız bir [yüksek oranda kullanılabilir yapılandırma](https://docs.microsoft.com/en-us/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

Azure uygulama hizmeti Azure yığın SQL Server'daki tüm uygulama hizmeti rollerden erişilebilir olması gerekir. SQL Server, varsayılan sağlayıcı abonelik Azure yığınında içinde dağıtılabilir. Veya yapabilirsiniz, kuruluşunuzdaki mevcut altyapısının (var olduğu sürece Azure yığın bağlantı) kullanın. Bir Azure Market görüntüsü kullanıyorsanız, güvenlik duvarı uygun şekilde yapılandırmak unutmayın. 

SQL Server rollerini birini için varsayılan bir örnek veya adlandırılmış bir örnek kullanabilirsiniz. Ancak, adlandırılmış bir örnek kullanırsanız, el ile SQL Browser hizmetini başlatmak ve bağlantı noktası 1434 açın emin olun.

## <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

Aşağıdakileri desteklemek için bir Azure AD hizmet sorumlusu yapılandırın:
- Sanal makine ölçek tümleştirme üzerinde çalışan katmanları ayarlayın.
- SSO için Azure işlevleri Geliştirici Portalı ve Gelişmiş araçlar.

Bu adımlar yalnızca Azure yığın ortamları güvenli Azure AD için geçerlidir.

Yöneticiler için SSO yapılandırmanız gerekir:
- Uygulama Hizmeti (Kudu) içinde Gelişmiş geliştirici araçları sağlar.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\AzureStackAdmin açın.
2. İndirilen ve içinde ayıklanan komut dosyalarının konumuna gidin [önkoşul adım](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Azure yığın PowerShell Yükle](azure-stack-powershell-install.md).
4. Çalıştırma **oluşturma AADIdentityApp.ps1** komut dosyası. Azure AD Kiracı Kimliğinizi için istendiğinde Azure yığın dağıtımınız için örneğin, myazurestack.onmicrosoft.com kullanmakta olduğunuz Azure AD Kiracı kimliği girin.
5. İçinde **kimlik bilgisi** penceresinde, Azure AD hizmet yönetici hesabı ve parolasını girin. **Tamam** düğmesine tıklayın.
6. Sertifika dosyası yolu ve sertifika parolasını girin [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika **sso.appservice.local.azurestack.external.pfx**.
7. Komut dosyası Azure AD kiracısında yeni bir uygulama oluşturur. PowerShell çıkışı döndürülen uygulama Kimliğini not edin. Yükleme sırasında bu bilgileri gerekir.
8. Yeni bir tarayıcı penceresi açın ve oturum açın Azure portalında (portal.azure.com) olarak **Azure Active Directory Hizmet Yöneticisi**.
9. Azure AD kaynak sağlayıcısı açın.
10. Tıklatın **uygulama kayıtlar**.
11. Arama **uygulama kimliği** adım 7 bir parçası olarak döndürdü. Bir uygulama hizmeti uygulaması listelenir.
12. Tıklatın **uygulama** listesinde
13. Tıklatın **gerekli izinleri** > **izinleri verin** > **Evet**.

| Oluşturma AADIdentityApp.ps1 parametresi | Gerekli/isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| DirectoryTenantName | Gerekli | Null | Azure AD Kiracı kimliği. GUID veya dize, örneğin, myazureaaddirectory.onmicrosoft.com sağlayın |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Kaynak Yöneticisi uç noktası, örneğin adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Gerekli | Null | Kiracı Azure Kaynak Yöneticisi uç noktası, örneğin: management.local.azurestack.external |
| AzureStackAdminCredential | Gerekli | Null | Azure AD hizmet yönetici kimlik bilgileri |
| CertificateFilePath | Gerekli | Null | Daha önce oluşturulan kimlik uygulama sertifika dosyasının yolu. |
| CertificatePassword | Gerekli | Null | Sertifikanın özel anahtarını korumak için kullanılan parola. |

## <a name="create-an-active-directory-federation-services-application"></a>Active Directory Federasyon Hizmetleri uygulama oluşturma

AD FS tarafından güvenliği sağlanan Azure yığın ortamlar için aşağıdakileri desteklemek için bir AD FS hizmet sorumlusu yapılandırmanız gerekir:
- Sanal makine ölçek tümleştirme üzerinde çalışan katmanları ayarlayın.
- SSO için Azure işlevleri Geliştirici Portalı ve Gelişmiş araçlar.

Yöneticiler için SSO yapılandırmanız gerekir:
- Sanal makine ölçek kümesi tümleştirmesi için bir hizmet sorumlusu üzerinde çalışan katmanları yapılandırın.
- Uygulama Hizmeti (Kudu) içinde Gelişmiş geliştirici araçları sağlar.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\azurestackadmin açın.
2. İndirilen ve içinde ayıklanan komut dosyalarının konumuna gidin [önkoşul adım](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Azure yığın PowerShell Yükle](azure-stack-powershell-install.md).
4.  Çalıştırma **oluşturma ADFSIdentityApp.ps1** komut dosyası.
5.  İçinde **kimlik bilgisi** penceresinde, AD FS bulut yönetici hesabı ve parolasını girin. **Tamam** düğmesine tıklayın.
6.  Sertifika dosyası yolu ve sertifika parolasını sağlamak [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika sso.appservice.local.azurestack.external.pfx yok.

| Oluşturma ADFSIdentityApp.ps1 parametresi | Gerekli/isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Örneğin, adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Gerekli | Null | Ayrıcalıklı uç noktası. Örneğin, AzS ERCS01. |
| CloudAdminCredential | Gerekli | Null | Azure yığın cloudadmin etki alanı hesabı kimlik bilgileri. Örneğin, Azurestack\CloudAdmin. |
| CertificateFilePath | Gerekli | Null | Kimlik uygulamanın Sertifika PFX dosyasının yolu. |
| CertificatePassword | Gerekli | Null | Sertifikanın özel anahtarını korumak için kullanılan parola. |


## <a name="next-steps"></a>Sonraki adımlar

[Uygulama hizmeti kaynak sağlayıcısı yüklemeniz](azure-stack-app-service-deploy.md).
