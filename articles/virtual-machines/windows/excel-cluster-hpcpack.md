---
title: HPC Pack kümesinde Excel ve SOA | Microsoft Docs
description: Azure'da bir HPC Pack kümesinde Excel ve SOA büyük ölçekli iş yüklerini çalıştırmaya başlama
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: aaf26e04fdb38fd76f4ab8211f9fdda8ebafd668
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971868"
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Azure'da bir HPC Pack kümesinde Excel ve SOA iş yüklerini çalıştırmaya başlama
Bu makalede Azure Hızlı Başlangıç şablonu veya isteğe bağlı olarak bir Azure PowerShell dağıtım betiğini kullanarak bir Azure sanal makineler'de Microsoft HPC Pack 2012 R2 kümesi dağıtmayı gösterir. Küme, Microsoft Excel veya hizmet odaklı mimari (SOA) iş yükleri HPC Pack ile çalışmak üzere tasarlanmış Azure Market VM görüntülerini kullanır. Bir şirket içi istemci bilgisayarından HPC Excel ve SOA hizmetlerini çalıştırmak için kümeyi kullanabilirsiniz. Excel çalışma kitabı aktarma ve Excel kullanıcı tanımlı işlevleri ya da UDF'ler Excel HPC hizmetleri içerir.

> [!IMPORTANT] 
> Bu makalede, özellikleri, şablonları ve betikler için HPC Pack 2012 R2 dayanır. Bu senaryoda, HPC Pack 2016'da şu anda desteklenmiyor.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Yüksek bir düzeyde, aşağıdaki diyagramda oluşturduğunuz HPC Pack kümesine gösterilmektedir.

![Excel iş yükleri çalıştıran düğümleriyle HPC Kümesi][scenario]

## <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar** -küme örnek Excel ve SOA iş göndermek için bir Windows tabanlı bir istemci bilgisayara ihtiyacı vardır. (Bu dağıtım yöntemi seçerseniz), Azure PowerShell küme dağıtım betiğini çalıştırmak için bir Windows bilgisayara da gerekir.
* **Azure aboneliği** -bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.
* **Çekirdek kota** -özellikle birden fazla çekirdekli VM boyutlarının birden çok küme düğümüyle dağıtırsanız, çekirdek kotasını artırmanız gerekebilir. Azure Hızlı Başlangıç şablonu kullanıyorsanız, Kaynak Yöneticisi'nde Çekirdek kotasını Azure Bölgesi ' dir. Bu durumda, belirli bir bölgede Kotayı artırmak gerekebilir. Bkz: [Azure abonelik limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md). Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ücret olmadan.
* **Microsoft Office lisansı** - işlem düğümleri, Microsoft Excel ile bir Market HPC Pack 2012 R2 sanal makine görüntüsünü kullanarak Microsoft Excel Professional Plus 2013 30 günlük değerlendirme sürümü yüklü dağıtın. Değerlendirme sürümünden sonra iş yüklerini çalıştırmaya devam etmek için Excel etkinleştirmek için geçerli bir Microsoft Office lisansı sağlamanız gerekir. Bkz: [Excel etkinleştirme](#excel-activation) bu makalenin ilerleyen bölümlerinde. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>1. Adım Azure'da bir HPC Pack kümesi ayarlama
HPC Pack 2012 R2 kümesini ayarlamak için iki seçenek göstereceğiz: ilk olarak, bir Azure Hızlı Başlangıç şablonu; Azure portal ile ve ikinci bir Azure PowerShell dağıtım betiğini kullanarak.

### <a name="option-1-use-a-quickstart-template"></a>1. Seçenek Hızlı Başlangıç şablonu kullanın
Azure Hızlı Başlangıç şablonu Azure portalında bir HPC Pack kümesine hızla dağıtmak için kullanın. Şablonu portalda açın, kümeniz için ayarları girin burada basit bir kullanıcı Arabirimi alın. Adımlar aşağıda verilmiştir. 

> [!TIP]
> İsterseniz, kullanan bir [Azure Market şablonu](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) Excel iş yükleri için özellikle benzer bir küme oluşturur. Adımları aşağıdakilerden biraz farklılık gösterir.
> 
> 

1. Ziyaret [HPC kümesi oluşturma şablonu GitHub sayfasında](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). İsterseniz, şablon ve kaynak kodu hakkında bilgileri gözden geçirin.
2. Tıklayın **azure'a Dağıt** şablonu Azure portalında bir dağıtım başlatmak üzere.
   
   ![Şablonu Azure'a dağıtma][github]
3. Portalda, HPC küme şablonu parametreleri girmek için aşağıdaki adımları izleyin.
   
   a. Üzerinde **parametreleri** sayfasında girin veya şablon parametreleri için değerleri değiştirin. (Yardım bilgileri için her ayarın yanındaki simgeye tıklayın.) Örnek değerler, aşağıdaki ekran gösterilir. Bu örnek adlı bir küme oluşturulmuştur *hpc01* içinde *hpc.local* etki alanı bir baş düğüm ve 2 oluşan işlem düğümleri. İşlem düğümleri, Microsoft Excel içeren bir HPC Pack VM görüntüden oluşturulur.
   
   ![Parametreleri girin][parameters-new-portal]
   
   > [!NOTE]
   > VM oluşturuldu otomatik olarak baş düğümü [son Market görüntüsü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) , HPC Pack 2012 R2'de Windows Server 2012 R2. Şu anda görüntü HPC Pack 2012 R2 güncelleştirme 3 temel alır.
   > 
   > İşlem düğümü Vm'leri seçili işlem düğümü ailesinin son görüntüden oluşturulur. Seçin **ComputeNodeWithExcel** işlem seçeneğiyle en son HPC Pack için Microsoft Excel Professional Plus 2013'in değerlendirme sürümünü içeren bir düğüm görüntü. Genel SOA oturumları Excel UDF aktarması için veya bir küme dağıtmak için seçmeyi **ComputeNode** (Excel yüklü olmadan) seçeneği.
   > 
   > 
   
   b. Aboneliği seçin.
   
   c. Küme için bir kaynak grubu gibi oluşturma *hpc01RG*.
   
   d. Orta ABD gibi bir kaynak grubu için bir konum seçin.
   
   e. Üzerinde **yasal koşulları** sayfasında, koşulları gözden geçirin. Kabul ediyorsanız tıklayın **satın alma**. Şablon değerlerini ayarlama işiniz bittiğinde, ardından tıklayın **Oluştur**.
4. Dağıtım tamamlandığında (genellikle yaklaşık 30 dakika sürer), küme baş düğümünden küme sertifika dosyasını dışarı aktarın. Daha sonraki bir adımda Güvenli HTTP bağlaması için sunucu tarafı kimlik doğrulaması sağlamak için istemci bilgisayarda ortak bu sertifikayı içeri aktarın.
   
   a. Azure portalında panoya baş düğümü seçin ve tıklayın Git **Connect** Uzak Masaüstü kullanarak bağlanmak için sayfanın üst kısmındaki.
   
    <!-- ![Connect to the head node][connect] -->
   
   b. Standart yordamları Sertifika Yöneticisi'nde (Cert: \LocalMachine\My altında bulunur) baş düğümüne sertifika özel anahtar olmadan dışarı aktarmak için kullanın. Bu örnekte, dışarı aktarma *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Sertifika dışarı aktarma][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>2. Seçenek HPC Pack Iaas dağıtım betiği kullanın
HPC Pack Iaas dağıtım betiği bir HPC Pack kümesine dağıtmak için çok yönlü başka bir yol sağlar. Azure Resource Manager dağıtım modeli şablonu kullanan ise Klasik dağıtım modelinde, bir küme oluşturur. Ayrıca, betiği bir abonelikte Azure Global veya Azure China hizmeti ile uyumludur.

**Ek Önkoşullar**

* **Azure PowerShell** - [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview) (0.8.10 sürümü veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** : indirin ve komut dosyasından en son sürümünü paketten [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyasının sürümünü denetleyin `New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.5.0 veya daha sonra komut dosyasını temel alır.

**Yapılandırma dosyası oluşturma**

 HPC Pack Iaas dağıtım betiği, HPC küme altyapısı açıklayan giriş olarak bir XML yapılandırma dosyasını kullanır. Bir baş düğüm ve Microsoft Excel içeren işlem düğümü görüntüden oluşturulan 18 işlem düğümleri oluşan bir küme dağıtmak için aşağıdaki örnek yapılandırma dosyası ortamınıza değerlerini değiştirin. Yapılandırma dosyası hakkında daha fazla bilgi için betik klasöründeki Manual.rtf dosyasına bakın ve [HPC Pack Iaas dağıtım betiği ile bir HPC kümesi oluşturma](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Yapılandırma dosyası ile ilgili notlar**

* **VMName** baş düğümün **gerekir** aynı **ServiceName**, veya SOA işleri başarısız çalıştırılacak.
* Belirttiğinizden emin olun **EnableWebPortal** baş düğümüne sertifika oluşturulur ve dışarı aktarılan.
* Dosyayı bir baş düğüm üzerinde çalışan PostConfig.ps1 yapılandırma sonrası PowerShell komut dosyası belirtir. Aşağıdaki örnek betik, Azure depolama bağlantı dizesi yapılandırır, işlem düğümü rolünü baş düğümünden kaldırır ve VM'ler dağıtıldığı sırada tüm düğümlerin çevrimiçi duruma getirir. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Betiği çalıştırın**

1. İstemci bilgisayarda PowerShell konsolunu yönetici olarak açın.
2. (Bu örnekte E:\IaaSClusterScript) betik klasörüne dizin değiştirin.
   
   ```
   cd E:\IaaSClusterScript
   ```
3. HPC Pack kümesine dağıtmak için aşağıdaki komutu çalıştırın. Bu örnek yapılandırma dosyası içinde E:\HPCDemoConfig.xml bulunduğunu varsayar.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

HPC Pack dağıtım betiği için biraz zaman çalışır. Dışarı aktarma ve küme sertifikasını indirin ve geçerli kullanıcının Belgeler klasöründe istemci bilgisayarda kaydetmek için komut bir şey var. Komut dosyası aşağıdakine benzer bir ileti oluşturur. Aşağıdaki bir adımda uygun sertifika deposundaki sertifikayı içeri aktarın.    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>2. Adım Excel çalışma kitaplarını yük boşaltma ve şirket içi bir istemciden UDF çalıştırma
### <a name="excel-activation"></a>Excel etkinleştirme
ComputeNodeWithExcel VM görüntüsünü üretim iş yükleri için kullanılırken, Excel ve işlem düğümleri üzerinde etkinleştirmek için geçerli bir Microsoft Office lisans anahtarı sağlamanız gerekir. Aksi halde, Excel değerlendirme sürümünü 30 gün sonra sona erer ve Excel çalışma kitaplarını çalıştıran (0x800AC472) COMException ile başarısız olur. 

Excel değerlendirme zaman başka bir 30 gün boyunca rearm: baş düğüm ve clusrun oturum `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` tüm Excel üzerinde işlem düğümleriyle HPC Küme Yöneticisi aracılığıyla. En fazla iki kez yeniden etkinleştirin. Bundan sonra geçerli bir Office lisans anahtarı sağlamanız gerekir.

Office Professional Plus 2013 yüklü bir VM görüntüsü üzerinde bir birim bir genel toplu lisans anahtarı (GVLK) ile sürümüdür. Anahtar Yönetim Hizmeti (KMS) aracılığıyla etkinleştirebilir / etkinleştirmeyi (AD BA) ya da çoklu etkinleştirme anahtarı (MAK). 

    * KMS/AD-BA kullanmak için mevcut bir KMS sunucusunu kullanabilir veya yeni bir Microsoft Office 2013 toplu lisans paketi kullanarak ayarlayın. (İsterseniz, baş düğüm sunucuda ayarlayın.) Ardından, Internet veya telefon üzerinden KMS ana bilgisayar anahtarını etkinleştirir. Ardından clusrun `ospp.vbs` KMS sunucusunda hem de bağlantı noktası ayarlayın ve tüm Office etkinleştirmek için Excel işlem düğümleri. 

    * MAK, ilk clusrun kullanılacak `ospp.vbs` Excel işlem düğümleri Internet veya telefon üzerinden anahtarı girin ve sonra tüm etkinleştirin. 

> [!NOTE]
> Office Professional Plus 2013 perakende ürün anahtarları, bu VM görüntüsü ile kullanılamaz. Geçerli anahtarları ve bu Office Professional Plus 2013 toplu edition dışında Office veya Excel sürümü için yükleme medyası varsa, bunun yerine kullanabilirsiniz. Önce bu birimin sürümü kaldırın ve sahip olduğunuz sürümü olarak yükle. Yeniden yüklenen Excel işlem düğümü, uygun ölçekte bir dağıtımında kullanmak için özelleştirilmiş bir VM görüntüsü olarak yakalanabilir.
> 
> 

### <a name="offload-excel-workbooks"></a>Excel çalışma kitaplarını boşaltma
Azure'da HPC Pack kümesinde çalışır, böylece bir Excel çalışma kitabı boşaltmak için bu adımları izleyin. Bunu yapmak için Excel 2010 veya 2013 istemci bilgisayarda zaten yüklü olması gerekir.

1. İşlem düğümü görüntü Excel ile bir HPC Pack kümesine dağıtmak için adım 1'deki seçeneklerden birini kullanın. Küme sertifikası dosyasını (.cer) ve küme kullanıcı adı ve parola edinin.
2. İstemci bilgisayarda Cert: \CurrentUser\Root altında küme sertifikası alın.
3. Excel yüklü olduğundan emin olun. Excel.exe istemci bilgisayarda aynı klasörde aşağıdaki içerikle Excel.exe.config dosyası oluşturun. Bu adım, HPC Pack 2012 R2 Excel COM eklentisi başarıyla yüklendiğini sağlar.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. HPC Pack kümesine göndermek için istemci ayarlayın. Bir seçenektir tam yüklemeye [HPC Pack 2012 R2 güncelleştirme 3'ü yükleme](http://www.microsoft.com/download/details.aspx?id=49922) ve HPC Pack istemciyi yükleyin. Alternatif olarak, indirme ve yükleme [HPC Pack 2012 R2 güncelleştirme 3 istemci programları](https://www.microsoft.com/download/details.aspx?id=49923) ve uygun Visual C++ 2010 için bilgisayarınızı yeniden dağıtılabilir ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).
5. Bu örnekte, ConvertiblePricing_Complete.xlsb adlı örnek Excel çalışma kitabını kullanırız. İndirebilirsiniz [burada](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Excel çalışma kitabını D:\Excel\Run gibi bir çalışma klasörüne kopyalayın.
7. Excel çalışma kitabını açın. Üzerinde **geliştirme** Şerit ye **COM eklentileri** ve HPC Pack Excel COM eklentisi başarıyla yüklendiğini doğrulayın.
   
   ![Excel için HPC Pack eklentisi][addin]
8. Aşağıdaki kodda gösterildiği gibi açıklamalı satırları değiştirerek VBA makrosu HPCControlMacros Excel'de düzenleyin. Ortamınız için uygun değerleri değiştirin.
   
   ![HPC Pack için Excel makrosu][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Excel çalışma kitabını D:\Excel\Upload gibi bir karşıya yükleme dizinine kopyalayın. Bu dizin VBA makrosu HPC_DependsFiles sabitinde belirtilir.
10. Çalışma kitabı azure'da kümede çalıştırmak için tıklayın **küme** çalışma sayfasında düğme.

### <a name="run-excel-udfs"></a>Excel UDF'leri çalıştırın
Excel UDF'leri çalıştırmak için istemci bilgisayar'kurmak için 1-3'ü önceki adımları izleyin. Excel UDF'leri için Excel uygulamanın işlem düğümlerinde yüklü olması gerekmez. Bu nedenle, düğümleri kümenizi oluşturma işlem olduğunda, normal bir işlem düğümünün görüntüsü yerine Excel işlem düğümü görüntüsüyle seçebilirsiniz.

> [!NOTE]
> 2013 Küme Bağlayıcısı iletişim kutusu ve Excel 2010'daki 34 karakter sınırlaması yoktur. UDF çalıştıran kümesi belirtmek için bu iletişim kutusunu kullanın. Tam küme adı uzunsa (örneğin, hpcexcelhn01.southeastasia.cloudapp.azure.com) iletişim kutusunda uygun değildir. Makine genelinde değişkeni ayarlamak için geçici çözüm olan *CCP_IAASHN* uzun küme adı değerine sahip. Ardından, girin *CCP_IAASHN %* küme baş düğümü adı olarak iletişim kutusunda. 
> 
> 

Küme başarıyla dağıtıldıktan sonra yerleşik bir örneği çalıştırmak için aşağıdaki adımlarla devam Excel UDF. Özelleştirilmiş Excel UDF'leri için bkz: [kaynakları](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) XLL'ler oluşturup bunları Iaas kümede dağıtın.

1. Yeni bir Excel çalışma kitabını açın. Üzerinde **geliştirme** Şerit ye **eklentileri**. İletişim kutusunda, ardından **Gözat**%CCP_HOME%Bin\XLL32 klasöre gidin ve örnek ClusterUDF32.xll seçin. İstemci makinesinde ClusterUDF32 yoksa, baş düğüm %CCP_HOME%Bin\XLL32 klasöründen kopyalayın.
   
   ![UDF seçin][udf]
2. Tıklayın **dosya** > **seçenekleri** > **Gelişmiş**. Altında **formülleri**, kontrol **XLL işlevler kullanıcı tarafından tanımlanan bir işlem kümesini çalıştırmak izin**. Ardından **seçenekleri** ve tam küme adı girin **küme baş düğümü adı**. (Bir uzun küme adı olmayan bir çözüm için belirtildiği gibi daha önce bu giriş kutusu 34 karakter ile sınırlıdır. "Bir makineye değişken için bir uzun küme adı kullanabilirsiniz.)
   
   ![UDF yapılandırın][options]
3. UDF hesaplamayı küme üzerinde çalıştırmak için değer =XllGetComputerNameC() içeren hücreyi tıklatın ve sonra Enter tuşuna basın. İşlevi yalnızca UDF üzerinde çalıştığı işlem düğümündeki adını alır. İlk çalıştırma için bir kimlik bilgileri iletişim kutusu, Iaas kümeye bağlanmak için kullanıcı adı ve parola ister.
   
   ![UDF çalıştırma][run]
   
   Hesaplamak için çok sayıda hücre olduğunda, tüm hücreleri üzerinde hesaplama çalıştırmak için Alt-Shift-Ctrl + F9 tuşuna basın.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>3. Adım Şirket içi bir istemciden bir SOA iş yükü çalıştırma
HPC Pack Iaas kümede genel SOA uygulamaları çalıştırmak için ilk yöntemlerden birini 1. adımda kümeyi dağıtmak için kullanın. Genel işlem düğümünün görüntüsü bu durumda, işlem düğümlerinde Excel gerekmediği belirtin. Ardından aşağıdaki adımları izleyin.

1. Küme sertifikası aldıktan sonra istemci bilgisayarının Cert: \CurrentUser\Root altında içe aktarın.
2. Yükleme [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) ve [HPC Pack 2012 R2 güncelleştirme 3 istemci programları](https://www.microsoft.com/download/details.aspx?id=49923). Bu araçlar, SOA istemci uygulamaları geliştirme ve çalıştırma olanak tanır.
3. HelloWorldR2 indirme [örnek kod](https://www.microsoft.com/download/details.aspx?id=41633). Visual Studio 2010 veya 2012 HelloWorldR2.sln açın. (Bu örnekte şu anda, Visual Studio'nun daha yeni sürümleriyle uyumlu değildir.)
4. EchoService projeyi ilk derleyin. Ardından, hizmet Iaas kümeye bir şirket içi kümesine dağıttığınız şekilde dağıtın. Ayrıntılı adımlar için HelloWordR2 içinde Benioku.doc bakın. Değiştirebilir ve bir Azure Iaas kümesinde çalışan SOA istemci uygulamaları oluşturmak için HellWorldR2 ve aşağıdaki bölümde açıklandığı gibi diğer projeler oluşturun.

### <a name="use-http-binding-with-azure-storage-queue"></a>Azure depolama kuyruğu ile HTTP bağlaması kullanın
HTTP bağlaması bir Azure depolama kuyruğu ile kullanmak için örnek koda birkaç değişiklik yapın.

* Küme adını güncelleştirin.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* İsteğe bağlı olarak, varsayılan TransportScheme SessionStartInfo içinde kullanın veya Http açıkça ayarlayın.

```
    info.TransportScheme = TransportScheme.Http;
```

* Varsayılan bağlama BrokerClient için kullanın.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Veya basicHttpBinding kullanarak açıkça ayarlayabilirsiniz.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* İsteğe bağlı olarak, true olarak SessionStartInfo UseAzureQueue bayrağını ayarlayın. Ayarlanmazsa, ayarlanacak, küme adının Azure etki alanı sonekleri sahip olduğunda ve TransportScheme HTTP'dir. varsayılan olarak true.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>HTTP bağlaması Azure depolama kuyruğu olmadan kullanın
HTTP bağlaması olmadan bir Azure depolama kuyruğu kullanmak için açıkça false olarak SessionStartInfo UseAzureQueue bayrağını ayarlayın.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp bağlama kullanın
NetTcp bağlama kullanmak için yapılandırmanın bir şirket içi kümesine bağlamakla aynıdır. Üstbilgi düğüm VM'ine birkaç Uç noktalara açmanız gerekir. Örneğin, kümeyi oluşturmak için HPC Pack Iaas dağıtım betiği kullandıysanız, Azure portalında uç noktaları şu şekilde ayarlayın.

1. Sanal Makineyi durdurun.
2. TCP bağlantı noktaları 9090'dır, 9087, ekleme, 9091 9094 oturumu için aracı, çalışan ve veri hizmetlerini, sırasıyla aracı
   
    ![Uç noktaları yapılandırma][endpoint-new-portal]
3. VM’yi başlatın.

SOA istemci uygulaması Iaas küme tam adı baş adının değiştirilmesi dışında herhangi bir değişiklik gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [bu kaynakları](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) HPC Pack ile Excel iş yükleri çalıştırma hakkında daha fazla bilgi için.
* Bkz: [SOA Hizmetleri'nde yönetme Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) HPC Pack ile SOA hizmetlerini dağıtıp yönetmeye hakkında daha fazla bilgi için.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
