---
title: Azure Active Directory'yi Azure MFA sunucusu - yükseltme
description: Adımlar ve daha yeni bir sürüme Azure multi-Factor Authentication Sunucusu'nu Yükseltme Kılavuzu.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c01c7a22800d633696382687feb7090a4ed8b60
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60358336"
---
# <a name="upgrade-to-the-latest-azure-multi-factor-authentication-server"></a>En son Azure multi-Factor Authentication Sunucusu'na yükseltme

Bu makalede Azure multi-Factor Authentication (MFA) sunucusu v6.0 yükseltme sürecini veya daha yüksek sizi yönlendirir. PhoneFactor Aracısı'nın eski bir sürümüne yükseltmeniz gerekirse, başvurmak [PhoneFactor Aracısı, Azure multi-Factor Authentication Sunucusu'na yükseltme](howto-mfaserver-deploy-upgrade-pf.md).

V6.x veya v7.x eski veya yeni yükseltiyorsanız, tüm bileşenleri .NET 2. 0 ' .NET 4.5 olarak değiştirin. Tüm bileşenleri ayrıca Microsoft Visual C++ 2015 yeniden dağıtılabilir güncelleştirme 1 veya üzeri gerekir. Bunlar zaten yüklü değilse MFA sunucusu Yükleyici bu bileşenlerin x86 ve x64 sürümlerini yükler. Kullanıcı Portalı ve mobil uygulama Web Hizmeti'nin ayrı sunucularda çalıştırırsanız, bu bileşenlerin yükseltmeden önce bu paketleri yüklemeniz gerekir. En son Microsoft Visual C++ 2015 yeniden dağıtılabilir güncelleştirme için arama yapabilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/). 

Bir bakışta yükseltme adımları:

* Azure MFA sunucularını (Astları sonra ana) yükseltin
* Kullanıcı Portalı örneklerini yükseltin
* AD FS bağdaştırıcısı örneklerini yükseltin

## <a name="upgrade-azure-mfa-server"></a>Azure MFA Sunucusu'nu yükseltme

1. Yönergeleri kullanın [Azure multi-Factor Authentication Sunucusu'nu indirmek](howto-mfaserver-deploy.md#download-the-mfa-server) Azure MFA sunucusu yükleyicisinin en son sürümünü almak için.
2. MFA sunucusu veri dosyasındaki C:\Program Files\Multi-Factor Authentication (varsayılan yükleme konumu varsayılarak) Server\Data\PhoneFactor.pfdata ana MFA sunucunuz üzerindeki konumunda bulunan bir yedeğini alın.
3. Yüksek kullanılabilirlik için birden fazla sunucu çalıştırıyorsanız, MFA sunucusuna kimlik doğrulaması ve böylece yükseltme sunucularına trafik gönderen Durdur istemci sistemleri değiştirin. Bir yük dengeleyici kullanırsanız, bağımlı bir MFA sunucusunu yük dengeleyiciden kaldırma, yükseltme yapın ve ardından sunucu grubuna geri ekleyin.
4. Her MFA sunucusunda yeni yükleyiciyi çalıştırın. Eski veri dosyasındaki Yöneticisi tarafından çoğaltılan okuyabilir bağımlı sunucularını ilk yükseltin.

   > [!NOTE]
   > Bir sunucu yükseltme sırasında herhangi bir loadbalancing veya diğer MFA sunucularıyla paylaşımı trafiği kaldırılmalıdır.
   >
   > Yükleyiciyi çalıştırmadan önce geçerli MFA sunucunuz kaldırmanız gerekmez. Yükleyici yerinde yükseltme gerçekleştirir. Aynı konuma (örneğin, C:\Program Files\Multi-Factor Authentication Server) yükler için yükleme yolu kayıt defterinden önceki yükleme seçilir.
  
5. Microsoft Visual C++ 2015 yeniden dağıtılabilir bir güncelleştirme paketini yüklemek için istenirse, istemi kabul edin. Paket x86 ve x64 sürümleri yüklenir.
6. Web hizmeti SDK'sı kullanıyorsanız, yeni Web hizmeti SDK'yı yüklemeniz istenir. Yeni Web hizmeti SDK'sı yükleme sırasında sanal dizin adı önceden yüklenmiş sanal dizin (örneğin, Phonefactorwebservicesdk) eşleştiğinden emin olun.
7. Tüm bağımlı sunucularında bu adımları yineleyin. Yeni ana olmasını Astları birine yükseltin ve ardından eski ana sunucuyu yükseltin.

## <a name="upgrade-the-user-portal"></a>Kullanıcı Portalı'nı yükseltme

Bu bölüme geçmeden önce MFA sunucularınızın tamamlayın.

1. Kullanıcı Portalı yükleme konumuna (örneğin, C:\inetpub\wwwroot\MultiFactorAuth) sanal dizininde web.config dosyasının yedeğini alın. Varsayılan tema için herhangi bir değişiklik yapıldıysa, App_Themes\Default klasörünü de yedeklemesini. Varsayılan tema değiştirme daha yeni bir tema oluşturun ve varsayılan klasörünün bir kopyasını oluşturmak daha iyidir.
2. MFA sunucusu bileşenleri ile aynı sunucuda kullanıcı portalını çalıştırıyorsa, MFA sunucusu yükleme Kullanıcı Portalı'nı güncelleştirmek isteyip istemediğinizi sorar. İstemini kabul etmek ve kullanıcı portalı güncelleştirmesini yükleyin. Sanal dizin adı önceden yüklenmiş sanal dizin (örneğin, MultiFactorAuth) eşleşip eşleşmediğini denetleyin.
3. Kullanıcı portalını kendi sunucusuna ise, MFA sunuculardan birinin yükleme konumundan multifactorauthenticationuserportalsetup64.msi dosyasını dosya kopyalayın ve kullanıcı portalı web sunucusunun yerleştirin. Yükleyiciyi çalıştırın.

   Belirten bir hata oluşursa, "Microsoft Visual C++ 2015 yeniden dağıtılabilir güncelleştirme 1 veya üzeri gerekiyor" yükleyip en son güncelleştirme paketinden [Microsoft Download Center](https://www.microsoft.com/download/). X86 ve x64 sürümlerini yükleyin.

4. Güncelleştirilmiş Kullanıcı Portalı yazılım yüklendikten sonra yeni web.config dosyasıyla 1. adımda oluşturduğunuz web.config yedeği karşılaştırın. Yeni özniteliklere yeni web.config dosyasında yoksa, yedekleme web.config dosyanızın üzerine yeni bir sanal dizinine kopyalayın. Başka bir seçenek kopyalama/appSettings değerleri ve Web hizmeti SDK URL'SİYLE yedekleme dosyasından yeni web.config yapıştırma oluşturmaktır.

Birden çok sunucuya kullanıcı portalını varsa, bunların tümünün yüklemesinde yineleyin.

## <a name="upgrade-the-mobile-app-web-service"></a>Mobil uygulama Web Hizmeti'ni yükseltme

> [!NOTE]
> 8.0 öncesi Azure MFA Sunucusundan 8.0 üzeri sürüme yükseltirken mobil uygulama web hizmeti yükseltme sonrasında kaldırılabilir

## <a name="upgrade-the-ad-fs-adapters"></a>AD FS Bağdaştırıcısı'nı yükseltme

Yükseltme MFA sunucularını ve kullanıcı portalı bu bölüme geçmeden önce tamamlayın.

### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>MFA AD FS değerinden farklı sunucularda çalıştırıyorsa

Multi-Factor Authentication Sunucusu'nun AD FS sunucularınızın ayrı olarak çalıştırıyorsanız bu yönergeler yalnızca geçerlidir. Her iki hizmet aynı sunucu üzerinde çalıştırıyorsanız, bu bölümü atlayın ve Kurulum adımlarına gidin. 

1. AD FS'de kaydedildiği MultiFactorAuthenticationAdfsAdapter.config dosyasının bir kopyasını kaydedin veya aşağıdaki PowerShell komutunu kullanarak yapılandırmayı dışarı aktar: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path to config file]`. Bağdaştırıcı adı "WindowsAzureMultiFactorAuthentication" veya "AzureMfaServerAuthentication" önceden yüklenmiş bağlı olarak sürümüdür.
2. Aşağıdaki dosyalar, AD FS sunucuları için MFA sunucusunu yükleme konumundan kopyalayın:

   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config

3. Ekleyerek Register-MultiFactorAuthenticationAdfsAdapter.ps1 betiğini düzenleyin `-ConfigurationFilePath [path]` sonuna `Register-AdfsAuthenticationProvider` komutu. Değiştirin *[yol]* MultiFactorAuthenticationAdfsAdapter.config tam yol ile dosya veya yapılandırma dosyasını dışarı önceki adımda.

   Eski yapılandırma dosyası eşleşmesi olup olmadığını görmek için yeni MultiFactorAuthenticationAdfsAdapter.config öznitelikleri kontrol edin. Herhangi bir özniteliği eklendi veya yeni bir sürüme kaldırıldığını, öznitelik değerleri için yeni bir tane eski yapılandırma dosyasından kopyalayın veya eşleştirmek için eski yapılandırma dosyasını değiştirin.

### <a name="install-new-ad-fs-adapters"></a>Yeni AD FS bağdaştırıcısı yükleme

> [!IMPORTANT]
> Kullanıcılarınızın bu bölümün 8 3. adımları sırasında iki aşamalı doğrulamayı gerçekleştirmek için gerekli değildir. Birden çok kümede yapılandırılmış AD FS varsa, Kaldır, yükseltme ve kapalı kalma süresini önlemek için diğer kümeler bağımsız olarak gruptaki her bir küme geri yükleyebilirsiniz.

1. Bazı AD FS sunucuları, gruptan kaldırın. Bu sunucular, diğer düğümler çalışırken güncelleştirin.
2. Yeni AD FS bağdaştırıcısını AD FS gruptan kaldırılan her bir sunucuya yükleyin. MFA sunucusu, her AD FS sunucusunda yüklü değilse, mfa'yı Sunucu Yöneticisi UX'i güncelleştirebilirsiniz. Aksi takdirde, MultiFactorAuthenticationAdfsAdapterSetup64.msi çalıştırarak güncelleştirin.

   Belirten bir hata oluşursa, "Microsoft Visual C++ 2015 yeniden dağıtılabilir güncelleştirme 1 veya üzeri gerekiyor" yükleyip en son güncelleştirme paketinden [Microsoft Download Center](https://www.microsoft.com/download/). X86 ve x64 sürümlerini yükleyin.

3. Git **AD FS** > **kimlik doğrulama ilkeleri** > **genel çok faktörlü kimlik doğrulama ilkesini Düzenle**. Onay kutusunu temizleyin **WindowsAzureMultiFactorAuthentication** veya **AzureMFAServerAuthentication** (bağlı olarak yüklenmiş geçerli sürümü).

   Bu adım tamamlandıktan sonra MFA sunucusu ile iki aşamalı doğrulama 8. adım tamamlanana kadar bu AD FS kümede kullanılabilir değil.

4. AD FS bağdaştırıcısı eski sürümü Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell betiğini çalıştırarak kaydını silin. Emin *-adı* parametresi ("WindowsAzureMultiFactorAuthentication" veya "AzureMFAServerAuthentication"), 3. adımda görüntülenen adı ile eşleşen. Merkezi bir yapılandırma olduğundan bu aynı AD FS kümesindeki tüm sunucular için geçerlidir.
5. Yeni AD FS bağdaştırıcısı Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell betiğini çalıştırarak kaydedin. Merkezi bir yapılandırma olduğundan bu aynı AD FS kümesindeki tüm sunucular için geçerlidir.
6. AD FS gruptan kaldırılan her sunucuda AD FS hizmetini yeniden başlatın.
7. Güncelleştirilmiş sunucuları AD FS grubuna geri ekleyin ve diğer sunucuları gruptan kaldırın.
8. Git **AD FS** > **kimlik doğrulama ilkeleri** > **genel çok faktörlü kimlik doğrulama ilkesini Düzenle**. Denetleme **AzureMfaServerAuthentication**.
9. Artık AD FS gruptan kaldırılan sunucularını güncelleştirmek için 2. adımı yineleyin ve bu sunucular üzerinde AD FS hizmetini yeniden başlatın.
10. Bu sunucular, AD FS grubuna geri ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Örnekleri alma [Azure multi-Factor Authentication ve üçüncü taraf VPN'ler ile Gelişmiş senaryolar](howto-mfaserver-nps-vpn.md)

* [MFA sunucusu Windows Server Active Directory ile eşitleyin](howto-mfaserver-dir-ad.md)

* [Windows kimlik doğrulamasını yapılandırma](howto-mfaserver-windows.md) uygulamalarınız için
