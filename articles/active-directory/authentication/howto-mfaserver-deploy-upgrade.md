---
title: Azure MFA sunucusu yükseltme | Microsoft Docs
description: Adımlar ve Azure multi-Factor Authentication sunucusu daha yeni bir sürüme yükseltmek için yönergeler.
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 514f8942105740ab76845c2e122abd91f04ba264
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="upgrade-to-the-latest-azure-multi-factor-authentication-server"></a>En son Azure multi-Factor Authentication Sunucusu'na yükseltme

Bu makalede Azure çok faktörlü kimlik doğrulama (MFA) sunucusu v6.0 yükseltme sürecini veya daha yüksek aracılığıyla anlatılmaktadır. PhoneFactor Aracısı'nın eski bir sürümüne yükseltme yapmanız oluştuysa, [PhoneFactor Aracısı'nı Azure multi-Factor Authentication Sunucusu'na yükseltme](howto-mfaserver-deploy-upgrade-pf.md).

V6.x veya v7.x eski veya yeni yükseltiyorsanız, tüm bileşenler için .NET 4.5 .NET 2. 0 ' değiştirin. Tüm bileşenler için Microsoft Visual C++ 2015 Redistributable Update 1 veya daha yüksek de gerekir. Bunlar zaten yüklü değilse MFA sunucusu Yükleyici bu bileşenlerin x86 hem x64 sürümlerini yükler. Kullanıcı Portalı ve mobil uygulama Web hizmeti ayrı sunucularda çalıştırırsanız, bu bileşenlerin yükseltmeden önce bu paketleri yüklemeniz gerekir. En son Microsoft Visual C++ 2015 Redistributable güncelleştirmesi için arama yapabilirsiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/). 

## <a name="install-the-latest-version-of-azure-mfa-server"></a>Azure MFA sunucusu en son sürümünü yükleyin

1. Konusundaki yönergeleri kullanın [Azure multi-Factor Authentication Sunucusu'nu indirmek](howto-mfaserver-deploy.md#download-the-mfa-server) Azure MFA sunucusu en son sürümünü almak için.
2. MFA sunucusu veri dosyasındaki C:\Program Files\Multi-Factor Authentication (varsayılan yükleme konumu varsayılarak) Server\Data\PhoneFactor.pfdata ana MFA sunucunuzda konumunda bulunan bir yedeğini alın.
3. Yüksek kullanılabilirlik için birden çok sunucu çalıştırırsanız, MFA sunucusu kimlik doğrulaması ve böylece yükseltiyorsanız sunucularına trafiği göndermeye Durdur istemci sistemleri değiştirin. Bir yük dengeleyici kullanırsanız, MFA sunucusu yük dengeleyiciden kaldırın, yükseltme yapın ve ardından sunucu grubuna geri ekleyin.
4. Yeni Yükleyici her MFA sunucusunda çalıştırın. Eski veri dosyasındaki Yöneticisi tarafından çoğaltılmasını okuyabilir bağımlı sunucular ilk yükseltin. 

  Yükleyiciyi çalıştırmadan önce geçerli MFA sunucusu kaldırmanız gerekmez. Yükleyici bir yerinde yükseltme gerçekleştirir. Aynı konuma (örneğin, C:\Program Files\Multi-Factor Authentication Server) yüklenmesini sağlayabilir yükleme yolunun önceki yüklemesinin kayıt defterinden kayıt. 
  
5. Microsoft Visual C++ 2015 Redistributable bir güncelleştirme paketini yüklemek için istenirse isteğini kabul edin. Paket x86 hem x64 sürümleri yüklenir.
5. Web hizmeti SDK'sı kullanıyorsanız, yeni Web hizmeti SDK'sı yüklemeniz istenir. Yeni Web hizmeti SDK'sı yüklediğinizde, sanal dizin adını önceden yüklenmiş sanal dizin (örneğin, Phonefactorwebservicesdk) eşleştiğinden emin olun.
6. Tüm bağımlı sunucularında adımları yineleyin. Yeni ana olmasını ast birini Yükselt sonra eski ana sunucuyu yükseltin. 

## <a name="upgrade-the-user-portal"></a>Kullanıcı Portalı yükseltme

1. Kullanıcı Portalı yükleme konumunu (örneğin, C:\inetpub\wwwroot\MultiFactorAuth) sanal dizinde bulunan web.config dosyasının yedeğini alın. Varsayılan tema için herhangi bir değişiklik yapıldıysa, App_Themes\Default klasörünün yanı sıra bir yedeğini alın. Varsayılan klasör bir kopyasını oluşturun ve varsayılan tema değiştirmek daha yeni bir tema oluşturmak daha iyidir.
2. Kullanıcı Portalı diğer MFA sunucusu bileşenleri ile aynı sunucuda çalışıyorsa, MFA sunucusu yükleme, Kullanıcı Portalı'nı güncelleştirmek ister. İstemini kabul etmek ve kullanıcı portalı güncelleştirmesini yükleyin. Sanal dizin adını önceden yüklenmiş sanal dizin (örneğin, MultiFactorAuth) eşleşip eşleşmediğini denetleyin.
3. Kullanıcı Portalı'nı kendi sunucusundaysa, MFA sunuculardan birinin yükleme konumundan multifactorauthenticationuserportalsetup64.msi dosyasını dosyasını kopyalayın ve kullanıcı portalı web sunucuya yerleştirin. Yükleyiciyi çalıştırın. 

  Belirten bir hata oluşursa, "Microsoft Visual C++ 2015 Redistributable Update 1 veya daha yüksek gereklidir" yükleyip en son güncelleştirme paketinden [Microsoft Download Center](https://www.microsoft.com/download/). X86 hem x64 sürümlerini yükleyin.

4. Güncelleştirilmiş Kullanıcı Portalı yazılım yüklendikten sonra yeni web.config dosyasına ile 1. adımda yaptığınız web.config yedekleme karşılaştırın. Yeni özniteliklere yeni web.config dosyasında yoksa, yedekleme web.config üzerine yeni bir sanal dizine kopyalayın. Başka bir seçenek kopyalayıp appSettings değerleri ve Web hizmeti SDK URL'si yedekleme dosyasından yeni özelliği web.config dosyasına yapıştırın oluşturmaktır.

Birden çok sunucu üzerinde kullanıcı portalı varsa, bunların tümünün yüklemesinde yineleyin. 


## <a name="upgrade-the-mobile-app-web-service"></a>Mobil uygulama Web Hizmeti'ni yükseltme

1. Mobil uygulama Web hizmeti yükleme konumu (örneğin, C:\inetpub\wwwroot\app veya C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) sanal dizinde bulunan web.config dosyasının yedeğini alın.
2. MFA sunucuları yükleme konumundan multifactorauthenticationmobileappwebservicesetup64.msi dosyasını dosyasını kopyalayın ve mobil uygulama kayıt web sunucuya yerleştirin.
3. Yükleyiciyi çalıştırın. 

  Microsoft Visual C++ 2015 Redistributable Update 1 veya daha yüksek gerekli olduğunu bildiren bir hata meydana gelirse, yükleyip en son güncelleştirme paketinden [Microsoft Download Center](https://www.microsoft.com/download/). X86 hem x64 sürümlerini yükleyin.

4. Güncelleştirilmiş mobil uygulama Web hizmeti yazılım yüklendikten sonra yeni web.config dosyasına ile 1. adımda yedeklenen web.config dosyasını karşılaştırın. Yeni öznitelik yeni web.config dosyasında yoksa, kaydedilen web.config sanal dizine kopyalayın ve yeni bir üzerine yazma. Başka bir seçenek kopyalayıp appSettings değerleri ve Web hizmeti SDK URL'si yedekleme dosyasından yeni özelliği web.config dosyasına yapıştırın oluşturmaktır.

Mobil uygulama Web hizmeti birden fazla sunucuda varsa, bunların tümünün yüklemesinde yineleyin. 

## <a name="upgrade-the-ad-fs-adapters"></a>AD FS bağdaştırıcısı yükseltme


### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>MFA ADFS değerinden farklı sunucularda çalışıyorsa

Multi-Factor Authentication sunucusu, AD FS sunucularından ayrı olarak çalıştırırsanız, bu yönergeler yalnızca geçerlidir. Her iki hizmet aynı sunucu üzerinde çalıştırıyorsanız, bu bölüm atlayın ve yükleme adımları gidin. 

1. AD FS'de kaydedildiği MultiFactorAuthenticationAdfsAdapter.config dosyasının bir kopyasını kaydedin veya aşağıdaki PowerShell komutunu kullanarak yapılandırmayı dışarı aktarma: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path to config file]`. Bağdaştırıcı adı "WindowsAzureMultiFactorAuthentication" veya "AzureMfaServerAuthentication" daha önce yüklenmiş bağlı olarak sürümüdür.
2. Aşağıdaki dosyaları MFA sunucusu yükleme konumundan AD FS sunucularına kopyalayın:

  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config

3. Ekleyerek Register-MultiFactorAuthenticationAdfsAdapter.ps1 betiğini düzenleyin `-ConfigurationFilePath [path]` sonuna `Register-AdfsAuthenticationProvider` komutu. Değiştir *[path]* MultiFactorAuthenticationAdfsAdapter.config tam yolu ile dosya veya yapılandırma dosyasını dışarı önceki adımda. 

  Eski yapılandırma dosyası eşleşip eşleşmediğini görmek için yeni MultiFactorAuthenticationAdfsAdapter.config öznitelikleri denetleyin. Öznitelik eklenmiş veya yeni sürümde kaldırılmış, öznitelik değerleri eski yapılandırma dosyasından yeni bir kopyalayın. veya eşleştirmek için eski yapılandırma dosyasını değiştirin.

### <a name="install-new-ad-fs-adapters"></a>Yeni AD FS bağdaştırıcısı yükleme

> [!IMPORTANT] 
> Kullanıcılarınız, bu bölümün adım 3-8 sırasında iki aşamalı doğrulamayı gerçekleştirmek için gerekli değildir. Birden çok kümelerde yapılandırılmış AD FS varsa, Kaldır, yükseltme ve kesinti süresini önlemek için diğer kümeleri bağımsız olarak gruptaki her küme geri yükleyebilirsiniz.

1. Bazı AD FS sunucuları grubundan kaldırın. Diğer düğümler çalışırken bu sunucuları güncelleştirin.
2. AD FS grubundan kaldırıldı her bir sunucuda yeni AD FS Bağdaştırıcısı'nı yükleyin. MFA sunucusu her AD FS sunucusunda yüklüyse, MFA sunucusu yönetim UX güncelleştirebilirsiniz Aksi takdirde MultiFactorAuthenticationAdfsAdapterSetup64.msi çalıştırarak güncelleştirin. 

  Belirten bir hata oluşursa, "Microsoft Visual C++ 2015 Redistributable Update 1 veya daha yüksek gereklidir" yükleyip en son güncelleştirme paketinden [Microsoft Download Center](https://www.microsoft.com/download/). X86 hem x64 sürümlerini yükleyin.

3. Git **AD FS** > **kimlik doğrulama ilkeleri** > **genel çok faktörlü kimlik doğrulama ilkesini Düzenle**. İşaretini **WindowsAzureMultiFactorAuthentication** veya **AzureMFAServerAuthentication** (bağlı olarak yüklenen geçerli sürüm). 

  Bu adım tamamlandıktan sonra MFA sunucusu üzerinden iki aşamalı doğrulamayı adım 8 tamamlanana kadar bu AD FS kümede kullanılabilir değil.

4. AD FS bağdaştırıcısı eski sürümü Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell betiğini çalıştırarak kaydını silin. Emin *-Name* parametresi ("WindowsAzureMultiFactorAuthentication" veya "AzureMFAServerAuthentication"), 3. adımda görüntülenen adı ile eşleşen. Merkezi bir yapılandırma olduğundan bu aynı AD FS kümesindeki tüm sunucular için geçerlidir.
5. Yeni AD FS Bağdaştırıcısı'nı Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell betiğini çalıştırarak kaydedin. Merkezi bir yapılandırma olduğundan bu aynı AD FS kümesindeki tüm sunucular için geçerlidir.
6. AD FS grubundan kaldırıldı her bir sunucuda AD FS hizmetini yeniden başlatın.
7. Güncelleştirilmiş sunucusu AD FS grubunu ekleyin ve diğer sunucuları grubundan kaldırın.
8. Git **AD FS** > **kimlik doğrulama ilkeleri** > **genel çok faktörlü kimlik doğrulama ilkesini Düzenle**. Denetleme **AzureMfaServerAuthentication**.
9. Şimdi AD FS grubundan kaldırıldı sunucuları güncelleştirme 2. adımı yineleyin ve bu sunucular üzerinde AD FS hizmetini yeniden başlatın.
10. Bu sunucular, AD FS grubuna geri ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Örnekleri alın [Azure çok faktörlü kimlik doğrulama ve üçüncü taraf VPN ile Gelişmiş senaryolar](howto-mfaserver-nps-vpn.md)

- [MFA sunucusu Windows Server Active Directory ile eşitleme](howto-mfaserver-dir-ad.md)

- [Windows kimlik doğrulamasını yapılandırmak](howto-mfaserver-windows.md) uygulamalarınız için
