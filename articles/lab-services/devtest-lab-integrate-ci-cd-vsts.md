---
title: Azure DevTest Labs, Azure işlem hatları sürekli tümleştirme ve teslim komut zinciriyle tümleştirmeyi | Microsoft Docs
description: Azure DevTest Labs, Azure işlem hatları sürekli tümleştirme ve teslim işlem hattı tümleştirme hakkında bilgi edinin
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
ms.openlocfilehash: 7806599c1a2f1396ff4b07d6f0538057654029d7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66157123"
---
# <a name="integrate-azure-devtest-labs-into-your-azure-devops-continuous-integration-and-delivery-pipeline"></a>Azure DevTest Labs, Azure DevOps, sürekli tümleştirme ve teslim işlem hattı tümleştirme
Kullanabileceğiniz *Azure DevTest Labs görevlerini* Azure DevOps kolayca yüklenir uzantısı, Azure DevTest Labs ile CI/CD derleme ve yayın işlem hattınızı tümleştirin. Üç görevin uzantıyı yükleyen: 
* VM oluşturma
* VM'den özel görüntü oluşturma
* VM silme 

İşlem, örneğin, belirli bir test görevi için "altın görüntü" hızlı bir şekilde dağıtmak ve test bittiğinde silmek kolaylaştırır.

Bu makale oluşturma ve bir VM dağıtma, özel bir görüntü oluşturun ve VM tam bir işlem hattı olarak tüm silin. Her görev normalde tek tek ve kendi özel yapı test etme-dağıtma hattında gerçekleştirmelisiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Azure DevTest Labs ile CI/CD ardışık düzeninizi tümleştirebilirsiniz önce uzantıyı Visual Studio Market'ten yüklemeniz gerekir.
1. Git [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
1. **Yükle**’yi seçin.
1. Sihirbazı tamamlayın.

## <a name="create-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu oluşturma
Bu bölümde, isteğe bağlı bir Azure sanal makinesi oluşturmak için kullandığınız Azure Resource Manager şablonunun nasıl oluşturulacağını açıklar.

1. Aboneliğinizde bir Resource Manager şablonu oluşturmak için yordamı tamamlayın. [bir Resource Manager şablonu kullanma](devtest-lab-use-resource-manager-template.md).
1. Resource Manager şablonu oluşturmadan önce ekleme [WinRM yapıt](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) VM oluşturma işleminin parçası olarak.

   WinRM erişimi gibi dağıtım görevlerini kullanmak için gerekli olduğunu *Azure dosya kopyalama* ve *PowerShell hedef makinede*.

   > [!NOTE]
   > WinRM paylaşılan bir IP adresi ile kullandığınızda, bir dış bağlantı noktası WinRM bağlantı noktasına eşlemek için bir NAT kuralı eklemeniz gerekir. Bir genel IP adresiyle VM oluşturursanız, bu adım gerekli değildir.
   >
   >

1. Şablonu, bilgisayarınızdaki bir dosya olarak kaydedin. Dosya adı **CreateVMTemplate.json**.
1. Şablon, kaynak denetim sistemine iade edin.
1. Bir metin düzenleyicisinde açın ve aşağıdaki komut dosyası içine yapıştırın.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Betik, kaynak denetim sistemine iade edin. Şöyle adlandırın **GetLabVMParams.ps1**.

   Bu betik, aracıda sürüm ardışık düzeninin bir parçası olarak çalıştırmak ve görev adımları gibi kullanırsanız *Azure dosya kopyalama* veya *PowerShell hedef makinede*, komut dosyası için gereken değerleri toplar. Uygulamanızı VM dağıtın. Normalde, uygulamalarını bir Azure VM dağıtmak için bu görevleri kullanmanız gerekir. Görevler, VM kaynak grubu adı, IP adresi ve tam etki alanı adı (FQDN) gibi değerleri gerektirir.

## <a name="create-a-release-pipeline-in-release-management"></a>Release Management'ta yayın işlem hattı oluşturma
Yayın işlem hattı oluşturmak için aşağıdakileri yapın:

1. Üzerinde **yayınlar** sekmesinde **derleme ve yayınlama** hub'ı artı (+) düğmesini seçin.
1. İçinde **Oluştur yayın tanımı** penceresinde **boş** şablonu ve ardından **sonraki**.
1. Seçin **daha sonra seçin**ve ardından **Oluştur** bağlantılı yapıt yok ve bir varsayılan ortam ile yeni bir yayın işlem hattı oluşturmak için.
1. Yeni yayın işlem hattında kısayol menüsünü açın, ortam adının yanındaki üç nokta (...) seçin ve ardından **değişkenlerini yapılandırma**. 
1. İçinde **Yapılandır - ortam** yayın işlem hattı görevleri'nde, kullandığınız değişkenler penceresinde aşağıdaki değerleri girin:

   a. İçin **vmName**, Azure portalında Resource Manager şablonu oluşturduğunuzda, sanal Makineye atanmış bir ad girin.

   b. İçin **kullanıcıadı**, Azure portalında Resource Manager şablonu oluşturduğunuzda, sanal Makineye atanan kullanıcı adını girin.

   c. İçin **parola**, Azure portalında Resource Manager şablonu oluşturduğunuzda, sanal Makineye atanan parolayı girin. Parolayı güvenli ve gizleme için "asma kilide" simgesini kullanın.

### <a name="create-a-vm"></a>VM oluşturma

Dağıtımın sonraki aşamasına sonraki dağıtımlarda "altın görüntü" kullanmak üzere VM oluşturmaktır. Azure DevTest Labs örneğinizin içinde bu amaç için özel olarak geliştirilen görev kullanarak VM oluşturma. 

1. Yayın işlem hattında seçin **görev ekleme**.
1. Üzerinde **Dağıt** sekmesinde, ekleme bir *Azure DevTest Labs VM Oluştur* görev. Görev aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > Sonraki dağıtımları için kullanmak üzere VM oluşturmak için bkz [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmeti bağlantıları** listelemek ya da Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   b. İçin **Laboratuvar adı**, daha önce oluşturduğunuz örneğinin adını seçin.

   c. İçin **şablon adı**, kaynak kod deponuza kaydettiğiniz şablon dosyasının adını ve tam yolunu girin. Yol, örneğin basitleştirmek için Release Management'ın yerleşik özelliklerini kullanabilirsiniz:

   ```
   $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
   ```

   d. İçin **şablon parametreleri**, şablonda tanımlanan değişkenler parametrelerini girin. Ortamında, örneğin tanımladığınız değişkenlerin adlarını kullanın:

   ```
   -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
   ```

   e. İçin **çıkış değişkenleri - Lab VM kimliği**, sonraki adımlar için yeni oluşturulan VM kimliği gerekir. Bu kimlikle otomatik olarak doldurulan bir ortam değişkeni varsayılan adını ayarlayın **çıkış değişkenleri** bölümü. Gerekirse değişkeni düzenleyin, ancak sonraki görevler için doğru adı kullanmanız gerektiğini unutmayın. Laboratuvar VM kimliği şu biçimde yazılır:

   ```
   /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
   ```

1. DevTest Labs sanal makinenin ayrıntıları toplamak için daha önce oluşturduğunuz komut dosyasını çalıştırın. 
1. Yayın işlem hattında seçin **görev ekleme** ve ardından **Dağıt** sekmesinde, ekleme bir *Azure PowerShell* görev. Görev aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > DevTest Labs sanal makinenin ayrıntıları toplamak için bkz: [Dağıt: Azure PowerShell](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzurePowerShellV3) betiği yürütün.

   a. İçin **Azure bağlantı türü**seçin **Azure Resource Manager**.

   b. İçin **Azure RM abonelik**, altındaki listeden bir bağlantı seçin **kullanılabilir Azure hizmeti bağlantıları**, ya da Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   c. İçin **betik türü**seçin **betiği**.
 
   d. İçin **betik yolu**, kaynak kod deponuza kaydettiğiniz komut dosyasının adını ve tam yolunu girin. Yol, örneğin basitleştirmek için Release Management'ın yerleşik özelliklerini kullanabilirsiniz:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   e. İçin **betik bağımsız değişkenleri**, otomatik olarak Laboratuvar sanal makinesi kimliği ile önceki görev tarafından örneğin doldurulup doldurulmadığına ortam değişkeni adını girin: 
      ```
      -labVmId '$(labVMId)'
      ```
    Betiği, gerekli değerleri toplar ve bunları kolayca sonraki adımlarda başvurabilmeniz için yayın işlem hattı içindeki ortam değişkenleri içindeki depolar.

1. Uygulamanızı yeni bir DevTest Labs VM dağıtın. Normalde uygulamayı dağıtmak için kullandığınız görevler *Azure dosya kopyalama* ve *PowerShell hedef makinede*.
   Bu görevlerin parametrelerini ihtiyacınız VM hakkında bilgi adlı üç yapılandırma değişkenlerinde depolanan **labVmRgName**, **labVMIpAddress**, ve **labVMFqdn**yayın işlem hattı içindeki. Yalnızca DevTest Labs VM ve özel bir görüntü için bir uygulamayı dağıtmadan oluşturmayla denemek istiyorsanız, bu adımı atlayabilirsiniz.

### <a name="create-an-image"></a>Görüntü oluştur

Azure DevTest Labs Örneğinizde yeni dağıtılan VM görüntüsü oluşturmak için sonraki aşamadır bakın. Ardından, bir geliştirme görevi yürütme veya bazı testler çalıştırmak istediğiniz zaman isteğe bağlı olarak VM kopyaları oluşturmak için görüntüyü kullanabilirsiniz. 

1. Yayın işlem hattında seçin **görev ekleme**.
1. Üzerinde **Dağıt** sekmesinde, ekleme bir **Azure DevTest Labs özel görüntü oluşturma** görev. Aşağıdaki gibi yapılandırın:

   > [!NOTE]
   > Görüntüyü oluşturmak için bkz: [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, **kullanılabilir Azure hizmeti bağlantıları** listesinde, listeden bir bağlantı seçin veya Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

   b. İçin **Laboratuvar adı**, daha önce oluşturduğunuz örneğinin adını seçin.

   c. İçin **özel görüntü adı**, özel görüntü için bir ad girin.

   d. (İsteğe bağlı) İçin **açıklama**, daha sonra doğru görüntüyü seçmek kolay hale getirmek için bir açıklama girin.

   e. İçin **kaynak Lab VM - kaynak Laboratuvar VM kimliği**, kimliği ' % s'Laboratuvar VM önceki bir görev tarafından otomatik olarak doldurulan bir ortam değişkeni varsayılan adını değiştirdiyseniz burada düzenleyin. Varsayılan değer **$(labVMId)** .

   f. İçin **çıkış değişkenleri - özel görüntü kimliği**, silmek veya yönetmek istediğinizde, yeni oluşturulan görüntü kimliği gerekir. Bu kimliği ile otomatik olarak doldurulur ortam değişkeni varsayılan adını kümesinde **çıkış değişkenleri** bölümü. Gerekirse değişkeni düzenleyebilirsiniz.

### <a name="delete-the-vm"></a>VM’yi silin

Son aşama, Azure DevTest Labs Örneğinizde dağıttığınız VM'yi silmektir. Geliştirme görevleri yürütmek ya da gereken dağıtılmış VM üzerinde testler sonra normalde VM siler. 

1. Yayın işlem hattında seçin **görev ekleme** ve ardından **Dağıt** sekmesinde, ekleme bir *Azure DevTest Labs Sil VM* görev. Aşağıdaki gibi yapılandırın:

      > [!NOTE]
      > Sanal Makineyi silmek için bkz: [Azure DevTest Labs görevlerini](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. İçin **Azure RM abonelik**, bir bağlantı seçin **kullanılabilir Azure hizmeti bağlantıları** listelemek ya da Azure aboneliğinize daha kısıtlı izinler bağlantı oluşturun. Daha fazla bilgi için [Azure Resource Manager hizmet uç noktası](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).
 
   b. İçin **Laboratuvar VM kimliği**, kimliği ' % s'Laboratuvar VM önceki bir görev tarafından otomatik olarak doldurulan bir ortam değişkeni varsayılan adını değiştirdiyseniz burada düzenleyin. Varsayılan değer **$(labVMId)** .

1. Yayın işlem hattı için bir ad girin ve kaydedin.
1. Yeni yayın oluştur, en son derlemeyi seçin ve işlem hattında tek bir ortama dağıtın.

Her aşamada DevTest Labs örneğinizin Azure portalındaki VM ve oluşturulan görüntü ve yeniden siliniyor VM görüntülemek için görünümü yenileyin.

Özel görüntü artık gerekli olduklarında, sanal makineler oluşturmak için de kullanabilirsiniz.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
* DevTest Labs Otomasyon için daha fazla hızlı başlangıç Resource Manager şablonları keşfedin [genel DevTest Labs GitHub deposunu](https://github.com/Azure/azure-quickstart-templates).
* Gerekirse, bkz. [Azure DevOps sorun giderme](https://docs.microsoft.com/azure/devops/pipelines/troubleshooting) sayfası.
 
