---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d14cfb82ae74f85425dbd3e8a365e8b99969641d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485211"
---
## <a name="using-vm-extensions"></a>VM uzantılarını kullanma
Azure VM uzantıları uygulama davranışları veya ya da Azure Vm'lerde bulunan iş diğer programları yardımcı olan özellikler (örneğin, **WebDeployForVSDevTest** uzantı Visual Studio Web dağıtımı çözümleri Azure vm'nizdeki sağlar) veya sağlayın başka bir davranışı destekleme yeteneği, sanal makine ile etkileşim kurmak (örneğin, PowerShell, Azure CLI ve REST istemcisinden VM erişimi uzantılarını sıfırlama veya Azure sanal Makinenize uzaktan erişim değerleri değiştirmek için kullanabilirsiniz).

> [!IMPORTANT]
> Uzantıları destekledikleri özellikler tarafından tam bir listesi için bkz. [Azure VM uzantıları ve özellikleri](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Her VM uzantısı, belirli bir özelliği desteklediğinden, tam olarak neleri yapıp sahip bir uzantı olamaz uzantının bağlıdır. Bu nedenle, sanal makinenizin değiştirmeden önce kullanmak istediğiniz VM uzantısı için belgeleri okuduğunuz emin olun. Bazı VM uzantıları kaldırılması desteklenmiyor; diğer önemli ölçüde VM davranışını değiştirmek ayarlanabilir özelliklere sahiptir.
> 
> 

En yaygın görevleri şunlardır:

1. Kullanılabilir uzantıları bulma
2. Yüklü uzantılar güncelleştiriliyor
3. Uzantılar ekleme
4. Uzantıları kaldırma

## <a name="find-available-extensions"></a>Kullanılabilir uzantılar bulun
Uzantı ve genişletilmiş bilgileri kullanarak bulabilirsiniz:

* PowerShell
* Azure platformlar arası komut satırı arabirimi (Azure CLI)
* Hizmet Yönetimi REST API'si

### <a name="azure-powershell"></a>Azure PowerShell
Bazı uzantılar, Powershell'den kendi yapılandırmasını daha kolay hale getirebilir, özel PowerShell cmdlet'leri vardır; ancak tüm VM uzantıları için aşağıdaki cmdlet'leri çalışır.

Kullanılabilir uzantılar hakkında bilgi edinmek için aşağıdaki cmdlet'leri kullanabilirsiniz:

* İçin kullanabileceğiniz örnek web rolü veya çalışan rolleri [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet'i.
* İçin kullanabileceğiniz sanal makine örnekleri, [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet'i.
  
   Örneğin, aşağıdaki kod örneği bilgilerini listelemek nasıl gösterir **IaaSDiagnostics** PowerShell kullanarak uzantısı.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Azure komut satırı arabirimi (Azure CLI)
Bazı uzantılar, kendilerine özgü Azure CLI komutları sahip (Docker VM uzantısını bir, kendi yapılandırmasını daha kolay hale getirebilir örnektir); ancak tüm VM uzantıları için aşağıdaki komutları çalışır.

Kullanabileceğiniz **azure vm uzantısı listesinde** kullanılabilir uzantılar hakkında bilgi edinmek ve kullanmak için komutu **–-json** bir veya daha fazla uzantıları ilgili tüm mevcut bilgileri görüntülemek için seçeneği. Bir uzantı adı kullanmazsanız, komut tüm kullanılabilir uzantılar JSON açıklamasını döndürür.

Örneğin, aşağıdaki kod örneği bilgilerini listelemek nasıl gösterir **IaaSDiagnostics** Azure CLI kullanarak uzantı **azure vm uzantısı listesinde** komut ve kullandığı **–-json**  tam bilgi almak için seçeneği.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Hizmet Yönetimi REST API'leri
Kullanılabilir uzantılar hakkında bilgi edinmek için aşağıdaki REST API'lerini kullanabilirsiniz:

* İçin kullanabileceğiniz örnek web rolü veya çalışan rolleri [kullanılabilir uzantılar listesi](https://msdn.microsoft.com/library/dn169559.aspx) işlemi. Kullanılabilir uzantılar sürümleri listelemek için kullanabileceğiniz [listesi uzantı sürümleri](https://msdn.microsoft.com/library/dn495437.aspx).
* İçin kullanabileceğiniz sanal makine örnekleri, [listesi kaynak uzantıları](https://msdn.microsoft.com/library/dn495441.aspx) işlemi. Kullanılabilir uzantılar sürümleri listelemek için kullanabileceğiniz [listesi kaynak uzantısı sürümleri](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Ekleme, güncelleştirme veya uzantıları devre dışı bırakma
Uzantıları, bir örneği oluşturulduğunda veya çalışan bir örneğe eklenebilir eklenebilir. Uzantılar güncelleştirildi, devre dışı veya kaldırılacak. Azure PowerShell cmdlet'lerini kullanarak veya hizmet yönetimi REST API'si işlemleri kullanarak bu eylemleri gerçekleştirebilirsiniz. Bazı uzantıları ayarlama ve yükleme için gerekli parametreler. Genel ve özel parametreler uzantıları için desteklenir.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell cmdlet'lerini kullanarak, ekleme ve uzantıları güncelleştirmek için en kolay yoludur. Uzantı cmdlet'leri kullanırken uzantı yapılandırmasını çoğunu sizin yerinize gerçekleştirilir. Bazen, program aracılığıyla bir uzantı eklemeniz gerekebilir. Bunu yapmak, ihtiyacınız olduğunda, uzantı yapılandırmasını sağlamalısınız.

Bir uzantı parametrelerinin ortak ve özel bir yapılandırma gerektirip gerektirmediğini öğrenmek için aşağıdaki cmdlet'leri kullanabilirsiniz:

* İçin kullanabileceğiniz örnek web rolü veya çalışan rolleri **Get-AzureServiceAvailableExtension** cmdlet'i.
* İçin kullanabileceğiniz sanal makine örnekleri, **Get-AzureVMAvailableExtension** cmdlet'i.

### <a name="service-management-rest-apis"></a>Hizmet Yönetimi REST API'leri
REST API'lerini kullanarak kullanılabilir uzantıların listesini aldığınızda, uzantıyı nasıl yapılandırılacağı hakkında bilgi alırsınız. Döndürülen bilgilerin ortak şema ve özel şema tarafından temsil edilen parametre bilgilerini gösterebilir. Genel parametre değerleri sorguların örnekleri hakkında döndürülür. Özel parametre değerlerini döndürülmez.

Bir uzantı parametrelerinin ortak ve özel bir yapılandırma gerektirip gerektirmediğini öğrenmek için aşağıdaki REST API'lerini kullanabilirsiniz:

* İçin web rolü veya çalışan rolleri örneklerini **PublicConfigurationSchema** ve **PrivateConfigurationSchema** öğeleri içeren gelen yanıt bilgileri [listesi kullanılabilir Uzantıları](https://msdn.microsoft.com/library/dn169559.aspx) işlemi.
* İçin sanal makine örneklerini **PublicConfigurationSchema** ve **PrivateConfigurationSchema** öğeleri içeren gelen yanıt bilgileri [listesi kaynak Uzantıları](https://msdn.microsoft.com/library/dn495441.aspx) işlemi.

> [!NOTE]
> Uzantılar, JSON ile tanımlanan yapılandırmaları da kullanabilirsiniz. Bu türde bir uzantısı kullanıldığında, yalnızca **SampleConfig** öğe kullanılır.
> 
> 

