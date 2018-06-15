---
title: Azure DevTest Labs, VSTS sürekli tümleştirme ve teslimat ardışık düzenine tümleştirme | Microsoft Docs
description: VSTS sürekli tümleştirme ve teslim ardışık düzen Azure DevTest Labs tümleştirileceği hakkında bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 1af195e644fe93e0c59f5e4402dd8942f5fe1aba
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787671"
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs VSTS sürekli tümleştirme ve teslim ardışık düzen tümleştirme
Kullanabileceğiniz *Azure DevTest Labs görevleri* , Visual Studio Team Services (VSTS) kolayca yüklü uzantısı CI/CD derleme bırakma hattınızı Azure DevTest Labs ile tümleştirme. Uzantı üç görev yükler: 
* VM oluşturma
* Bir sanal makineden özel bir görüntü oluşturun
* VM silme 

İşlem, örneğin, belirli test görev için "altın görüntü" hızlı bir şekilde dağıtmak ve test bittiğinde silmek kolaylaştırır.

Bu makalede, oluşturma ve bir VM'yi dağıtmak, özel bir görüntü oluşturun ve VM tüm bir tam ardışık olarak silin gösterilmektedir. Her görev normalde ayrı ayrı ve kendi özel yapı test dağıtımını ardışık düzeninde gerçekleştirmelisiniz.

## <a name="before-you-begin"></a>Başlamadan önce
CI/CD hattınızı Azure DevTest Labs ile tümleştirebilirsiniz önce Visual Studio Marketi'nden uzantısını yüklemeniz gerekir.
1. Git [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
1. Seçin **yükleme**.
1. Sihirbazı tamamlayın.

## <a name="create-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu oluşturma
Bu bölümde, isteğe bağlı bir Azure sanal makinesini oluşturmak için kullandığınız Azure Resource Manager şablonu oluşturmayı açıklar.

1. Aboneliğinizde bir Resource Manager şablonu oluşturmak için yordamını tamamlayın [bir Resource Manager şablonu kullanmak](devtest-lab-use-resource-manager-template.md).
1. Resource Manager şablonu oluşturmadan önce eklemek [WinRM yapı](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) VM oluşturmanın bir parçası olarak.

   WinRM erişim dağıtım görevlerini gibi kullanmak için gerekli olduğunu *Azure dosya kopyalama* ve *PowerShell hedef makinede*.

   > [!NOTE]
   > WinRM paylaşılan bir IP adresi ile kullandığınızda, WinRM bağlantı noktasına bir dış bağlantı noktası eşleme için NAT kuralı eklemeniz gerekir. Bir ortak IP adresi ile VM oluşturursanız, bu adım gerekli değildir.
   >
   >

1. Şablon, bilgisayarınızdaki bir dosya olarak kaydedin. Dosya adı **CreateVMTemplate.json**.
1. Şablonu kaynak denetim sisteminiz denetleyin.
1. Bir metin düzenleyicisinde açın ve aşağıdaki komut dosyası yapıştırın.

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

1. Betik kaynak denetim sisteminiz iade edildi. Gibi bir ad **GetLabVMParams.ps1**.

   Yayın tanımının bir parçası aracı üzerinde bu komut dosyasını çalıştırdığınızda ve görev adımları gibi kullanırsanız *Azure dosya kopyalama* veya *PowerShell hedef makinede*, komut dosyası için gereken değerleri toplar Uygulamanızı VM'ye dağıtın. Bir Azure VM uygulamaları dağıtmak için bu görevleri genellikle kullanırsınız. Görevleri VM kaynak grubu adı, IP adresi ve tam etki alanı adı (FDQN) gibi değerler gerektirir.

## <a name="create-a-release-definition-in-release-management"></a>Yayın Yönetimi'nde bir yayın tanımı oluşturun
Yayın tanımı oluşturmak için aşağıdakileri yapın:

1. Üzerinde **sürümleri** sekmesinde **yapı & yayın** hub, artı işareti (+) düğmesini seçin.
2. İçinde **oluşturma yayın tanımı** penceresinde, seçin **boş** şablonu ve ardından **sonraki**.
3. Seçin **daha sonra seçmek**ve ardından **oluşturma** bir varsayılan ortamı ve hiçbir bağlı yapıları ile yeni bir sürüm tanımı oluşturmak için.
4. Kısayol menüsünü açmak için yeni sürüm tanımı, ortam adının yanındaki üç nokta (...) seçin ve ardından **değişkenleri yapılandırmanızı**. 
5. İçinde **yapılandırma - ortamı** penceresinde, değişkenleri, sürüm tanımı görevleri kullanmak için aşağıdaki değerleri girin:

   a. İçin **vmName**, Resource Manager şablonu Azure portalında oluşturduğunuz zaman VM'ye atanan adı girin.

   b. İçin **kullanıcıadı**, Resource Manager şablonu Azure portalında oluşturduğunuz zaman, VM'ye atanan kullanıcı adı girin.

   c. İçin **parola**, Azure portalında Resource Manager şablonu oluştururken VM'ye atanan parolayı girin. Parolayı güvenli ve gizleme için "asma kilit" simgesini kullanın.

### <a name="create-a-vm"></a>VM oluşturma

Dağıtımın sonraki aşamasına sonraki dağıtımları için "altın görüntü" kullanmak üzere VM oluşturmaktır. Bu amaç için özel olarak geliştirilen görevini kullanarak Azure DevTest laboratuvarı örneğinizi içinde VM oluşturun. 

1. Yayın tanımı'nda seçin **görev ekleme**.
2. Üzerinde **dağıtma** sekmesinde, eklemek bir *Azure DevTest Labs VM Oluştur* görev. Görev aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > Sonraki dağıtımları için kullanmak üzere VM oluşturmak için bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** listelemek veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. İçin **Laboratuvar adı**, daha önce oluşturduğunuz örneğinin adını seçin.

   c. İçin **şablon adı**, kaynak kod deponuza kaydettiğiniz şablon dosyasının adını ve tam yolunu girin. Yolu örneğin basitleştirmek için yayın Yönetimi yerleşik özelliklerini kullanabilirsiniz:

   ```
   $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
   ```

   d. İçin **şablon parametreleri**, şablonda tanımlanan değişkenler parametrelerini girin. Ortamında, örneğin tanımlı değişkenlerin adlarını kullanın:

   ```
   -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
   ```

   e. İçin **çıkış değişkenleri - Laboratuvar VM kimliği**, yeni oluşturulan VM kimliği için sonraki adımları gerekir. Bu kimlikle otomatik olarak doldurulur ortam değişkeni varsayılan adını ayarlamak **çıkış değişkenleri** bölümü. Gerekirse, değişkeni düzenleyin, ancak sonraki görevlerinde doğru adı kullanmayı unutmayın. Laboratuvar VM kimliği şu biçimde yazılır:

   ```
   /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
   ```

3. DevTest Labs VM ayrıntıları toplamak için daha önce oluşturduğunuz betiğini yürütün. 
4. Yayın tanımı'nda seçin **görev ekleme** ve ardından **dağıtma** sekmesinde, eklemek bir *Azure PowerShell* görev. Görev aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > DevTest Labs VM ayrıntıları toplamak için bkz: [Dağıt: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) ve komut dosyasını çalıştırın.

   a. İçin **Azure bağlantı türü**seçin **Azure Resource Manager**.

   b. İçin **Azure RM abonelik**, altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları**, ya da Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   c. İçin **komut dosyası türü**seçin **komut dosyası**.
 
   d. İçin **betik yolu**, kaynak kod deponuza kaydettiğiniz komut dosyasının adını ve tam yolunu girin. Yolu örneğin basitleştirmek için yayın Yönetimi yerleşik özelliklerini kullanabilirsiniz:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   e. İçin **komut dosyası değişkenleri**, otomatik olarak VM Laboratuvar Kimliğiyle önceki görev tarafından örneğin doldurulmuş olduğu ortam değişkeninin adını girin: 
      ```
      -labVmId '$(labVMId)'
      ```
    Komut dosyasını gerekli değerleri toplar ve bunları kolayca sonraki adımlarda başvurabilmeniz için bunları ortam değişkenleri yayın tanımı içinde depolar.

5. Uygulamanızı yeni bir DevTest Labs VM dağıtın. Normalde uygulamayı dağıtmak için kullandığınız görevler *Azure dosya kopyalama* ve *PowerShell hedef makinede*.
   Bu görevleri parametreleri için gereken VM hakkında bilgi adında üç yapılandırma değişkenleri depolanan **labVmRgName**, **labVMIpAddress**, ve **labVMFqdn**yayın tanımı içinde. Yalnızca bir DevTest Labs VM ve özel bir görüntü uygulama dağıtmadan oluşturmayla denemek istiyorsanız, bu adımı atlayabilirsiniz.

### <a name="create-an-image"></a>Görüntü oluştur

Sonraki aşamaya Azure DevTest Labs örneğinizi yeni dağıtılan VM görüntüsü oluşturmaktır. Görüntü sonra geliştirme görevini yürütme veya bazı testleri çalıştırmak istediğiniz zaman isteğe bağlı olarak VM kopyasını oluşturmak için de kullanabilirsiniz. 

1. Yayın tanımı'nda seçin **görev ekleme**.
2. Üzerinde **dağıtma** sekmesinde, eklemek bir **Azure DevTest Labs özel görüntü oluşturma** görev. Aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > Görüntüsü oluşturmak için bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, **kullanılabilir Azure hizmet bağlantıları** listesinde, listeden bir bağlantı seçin veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. İçin **Laboratuvar adı**, daha önce oluşturduğunuz örneğinin adını seçin.

   c. İçin **özel görüntü adı**, özel görüntü için bir ad girin.

   d. (İsteğe bağlı) İçin **açıklama**, daha sonra doğru görüntüyü seçmek kolaylaştırmak için bir açıklama girin.

   e. İçin **kaynak Laboratuvar VM - kaynak Laboratuvar VM kimliği**, önceki bir görev tarafından VM Laboratuvar Kimliğini otomatik olarak doldurulur ortam değişkeninin varsayılan adıyla değiştirdiyseniz burada düzenleyin. Varsayılan değer **$(labVMId)**.

   f. İçin **çıkış değişkenleri - özel görüntü kimliği**, yönetme veya silmek istediğiniz zaman yeni oluşturulan görüntü kimliği gerekir. Bu Kimliğe sahip otomatik olarak doldurulur ortam değişkeni varsayılan adını kümesinde **çıkış değişkenleri** bölümü. Gerekirse, değişkeni düzenleyebilirsiniz.

### <a name="delete-the-vm"></a>VM silme

Son aşaması, Azure DevTest Labs örneğinde dağıtılan VM silmektir. Geliştirme görevleri yürütün veya gereksinim duyduğunuz testleri dağıtılan VM üzerinde çalışan sonra normalde VM siler. 

1. Yayın tanımı'nda seçin **görev ekleme** ve ardından **dağıtma** sekmesinde, eklemek bir *Azure DevTest Labs Sil VM* görev. Aşağıdaki gibi yapılandırın:

      > [!NOTE]
      > VM silmek için bkz: [Azure DevTest Labs görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmet bağlantıları** listelemek veya Azure aboneliğinizi daha kısıtlı izinleri bağlantı oluşturun. Daha fazla bilgi için bkz: [Azure Resource Manager Hizmeti uç noktası](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
 
   b. İçin **Laboratuvar VM kimliği**, önceki bir görev tarafından VM Laboratuvar Kimliğini otomatik olarak doldurulur ortam değişkeninin varsayılan adıyla değiştirdiyseniz burada düzenleyin. Varsayılan değer **$(labVMId)**.

2. Yayın tanımı için bir ad girin ve sonra kaydedin.
3. Yeni bir sürüm oluşturmak, en son sürüme seçin ve tanımı'ndaki tek ortamı dağıtın.

Her aşamada DevTest Labs örneğinizi Azure Portal'daki VM ve oluşturulan görüntü ve yeniden siliniyor VM görüntülemek için görünümü yenileyin.

Özel görüntü artık gerekli olduğunda VM'ler oluşturmak için de kullanabilirsiniz.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
* Daha fazla hızlı başlangıç Resource Manager şablonları DevTest Labs Otomasyon keşfedin [ortak DevTest Labs GitHub deposuna](https://github.com/Azure/azure-quickstart-templates).
* Gerekirse, bakın [VSTS sorun giderme](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) sayfası.
 
