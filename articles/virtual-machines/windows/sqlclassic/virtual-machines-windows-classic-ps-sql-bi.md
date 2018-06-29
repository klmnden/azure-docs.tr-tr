---
title: SQL Server Business Intelligence | Microsoft Docs
description: Bu konu, Klasik dağıtım modeli kullanılarak oluşturulmuş kaynaklarını kullanır ve Azure sanal makineleri (VM'ler) üzerinde çalışan SQL Server için kullanılabilir iş zekası (BI) özellikleri açıklar.
services: virtual-machines-windows
documentationcenter: na
author: markingmyname
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: c681e7a7-eeda-48aa-bc35-6277f4828244
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/30/2017
ms.author: maghan
ms.openlocfilehash: a41dcd5f2c93e5c1279e1c7511e10e6d72574b3b
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098755"
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Azure Sanal Makinelerde SQL Server İş Zekası
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Microsoft Azure sanal makine Galerisi SQL Server yüklemelerini içeren resimler içerir. Galeri görüntüleri desteklenen SQL Server sürümlerinde, şirket içi bilgisayarlar ve sanal makineler için yükleyebilmek için aynı yükleme dosyalarıdır. Bu konuda, bir sanal makine sağlandıktan sonra gerekli yapılandırma adımları ve görüntülerinde yüklü SQL Server Business Intelligence (BI) özellikleri özetlenmektedir. Bu konu ayrıca BI özellikleri ve en iyi uygulamalar için desteklenen dağıtım topolojileri açıklanır.

## <a name="license-considerations"></a>Lisans konuları
Microsoft Azure Virtual Machines'de SQL Server Lisans için iki yol vardır:

1. Yazılım Güvencesi parçası olan lisans mobility avantajları. Daha fazla bilgi için bkz: [azure'da Yazılım Güvencesi ile lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/).
2. Saat hızı yüklü SQL Server ile Azure sanal makineleri başına ücret ödersiniz. "SQL Server" bölümüne bakın [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Lisans ve geçerli oranları hakkında daha fazla bilgi için bkz: [sanal makineleri Lisansı hakkında SSS](https://azure.microsoft.com/pricing/licensing-faq/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server görüntülerinde kullanılabilir Azure sanal makineye Galerisi
Microsoft Azure sanal makineye Galerisi, Microsoft SQL Server içeren birkaç görüntüyü içerir. Sanal makine görüntülerini üzerinde yüklü olan yazılım işletim sistemi sürümü ve SQL Server sürümüne göre değişir. Azure sanal makinesi galerideki görüntülerin listesini sık değiştirir.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![Azure VM galerisinde SQL görüntüsü](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Aşağıdaki PowerShell betiğini "SQL-Server" görüntü adı içeren Azure görüntüleri listesini döndürür:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "Subscriptions" menu in Azure portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Sürümleri ve SQL Server tarafından desteklenen özellikler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [SQL Server sürümleri](https://www.microsoft.com/sql-server/sql-server-2017-editions)
* [SQL Server 2016 sürümleri tarafından desteklenen özellikler](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>SQL Server sanal makineye Galerisi görüntülerinde yüklü BI özellikleri
Aşağıdaki tabloda, SQL Server için genel Microsoft Azure sanal makine Galerisi görüntülerinde yüklü iş zekası özellikleri özetlenmektedir:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 Standard

| SQL Server BI özelliği | Galeri görüntüde yüklü | Notlar |
| --- | --- | --- |
| **Raporlama Hizmetleri yerel modu** |Evet |Yüklü, ancak Rapor Yöneticisi URL'si dahil olmak üzere yapılandırma gerektirir. Bölümüne bakın [Raporlama Hizmetleri Yapılandırma](#configure-reporting-services). |
| **SharePoint modu Raporlama Hizmetleri** |Hayır |Microsoft Azure sanal makinesi galeri görüntüsü, SharePoint veya SharePoint içermeyen yükleme dosyaları. <sup>1</sup> |
| **Analysis Services çok boyutlu ve veri madenciliği (OLAP)** |Evet |Yüklenir ve varsayılan Analysis Services örneği olarak yapılandırılır. |
| **Analysis Services tablo** |Hayır |SQL Server 2012'de desteklenen, 2014 ve 2016 görüntüler, ancak değil varsayılan olarak yüklenir. Analysis Services'ın başka bir örneği yükleyin. Bölümüne bakın, bu konudaki diğer SQL Server Hizmetleri ve özellikleri yükleyin. |
| **Analysis Services Power Pivot SharePoint için** |Hayır |Microsoft Azure sanal makinesi galeri görüntüsü, SharePoint veya SharePoint içermeyen yükleme dosyaları. <sup>1</sup> |

<sup>1</sup> SharePoint ve Azure sanal makineler hakkında ek bilgi için bkz: [SharePoint 2013 için Microsoft Azure mimariler](https://technet.microsoft.com/library/dn635309.aspx) ve [SharePoint dağıtım Microsoft Azure Virtual Machines'de](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Bu hizmetin adı "SQL" içeren yüklü hizmetlerin bir listesini almak için aşağıdaki PowerShell komutunu çalıştırın.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Genel öneriler ve en iyi yöntemler
* Bir sanal makine için önerilen boyutu en az **A3** SQL Server Enterprise Edition kullanırken. **A4** sanal makine boyutu, Analysis Services ve Reporting Services SQL Server BI dağıtımları için önerilir.
  
    Geçerli VM boyutları hakkında daha fazla bilgi için bkz: [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Veri, günlük ve sürücülerindeki yedekleme dosyaları dışında depolamak için disk yönetimi en iyi yöntem olduğu **C**: ve **D**:. Örneğin, veri diskleri oluşturma **E**: ve **F**:.
  
  * İlke varsayılan sürücüsü için önbelleğe alma sürücü **C**: verilerle çalışmak için uygun değil.
  * **D**: öncelikle sayfa dosyası için kullanılan geçici bir sürücü sürücüdür. **D**: sürücü kalıcı ve blob depolama alanına kaydedilmez. Sanal makine için bir değişiklik gibi yönetim görevleri boyut sıfırlama **D**: sürücü. İçin önerilen **değil** kullanmak **D**: tempdb dahil olmak üzere, veritabanı dosyalarının sürücü.
    
    Oluşturma ve diskleri ekleme hakkında daha fazla bilgi için bkz: [nasıl bir sanal makineye bir veri diski Ekle](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Durdurun veya kullanmayı planlıyor musunuz services'ı kaldırın. Örnek Raporlama Hizmetleri için sanal makine yalnızca kullandıysanız, durdurmak veya Analysis Services ve SQL Server Integration Services kaldırmak için. Aşağıdaki resimde, varsayılan olarak başlatılan hizmetler örneğidir.
  
    ![SQL Server Hizmetleri](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > SQL Server veritabanı motorunun desteklenen BI senaryolarda gereklidir. Tek bir sunucu VM topoloji, veritabanı altyapısı, aynı VM üzerinde çalışıyor olması gereklidir.
  
    Daha fazla bilgi için aşağıdakilere bakın: [Raporlama hizmetlerini kaldırmak](https://msdn.microsoft.com/library/hh479745.aspx) ve [bir örneği, Analysis Services kaldırma](https://msdn.microsoft.com/library/ms143687.aspx).
* Denetleme **Windows Update** için yeni 'önemli güncelleştirmeleri'. Microsoft Azure sanal makine görüntülerini sık yenilenmesini; Bununla birlikte önemli güncelleştirmeler kullanılabilir hale gelebilir **Windows Update** VM görüntüsü en son yenilendikten sonra.

## <a name="example-deployment-topologies"></a>Örnek dağıtım topolojileri
Microsoft Azure sanal makineler kullanan örnek dağıtımlar aşağıda verilmiştir. Bu diyagramları topolojide yalnızca SQL Server BI özellikleri ve Microsoft Azure sanal makineler ile kullanabileceğiniz olası topolojileri bazılarıdır.

### <a name="single-virtual-machine"></a>Tek bir sanal makine
Analysis Services, Reporting Services, SQL Server veritabanı altyapısı ve veri kaynaklarını tek bir sanal makinede.

![1 sanal makine ile bi iass senaryosu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>İki sanal makineler
* Analysis Services, Reporting Services ve tek bir sanal makinede SQL Server veritabanı altyapısı. Bu dağıtım rapor sunucusu veritabanı içerir.
* İkinci bir VM'de veri kaynakları. İkinci VM, SQL Server veritabanı altyapısı bir veri kaynağı olarak içerir.

![2 sanal makine ile bi iass senaryosu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Karma Azure – veri Azure SQL veritabanı
* Analysis Services, Reporting Services ve tek bir sanal makinede SQL Server veritabanı altyapısı. Bu dağıtım rapor sunucusu veritabanı içerir.
* Azure SQL veritabanına veri kaynağıdır.

![Bi iaas senaryoları vm ve veri kaynağı olarak AzureSQL](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Karma – veri şirket içi
* Bu örnek dağıtımda Analysis Services, Reporting Services ve SQL Server veritabanı altyapısı tek bir sanal makinede çalıştırın. Sanal makine report server veritabanlarını barındırır. Sanal makinenin Azure sanal ağ üzerinden bir şirket içi etki alanına ya da çözüm tünel başka bir VPN için birleştirilir.
* Şirket içi veri kaynağıdır.

![Bi ıaas senaryoları vm ve şirket içi veri kaynakları](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Raporlama Hizmetleri yerel modu yapılandırma
Rapor sunucusu yapılandırılmamış ancak SQL Server için sanal makine galeri görüntüsü yüklü, Reporting Services yerel mod içerir. Bu bölümdeki adımları Raporlama Hizmetleri rapor sunucusu yapılandırın. Raporlama Hizmetleri yerel modu yapılandırma hakkında daha ayrıntılı bilgi için bkz: [yükleme Raporlama Hizmetleri yerel modu rapor sunucusu (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Rapor sunucusu yapılandırmak için Windows PowerShell komut dosyalarını kullanan benzer içeriğine bakın [kullanımı bir Azure VM ile bir yerel moddaki bir rapor sunucusunda oluşturmak için PowerShell](../classic/ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Sanal makineye bağlanın ve Raporlama Hizmetleri Yapılandırma Yöneticisi'ni başlatın
Bir Azure sanal makineye bağlanmak için iki ortak iş akışı vardır:

* İçinde öğesine tıklayın sanal makinenin adını bağlanın ve ardından **Bağlan**. Uzak Masaüstü Bağlantısı açar ve bilgisayar adını otomatik olarak doldurulur.
  
    ![Azure sanal makineye bağlanma](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Windows Uzak Masaüstü Bağlantısı ile sanal makineye bağlanabilirsiniz. Uzak Masaüstü kullanıcı arabiriminde:
  
  1. Tür **bulut hizmeti adı** bilgisayar adı olarak.
  2. İki nokta üst üste (:) ve TCP Uzak Masaüstü uç noktası için yapılandırılmış ortak bağlantı noktası numarasını yazın.
     
      Myservice.cloudapp.net:63133
     
      Daha fazla bilgi için bkz: [bir bulut hizmeti nedir?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Reporting Services Configuration Manager'ı Başlat**

İçinde **Windows Server 2012/2016**:

1. Gelen **Başlat** ekranında, yazın **Reporting Services** uygulamaların bir listesini görmek için.
2. Sağ **Reporting Services Configuration Manager** tıklatıp **yönetici olarak çalıştır**.

İçinde **Windows Server 2008 R2**:

1. Tıklatın **Başlat**ve ardından **tüm programlar**.
2. Tıklatın **Microsoft SQL Server 2016**.
3. Tıklatın **yapılandırma araçları**.
4. Sağ **Reporting Services Configuration Manager** tıklatıp **yönetici olarak çalıştır**.

Ya da:

1. Tıklatın **Başlat**.
2. İçinde **programları ve dosyaları ara** iletişim türü **Raporlama Hizmetleri**. VM, Windows Server 2012 çalıştırıyorsa, yazın **Raporlama Hizmetleri** Windows Server 2012 Başlat ekranında.
3. Sağ **Reporting Services Configuration Manager** tıklatıp **yönetici olarak çalıştır**.
   
    ![SSRS Yapılandırma Yöneticisi için arama](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Raporlama hizmetlerini yapılandırma
**Hizmet hesabı ve web hizmeti URL'si:**

1. Doğrulama **sunucu adı** tıklayın ve yerel sunucu adı olan **Bağlan**.
2. Boş Not **rapor sunucusu veritabanı adı**. Yapılandırma tamamlandığında veritabanı oluşturulur.
3. Doğrulama **rapor sunucusu durumu** olan **başlatıldı**. Windows Sunucu Yöneticisi'nde doğrulamak istiyorsanız, hizmetidir **SQL Server Reporting Services** Windows hizmeti.
4. Tıklatın **hizmet hesabı** ve hesap gerektiği gibi değiştirin. Sanal makine bir etki alanı olmayan birleştirilmiş ortamında, yerleşik kullandıysanız **ReportServer** hesabıdır yeterli. Hizmet hesabı hakkında daha fazla bilgi için bkz: [hizmet hesabı](https://msdn.microsoft.com/library/ms189964.aspx).
5. Tıklatın **Web hizmeti URL'si** sol bölmede.
6. Tıklatın **Uygula** varsayılan değerlerle yapılandırılır.
7. Not **rapor sunucusu Web hizmeti URL'leri**. Varsayılan TCP bağlantı noktası 80'dir ve URL'SİNİN bir parçası olduğunu unutmayın. Bir sonraki adımda Microsoft Azure sanal makine uç noktası için bağlantı noktası oluşturun.
8. İçinde **sonuçları** bölmesinde Eylemler başarıyla tamamlandı doğrulayın.

**Veritabanı:**

1. Tıklatın **veritabanı** sol bölmede.
2. Tıklatın **değiştirmek veritabanı**.
3. Doğrulama **yeni bir rapor sunucusu veritabanı oluşturun** seçilir ve İleri'yi tıklatın.
4. Doğrulama **sunucu adı** tıklatıp **Bağlantıyı Sına**.
5. Sonuç ise **başarılı Bağlantıyı Sına**, tıklatın **Tamam** ve ardından **sonraki**.
6. Veritabanı adı Not **ReportServer** ve **rapor sunucusu modu** olan **yerel** ardından **sonraki**.
7. Tıklatın **sonraki** üzerinde **kimlik bilgileri** sayfası.
8. Tıklatın **sonraki** üzerinde **Özet** sayfası.
9. Tıklatın **sonraki** üzerinde **ilerleme ve son** sayfası.

**2012 ve 2014 için web portalı URL'si veya Rapor Yöneticisi URL'si:**

1. Tıklatın **Web portalı URL'si**, veya **Rapor Yöneticisi URL'si** 2014 ve sol bölmede 2012.
2. **Uygula**'ya tıklayın.
3. İçinde **sonuçları** bölmesinde Eylemler başarıyla tamamlandı doğrulayın.
4. Tıklatın **çıkış**.

Rapor sunucusu izinleri hakkında daha fazla bilgi için bkz: [Yerel moddaki bir rapor sunucusunda izinleri verme](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Yerel Rapor Yöneticisi Gözat
Yapılandırmayı doğrulamak için Rapor Yöneticisi VM'de göz atın.

1. VM üzerinde yönetici ayrıcalıklarına sahip Internet Explorer'ı başlatın.
2. Gözat http://localhost/reports VM üzerinde.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Uzak web portalı veya Rapor Yöneticisi 2014 ve 2012 için Bağlan
2014 ve uzak bir bilgisayardan sanal makinedeki 2012 için web portalı veya Rapor Yöneticisi bağlanmak istiyorsanız, yeni bir sanal makine TCP uç noktası oluşturun. Rapor sunucusu varsayılan olarak, HTTP isteklerini dinler **bağlantı noktası 80**. Rapor sunucusu URL'leri farklı bir bağlantı noktası kullanacak şekilde yapılandırırsanız aşağıdaki yönergelerde yer bağlantı noktası numarasını belirtmeniz gerekir.

1. Bir uç nokta TCP bağlantı noktası 80 için sanal makine oluşturun. Daha fazla bilgi için [sanal makine uç noktaları ve güvenlik duvarı bağlantı noktalarını](#virtual-machine-endpoints-and-firewall-ports) bu belgede bölüm.
2. Bağlantı noktası 80 sanal makinenin Güvenlik Duvarı'nı açın.
3. Web Portalı'na göz atın veya Rapor Yöneticisi'ni, Azure sanal makine kullanarak **DNS adı** URL sunucu adı olarak. Örneğin:
   
    **Rapor sunucusu**: http://uebi.cloudapp.net/reportserver **Web portalı**:   http://uebi.cloudapp.net/reports
   
    [Güvenlik duvarını rapor sunucusu erişimi için yapılandırma](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Oluşturma ve Azure sanal makinesi raporları yayımlamak için
Aşağıdaki tabloda Microsoft Azure sanal makine üzerinde barındırılan bir rapor sunucusu için bir şirket içi bilgisayardan varolan raporları yayınlamak için kullanılabilir seçenekleri özetlenmektedir:

* **Rapor Oluşturucu**: sanal makine tıklatın içerir-SQL 2014 ve 2012 için Microsoft SQL Server Rapor Oluşturucusu sürümünü bir kez. Sanal makinede SQL 2016 ile Rapor Oluşturucusu'nu ilk zamanı başlatmak için:
  
  1. Tarayıcınız yönetici ayrıcalıklarıyla başlatın.
  2. Web portalında sanal makinenin bulun ve seçin **karşıdan** sağ üst simgesi.
  3. Seçin **Rapor Oluşturucu**.
     
     Daha fazla bilgi için bkz: [rapor oluşturucusu başlatma](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server veri Araçları**: VM: SQL Server veri araçları sanal makinede yüklü olduğundan ve oluşturmak için kullanılan **rapor sunucusu projeleri** ve sanal makinede raporlar. SQL Server veri araçları raporları, rapor sunucusu üzerinde sanal makine için yayımlayabilirsiniz.
* **SQL Server veri araçları: Uzaktan**: yerel bilgisayarınızda SQL Server veri araçları Raporlama Hizmetleri raporlarını içeren bir Reporting Services projesi oluşturun. Proje için web hizmeti URL'si bağlanmak için yapılandırın.
  
    ![SSRS projesi için SSDT proje özellikleri](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Oluşturma bir. Raporları içeren ve karşıya yükleyin ve sürücü ekleme VHD sabit sürücü.
  
  1. Oluşturma bir. VHD raporlarınızı içeren yerel bilgisayarınızdaki sabit sürücü.
  2. Oluşturun ve bir yönetim sertifikası yükleyin.
  3. Add-AzureVHD cmdlet'ini kullanarak Azure VHD dosyasını karşıya [Azure için Windows Server VHD oluşturun ve yükleyin](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. Disk sanal makineye ekleyin.

## <a name="install-other-sql-server-services-and-features"></a>Diğer SQL Server hizmetlerini ve özellikleri yükleme
Analysis Services tabular modunda gibi ek SQL Server hizmetlerini yüklemek için SQL server Kurulum Sihirbazı'nı çalıştırın. Kurulum, sanal makinenin yerel diskte dosyalarıdır.

1. Tıklatın **Başlat** ve ardından **tüm programlar**.
2. Tıklatın **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** veya **Microsoft SQL Server 2012** ve ardından **yapılandırma araçları** .
3. Tıklatın **SQL Server Yükleme Merkezi'ni**.

Veya C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe veya C:\SQLServer_11.0_full\setup.exe çalıştırın

> [!NOTE]
> SQL Server Kurulumu çalıştırdığınız ilk kez daha fazla Kurulum dosyaları indirilebilir ve sanal makinenin yeniden başlatılması ve SQL Server Kurulumu'nu yeniden başlatılmasını gerektirir.
> 
> Sürekli olarak Microsoft Azure sanal makineden seçili görüntüsünü özelleştirmek gerekiyorsa, kendi SQL Server görüntü oluşturmayı düşünün. Çözümleme Hizmetleri SysPrep işlevselliği ile SQL Server 2012 SP1 CU2 etkinleştirildi. Daha fazla bilgi için bkz: [SQL Server kullanarak Sysprep yükleme konuları](https://msdn.microsoft.com/library/ee210754.aspx) ve [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="to-install-analysis-services-tabular-mode"></a>Analysis Services Tabular modunda yüklemek için
Bu bölümdeki adımları **özetlemek** Analysis Services tabular modunda yükleme. Daha fazla bilgi için aşağıdakilere bakın:

* [Analysis Services Tabular modunda yükleme](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tablo modelleme (Adventure Works öğretici)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services Tabular modunda yüklemek için:**

1. SQL Server Yükleme Sihirbazı'nda tıklatın **yükleme** sol bölmesinde ve ardından **yeni SQL server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**.
   
   * Görürseniz **klasöre Gözat**, c:\SQLServer_13.0_full, c:\SQLServer_12.0_full veya c:\SQLServer_11.0_full göz atın ve ardından **Tamam**.
2. Tıklatın **sonraki** ürün güncelleştirmeleri sayfasında.
3. Üzerinde **yükleme türü** sayfasında, **SQL Server'ın yeni bir yükleme gerçekleştirmek** tıklatıp **sonraki**.
4. Üzerinde **Kurulum rolü** sayfasında, **SQL Server özellikleri yükleme**.
5. Üzerinde **özellik seçimi** sayfasında, **Analysis Services**.
6. Üzerinde **örnek Yapılandırması** sayfasında, gibi açıklayıcı bir ad yazın **tablolu** içine **adlı örneği** ve **örnek kimliği** metin kutuları .
7. Üzerinde **Çözümleme Hizmetleri Yapılandırması** sayfasında, **Tabular modda**. Geçerli kullanıcının yönetici izinleri listesine ekleyin.
8. Tamamlayın ve SQL Server Yükleme Sihirbazı'nı kapatın.

## <a name="analysis-services-configuration"></a>Çözümleme Hizmetleri Yapılandırması
### <a name="remote-access-to-analysis-services-server"></a>Analysis Services sunucusu için uzaktan erişim
Analysis Services sunucusu, yalnızca windows kimlik doğrulamasını destekler. Analysis Services, SQL Server Management Studio veya SQL Server veri araçları gibi istemci uygulamalardan uzaktan erişmek için sanal makinenin Azure sanal ağ'ı kullanarak, yerel etki alanına katılması gerekir. Daha fazla bilgi için [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

A **varsayılan örnek** Analysis Services TCP bağlantı noktasını dinler **2383**. Sanal makineler güvenlik duvarında bağlantı noktasını açın. Kümelenmiş Analysis Services örneği dinlediği bağlantı noktası adı da **2383**.

İçin bir **adlandırılmış örneği** Analysis Services SQL Server Browser hizmeti bağlantı noktası erişimi yönetmek için gereklidir. Bağlantı noktası SQL Server Browser varsayılan yapılandırmadır **2382**.

Sanal makineler Güvenlik Duvarı'nda bağlantı noktası açmak **2382** ve statik bir Analysis Services örneğinin bağlantı noktası adlandırılmış oluşturun.

1. VM ve bağlantı noktalarını hangi işlemin kullandığını zaten kullanılan bağlantı noktaları doğrulamak için yönetici ayrıcalıklarıyla aşağıdaki komutu çalıştırın:
   
        netstat /ao
2. Statik bir Analysis Services oluşturmak için kullanım SQL Server Management Studio adlı örnek bağlantı noktası 'Bağlantı noktası' güncelleştirerek tablo AS değerinde örnek genel özellikleri. Daha fazla bilgi için "sabit bir bağlantı noktası için bir varsayılan kullanın veya adlandırılmış örneğine" içinde bakın [izin Analysis Services erişimi için Windows Güvenlik Duvarı'nı yapılandırma](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Analysis Services hizmet tablo örneğini yeniden başlatın.

Daha fazla bilgi için **sanal makine uç noktaları ve güvenlik duvarı bağlantı noktalarını** bu belgede bölüm.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Sanal makine uç noktaları ve güvenlik duvarı bağlantı noktaları
Bu bölüm oluşturmak için Microsoft Azure sanal makine uç ve sanal makine güvenlik duvarları içinde açmak için bağlantı noktalarını özetler. Aşağıdaki tabloda özetlenmiştir **TCP** için uç noktaları oluşturmak için ve sanal makineleri Güvenlik Duvarı'nda bağlantı noktaları.

* Tek bir VM'ye kullanıyorsanız ve aşağıdaki iki öğeyi doğruysa, VM uç noktalar oluşturmanız gerekmez ve VM üzerindeki güvenlik duvarında bağlantı noktalarını açmanız gerekmez.
  
  * Uzaktan VM'de SQL Server özellikleri bağlandığınız değil. Bir Uzak Masaüstü Bağlantısı VM oluşturma ve yerel olarak VM üzerinde SQL Server özelliklerine erişme SQL Server özelliklerini uzak bağlantı olarak kabul edilmez.
  * VM, Azure sanal ağ veya başka bir VPN tünel çözüm aracılığıyla şirket içi etki katılmayın.
* Sanal makine bir etki alanına katılmamış ancak uzaktan istiyorsanız VM üzerinde SQL Server özellikleri Bağlan:
  
  * VM üzerindeki Güvenlik Duvarı'nda bağlantı noktalarını açın.
  * Belirtilen bağlantı noktaları (*) için sanal makine uç noktaları oluşturun.
* Sanal makinenin Azure sanal ağ gibi bir VPN tüneli kullanarak bir etki alanına katılmışsa, uç noktaları gerekli değildir. Ancak VM üzerinde Güvenlik Duvarı'nda bağlantı noktalarını açın.
  
  | Bağlantı noktası | Tür | Açıklama |
  | --- | --- | --- |
  | **80** |TCP |Rapor sunucusu uzaktan erişim (*). |
  | **1433** |TCP |SQL Server Management Studio'yu (*). |
  | **1434** |UDP |SQL Server Browser. VM bir etki alanına katıldığında bu gereklidir. |
  | **2382** |TCP |SQL Server Browser. |
  | **2383** |TCP |SQL Server Analysis Services varsayılan örneği ve kümelenmiş adlandırılmış örnekleri. |
  | **Kullanıcı tanımlı** |TCP |Seçtiğiniz bir bağlantı noktası numarası için örnek bağlantı noktası adlı statik bir Analysis Services oluşturun ve Güvenlik Duvarı'nda bağlantı noktası numarası Engellemeyi Kaldır. |

Uç noktaları oluşturma hakkında daha fazla bilgi için aşağıdakilere bakın:

* Uç noktalar oluşturursunuz:[nasıl bir sanal makineye uç noktaları ayarlamak için](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL Server: "Kullanarak SQL Server Management Studio sanal makineye bağlanmak için tam yapılandırma adımları" bölümüne bakın [Azure üzerinde bir SQL Server sanal makine sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Aşağıdaki diyagram, VM özelliklerini ve bileşenlerini uzaktan erişime izin vermek için VM Güvenlik Duvarı'nda açmak için bağlantı noktalarını gösterir.

![Azure vm'lerinde bi uygulamaları için açılacak bağlantı noktaları](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Kaynaklar
* Azure sanal makine ortamında kullanılan Microsoft sunucu yazılımı için destek ilkesini inceleyin. Aşağıdaki konuda, BitLocker, Yük Devretme Kümelemesi ve Ağ Yükü Dengeleme gibi özellikleri için destek özetlenmektedir. [Microsoft sunucu yazılımı desteği için Azure sanal makineleri](http://support.microsoft.com/kb/2721672).
* [SQL Server üzerinde Azure sanal makinelere genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Azure üzerinde bir SQL Server sanal makine sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Nasıl bir sanal makineye bir veri diski Ekle](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [SQL Server Azure VM'de bir veritabanını geçirme](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Sunucu modunda bir Analysis Services örneğinin belirleme](https://msdn.microsoft.com/library/gg471594.aspx)
* [Çok boyutlu modelleme (Adventure Works öğretici)](https://technet.microsoft.com/library/ms170208.aspx)
* [Azure Belge Merkezi'nde](https://azure.microsoft.com/documentation/)
* [Power BI karma bir ortamda kullanma](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Geri bildirim ve Microsoft SQL Server bağlantı üzerinden iletişim bilgileri Gönder](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Topluluk içeriği
* [PowerShell ile Azure SQL veritabanı yönetimi](https://azure.microsoft.com/blog/windows-azure-sql-database-management-with-powershell/)

