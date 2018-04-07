---
title: HPC Pack küme Excel ve SOA | Microsoft Docs
description: Büyük ölçekli Excel ve SOA iş yüklerini Azure HPC Pack kümede çalışan kullanmaya başlama
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
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Excel ve SOA iş yüklerini Azure HPC Pack kümede çalışan kullanmaya başlama
Bu makalede Azure Hızlı Başlangıç şablonu veya isteğe bağlı olarak bir Azure PowerShell dağıtım komut dosyası kullanarak bir Azure sanal makineler Microsoft HPC Pack 2012 R2 kümesinde dağıtma gösterilmektedir. Küme, Microsoft Excel ya da hizmet odaklı mimari (SOA) iş yükleri HPC paketi ile çalışacak biçimde tasarlanan Azure Market VM görüntüleri kullanır. Küme, bir şirket içi istemci bilgisayarından Excel HPC ve SOA hizmetlerini çalıştırmak için kullanabilirsiniz. Excel çalışma kitabı aktarma ve Excel kullanıcı tanımlı işlevler veya UDF'ler Excel HPC hizmetleri içerir.

> [!IMPORTANT] 
> Bu makalede özellikleri, şablonları ve komut dosyaları HPC Pack 2012 R2 için temel alır. Bu senaryo HPC Pack 2016'şu anda desteklenmiyor.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Yüksek düzeyde, aşağıdaki diyagramda, oluşturduğunuz HPC paketi küme gösterir.

![Excel iş yükleri çalıştıran düğümlerle HPC Kümesi][scenario]

## <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar** -kümeye örnek Excel ve SOA işleri göndermek için bir Windows tabanlı bir istemci bilgisayar gerekir. (Bu dağıtım yöntemi seçerseniz) Azure PowerShell küme dağıtım betiğini çalıştırmak için bir Windows bilgisayar da gerekir.
* **Azure aboneliği** -bir Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika içinde.
* **Çekirdek kota** -özellikle çok çekirdekli VM boyutları birkaç küme düğümleri dağıtırsanız, çekirdek, Kotayı artırmak gerekebilir. Çekirdek kota Kaynağı Yöneticisi'nde bir Azure Hızlı Başlangıç şablonu kullanıyorsanız, Azure bölgesidir. Bu durumda, belirli bir bölgedeki Kotayı artırmak gerekebilir. Bkz: [Azure aboneliği sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md). Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açma](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) herhangi bir ücret alınmaz.
* **Microsoft Office lisansı** - işlem düğümleri, Microsoft Excel ile bir Market HPC Pack 2012 R2 VM görüntüsü kullanarak Microsoft Excel Professional Plus 2013 30 günlük değerlendirme sürümü yüklüyse dağıtırsanız. Değerlendirme sürümünden sonra iş yüklerini çalıştırmaya devam etmek için Excel etkinleştirmek için geçerli bir Microsoft Office lisansı sağlamanız gerekir. Bkz: [Excel etkinleştirme](#excel-activation) bu makalenin ilerisinde yer. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>1. Adım Azure HPC Pack kümede ayarlama
HPC Pack 2012 R2 kümesini ayarlamak için iki seçenek gösteriyoruz: öncelikle, bir Azure Hızlı Başlangıç şablonu ile Azure Portalı '; ve ikincisi, bir Azure PowerShell dağıtım komut dosyası kullanarak.

### <a name="option-1-use-a-quickstart-template"></a>1. Seçenek Hızlı Başlatma şablonunu kullanma
HPC Pack küme Azure portalında hızlı bir şekilde dağıtmak için bir Azure Hızlı Başlangıç şablonu kullanın. Portalda şablonu açtığınızda ayarları, kümeniz için girdiğiniz basit bir kullanıcı Arabirimi alma. Adımlar şunlardır. 

> [!TIP]
> İsterseniz, kullanın bir [Azure Marketi şablonu](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) özellikle Excel iş yükleri için benzer bir küme oluşturur. Adımları aşağıdakiler arasından biraz farklılık gösterir.
> 
> 

1. Ziyaret [HPC küme oluşturma şablonu GitHub sayfasında](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). İsterseniz, şablon ve kaynak kodu hakkında bilgileri gözden geçirin.
2. Tıklatın **Azure'a Dağıt** Azure portalında şablonuyla bir dağıtımı başlatmak için.
   
   ![Şablon Azure'a dağıtma][github]
3. Portalda, HPC küme şablonu parametreleri girmek için şu adımları izleyin.
   
   a. Üzerinde **parametreleri** sayfasında, girin veya şablon parametre değerlerini değiştirin. (Yardım bilgileri için her ayarın yanındaki simgesine tıklayın.) Örnek değerler aşağıdaki ekran görüntüsünde gösterilir. Bu örnek adlı bir küme oluşturur *hpc01* içinde *hpc.local* bir baş düğüm ve 2 oluşan etki alanı işlem düğümlerini. İşlem düğümleri, Microsoft Excel içeren bir HPC Pack VM görüntüden oluşturulur.
   
   ![Parametreleri girin][parameters-new-portal]
   
   > [!NOTE]
   > VM oluşturuldu. otomatik olarak baş düğüm [son Market görüntüsü](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC paketi 2012 R2'in Windows Server 2012 R2. Şu anda görüntü HPC Pack 2012 R2 güncelleştirme 3 ' temel alır.
   > 
   > İşlem düğümü VM'ler seçilen işlem düğümü ailesi son görüntüden oluşturulur. Seçin **ComputeNodeWithExcel** seçeneği en son HPC paketi için Microsoft Excel Professional Plus 2013'in değerlendirme sürümünü içeren bir düğüm görüntü işlem. Genel SOA oturumları için veya Excel UDF aktarması için bir küme dağıtmak için tercih **ComputeNode** (Excel yüklü olmadan) seçeneği.
   > 
   > 
   
   b. Abonelik seçin.
   
   c. Küme için bir kaynak grubu oluşturun *hpc01RG*.
   
   d. Orta ABD gibi kaynak grubu için bir konum seçin.
   
   e. Üzerinde **yasal koşulları** sayfasında, koşulları gözden geçirin. Kabul ediyorsa **satın alma**. Şablon için değerleri ayarlama işiniz bittiğinde, ardından **oluşturma**.
4. Dağıtım tamamlandığında (genellikle yaklaşık 30 dakika sürer), küme baş düğümünden export küme sertifika dosyası. Bir sonraki adımda Güvenli HTTP bağlama için sunucu tarafı kimlik doğrulaması sağlamak için istemci bilgisayarda ortak bu sertifikayı içeri aktarın.
   
   a. Azure portalında panoya baş düğüm seçin ve tıklayın Git **Bağlan** sayfanın üst kısmındaki Uzak Masaüstü kullanarak bağlanmak için.
   
    <!-- ![Connect to the head node][connect] -->
   
   b. Standart yordamları Sertifika Yöneticisi'nde (Cert: \LocalMachine\My altında bulunur) baş düğümüne sertifika özel anahtarı dışarı aktarmak için kullanın. Bu örnekte, verme *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Sertifika verme][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>2. Seçenek HPC Pack Iaas dağıtım komut dosyası kullan
HPC Pack Iaas dağıtım betiği bir HPC Pack kümeyi dağıtma için çok yönlü başka bir yol sağlar. Şablon Azure Resource Manager dağıtım modelini kullanır ancak klasik dağıtım modelinde bir küme oluşturur. Ayrıca, komut dosyası Azure genel veya Azure Çin hizmetindeki bir abonelik ile uyumludur.

**Ek önkoşulları**

* **Azure PowerShell** - [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview) (sürüm 0.8.10 veya üzeri) istemci bilgisayarınızda.
* **HPC Pack Iaas dağıtım betiği** - karşıdan yükleme ve komut dosyasını en son sürümünü paket [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Komut dosyası sürümünü çalıştırarak denetleme `New-HPCIaaSCluster.ps1 –Version`. Bu makalede, sürüm 4.5.0 veya daha sonra komut dosyasını temel alır.

**Yapılandırma dosyası oluşturma**

 HPC Pack Iaas dağıtım betiği HPC küme altyapısı açıklayan giriş olarak bir XML yapılandırma dosyasını kullanır. Bir baş düğüm ve Microsoft Excel içeren işlem düğümü görüntüden oluşturulan 18 işlem düğümleri oluşan bir küme dağıtmak için aşağıdaki örnek yapılandırma dosyası ortamınıza değerlerini değiştirin. Yapılandırma dosyası hakkında daha fazla bilgi için komut dosyası klasöründeki Manual.rtf dosyasına bakın ve [HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

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

* **VMName** baş düğümü **gerekir** aynı **ServiceName**, veya SOA işleri başarısız çalıştırmak.
* Belirttiğinizden emin olun **EnableWebPortal** baş düğümüne sertifika oluşturulan dışarı aktarılır ve böylece.
* Dosyayı bir yapılandırma sonrası PowerShell komut dosyası baş düğüm üzerinde çalışan PostConfig.ps1 belirtir. Aşağıdaki örnek komut dosyası Azure depolama bağlantı dizesi yapılandırır, bilgi işlem düğümü rolü baş düğümünden kaldırır ve bunlar dağıtıldığında tüm düğümlerin çevrimiçi duruma getirir. 

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

**Komut dosyasını çalıştır**

1. İstemci bilgisayarda PowerShell konsolunu yönetici olarak açın.
2. (Bu örnekte E:\IaaSClusterScript) komut dosyası klasöre dizini değiştirin.
   
   ```
   cd E:\IaaSClusterScript
   ```
3. HPC Pack küme dağıtmak için aşağıdaki komutu çalıştırın. Bu örnekte, yapılandırma dosyasını E:\HPCDemoConfig.xml içinde bulunduğu varsayılır.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

HPC Pack dağıtım betiği süre için çalışır. Komut dosyası gerçekleştirir bir dışarı aktarma ve küme sertifikayı indirin ve geçerli kullanıcının Belgeler klasöründe istemci bilgisayarda kaydetmek için şeydir. Komut dosyası aşağıdakine benzer bir ileti oluşturur. Aşağıdaki adımda, uygun sertifika deposundaki sertifikayı içeri aktarın.    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>2. Adım Excel çalışma kitaplarını boşaltması ve bir şirket içi istemciden UDF'ler çalıştırın
### <a name="excel-activation"></a>Excel etkinleştirme
ComputeNodeWithExcel VM görüntüsü üretim iş yükleri için kullanırken, işlem düğümlerinde Excel etkinleştirmek için geçerli bir Microsoft Office lisans anahtarı sağlamanız gerekir. Aksi takdirde Excel Değerlendirme sürümü 30 gün sonra süresi dolar ve Excel çalışma kitaplarını çalıştıran (0x800AC472) COMException ile başarısız olur. 

Excel başka bir sonraki 30 gün için değerlendirme süresi rearm: oturum açın baş düğüm ve clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` tüm Excel işlem düğümleri aracılığıyla HPC Küme Yöneticisi. En fazla iki kez yeniden etkinleştirin. Bundan sonra geçerli bir Office lisans anahtarı sağlamanız gerekir.

Office Professional Plus 2013 VM görüntüsü üzerinde yüklü bir birim bir genel toplu lisans anahtarı (GVLK) ile sürümüdür. Anahtar Yönetim Hizmeti (KMS) aracılığıyla etkinleştirebilir / etkinleştirmeyi (AD BA) veya çoklu etkinleştirme anahtarı (MAK). 

    * AD/KMS-BA kullanmak için var olan bir KMS sunucusu kullanın veya yeni bir Microsoft Office 2013 toplu lisans paketi kullanarak ayarlayın. (İstiyorsanız, sunucuyu baş düğüm ayarlayın.) Ardından Internet veya telefon üzerinden KMS ana bilgisayar anahtarı etkinleştirin. Ardından clusrun `ospp.vbs` Excel işlem düğümlerini KMS sunucusunu ve bağlantı noktası ayarlamak ve tüm Office etkinleştirmek için. 

    * MAK, ilk clusrun kullanmak için `ospp.vbs` Excel işlem düğümlerini Internet veya telefon üzerinden anahtarı girin ve ardından tüm etkinleştirin. 

> [!NOTE]
> Office Professional Plus 2013 için perakende ürün anahtarlarını bu VM görüntüsü ile kullanılamaz. Geçerli anahtarlar ve Office veya Excel sürümü bu Office Professional Plus 2013 toplu edition dışında bir yükleme medyası varsa, bunun yerine kullanabilirsiniz. Önce bu birimin sürümü kaldırın ve sahip sürümü yükleyin. Yeniden yüklenmesi Excel işlem düğümü ölçekte bir dağıtımında kullanmak için özelleştirilmiş bir VM görüntü olarak yakalanır.
> 
> 

### <a name="offload-excel-workbooks"></a>Excel çalışma kitaplarını boşaltma
Böylece Azure HPC Pack kümesinde çalışan bir Excel çalışma kitabı boşaltmak için şu adımları izleyin. Bunu yapmak için Excel 2010 veya 2013 istemci bilgisayarda yüklü olması gerekir.

1. İşlem düğümü görüntü Excel ile bir HPC paketi küme dağıtmak için adım 1'deki seçeneklerden birini kullanın. Küme sertifika dosyasını (.cer) ve küme kullanıcı adı ve parola edinin.
2. İstemci bilgisayarda Cert: \CurrentUser\Root altında küme sertifikasını içeri aktarın.
3. Excel yüklü olduğundan emin olun. Aynı klasörde Excel.exe istemci bilgisayarda aşağıdaki içeriğe sahip bir Excel.exe.config dosyası oluşturun. Bu adım, HPC Pack 2012 R2 Excel COM eklentisi başarıyla yüklendiğini sağlar.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. HPC Pack kümeye iş göndermek için istemcisi ayarlama. Bir seçenektir tam yüklemeye [HPC Pack 2012 R2 güncelleştirme 3'ü yükleme](http://www.microsoft.com/download/details.aspx?id=49922) ve HPC Pack istemcisini yükleyin. Alternatif olarak, indirin ve yükleyin [HPC Pack 2012 R2 güncelleştirme 3 istemci yardımcı programları](https://www.microsoft.com/download/details.aspx?id=49923) ve uygun Visual C++ 2010 bilgisayarınızı yeniden dağıtılabilir ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).
5. Bu örnekte, ConvertiblePricing_Complete.xlsb adlandırılmış bir örnek Excel çalışma kitabı kullanırız. Karşıdan yükleyebileceğiniz [burada](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Excel çalışma kitabı D:\Excel\Run gibi çalışma klasörüne kopyalayın.
7. Excel çalışma kitabı açın. Üzerinde **geliştirme** Şerit, tıklatın **COM eklentileri** ve HPC Pack Excel COM eklentisi başarıyla yüklendiğini doğrulayın.
   
   ![HPC Pack eklentisi excel][addin]
8. Aşağıdaki kodda gösterildiği gibi açıklamalı satırları değiştirerek VBA makrosu Excel'de HPCControlMacros düzenleyin. Ortamınız için uygun değerleri değiştirin.
   
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
9. Excel çalışma kitabı D:\Excel\Upload gibi bir karşıya yükleme dizinine kopyalayın. Bu dizin VBA makrosu HPC_DependsFiles sabitinde belirtildi.
10. Çalışma kitabı Azure kümesinde çalıştırmak için tıklatın **küme** çalışma sayfası üzerinde düğmesi.

### <a name="run-excel-udfs"></a>Excel UDF'leri çalıştırın
Excel UDF'leri çalıştırmak için istemci bilgisayarı ayarlamak için 1-3'ü yukarıdaki adımları izleyin. Excel UDF'leri için Excel uygulamanın işlem düğümlerinde yüklü olması gerekmez. Bu nedenle, düğümleri, küme oluşturma işlem zaman normal işlem düğümü görüntü Excel Hesaplama düğümü görüntüsüyle yerine seçebilir.

> [!NOTE]
> 34 karakter sınırı Excel 2010 ve 2013 Küme Bağlayıcısı iletişim kutusu yok. UDF'ler çalıştıran küme belirtmek için bu iletişim kutusunu kullanın. Tam küme adı uzunsa (örneğin, hpcexcelhn01.southeastasia.cloudapp.azure.com) iletişim kutusunda uygun değildir. Makine genelinde değişkeni ayarlamak için geçici bir çözüm değildir *CCP_IAASHN* uzun küme adı değerine sahip. Ardından, girin *CCP_IAASHN %* küme baş düğüm adı olarak iletişim kutusunda. 
> 
> 

Küme başarıyla dağıtıldıktan sonra yerleşik bir örneği çalıştırmak için aşağıdaki adımlarla devam Excel UDF. Bu özelleştirilmiş Excel UDF'leri bkz [kaynakları](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) XLL'ler oluşturmak ve bunları Iaas kümede dağıtmak için.

1. Yeni bir Excel çalışma kitabı açın. Üzerinde **geliştirme** Şerit, tıklatın **eklentileri**. İletişim kutusunda, ardından **Gözat**%CCP_HOME%Bin\XLL32 klasörüne gidin ve örnek ClusterUDF32.xll seçin. İstemci makinesinde ClusterUDF32 yoksa, baş düğüm %CCP_HOME%Bin\XLL32 klasöründen kopyalayın.
   
   ![UDF seçin][udf]
2. Tıklatın **dosya** > **seçenekleri** > **Gelişmiş**. Altında **formüller**, denetleme **XLL işlevlerini kullanıcı tanımlı bir işlem kümesi çalışmasına izin**. Ardından **seçenekleri** ve tam küme adını girin **küme baş düğüm adı**. (Bir uzun küme adını değil sığar belirtildiği gibi daha önce bu giriş kutusu 34 karakterle sınırlıdır. "Bir makineye değişken uzun küme adı için kullanabilirsiniz.)
   
   ![UDF yapılandırın][options]
3. UDF hesaplama küme üzerinde çalıştırmak için değer =XllGetComputerNameC() içeren hücreyi tıklatın ve Enter tuşuna basın. İşlevi yalnızca UDF çalıştığı işlem düğümündeki adını alır. İlk çalıştırmak için bir kimlik bilgileri iletişim kutusu, Iaas kümeye bağlanmak için kullanıcı adı ve parola ister.
   
   ![UDF çalıştırın][run]
   
   Hesaplamak için çok sayıda hücre olduğunda Alt Shift Ctrl + F9 tüm hücre hesaplaması çalıştırmak için tuşuna basın.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>3. Adım Bir şirket içi istemciden bir SOA iş yükü çalıştırın
HPC Pack Iaas kümede genel SOA uygulamalarını çalıştırmak için önce yöntemlerden birini adım 1'de küme dağıtmak için kullanın. İşlem düğümlerinde Excel gerekmediği için genel işlem düğümü görüntüsü bu durumda, belirtin. Ardından aşağıdaki adımları izleyin.

1. Küme sertifikası aldıktan sonra onu Cert: \CurrentUser\Root altında istemci bilgisayarda içeri aktarın.
2. Yükleme [HPC Pack 2012 R2 güncelleştirme 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) ve [HPC Pack 2012 R2 güncelleştirme 3 istemci yardımcı programları](https://www.microsoft.com/download/details.aspx?id=49923). Bu araçlar geliştirme ve SOA istemci uygulamaları çalıştırmak etkinleştirin.
3. HelloWorldR2 karşıdan [örnek koduna](https://www.microsoft.com/download/details.aspx?id=41633). Visual Studio 2010 veya 2012 HelloWorldR2.sln açın. (Bu örnek, Visual Studio'nun daha yeni sürümleri ile şu anda uyumlu değildir.)
4. EchoService projeyi önce oluşturun. Ardından, hizmet için bir şirket içi kümeyi dağıtma aynı şekilde Iaas kümeye dağıtın. Ayrıntılı adımlar için HelloWordR2 içinde Benioku.doc bakın. Değiştirin ve yapı HellWorldR2 ve diğer projeler aşağıdaki bölümde açıklandığı gibi Azure Iaas kümede çalışan SOA istemci uygulamaları oluşturmak için.

### <a name="use-http-binding-with-azure-storage-queue"></a>HTTP bağlaması ile Azure depolama kuyruğu kullanın
HTTP bağlaması bir Azure depolama kuyruğu ile kullanmak için örnek kod birkaç değişiklikleri yapın.

* Küme adını güncelleştirin.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* İsteğe bağlı olarak, TransportScheme varsayılan SessionStartInfo kullanın veya açık olarak Http için ayarlayın.

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
  
    Veya açıkça basicHttpBinding kullanarak ayarlayın.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* İsteğe bağlı olarak UseAzureQueue bayrağını SessionStartInfo true olarak ayarlayın. Ayarlanmazsa, ayarlanacak varsa Azure etki alanı sonekleri küme adına sahip ve TransportScheme Http olduğunda varsayılan olarak true.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Azure depolama kuyruğu olmadan HTTP bağlaması kullanın
Bir Azure depolama kuyruğu olmadan HTTP bağlaması kullanmak için açıkça UseAzureQueue bayrağı false olarak SessionStartInfo ayarlayın.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp bağlama kullanın
NetTcp bağlama kullanmak için yapılandırmayı bir şirket içi kümeye bağlanma benzer. Baş düğüm VM birkaç uç noktaları açmanız gerekir. Örneğin, küme oluşturmak için HPC Pack Iaas dağıtım betiği kullandıysanız, Azure portalında uç noktaları aşağıdaki gibi ayarlayın.

1. VM'yi durdurun.
2. TCP bağlantı noktaları 9090, 9087, ekleme 9091, oturum için 9094 Aracısı, alt ve Veri Hizmetleri, sırasıyla Aracısı
   
    ![Uç noktaları yapılandırma][endpoint-new-portal]
3. VM’yi başlatın.

SOA istemci uygulaması Iaas küme tam adı baş adına değiştirme dışında herhangi bir değişiklik gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [bu kaynakları](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) Excel iş yükleri HPC paketi ile çalıştırma hakkında daha fazla bilgi için.
* Bkz: [SOA Hizmetleri'nde yönetme Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) dağıtma ve SOA Hizmetleri HPC paketi ile yönetme hakkında daha fazla bilgi için.

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
