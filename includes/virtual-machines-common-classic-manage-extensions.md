


## <a name="using-vm-extensions"></a>VM uzantıları kullanma
Azure VM uzantıları uygulamak davranışları veya ya da Azure Vm'lerde iş diğer programları yardımcı olan özellikleri (örneğin, **WebDeployForVSDevTest** uzantısı Visual Studio Web dağıtımı çözümleri, Azure VM'de izin verir) veya sağlayın başka bir davranışı destek yeteneği, VM ile etkileşim kurmak (örneğin, PowerShell, Azure CLI ve REST istemcilerden VM erişim uzantıları sıfırlama veya Azure VM üzerinde uzaktan erişim değerlerini değiştirmek için kullanabileceğiniz).

> [!IMPORTANT]
> Uzantılar destekledikleri özellikleri için tam bir listesi için bkz: [Azure VM uzantıları ve özellikleri](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Her VM uzantısı belirli bir özelliği desteklediğinden, tam olarak ne yapıp uzantılı olamaz uzantısı bağlıdır. Bu nedenle, VM değiştirmeden önce kullanmak istediğiniz VM uzantısı belgelerini okuyun emin olun. Bazı VM uzantıları kaldırma desteklenmiyor; diğer VM davranışı tüketicisinin değiştirmek ayarlanabilir özelliklere sahiptir.
> 
> 

En yaygın görevleri şunlardır:

1. Kullanılabilir uzantılar bulma
2. Yüklenen uzantıları güncelleştiriliyor
3. Uzantılar ekleme
4. Uzantılarını kaldırma

## <a name="find-available-extensions"></a>Kullanılabilir uzantılar bulun
Uzantı ve genişletilmiş bilgileri kullanarak bulabilirsiniz:

* PowerShell
* Azure platformlar arası komut satırı arabirimi (Azure CLI)
* Hizmet Yönetimi REST API'si

### <a name="azure-powershell"></a>Azure PowerShell
Bazı uzantılar yapılandırmalarını Powershell'den daha kolay hale getirebilir kendilerine özgü PowerShell cmdlet'lerini; yine de sahip istiyor musunuz? ancak tüm VM uzantıları için aşağıdaki cmdlet'leri çalışma.

Kullanılabilir uzantılar hakkında bilgi edinmek için aşağıdaki cmdlet'leri kullanın:

* İçin kullanabileceğiniz web rolleri veya çalışan rolleri örneklerini, [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet'i.
* İçin kullanabileceğiniz sanal makineleri örnekleri, [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet'i.
  
   Örneğin, aşağıdaki kod örneğinde bilgilerini listelemek gösterilmiştir **IaaSDiagnostics** PowerShell kullanarak uzantısı.
  
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
Kendilerine özgü Azure CLI komutlarının bazı uzantılar vardır (Docker VM uzantısı bir örnek, yapılandırmalarını daha kolay hale getirebilir olması); Ancak, aşağıdaki komutlar için tüm VM uzantıları çalışır.

Kullanabileceğiniz **azure vm uzantısı listesinde** kullanılabilir uzantıları hakkında bilgi edinme ve kullanma komutu **–-json** bir veya daha fazla uzantıları hakkında tüm kullanılabilir bilgi görüntülemek için seçeneği. Bir uzantı adı kullanmıyorsanız komutu tüm kullanılabilir uzantıları JSON açıklamasını döndürür.

Örneğin, aşağıdaki kod örneğinde bilgilerini listelemek gösterilmiştir **IaaSDiagnostics** uzantısını Azure CLI kullanarak **azure vm uzantısı listesinde** komut ve kullandığı **–-json**  tam bilgi almak için seçeneği.

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
Kullanılabilir uzantılar hakkında bilgi edinmek için aşağıdaki REST API'ler kullanabilirsiniz:

* İçin kullanabileceğiniz web rolleri veya çalışan rolleri örneklerini, [listesi kullanılabilir uzantıları](https://msdn.microsoft.com/library/dn169559.aspx) işlemi. Kullanılabilir uzantılar sürümleri listelemek için kullanabileceğiniz [listesi uzantısı sürümleri](https://msdn.microsoft.com/library/dn495437.aspx).
* İçin kullanabileceğiniz sanal makineleri örnekleri, [listesi kaynak uzantıları](https://msdn.microsoft.com/library/dn495441.aspx) işlemi. Kullanılabilir uzantılar sürümleri listelemek için kullanabileceğiniz [listesi kaynak uzantısı sürümleri](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Ekleme, güncelleştirme ya da uzantılarını devre dışı bırakma
Uzantıları örneği oluşturulduğunda veya çalışan bir örneğine eklenen eklenebilir. Uzantıları güncelleştirildi, devre dışı veya kaldırılabilir. Azure PowerShell cmdlet'lerini kullanarak veya hizmet yönetimi REST API işlemleri kullanarak bu eylemleri gerçekleştirebilir. Parametreleri yüklemek ve bazı uzantılar ayarlamak için gereklidir. Ortak ve özel parametreleri uzantılar için desteklenir.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell cmdlet'lerini kullanarak, Ekle ve uzantıları güncelleştirmek için kolay bir yoludur. Uzantı cmdlet'leri kullandığınızda, uzantı yapılandırmasını çoğunu yapılır sizin için. Bazen, bir uzantı programlı olarak eklemeniz gerekebilir. Bunu yapmak gereksinim duyduğunuzda uzantısı yapılandırma sağlamanız gerekir.

Uzantı genel ve özel parametrelerinin yapılandırması gerekli olup olmadığını öğrenmek için aşağıdaki cmdlet'leri kullanın:

* İçin kullanabileceğiniz web rolleri veya çalışan rolleri örneklerini, **Get-AzureServiceAvailableExtension** cmdlet'i.
* İçin kullanabileceğiniz sanal makineleri örnekleri, **Get-AzureVMAvailableExtension** cmdlet'i.

### <a name="service-management-rest-apis"></a>Hizmet Yönetimi REST API'leri
REST API'lerini kullanarak kullanılabilir uzantıları listesini almak için uzantı nasıl yapılandırılması hakkında bilgi alıyorsunuz. Döndürülen bilgilerin bir ortak şema ve özel şema tarafından temsil edilen parametre bilgilerini gösterebilir. Ortak parametre değerlerini örnekleri hakkında sorgularda döndürülür. Özel parametre değerlerini döndürülmez.

Uzantı genel ve özel parametrelerinin yapılandırması gerekli olup olmadığını öğrenmek için aşağıdaki REST API'ler kullanabilirsiniz:

* İçin web rolleri veya çalışan rolleri örneklerini **PublicConfigurationSchema** ve **PrivateConfigurationSchema** öğeleri içeren yanıttan bilgileri [listesi kullanılabilir Uzantıları](https://msdn.microsoft.com/library/dn169559.aspx) işlemi.
* İçin sanal makine örneklerini **PublicConfigurationSchema** ve **PrivateConfigurationSchema** öğeleri içeren yanıttan bilgileri [listesi kaynak Uzantıları](https://msdn.microsoft.com/library/dn495441.aspx) işlemi.

> [!NOTE]
> Uzantılar, JSON ile tanımlanan yapılandırmaları da kullanabilirsiniz. Bu tür uzantıları kullanıldığında, yalnızca **SampleConfig** öğe kullanılır.
> 
> 

