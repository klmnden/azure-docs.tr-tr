---
title: "Sürekli teslimat ile uzaktan hata ayıklamayı etkinleştirme | Microsoft Docs"
description: "Azure'a dağıtmak için kesintisiz teslim kullanırken, uzaktan hata ayıklamayı etkinleştirme öğrenin"
services: cloud-services
documentationcenter: .net
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 7a8a853a93e3e9915f687a20c871444e6a0de50d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Azure'da yayımlamak için sürekli teslim kullanılırken uzaktan hata ayıklamayı etkinleştirme
Kullandığınızda uzaktan Azure'da, bulut Hizmetleri ya da sanal makineleri için hata ayıklamayı etkinleştirebilirsiniz [kesintisiz teslim](cloud-services-dotnet-continuous-delivery.md) Azure için aşağıdaki adımları izleyerek yayımlamak için.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Bulut Hizmetleri için uzaktan hata ayıklama etkinleştirme
1. Yapı aracısında kısmında özetlendiği gibi ilk ortamını Azure için ayarlayamıyor [komut satırı derleme Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Uzaktan hata ayıklama çalışma zamanı (msvsmon.exe) paket için gerekli olmadığından yükleme **uzak araçlar Visual Studio için**.

    * [Visual Studio 2017 için Uzak araçları](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [Visual Studio 2015 için Uzak araçları](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [Visual Studio 2013 güncelleştirme 5 için Uzak araçları](https://www.microsoft.com/download/details.aspx?id=48156)
    
    Alternatif olarak, Visual Studio yüklü olan bir sistemden uzaktan hata ayıklama ikili dosyalarının kopyalayabilirsiniz.

3. Kısmında özetlendiği gibi bir sertifika oluşturmak [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md). RDP sertifikası parmak izi ve .pfx tutmak ve hedef bulut hizmetine sertifika yükleyin.
4. Derleme ve etkin uzaktan hata ayıklama ile paket için MSBuild komut satırında aşağıdaki seçenekleri kullanın. (Sistem ve proje dosyalarınıza açı ayraçlı öğeleri için gerçek yolları değiştirin.)
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"
   
    `VSX64RemoteDebuggerPath`klasörün yolunu msvsmon.exe uzak araçlar Visual Studio için içeren.
    `RemoteDebuggerConnectorVersion`Bulut hizmetinizde Azure SDK sürümüdür. Ayrıca Visual Studio ile yüklenen sürümüyle eşleşmesi gerekir.
5. Hedef bulut hizmetine, önceki adımda oluşturulan paket ve .cscfg dosyasını kullanarak yayımlayın.
6. Sertifikayı (.pfx dosyası) yüklü .NET için Azure SDK ile Visual Studio bulunduğu makineyi içeri aktarın. Alınacak emin olun `CurrentUser\My` sertifika deposu, aksi takdirde Visual Studio hata ayıklayıcısı ekleme başarısız olur.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Sanal makineler için uzaktan hata ayıklama etkinleştirme
1. Bir Azure sanal makine oluşturun. Bkz: [Windows Server çalıştıran bir sanal makine oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Visual Studio'da Azure sanal makineler oluşturun ve yönetin](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. Üzerinde [Azure Klasik portal sayfası](http://go.microsoft.com/fwlink/p/?LinkID=269851), sanal makinenin görmek için sanal makinenin Panoyu görebilmek **RDP sertifikası parmak İZİ**. Bu değer için kullanılan `ServerThumbprint` uzantısı yapılandırma değeri.
3. Belirtilen bir istemci sertifikası oluşturma [Azure Cloud Services sertifikalarına genel bakış](cloud-services-certs-create.md) (.pfx ve RDP sertifikası parmak izi tutun).
4. Azure PowerShell'i yükleyin (sürüm 0.7.4 veya sonrası) kısmında özetlendiği gibi [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
5. RemoteDebug uzantısını etkinleştirmek için aşağıdaki betiği çalıştırın. Kişisel veri ve yolları kendi abonelik adı, hizmet adı ve parmak izi gibi değiştirin.
   
   > [!NOTE]
   > Bu komut dosyasını Visual Studio 2015 için yapılandırılır. Visual Studio 2013 veya Visual Studio 2017 kullanıyorsanız değiştirin `$referenceName` ve `$extensionName` aşağıda atamalar `RemoteDebugVS2013` veya `RemoteDebugVS2017`.

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. Sertifika (.pfx) yüklü .NET için Azure SDK ile Visual Studio bulunduğu makineyi içeri aktarın.

