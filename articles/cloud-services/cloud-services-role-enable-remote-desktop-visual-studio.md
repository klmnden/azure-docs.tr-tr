---
title: Azure bulut Hizmetleri rolünde için Uzak Masaüstü Bağlantısı etkinleştir
description: Azure bulut hizmeti uygulamanız Uzak Masaüstü bağlantılarına izin verecek şekilde yapılandırma
services: cloud-services
author: ghogen
manager: douge
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.topic: conceptual
ms.workload: azure
ms.date: 03/06/2018
ms.author: ghogen
ms.openlocfilehash: fe8b2b59616246743b38aa3b7a7972c092529b5d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Visual Studio kullanarak Azure Cloud Services rolünde için Uzak Masaüstü Bağlantısı etkinleştir

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan rolü Masaüstü erişmenize olanak tanır. Uzak Masaüstü Bağlantısı çalışırken, uygulamanızın sorunları tanılamak ve gidermek için kullanabilirsiniz.

Visual Studio için bulut hizmetleri sağlayan Yayımlama Sihirbazı'nı, sağladığınız kimlik bilgilerini kullanarak yayımlama işlemi sırasında Uzak Masaüstü'nü etkinleştirmek için bir seçenek içerir. Bu seçeneği kullanarak Visual Studio 2017 15.4 ve önceki sürümü kullanılırken uygundur.

Visual Studio 2017 ile 15,5 ve sonraki sürümleri, ancak, yalnızca tek bir geliştirici olarak çalıştığınız sürece Uzak Masaüstü Yayımlama Sihirbazı'nı etkinleştirme olmaması önerilir. Hiçbir durumda proje diğer geliştiriciler tarafından açılması, bunun yerine Uzak Masaüstü'nü PowerShell aracılığıyla ya da bir iş akışındaki sürekli dağıtım sürüm tanımından Azure Portalı aracılığıyla sağlar. Bu öneri nasıl Visual Studio ile Uzak Masaüstü bulut hizmeti VM, bu makalede açıklanan iletişim kurduğu bir değişikliğin kaynaklanır.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Visual Studio 2017 15.4 ve önceki sürümü aracılığıyla, Uzak Masaüstü'nü yapılandırma

Visual Studio 2017 15.4 ve daha önceki bir sürümünü kullanırken, kullanabileceğiniz **tüm roller için Uzak Masaüstü'yi etkinleştirmek** Yayımlama Sihirbazı'nda seçeneği. Sihirbaz Visual Studio 2017 15,5 ve sonraki sürümleri ile kullanmaya devam edebilirsiniz, ancak Uzak Masaüstü seçeneği kullanmayın.

1. Çözüm Gezgini'nde bulut hizmet projenize sağ tıklayıp seçme Visual Studio'da Yayımlama Sihirbazını Başlat **Yayımla**.

2. Seçin ve gerekiyorsa Azure aboneliğinize imzalamak **sonraki**.

3. Üzerinde **ayarları** sayfasında, **tüm roller için Uzak Masaüstü'yi etkinleştirmek**seçeneğini belirleyip **ayarları...**  açmak için bağlantı **uzak masaüstü yapılandırması** iletişim kutusu.

4. İletişim kutusunun altında seçin **diğer seçenekler**. Bu komut, oluşturun veya Uzak Masaüstü aracılığıyla bağlanırken kimlik bilgilerini şifrelemek için bir sertifika seçin açılan listesini görüntüler.

   > [!Note]
   > Bir Uzak Masaüstü bağlantısı için gereken sertifikalar, diğer Azure işlemleri için kullandığı sertifikalar farklıdır. Uzaktan erişim sertifikasının özel anahtarı olması gerekir.

5. Listeden bir sertifika seçin veya  **&lt;oluştur... &gt;**. Yeni bir sertifika oluşturuyorsanız, istendiğinde Yeni sertifika için kolay bir ad sağlayın ve seçin **Tamam**. Yeni sertifika aşağı açılan liste kutusunda görüntülenir.

6. Bir kullanıcı adı ve parola sağlayın. Var olan bir hesap kullanamazsınız. "Yönetici" yeni hesap için kullanıcı adı olarak kullanmayın.

7. Hesap sona erecek ve hangi Uzak Masaüstü bağlantıları engellenir sonra bir tarihi seçin.

8. Gerekli tüm bilgileri sağladığınız sonra seçin **Tamam**. Visual Studio, projenizin uzak masaüstü ayarlarını ekler `.cscfg` ve `.csdef` seçilen sertifika kullanılarak şifrelenmiş parola dahil dosyaları.

9. Kullanarak tüm kalan adımları tamamlayın **sonraki** düğmesine ve ardından **Yayımla** bulut hizmetinizi yayımlayın hazır olduğunuzda. Yayımlamaya hazır değilseniz seçin **iptal** ve yanıt **Evet** değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda. Bu ayarları daha sonra bulut hizmetinizin yayımlayabilirsiniz.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 15,5 ve sonraki sürümleri kullanırken, Uzak Masaüstü'nü yapılandırma

Visual Studio 2017 ile 15,5 ve sonraki sürümleri, bir bulut hizmeti projesini ile Yayımlama Sihirbazı'nı kullanmaya devam edebilirsiniz. Aynı zamanda **tüm roller için Uzak Masaüstü'yi etkinleştirmek** yalnızca tek bir geliştirici olarak çalışıyorsanız seçeneği.

Bir ekibin parçası çalışıyorsanız, bunun yerine Azure bulut hizmeti üzerinde Uzak Masaüstü kullanarak etkinleştirmelisiniz [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md) veya [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

Visual Studio 2017 15,5 ve sonraki sürümleri VM bulut hizmetiyle nasıl iletişim kurduğu bir değişiklik nedeniyle bu önerilir. Uzak Masaüstü Yayımlama Sihirbazı'nı etkinleştirdiğinizde, Visual Studio'nun önceki sürümleri ne "RDP eklenti." çağırdı aracılığıyla VM ile iletişim Visual Studio 2017 15,5 ve sonraki sürümleri ile iletişim kurar "RDP uzantısı" kullanmayı daha güvenli ve daha esnektir. Bu değişiklik ayrıca Azure portal ve Uzak Masaüstü'nü etkinleştirmek için PowerShell yöntemler de RDP uzantısı kullanmanızı olgu ile hizalar.

Visual Studio RDP uzantısı ile iletişim kurduğunda, iletme bir düz metin parola SSL üzerinden. Ancak, bunu şifrelemek için kullanılan yerel sertifika ile düz metin şifresi yalnızca bir şifrelenmiş parola projenin yapılandırma dosyalarını depolar.

Her zaman aynı geliştirme bilgisayardan bulut hizmeti projesini dağıtırsanız, bu yerel sertifika kullanılabilir. Bu durumda, kullanmaya devam edebilirsiniz **tüm roller için Uzak Masaüstü'yi etkinleştirmek** Yayımlama Sihirbazı'nda seçeneği.

Ancak, siz veya diğer geliştiriciler farklı bilgisayarlardan bulut hizmeti projesini dağıtmak istiyorsanız, diğer bilgisayarlarda Parola şifresini çözmek üzere gerekli sertifikaya olmayacaktır. Sonuç olarak, aşağıdaki hata iletisini görürsünüz:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

Bulut hizmetinin dağıtılacağı, ancak bu eylem herkes için Uzak Masaüstü'nü kullanmak için zor olabilir her zaman parola değiştirebilir.

Bir ekip projesi paylaşıyorsanız, daha sonra Yayımlama Sihirbazı'nda seçeneğinin işaretini kaldırın ve bunun yerine doğrudan ile Uzak Masaüstü etkinleştirmek en iyisidir [Azure portal](cloud-services-role-enable-remote-desktop-new-portal.md) veya kullanarak [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 15,5 ve sonraki sürümleri ile bir yapı sunucudan dağıtma

Bir bulut hizmeti projesini hangi Visual Studio 2017 üzerinde derleme aracısı 15,5 veya sonraki bir sürümü yüklü bir sunucudan yapı (örneğin, Visual Studio Team Services ile) dağıtabilirsiniz. Bu düzenleme ile dağıtım şifreleme sertifikası kullanılabilir bilgisayardan olur.

Visual Studio Team Services RDP uzantısından kullanmak için derleme tanımı'nda aşağıdaki ayrıntıları içerir:

1. Dahil `/p:ForceRDPExtensionOverPlugin=true` dağıtım RDP eklentisi yerine RDP uzantısı ile çalıştığından emin olmak için MSBuild bağımsız değişkenleri. Örneğin:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Derleme adımlarınız sonra Ekle **Azure bulut hizmeti dağıtımı** adım ve özelliklerini ayarlayın.

1. Dağıtım adımından sonra Ekle bir **Azure Powershell** adım, Ayarla kendi **görünen adı** özelliği "Azure dağıtım: etkinleştirmek RDP uzantısı" (veya başka bir uygun adı) ve uygun Azure seçin aboneliği.

1. Ayarlama **komut dosyası türü** "İçi" uygulamasına aşağıdaki kodu yapıştırın **satır içi betiği** alan. (De oluşturabilirsiniz bir `.ps1` dosya projenizdeki bu komut, ayarlamak **komut dosyası türü** "Komut dosyası yolunu" ve kümesi **betik yolu** dosyasına işaret edecek şekilde.)

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

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Uzak Masaüstü'nü kullanarak bir Azure rolüne bağlayın

Azure üzerinde bulut hizmetinizi yayımlayın ve Uzak Masaüstü etkinleştirdikten sonra VM bulut hizmetinde oturum için Visual Studio Sunucu Gezgini kullanabilirsiniz:

1. Server Explorer'da genişletin **Azure** düğümünü ve bir bulut hizmeti ve örneklerinin bir listesini görüntülemek için kendi rollerinden birini düğümünü genişletin.

2. Bir örnek düğümünü sağ tıklatın ve seçin **Uzak Masaüstü kullanarak bağlanmak**.

3. Kullanıcı adı ve daha önce oluşturduğunuz parolayı girin. Şimdi uzak oturumunuza günlüğe kaydedilir.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)