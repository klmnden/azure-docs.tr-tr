---
title: "Azure AD Connect: Önkoşulları ve donanım | Microsoft Docs"
description: "Bu konu ön koşullar ve Azure AD Connect için donanım gereksinimlerini açıklar"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: billmath
ms.openlocfilehash: d6d6eadf0ae8996b019a0564715f843913101944
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD için Önkoşullar Bağlan
Bu konu ön koşullar ve Azure AD Connect için donanım gereksinimlerini açıklar.

## <a name="before-you-install-azure-ad-connect"></a>Azure AD Connect yüklemeden önce
Azure AD Connect yüklemeden önce gereken birkaç nokta vardır.

### <a name="azure-ad"></a>Azure AD
* Bir Azure aboneliği veya bir [Azure deneme aboneliği](https://azure.microsoft.com/pricing/free-trial/). Bu aboneliğin yalnızca olduğu Azure portalına erişim için ve Azure AD Connect kullanmayan için gereklidir. Ardından PowerShell veya Office 365 kullanıyorsanız, Azure AD Connect kullanmak için bir Azure aboneliği gerekmez. Bir Office 365 lisansınız varsa, Office 365 Portalı'nı da kullanabilirsiniz. Ücretli bir Office 365 lisansına sahip Azure Portalı'na Office 365 Portalı'ndan alabilirsiniz.
  * Aynı zamanda [Azure portal](https://portal.azure.com). Bu portalı bir Azure AD lisansı gerektirmez.
* [Ekleme ve etki alanı doğrulama](../active-directory-domains-add-azure-portal.md) Azure AD'de kullanmayı planladığınız. Örneğin, contoso.com kullanıcılarınız için kullanın sonra emin olun planlıyorsanız, bu etki alanı doğrulandı ve yalnızca contoso.onmicrosoft.com varsayılan etki alanı kullanmakta olduğunuz değil.
* Azure AD kiracısı varsayılan 50 k nesneleriyle sağlar. Etki alanınızı doğruladığınızda sınırı 300 k nesnelere artar. Azure AD'de daha da fazla nesneleri ihtiyacınız varsa daha artan sınırı olacak şekilde bir destek servis talebi açmanız gerekir. Sonra 500'den fazla k nesneleri gerekiyorsa, Office 365, Azure AD temel, Azure AD Premium veya Enterprise Mobility ve güvenlik gibi bir lisansınız olmalıdır.
* ADSyncPrep için Azure AD Connect, Active Directory ortamınızı hazırlamak için kullanılan işlevler sağlayan bir PowerShell komut dosyası modülüdür.  ADSyncPrep gerektirir [Azure AD Microsoft çevrimiçi v1.1 PowerShell Modülü](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).  Sürüm 2 çalışmaz.  Modülünü kullanarak yükleyebilirsiniz `Install-Module` cmdlet'i.  Sağlanan bağlantı daha fazla bilgi için bkz.

### <a name="prepare-your-on-premises-data"></a>Şirket içi verilerinizi hazırlama
* Kullanım [Idfix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) Azure AD ile eşitlemeden önce çoğaltmaları ve dizininizde biçimlendirme sorunları gibi hataları belirlemek için ve Office 365.
* Gözden geçirme [isteğe bağlı eşitleme özelliklerini Azure AD içinde etkinleştirebilir](active-directory-aadconnectsyncservice-features.md) ve etkinleştirmelisiniz hangi özelliklerin değerlendirin.

### <a name="on-premises-active-directory"></a>Şirket içi Active Directory
* AD şema sürümü ve orman işlev düzeyi Windows Server 2003 veya üstü olmalıdır. Şema ve orman düzeyi gereksinimleri karşılandığında sürece etki alanı denetleyicileri herhangi bir sürümünü çalıştırabilirsiniz.
* Özelliğini kullanmayı planlıyorsanız, **parola geri yazma**, etki alanı denetleyicileri (en son SP ile) Windows Server 2008 veya üzeri olması gerekir. (R2 öncesi) 2008'de, DC'lerin olan sonra da uygulamalısınız [düzeltme KB2386717](http://support.microsoft.com/kb/2386717).
* Azure AD tarafından kullanılan etki alanı denetleyicisi yazılabilir olmalıdır. Bu **desteklenmiyor** RODC (salt okunur etki alanı denetleyicisi) ve Azure AD Connect herhangi yazma yeniden yönlendirmeleri izleyin değil.
* Bu **desteklenmiyor** şirket içi ormanlar/SLD'ler (tek etiketli etki alanları) kullanarak etki alanlarını kullanmak için.
* Bu **desteklenmiyor** şirket içi ormanlar/etki "noktalı" kullanarak kullanmak için (adı içeren bir süre ".") NetBIOS adları.
* İçin önerilen [Active Directory Geri Dönüşüm Kutusu'nu etkinleştirmek](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect sunucusu
* Azure AD Connect Small Business Server veya Windows Server Essentials yüklenemez. Sunucu Windows Server standard ya da daha iyi kullanmanız gerekir.
* Azure AD Connect sunucusu tam GUI yüklü olması gerekir. Bu **desteklenmiyor** sunucu çekirdeğine yüklenecek.
* Azure AD Connect, Windows Server 2008 veya sonraki sürümlerinde yüklenmelidir. Bu sunucu bir etki alanı denetleyicisi veya üye sunucu hızlı ayarları kullanarak olabilir. Özel ayarları kullanırsanız, sunucunun da tek başına olabilir ve bir etki alanına katılması gerekmez.
* Windows Server 2008 veya Windows Server 2008 R2 üzerinde Azure AD Connect yüklerseniz, Windows Update'ten en son düzeltmeler uygulamak emin olun. Yükleme yüklenmemiş bir sunucuyla başlatmanız mümkün değil.
* Özelliğini kullanmayı planlıyorsanız, **parola eşitleme**, Azure AD Connect sunucusu Windows Server 2008 R2 SP1 veya üzeri olması gerekir.
* Kullanmayı planlıyorsanız, bir **Grup yönetilen hizmet hesabı**, Azure AD Connect sunucusu Windows Server 2012 veya üzeri olması gerekir.
* Azure AD Connect sunucusu olmalıdır [.NET Framework 4.5.1](#component-prerequisites) veya üstü ve [Microsoft PowerShell 3.0](#component-prerequisites) veya sonraki bir sürümü yüklü.
* Azure AD Connect sunucusu PowerShell Transcription Grup İlkesi etkin olmaması gerekir.
* Active Directory Federasyon Hizmetleri dağıtılmışsa, AD FS veya Web uygulaması proxy'si yüklendiği sunucuları Windows Server 2012 R2 olmalıdır veya sonraki bir sürümü. [Windows Uzaktan Yönetim](#windows-remote-management) bu sunuculara uzaktan yükleme için etkinleştirilmesi gerekir.
* Active Directory Federasyon Hizmetleri dağıtılmışsa, gereksinim duyduğunuz [SSL sertifikalarını](#ssl-certificate-requirements).
* Active Directory Federasyon Hizmetleri dağıtıldığından sonra yapılandırmanız gereken [ad çözümlemesi](#name-resolution-for-federation-servers).
* Genel Yöneticiler varsa, MFA etkinse, ardından URL **https://secure.aadcdn.microsoftonline-p.com** Güvenilen siteler listesinde olmalıdır. Bir MFA sınaması için istenir ve bunu önce eklenmedi olduğunda bu siteyi Güvenilen siteler listesine eklemek için istenir. Internet Explorer güvenilen sitelerinize eklemek için kullanabilirsiniz.

### <a name="sql-server-used-by-azure-ad-connect"></a>Azure AD Connect tarafından kullanılan SQL Server
* Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Varsayılan olarak, bir SQL Server 2012 Express LocalDB'nı (SQL Server Express açık bir sürüm) yüklenir. SQL Server Express yaklaşık 100.000 nesneye yönetmenize olanak veren bir 10 GB boyutu sınırı vardır. Dizin nesnelerini daha yüksek bir hacmi yönetmeniz gerekiyorsa, SQL Server'ın farklı bir yükleme için Yükleme Sihirbazı'nı noktası gerekir.
* Ayrı bir SQL Server kullanıyorsanız, bu gereksinimleri geçerlidir:
  * Azure AD Connect SQL Server 2008 (en son hizmet paketiyle) SQL Server 2016 SP1, Microsoft SQL Server'ın tüm sürümlerini destekler. Microsoft Azure SQL veritabanı **desteklenmiyor** veritabanı olarak.
  * Büyük küçük harf duyarsız bir SQL harmanlaması kullanmanız gerekir. Bu harmanlamaları ile tanımlanan bir \_CI_ kendi adı. Bu **desteklenmiyor** tarafından tanımlanan büyük küçük harfe duyarlı bir harmanlamayı kullanacak şekilde \_CS_ kendi adı.
  * Bir eşitleme Altyapısı SQL örneği başına yalnızca olabilir. Bu **desteklenmiyor** bir SQL örneğine FIM/MIM eşitleme, DirSync veya Azure AD eşitleme ile paylaşmak için.

### <a name="accounts"></a>Hesaplar
* Azure AD kiracısı Azure AD genel yönetici hesabı ile tümleştirmek istiyor. Bu hesap olmalıdır bir **okul veya kuruluş hesabı** ve olamaz bir **Microsoft hesabı**.
* Hızlı ayarları kullanın veya Dirsync'ten yükseltme, yerel Active Directory'nizi için bir kuruluş yöneticisi hesabı olması gerekir.
* [Active Directory'de hesapları](active-directory-aadconnect-accounts-permissions.md) özel ayarlarını yükleme yolu kullanır.

### <a name="connectivity"></a>Bağlantı
* Azure AD Connect sunucusu, hem intranet hem de Internet için DNS çözümlemesi gerekir. DNS sunucusu adları hem de şirket içi Active Directory ve Azure AD uç noktaları çözümleyebilmesi olması gerekir.
* Intranet üzerindeki güvenlik duvarları varsa ve Azure AD Connect sunucuları ve etki alanı denetleyicileriniz arasındaki bağlantı noktalarını açın ve ardından görmek gereken [Azure AD Connect bağlantı noktaları](active-directory-aadconnect-ports.md) daha fazla bilgi için.
* Proxy veya güvenlik duvarı sınırlamak hangi URL'leri erişilebilir sonra URL'leri açıklandığı [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açılması gerekir.
  * Microsoft Cloud Almanya içinde veya Microsoft Azure kamu bulut kullanıyorsanız, daha sonra bkz [Azure AD Connect eşitleme hizmeti örnekleri konuları](active-directory-aadconnect-instances.md) URL'ler için.
* Azure AD Connect (sürüm 1.1.614.0 ve sonra) varsayılan olarak TLS 1.2 eşitleme altyapısı ve Azure AD arasındaki iletişimi şifrelemek için kullanır. TLS 1.2 temel işletim sisteminde kullanılabilir durumda değilse, Azure AD Connect artımlı olarak geri eski protokollere (TLS 1.1 ve TLS 1.0) döner. Örneğin, Windows Server 2008 üzerinde çalışan Azure AD Connect TLS 1.0 kullanır, çünkü Windows Server 2008, TLS 1.1 ve TLS 1.2 desteklemiyor.
* Sürüm 1.1.614.0 önce Azure AD Connect varsayılan olarak, TLS 1.0 eşitleme altyapısı ve Azure AD arasındaki iletişimi şifrelemek için kullanır. TLS 1.2 değiştirmek için adımları [Azure AD Connect'i etkinleştirme TLS 1.2](#enable-tls-12-for-azure-ad-connect).
* Aşağıdaki ayarında Internet'e bağlanmak için bir giden proxy kullanıyorsanız, **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** dosyası eklediyseniz, Internet ve Azure AD connect yapabilmek yükleme sihirbazını ve Azure AD Connect eşitleme için. Bu metin dosyanın sonunda girilmesi gerekir. Bu kodda, &lt;PROXYADRESS&gt; gerçek proxy IP adresi veya ana bilgisayar adını temsil eder.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Proxy sunucusu kimlik doğrulaması gerektiriyorsa sonra [hizmet hesabı](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) etki alanında bulunmalıdır ve özelleştirilmiş ayarları yükleme yolunu belirtmek için kullanmanız gerekir bir [özel hizmet hesabı](active-directory-aadconnect-get-started-custom.md#install-required-components). Machine.config farklı bir değişiklik de gerekir. Machine.config bu değişikliği ile Yükleme Sihirbazı'nı ve eşitleme altyapısı kimlik doğrulama isteklerini proxy sunucudan yanıt. Hariç tüm Yükleme Sihirbazı sayfalarındaki **yapılandırma** sayfası, imzalı kullanıcının kimlik bilgileri kullanılır. Üzerinde **yapılandırma** sayfa Yükleme Sihirbazı, bağlam sonunda geçti [hizmet hesabı](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) , tarafından oluşturulmuş. Machine.config bölümü aşağıdaki gibi görünmelidir.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Azure AD Connect dizin eşitlemesi bir parçası olarak Azure AD ile bir web isteği gönderdiğinde, Azure AD yanıt 5 dakika kadar sürebilir. Bağlantı boşta kalma zaman aşımı yapılandırması sağlamak proxy sunucuları için yaygın bir durumdur. Lütfen yapılandırmayı en az 6 dakika veya daha fazla ayarlandığından emin olun.

Daha fazla bilgi için MSDN gördükleri hakkında [proxy öğesi varsayılan](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Bağlantı sorunlarınız, daha fazla bilgi için bkz: [bağlantı sorunlarını giderme](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Diğer
* İsteğe bağlı: eşitleme doğrulamak için bir test kullanıcı hesabı.

## <a name="component-prerequisites"></a>Bileşen önkoşulları
### <a name="powershell-and-net-framework"></a>PowerShell ve .net Framework
Azure AD Connect Microsoft PowerShell ve .NET Framework 4.5.1 bağlıdır. Bu sürüm veya sunucunuzda yüklü sonraki bir sürümü gerekir. Windows Server sürümüne bağlı olarak aşağıdakileri yapın:

* Windows Server 2012R2
  * Microsoft PowerShell varsayılan olarak yüklenir. Hiçbir eylem gerekli değildir.
  * .NET framework 4.5.1 veya sonraki sürümlerinde Windows Update aracılığıyla sunulur. Denetim Masası'ndaki Windows Server için en son güncelleştirmeleri yüklediğinizden emin olun.
* Windows Server 2008R2 ve Windows Server 2012
  * Microsoft PowerShell'in en son sürümünü kullanılabilir **Windows Management Framework 4.0**, kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 ve sonraki sürümlerinde kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * PowerShell en son desteklenen sürümünü kullanılabilir **Windows Management Framework 3.0**, kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 ve sonraki sürümlerinde kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Azure AD Connect için TLS 1.2 etkinleştir
Sürüm 1.1.614.0 önce Azure AD Connect varsayılan olarak, TLS 1.0 eşitleme altyapısı sunucusu ve Azure AD arasındaki iletişimi şifrelemek için kullanır. Bu sunucuda varsayılan olarak TLS 1.2 kullanmak için .net uygulamaları yapılandırarak değiştirebilirsiniz. TLS 1.2 hakkında daha fazla bilgi bulunabilir [Microsoft güvenlik önerisi 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. Windows Server 2008'de TLS 1.2 etkinleştirilemez. Windows Server 2008R2 gerekir ya da daha sonra. İşletim sisteminiz için yüklü .net 4.5.1'in düzeltme Bkz emin [Microsoft güvenlik önerisi 2960358](https://technet.microsoft.com/security/advisory/2960358). Bu düzeltmeyi veya sonraki bir sürümü sunucunuz üzerinde zaten yüklü olabilir.
2. Windows Server 2008R2 kullanırsanız, TLS 1.2 etkin olduğundan emin olun. Windows Server 2012 sunucu ve sonraki sürümlerinde, TLS 1.2 zaten etkinleştirilmiş olmalıdır.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Tüm işletim sistemleri için bu kayıt defteri anahtarını ayarlayın ve sunucuyu yeniden başlatın.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Ayrıca eşitleme altyapısı sunucusu ve uzak bir SQL Server arasında TLS 1.2 etkinleştirmek istediğiniz sonra emin olun yüklü için gerekli sürümlerinden sahip [Microsoft SQL Server için TLS 1.2 desteği](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Federasyon yükleme ve yapılandırma için Önkoşullar
### <a name="windows-remote-management"></a>Windows Uzaktan Yönetim
Azure AD Connect, Active Directory Federasyon Hizmetleri veya Web uygulaması proxy'si dağıtmak için kullanırken bu gereksinimleri denetleyin:

* Hedef sunucunun etki alanına katılmış ise, sonra Windows Uzaktan yönetilen etkinleştirildiğinden emin olun
  * Yükseltilmiş bir PSH komut penceresinde komutunu kullanın `Enable-PSRemoting –force`
* Hedef sunucu ise, WAP makine bir etki alanına katılmış sonra birkaç ek gereksinimler vardır
  * Hedef makinede (WAP makinesi):
    * Winrm emin olun (Windows Uzaktan Yönetimi / WS-Management) hizmeti Hizmetler ek bileşenini çalıştıran
    * Yükseltilmiş bir PSH komut penceresinde komutunu kullanın `Enable-PSRemoting –force`
  * Sihirbazı'nı (hedef makine etki alanına katılmış veya güvenilmeyen etki alanı ise) çalıştığı makinede:
    * Yükseltilmiş bir PSH komut penceresinde komutunu kullanın `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * Sunucu Yöneticisi'nde:
      * DMZ WAP ana makine havuzuna ekleyin (Sunucu Yöneticisi -> Yönet -> sunucuları Ekle...DNS sekmesini kullanın)
      * Sunucu Yöneticisi'ni tüm sunucuları sekmesi: WAP sunucuya sağ tıklayın ve Yönet as..., seçin, WAP makine için yerel (etki alanı değil) kimlik bilgilerini girin
      * Sunucu Yöneticisi'ni tüm sunucuları sekmesinde uzak PSH bağlanabilirliği doğrulamak için: WAP sunucuya sağ tıklayın ve Windows PowerShell'i seçin. Uzak PowerShell oturumlarınızı kurulabilir emin olmak için Uzak PSH oturum açmanız gerekir.

### <a name="ssl-certificate-requirements"></a>SSL sertifikası gereksinimleri
* AD FS grubunuzu tüm düğümlerinin ve tüm Web uygulaması proxy sunucuları arasında aynı SSL sertifikası kullanmak üzere önemle tavsiye.
* Sertifika bir X509 olmalıdır sertifika.
* Bir test laboratuvarı ortamında Federasyon sunucularında otomatik olarak imzalanan sertifika kullanabilirsiniz. Ancak, bir üretim ortamı için genel bir CA'dan sertifika elde öneririz.
  * Genel olarak güvenilir olmayan bir sertifika kullanıyorsanız, her Web uygulaması proxy'si sunucuda yüklü sertifikanın her iki yerel sunucuya ve tüm Federasyon sunucularında güvenilir olduğundan emin olun
* Sertifika kimlik Federasyon Hizmeti adı (örneğin, sts.contoso.com) eşleşmesi gerekir.
  * Kimliktir ya da bir konu alternatif adı (SAN) uzantısı türü dNSName veya SAN girdi yoksa, konu adı ortak ad olarak belirtildi.  
  * Bunlardan birini Federasyon Hizmeti adıyla eşleşen sağlanan birden çok SAN girişleri sertifikasında mevcut olabilir.
  * Çalışma alanına katılma kullanmayı planlıyorsanız, ek bir SAN değeriyle gereklidir **enterpriseregistration.** Kuruluşunuz, örneğin, kullanıcı asıl adı (UPN) soneki tarafından izlenen **enterpriseregistration.contoso.com**.
* CryptoAPI İleri nesil (CNG) anahtarları ve anahtar depolama sağlayıcıları dayalı sertifikalar desteklenmez. Başka bir deyişle, bir CSP (şifreleme hizmeti sağlayıcısı) ve KSP'ye değil (anahtar depolama sağlayıcısı) dayalı bir sertifika kullanmanız gerekir.
* Joker karakteriyle sertifikalar desteklenir.

### <a name="name-resolution-for-federation-servers"></a>Federasyon sunucuları için ad çözümlemesi
* (İç DNS sunucunuzun) intranet ve extranet (ortak DNS etki alanı kayıt aracılığıyla) için AD FS Federasyon Hizmeti adı (örneğin sts.contoso.com) için DNS kayıtlarını ayarlayın. İntranet DNS kaydı için A kullandığınızdan emin olun ve CNAME kayıtlarını değil. Bu etki alanına katılmış makinenizden düzgün çalışması için windows kimlik doğrulaması için gereklidir.
* Birden fazla AD FS sunucusunun veya Web uygulaması Ara sunucusu dağıtıyorsanız, AD FS Federasyon Hizmeti adı (örneğin sts.contoso.com) için DNS kayıtlarını yük dengeleyiciye gelin ve yük dengeleyici yapılandırdığınızdan emin olun.
* Internet Explorer kullanarak, intranet tarayıcı uygulamaları için iş için windows tümleşik kimlik doğrulaması için AD FS Federasyon Hizmeti adı (örneğin sts.contoso.com) IE'de intranet bölgesine eklendiğinden emin olun. Bu Grup İlkesi denetlenen ve değiştirebilirsiniz tüm etki alanına katılmış bilgisayarlara dağıtılır.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect bileşenleri destekleme
Azure AD Connect, Azure AD Connect yüklü olduğu sunucuya yüklenen bileşenlerin bir listesi verilmiştir. İçin temel bir Express yüklemesi listesidir. Farklı bir SQL Server yükleme Eşitleme hizmetlerini sayfasında kullanmayı seçerseniz, SQL Express LocalDB yerel olarak yüklenmedi.

* Azure AD Connect Health
* Microsoft Online Services oturum açma Yardımcısı (hiçbir bağımlılığını ancak yüklenir) BT uzmanları için
* Microsoft SQL Server 2012 komut satırı yardımcı programları
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 yeniden dağıtım paketi

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure AD Connect için donanım gereksinimleri
Aşağıdaki tabloda Azure AD Connect eşitleme bilgisayarın en düşük gereksinimlerini gösterir.

| Active Directory içindeki nesneleri sayısı | CPU | Bellek | Sabit sürücü boyutu |
| --- | --- | --- | --- |
| 10. 000'den az |1.6 GHz |4 GB |70 GB |
| 10,000–50,000 |1.6 GHz |4 GB |70 GB |
| 50,000–100,000 |1.6 GHz |16 GB |100 GB |
| 100.000 ya da daha fazla nesneler için SQL Server'ın tam sürümünü gereklidir | | | |
| 100,000–300,000 |1.6 GHz |32 GB |300 GB |
| 300,000–600,000 |1.6 GHz |32 GB |450 GB |
| Birden fazla 600.000 |1.6 GHz |32 GB |500 GB |

AD FS ve Web uygulama sunucuları çalıştıran bilgisayarlar için en düşük gereksinimler aşağıda verilmiştir:

* CPU: Çift çekirdek 1,6 GHz veya üzeri
* Bellek: 2 GB veya üzeri
* Azure VM: A2 yapılandırma veya üzeri

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
