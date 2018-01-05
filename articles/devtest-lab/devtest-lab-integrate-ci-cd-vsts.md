---
title: "Azure DevTest Labs, VSTS sürekli tümleştirme ve teslimat ardışık düzenine tümleştirme | Microsoft Docs"
description: "VSTS sürekli tümleştirme ve teslim ardışık düzen Azure DevTest Labs tümleştirileceği hakkında bilgi edinin"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: tarcher
ms.openlocfilehash: 3cd6939e890f59e7c5b188edd8620da5c9fcc935
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs VSTS sürekli tümleştirme ve teslim ardışık düzen tümleştirme
Kullanabileceğiniz **Azure DevTest Labs görevler uzantısı** içinde Visual Studio Team Services (kolayca CI/CD yapınızın tümleştirmek ve Azure DevTest Labs sahip işlem hattı yayın VSTS) yüklü. Uzantısı bir VM oluşturun, bir VM'den özel bir görüntü oluşturun ve bir VM silmek için üç görevleri yükler. Bu işlem, örneğin, hızlı bir şekilde belirli test görev için "altın görüntü" dağıtın ve ardından test bittiğinde silmek kolaylaştırır.

Bu makalede, oluşturmak ve bir VM'yi dağıtmak, özel bir görüntü oluşturun, sonra VM tüm bir tam ardışık olarak sil gösterilmektedir. Genellikle, yine de, görevleri tek tek kendi özel yapı test dağıtımını ardışık düzeninde kullanırsınız.

## <a name="before-you-begin"></a>Başlamadan önce
CI/CD hattınızı Azure DevTest Labs ile tümleştirebilirsiniz önce Visual Studio Marketi'nden ilk uzantısını yüklemeniz gerekir.
1. Git [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)
1. Yükleme seçin
1. Sihirbazı tamamlayın

## <a name="create-a-resource-manager-template"></a>Resource Manager şablonu oluşturma
Bu bölümde, isteğe bağlı bir Azure sanal makine oluşturmak için kullandığınız Azure Resource Manager şablonu oluşturmayı açıklar.

1. Bölümündeki adımları izleyin [bir Resource Manager şablonu kullanmak](devtest-lab-use-resource-manager-template.md) aboneliğinizde bir Resource Manager şablonu oluşturmak için.
1. Ekleme [WinRM yapı](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) (Resource Manager şablonu oluşturmadan önce) VM oluşturmanın bir parçası olarak.

   WinRM erişim için gerekli kullanım dağıtmak görevler gibi **Azure dosya kopyalama** ve **PowerShell hedef makinede**.

   > [!NOTE]
   > WinRM paylaşılan IP adresi ile kullanırken, WinRM bağlantı noktasına bir dış bağlantı noktası eşleme için NAT kuralı eklemeniz gerekir. VM bir ortak IP adresiyle oluşturduysanız bu gerekli değildir.
   >
   >

1. Şablon, bilgisayarınızdaki bir dosya olarak kaydedin. Dosya adı **CreateVMTemplate.json**.
1. Şablonu kaynak denetimi sisteminize denetleyin.
1. Bir metin düzenleyicisinde açın ve aşağıdaki komut dosyasını buraya kopyalayın.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzureRmResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzureRmResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzureRmResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Betik, kaynak denetim sisteminizle denetleyin. Şuna benzer bir öğe adı **GetLabVMParams.ps1**.

   Aracı üzerinde yayın tanımının bir parçası çalıştırdığınızda bu komut, görev adımları gibi kullanırsanız VM uygulamanızı dağıtmak için gereken değerleri toplayan **Azure dosya kopyalama** veya **PowerShell hedef makinede**. VM kaynak grubu adı, IP adresi ve tam etki alanı adı (FDQN) gibi değerler gerektirir ve bir Azure VM uygulamaları dağıtmak için bu görevleri kullanın.

## <a name="create-the-release-definition-in-release-management"></a>Yayın Yönetimi'nde yayın tanımı oluşturun
Yayın tanımı oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Açık **sürümleri** sekmesinde **yapı & yayın** hub ve seçin "**+**" simgesi.
1. İçinde **oluşturma yayın tanımı** iletişim kutusunda **boş** şablonunu seçin **sonraki**.
1. Sonraki sayfaya seçin **daha sonra seçmek** ve ardından **oluşturma** bir varsayılan ortamı ve hiçbir bağlı yapıları ile yeni bir sürüm tanımı oluşturmak için.
1. Yeni sürüm tanımı, kısayol menüsünü açın ve seçmek için ortam adının yanındaki üç nokta (...) seçin **değişkenleri yapılandırmanızı**. 
1. İçinde **yapılandırma - ortamı** iletişim kutusunda, yayın tanımı görevleri kullandığınız değişkenleri için aşağıdaki değerleri girin:
   - **vmName**: Resource Manager şablonu Azure portalında oluşturduğunuz zaman VM atadığınız adı girin.
   - **Kullanıcı adı**: Resource Manager şablonu Azure portalında oluşturduğunuz zaman VM atadığınız kullanıcı adı girin.
   - **Parola**: Resource Manager şablonu Azure portalında oluşturduğunuz zaman VM atadığınız parolayı girin. Parolayı güvenli ve gizleme için "asma kilit" simgesini kullanın.

   <a name="create-a-vm"></a>VM oluşturma
   ---

   Bu dağıtım ilk aşamada, sonraki dağıtımları için "altın görüntü" kullanmak üzere VM oluşturmaktır. Bu amaç için özel olarak geliştirilen görevini kullanarak Azure DevTest laboratuvarı örneğinizi içinde VM oluşturun. Yayın tanımı'nda seçin **+ Ekle görevleri** ve ekleme bir **Azure DevTest Labs VM Oluştur** dağıtma sekmesinden görev. Aşağıdaki gibi yapılandırın:

   Bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) sonraki dağıtımları için kullanmak üzere VM oluşturmak için.
   - **Azure RM abonelik**: altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Laboratuvar adı**: daha önce oluşturduğunuz örneğinin adını seçin.
   - **Şablon adı**: kaynak kod deponuza kaydettiğiniz şablon dosyasının adını ve tam yolunu girin. Yolu örneğin basitleştirmek için yayın Yönetimi yerleşik özelliklerini kullanabilirsiniz:
      ```
      $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
      ```
   - **Şablon parametreleri**: şablonda tanımlanan değişkenler parametrelerini girin. Ortamında, örneğin tanımlı değişkenlerin adlarını kullanın:
      ```
      -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
      ```
   - **Değişkenleri - Laboratuvar VM kimliği çıktı**: yeni oluşturulan VM kimliği için sonraki adımları gerekir. Bu Kimliğe sahip otomatik olarak doldurulur ortam değişkeni varsayılan adını kümesinde **çıkış değişkenleri** bölümü. Gerekirse, değişkeni düzenleyin, ancak sonraki görevlerinde doğru adı kullanmayı unutmayın. Laboratuvar VM kimliği şu şekildedir:
      ```
      /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
      ```
1. DevTest Labs VM ayrıntıları toplamak için daha önce oluşturduğunuz betiğini yürütün. Yayın tanımı'nda seçin **+ Ekle görevleri** ve ekleme bir **Azure PowerShell** gelen görev **dağıtma** sekmesi. Görev aşağıdaki gibi yapılandırın:

   Bkz: [Dağıt: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) ve DevTest Labs VM ayrıntıları toplamak üzere betiğini yürütün.

   - **Azure bağlantı türü**: Azure Resource Manager.
   - **Azure RM abonelik**: altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Komut dosyası türü**: komut dosyası.
   - **Komut dosyası yolu**: kaynak kod deponuza kaydettiğiniz komut dosyasının adını ve tam yolunu girin. Yolu örneğin basitleştirmek için yayın Yönetimi yerleşik özelliklerini kullanabilirsiniz:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   - **Komut bağımsız değişkenleri**: betik bağımsız değişken olarak otomatik olarak VM Laboratuvar Kimliğiyle önceki görev tarafından örneğin doldurulmuş olduğu ortam değişkeninin adını girin: 
      ```
      -labVmId '$(labVMId)'
      ```
   Komut dosyasını gerekli değerleri toplar ve bunları kolayca sonraki görev adımlarda başvurabilmeniz için bunları ortam değişkenleri yayın tanımı içinde depolar.

1. Artık uygulamanızı yeni bir DevTest Labs VM dağıtabilirsiniz. Tipik olarak dağıtmak için kullandığınız görevler **Azure dosya kopyalama** ve **PowerShell hedef makinede**.
   - Bu görevleri parametreleri için gereken VM hakkında bilgi adında üç yapılandırma değişkenleri depolanan **labVmRgName**, **labVMIpAddress**, ve **labVMFqdn**yayın tanımı içinde.
   - Yalnızca bir DevTest Labs VM ve özel bir görüntü uygulama dağıtmadan oluşturmayla denemek istiyorsanız, bu adımı atlayabilirsiniz.

   <a name="create-an-image"></a>Görüntü oluştur
   ---

   Sonraki aşamaya Azure DevTest Labs örneğinizi yeni dağıtılan VM görüntüsü oluşturmaktır. Bu görüntü sonra bir geliştirme görevini yürütemiyor veya bazı testleri çalıştırmak istediğiniz zaman isteğe bağlı olarak VM kopyasını oluşturmak için de kullanabilirsiniz. Yayın tanımı'nda seçin **+ Ekle görevleri** ve ekleme bir **Azure DevTest Labs özel görüntü oluşturma** gelen görev **dağıtma** sekmesi. Aşağıdaki gibi yapılandırın:

   Bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) görüntüsü oluşturulamadı.
   - **Azure RM abonelik**: altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Laboratuvar adı**: daha önce oluşturduğunuz örneğinin adını seçin.
   - **Özel görüntü adı**: özel görüntü için bir ad girin.
   - **Açıklama**: isteğe bağlı olarak daha sonra doğru görüntüyü seçmek kolaylaştırmak için bir açıklama girin.
   - **Laboratuvar VM - kaynak Laboratuvar VM kimliği kaynak**: önceki bir görev tarafından VM Laboratuvar Kimliğini otomatik olarak doldurulur ortam değişkeninin varsayılan adıyla değiştirdiyseniz, burada düzenleyin. Varsayılan değer *$(labVMId)*.
   - **Değişkenleri - özel görüntü kimliği çıktı**: yönetmek veya bunu silmek istediğinizde yeni oluşturulan görüntünün kimliği gerekir. Bu Kimliğe sahip otomatik olarak doldurulur ortam değişkeni varsayılan adını kümesinde **çıkış değişkenleri** bölümü. Gerekirse, değişkeni düzenleyebilirsiniz.
   
   <a name="delete-the-vm"></a>VM silme
   ---
   
   Bu örnekte son aşaması, Azure DevTest Labs örneğinde dağıtılan VM silmektir. Genellikle geliştirme görevleri yürütün ya da ihtiyaç duyduğunuz testleri dağıtılan VM üzerinde çalışan sonra VM silin. Yayın tanımı'nda seçin **+ Ekle görevleri** ve ekleme bir **Azure DevTest Labs Sil VM** gelen görev **dağıtma** sekmesi. Aşağıdaki gibi yapılandırın:

   Bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) VM silmek için.
   - **Azure RM abonelik**: altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Laboratuvar VM kimliği**: önceki bir görev tarafından VM Laboratuvar Kimliğini otomatik olarak doldurulur ortam değişkeninin varsayılan adıyla değiştirdiyseniz, burada düzenleyin. Varsayılan değer *$(labVMId)*.
1. Yayın tanımı için bir ad girin ve kaydedin.
1. Yeni bir sürüm oluşturmak, en son sürüme seçin ve tanımı'ndaki tek ortamı dağıtın.

Her aşamada DevTest Labs örneğinizi VM ve oluşturulan görüntü görmek için Azure portalında ve yeniden silinen VM görünümünü yenileyin.

Özel görüntü şimdi gerektiğinde VM'ler oluşturmak için de kullanabilirsiniz.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
* Daha fazla hızlı başlangıç Resource Manager şablonları DevTest Labs Otomasyon keşfedin [ortak DevTest Labs GitHub deposuna](https://github.com/Azure/azure-quickstart-templates).
* Gerekirse, bakın [VSTS sorun giderme](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) sayfası.
