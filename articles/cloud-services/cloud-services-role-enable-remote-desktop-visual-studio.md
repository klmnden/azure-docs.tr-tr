---
title: Azure Cloud Services'ta bir rol için Uzak Masaüstü bağlantısını etkinleştirme
description: Azure bulut hizmeti uygulamanızı Uzak Masaüstü bağlantılarına izin verecek şekilde yapılandırma
services: cloud-services
author: ghogen
manager: douge
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.workload: azure-vs
ms.date: 03/06/2018
ms.author: ghogen
ms.openlocfilehash: 924719a8371f4d41cb9ead09252d8f3d3424326a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717744"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Visual Studio kullanarak Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme

> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolünün erişmenizi sağlar. Sorun giderme ve çalışırken uygulamanızdaki sorunları tanılamak için Uzak Masaüstü Bağlantısı'nı kullanabilirsiniz.

Bulut Hizmetleri için Visual Studio sunan Yayımla Sihirbazı'nı, sağladığınız kimlik bilgilerini kullanarak yayımlama işlemi sırasında Uzak Masaüstü'nü etkinleştirmek için bir seçenek içerir. Bu seçeneği kullanarak Visual Studio 2017 sürüm 15.4 ve önceki kullanırken uygundur.

Visual Studio 2017 sürüm 15.5 ve daha sonra Bununla birlikte, yalnızca tek bir geliştirici olarak çalıştığınız sürece, Uzak Masaüstü Yayımla Sihirbazı'nı etkinleştirme önlemek önerilir. Proje diğer geliştiriciler tarafından açılması herhangi bir durum için bunun yerine Uzak Masaüstü Azure portalı, PowerShell üzerinden veya bir sürekli dağıtım iş akışında bir yayın ardışık düzeni üzerinden olanak sağlar. Nasıl Visual Studio Uzak Masaüstü ile bulut hizmeti VM üzerinde bu makalede açıklandığı gibi iletişim kurduğu bir değişiklik nedeniyle bu önerilir.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Uzak Masaüstü aracılığıyla Visual Studio 2017 sürüm 15.4 ve önceki yapılandırma

Visual Studio 2017 sürüm 15.4 ve önceki kullanırken kullanabileceğiniz **tüm roller için Uzak masaüstünü etkinleştir** Yayımla Sihirbazı'nda seçeneği. Sihirbaz Visual Studio 2017 sürüm 15.5 ve sonrasında kullanmaya devam edebilirsiniz, ancak Uzak Masaüstü seçeneği kullanmayın.

1. Bulut hizmeti projenizi Çözüm Gezgini'nde sağ tıklayıp seçerek Visual Studio'da Yayımlama Sihirbazını Başlat **Yayımla**.

2. Gerekirse Azure aboneliğinizde oturum açın ve seçin **sonraki**.

3. Üzerinde **ayarları** sayfasında **tüm roller için Uzak masaüstünü etkinleştir**, ardından **ayarları...**  açmak için bağlantıyı **uzak masaüstü yapılandırması** iletişim kutusu.

4. İletişim kutusunun alt kısmında seçin **diğer seçenekler**. Bu komut, oluşturduğunuz veya Uzak Masaüstü aracılığıyla bağlanırken kimlik bilgileri şifrelemek için bir sertifika seçin açılan listesini görüntüler.

   > [!Note]
   > Uzak Masaüstü bağlantısı için gereken sertifikaları, Azure diğer işlemleri için kullandığınız sertifikaların farklıdır. Uzaktan erişim sertifikasının özel anahtarı olmalıdır.

5. Listeden bir sertifika seçin ya da seçin  **&lt;oluştur... &gt;** . Yeni bir sertifika oluşturuyorsanız, istendiğinde Yeni sertifika için bir kolay ad girin ve seçin **Tamam**. Yeni sertifika aşağı açılan liste kutusunda görüntülenir.

6. Bir kullanıcı adı ve parola sağlayın. Var olan bir hesap kullanamazsınız. "Yönetici", yeni hesap için kullanıcı adı olarak kullanmayın.

7. Hesap sona erer ve hangi Uzak Masaüstü bağlantılarında engellenir sonra bir tarih seçin.

8. Tüm gerekli bilgileri sağladıktan sonra seçin **Tamam**. Visual Studio, projenizin uzak masaüstü ayarlarını ekler `.cscfg` ve `.csdef` seçilen sertifika kullanılarak şifrelenir parola dahil dosyaları.

9. Kullanarak tüm kalan adımları tamamlayın **sonraki** düğmesine ve ardından **Yayımla** bulut hizmetinizi yayımlamaya hazır olduğunuzda. Yayımlamaya hazır değilseniz seçin **iptal** ve yanıt **Evet** değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda. Bulut hizmetinizin daha sonra bu ayarlarla yayımlayabilirsiniz.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 sürüm 15.5 ve üzeri kullanırken, Uzak Masaüstü'nü yapılandırma

Visual Studio 2017 sürüm 15.5 ve daha sonra bir bulut hizmeti projesi ile Yayımlama Sihirbazı'nı kullanmaya devam edebilirsiniz. Ayrıca **tüm roller için Uzak masaüstünü etkinleştir** yalnızca tek bir geliştirici olarak çalışıyorsanız seçeneği.

Bir ekibin bir parçası çalışıyorsanız, bunun yerine Azure bulut hizmetinde Uzak Masaüstü kullanarak etkinleştirmelisiniz [Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md) veya [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

Visual Studio 2017 sürüm 15.5 ve daha sonra VM bulut hizmetiyle nasıl iletişim kurduğu bir değişiklik nedeniyle bu önerilir. Uzak Masaüstü Yayımla Sihirbazı'nı etkinleştirdiğinizde, Visual Studio'nun önceki sürümlerini ne "RDP eklentiyi." çağırdı aracılığıyla VM ile iletişim kurar Visual Studio 2017 sürüm 15.5 ve üzeri, bunun yerine "RDP uzantısının" kullanarak iletişim kurar daha güvenli ve daha esnektir. Bu değişiklik, ayrıca Azure portalı ve Uzak Masaüstü'nü etkinleştirmek için PowerShell yöntemlerini de RDP uzantısının kullanılması, olgu ile hizalar.

Visual Studio RDP uzantısı ile iletişim kurduğunda, iletme bir düz metin parola SSL üzerinden. Ancak, hangi cihazı şifrelemek için kullanılan yerel sertifika ile düz metin şifresi yalnızca şifreli bir parola, projenin yapılandırma dosyalarını depolar.

Her zaman aynı Geliştirme bilgisayarınızdan bulut hizmeti projesi dağıtırsanız, bu yerel sertifika kullanılabilir. Bu durumda, kullanmaya devam edebilirsiniz **tüm roller için Uzak masaüstünü etkinleştir** Yayımla Sihirbazı'nda seçeneği.

Ancak, sizin veya diğer geliştiriciler bulut hizmeti projesi farklı bilgisayarlardan dağıtmak istiyorsanız, diğer bilgisayarlara parolasının şifresini çözmek için gerekli sertifika olmaz. Sonuç olarak, aşağıdaki hata iletisini görürsünüz:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

Bulut hizmetini dağıtmak, ancak bu eylemi herkes için Uzak Masaüstü kullanmak zor her zaman parola değiştirebilirsiniz.

Bir ekip projesi paylaşıyorsanız, daha sonra Yayımlama Sihirbazı'nda seçeneğini kaldırın ve bunun yerine Uzak Masaüstü aracılığıyla doğrudan etkinleştirmek en iyisidir [Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md) kullanarak veya [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 sürüm 15.5 ve üzeri ile derleme sunucusundan dağıtma

Hangi Visual Studio 2017'de sürüm 15.5 veya üstünü yapı aracısının yüklü olduğu bir yapı sunucuda (örneğin, Azure hizmetleriyle DevOps) bulut hizmeti projesinden dağıtabilirsiniz. Bu düzenleme ile dağıtım şifreleme sertifikası kullanılabilir bilgisayardan'olmuyor.

Azure DevOps Services'dan RDP uzantısının kullanılması için derleme işlem hattı, şu ayrıntıları bildirin:

1. Dahil `/p:ForceRDPExtensionOverPlugin=true` dağıtım RDP eklentisi yerine RDP uzantısının ile çalıştığından emin olmak için MSBuild bağımsız. Örneğin:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Derleme adımlarınızı sonra Ekle **Azure bulut hizmeti dağıtımı** adım ve özelliklerini ayarlayın.

1. Dağıtım adımından sonra ekleyin bir **Azure Powershell** adım olarak ayarlayın, **görünen ad** özelliğini "Azure dağıtım: RDP uzantısı etkinleştir"(veya başka bir uygun ad) ve uygun Azure aboneliğinizi seçin.

1. Ayarlama **betik türü** "İçi" ve içine aşağıdaki kodu yapıştırın **satır içi betik** alan. (De oluşturabilirsiniz bir `.ps1` dosya projenizde bu betik, ayarlama **betik türü** "Betik dosyası yolu" ve küme **betik yolu** dosyasına işaret edecek şekilde.)

    ```ps
    Param(
        [Parameter(Mandatory=$True)]
        [string]$username,

        [Parameter(Mandatory=$True)]
        [string]$password,

        [Parameter(Mandatory=$True)]
        [string]$serviceName,

        [Datetime]$expiry = ($(Get-Date).AddYears(1))
    )

    Write-Host "Service Name: $serviceName"
    Write-Host "User Name: $username"
    Write-Host "Expiry: $expiry"

    $securepassword = ConvertTo-SecureString -String $password -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential $username,$securepassword

    # Try to remote existing RDP Extensions
    try
    {
        $existingRDPExtension = Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
        if ($existingRDPExtension -ne $null)
        {
            Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
        }
    }
    catch
    {
    }

    Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry -Verbose
    ```

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Uzak Masaüstü kullanarak bir Azure rolüne bağlayın

Azure'da bulut hizmetinizi yayımlayın ve Uzak Masaüstü etkinleştirdikten sonra VM bulut hizmetinde oturum açmak için Visual Studio sunucu Gezgini'nde kullanabilirsiniz:

1. Sunucu Gezgini'nde **Azure** düğümünü ve ardından bir bulut hizmeti ve örneklerinin listesini görüntülemek için rollerden biri için düğümünü genişletin.

2. Bir örneği düğümüne sağ tıklayıp **Uzak Masaüstü kullanarak bağlanmak**.

3. Kullanıcı adı ve daha önce oluşturduğunuz parolayı girin. Şimdi uzak oturumunuz kaydedilir.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
