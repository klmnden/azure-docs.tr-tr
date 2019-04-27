---
title: SQL Server Business Intelligence | Microsoft Docs
description: Bu konu, Klasik dağıtım modeliyle oluşturulan kaynaklar kullanılmaktadır ve Azure sanal makineler (VM) üzerinde çalışan SQL Server Business Intelligence (BI) özellikleri açıklar.
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
ms.openlocfilehash: 29e851772e665b4130ee58b04c264d55bcd54523
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609326"
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Azure Sanal Makinelerde SQL Server İş Zekası
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Microsoft Azure sanal makine Galerisi SQL Server yüklemelerini içeren görüntüleri içerir. Galeri görüntüleri desteklenen SQL Server sürümlerinde, şirket içi bilgisayarlarını ve sanal makineleri için yükleyebilmek için aynı yükleme dosyalarıdır. Bu konuda, bir sanal makine sağlandıktan sonra gerekli yapılandırma adımları ve görüntülerin üzerine yüklü SQL Server Business Intelligence (BI) özellikleri özetlenmektedir. Bu konuda, aynı zamanda BI özellikleri ve en iyi uygulamalar için desteklenen dağıtım topolojileri açıklanır.

## <a name="license-considerations"></a>Lisans konuları
Microsoft Azure Virtual Machines'de SQL Server lisansı için iki yol vardır:

1. Yazılım Güvencesi kapsamındaki lisans taşınabilirliği avantajlarından. Daha fazla bilgi için [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/).
2. Saat hızı yüklü SQL sunucunuz ile Azure sanal makinelerinin başına ödeme yapın. "SQL Server" bölümüne bakın [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Geçerli ve lisans ücretleri hakkında daha fazla bilgi için bkz. [sanal makineler lisanslama SSS](https://azure.microsoft.com/pricing/licensing-faq/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server görüntülerinin kullanılabilir Azure sanal makine Galerisi
Microsoft Azure sanal makine Galerisi Microsoft SQL Server içeren birkaç görüntüyü içerir. Sanal makine görüntülerinde yüklü yazılım, işletim sistemi ve SQL Server sürümüne göre değişir. Azure sanal makine galerisindeki kullanılabilir görüntülerin listesini sık değiştirir.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![Azure VM galerisinde SQL görüntüsü](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Aşağıdaki PowerShell betiğini "SQL-Server" IMAGENAME içeren Azure görüntü listesini döndürür:

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

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>SQL Server sanal makine galeri görüntüleri üzerinde yüklü BI özellikleri
Aşağıdaki tabloda yaygın Microsoft Azure sanal makine galeri görüntüleri üzerinde SQL Server için yüklenen iş zekası özellikleri özetlenmektedir:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 standart
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 standart

| SQL Server BI özelliği | Galeri görüntüsü üzerinde yüklü | Notlar |
| --- | --- | --- |
| **Raporlama Hizmetleri yerel modu** |Evet |Yüklü, ancak Rapor Yöneticisi URL'si de dahil olmak üzere yapılandırması gerektirir. Bölümüne bakın [Raporlama hizmetlerini yapılandırın](#configure-reporting-services). |
| **Reporting Services SharePoint modu** |Hayır |Microsoft Azure sanal makine galeri görüntüsü, SharePoint veya SharePoint içermez yükleme dosyaları. <sup>1</sup> |
| **Analysis Services çok boyutlu ve veri madenciliği (OLAP)** |Evet |Yüklenmiş ve yapılandırılmış varsayılan Analysis Services örneği |
| **Analysis Services-tablo** |Hayır |SQL Server 2012'de desteklenen, 2014 ve 2016 görüntüleri ancak değil varsayılan olarak yüklenir. Başka bir Analysis Services örneğini yükleyin. Bölümüne bakın, bu konudaki diğer SQL Server Hizmetleri ve özellikleri yükleyin. |
| **SharePoint için Analysis Services Power Pivot** |Hayır |Microsoft Azure sanal makine galeri görüntüsü, SharePoint veya SharePoint içermez yükleme dosyaları. <sup>1</sup> |

<sup>1</sup> SharePoint ve Azure sanal makineleri hakkında ek bilgi için bkz. [Microsoft SharePoint 2013 için Azure mimarileri](https://technet.microsoft.com/library/dn635309.aspx) ve [SharePoint dağıtım Microsoft Azure sanal makinelerinde](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Hizmetin adı "SQL" içeren yüklü hizmetlerin bir listesini almak için aşağıdaki PowerShell komutunu çalıştırın.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Genel öneriler ve en iyi uygulamalar
* Bir sanal makine için önerilen boyutu en az **A3** SQL Server Enterprise Edition'ı kullanırken. **A4** sanal makine boyutu, Analysis Services ve Reporting Services SQL Server BI dağıtımları için önerilir.
  
    Geçerli VM boyutları hakkında daha fazla bilgi için bkz: [Azure için sanal makine boyutlarını](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Veri ve günlük sürücüleri yedekleme dosyaları dışında depolamak için disk yönetimi için en iyi uygulama olan **C**: ve **D**:. Örneğin, veri diskleri oluşturma **E**: ve **F**:.
  
  * Önbelleğe alma ilkesi varsayılan sürücüsü için sürücü **C**: verilerle çalışmak için uygun değil.
  * **D**: öncelikle sayfa dosyası için kullanılan geçici sürücü sürücüdür. **D**: sürücü kalıcı değildir ve blob depolama alanında kaydedilmez. Sanal makine için bir değişiklik gibi yönetim görevleri boyutu Sıfırla **D**: sürücü. Önerilir **değil** kullanın **D**: tempdb dahil olmak üzere, veritabanı dosyaları sürücüsü.
    
    Oluşturma ve diskleri ekleme hakkında daha fazla bilgi için bkz. [bir sanal makineye bir veri diski ekleme](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Durdurmak veya kullanmayı planlamıyorsanız hizmetlerini kaldırın. Örnek sanal makine yalnızca Raporlama Hizmetleri için kullanılıyorsa, durdurmak veya Analysis Services ve SQL Server Integration Services'ı kaldırın. Aşağıdaki görüntüde, varsayılan olarak başlatılan hizmetler örneğidir.
  
    ![SQL Server Hizmetleri](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > SQL Server veritabanı altyapısı desteklenen bir BI senaryolarda gereklidir. Tek bir sunucu VM topoloji, veritabanı altyapısı, aynı sanal makinede çalıştırılması için gereklidir.
  
    Daha fazla bilgi için, aşağıdakilere bakın: [Reporting Services'ı kaldırmanız](https://msdn.microsoft.com/library/hh479745.aspx) ve [Analysis Services örneğini kaldırma](https://msdn.microsoft.com/library/ms143687.aspx).
* Denetleme **Windows Update** yeni 'önemli güncelleştirmeleri'. Microsoft Azure sanal makine görüntüleri sıklıkla yenilendiğinden; Bununla birlikte önemli güncelleştirmeler kullanılabilir hale gelebilir **Windows Update** VM görüntüsünün son yenilenme sonra.

## <a name="example-deployment-topologies"></a>Örnek dağıtım topolojileri
Microsoft Azure sanal makineleri'nin örnek dağıtımlar aşağıda verilmiştir. Bu diyagramları Topolojilerde yalnızca SQL Server BI özellikleri ve Microsoft Azure sanal makineler ile kullanabileceğiniz olası topolojiler bazılarıdır.

### <a name="single-virtual-machine"></a>Tek Sanal Makine
Analiz Hizmetleri, Reporting Services, SQL Server veritabanı altyapısı ve veri kaynakları tek bir sanal makine üzerinde.

![1 sanal makine ile bi iass senaryosu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>İki sanal makine
* Analysis Services, Reporting Services ve tek bir sanal makinede SQL Server veritabanı altyapısı. Bu dağıtım, rapor sunucusu veritabanlarını içerir.
* İkinci bir sanal makine üzerindeki veri kaynakları. İkinci bir sanal makine, bir veri kaynağı olarak SQL Server veritabanı altyapısı içerir.

![2 sanal makine ile bi iass senaryosu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Karma Azure – Azure SQL veritabanında veri
* Analysis Services, Reporting Services ve tek bir sanal makinede SQL Server veritabanı altyapısı. Bu dağıtım, rapor sunucusu veritabanlarını içerir.
* Azure SQL veritabanına veri kaynağıdır.

![Bi iaas senaryoları vm ve veri kaynağı olarak AzureSQL](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Karma – şirket içi veri
* Reporting Services ve SQL Server veritabanı altyapısı, Analysis Services bu örnek dağıtımda, tek bir sanal makinede çalıştırın. Sanal makine, rapor sunucusu veritabanlarını barındırır. Sanal makine, Azure sanal ağ üzerinden bir şirket içi etki alanına veya çözüm tünel bazı VPN birleştirilir.
* Şirket içi veri kaynağıdır.

![BI ıaas senaryoları vm ve şirket içi veri kaynakları](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Reporting Services yerel modu yapılandırması
Rapor sunucusu yapılandırılmamış ancak sanal makine galeri görüntüsü için SQL Server yüklü bir Reporting Services yerel modu içerir. Bu bölümdeki adımları Reporting Services rapor sunucusunu yapılandırın. Reporting Services yerel modu yapılandırma hakkında daha ayrıntılı bilgi için bkz: [yüklemek Raporlama Hizmetleri yerel mod rapor sunucusu (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Rapor sunucusunu yapılandırmak için Windows PowerShell komut dosyalarını kullanan benzer içeriğine bakın [bir Azure VM ile bir yerel mod rapor sunucusu oluşturmak için PowerShell kullanma](../classic/ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Sanal makineye bağlanın ve Raporlama Hizmetleri Yapılandırma Yöneticisi'ni Başlat
Bir Azure sanal makinesine bağlanmak için iki ortak iş akışı vardır:

* , Bağlan'a tıklayın sanal makinenin adını ve ardından **Connect**. Uzak Masaüstü Bağlantısı açar ve bilgisayar adını otomatik olarak doldurulur.
  
    ![Azure sanal makinesine bağlanın](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Windows Uzak Masaüstü Bağlantısı ile sanal makineye bağlanabilirsiniz. Uzak Masaüstü kullanıcı arabiriminde:
  
  1. Tür **bulut hizmeti adı** bilgisayar adı.
  2. İki nokta üst üste (:) ve TCP Uzak Masaüstü uç noktası için yapılandırılmış ortak bağlantı noktası numarasını yazın.
     
      Myservice.cloudapp.net:63133
     
      Daha fazla bilgi için [bir bulut hizmeti nedir?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Reporting Services Configuration Manager'ı Başlat**

İçinde **Windows Server 2012/2016**:

1. Gelen **Başlat** ekranında, yazın **Reporting Services** uygulamaların bir listesini görmek için.
2. Sağ **Reporting Services Yapılandırma Yöneticisi** tıklatıp **yönetici olarak çalıştır**.

İçinde **Windows Server 2008 R2**:

1. Tıklayın **Başlat**ve ardından **tüm programlar**.
2. Tıklayın **Microsoft SQL Server 2016**.
3. Tıklayın **yapılandırma araçları**.
4. Sağ **Reporting Services Yapılandırma Yöneticisi** tıklatıp **yönetici olarak çalıştır**.

veya:

1. **Başlat**'a tıklayın.
2. İçinde **programları ve dosyaları ara** iletişim kutusuna **reporting services**. Sanal makine Windows Server 2012 çalıştırıyorsa, yazın **reporting services** Windows Server 2012 Başlat ekranında.
3. Sağ **Reporting Services Yapılandırma Yöneticisi** tıklatıp **yönetici olarak çalıştır**.
   
    ![ssrs Yapılandırma Yöneticisi'ni arayın](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Raporlama Hizmetleri Yapılandırma
**Hizmet hesabı ve web hizmeti URL'si:**

1. Doğrulama **sunucu adı** tıklayın ve yerel sunucu adı olduğunu **Connect**.
2. Boş Not **rapor sunucusu veritabanı adı**. Yapılandırma tamamlandığında veritabanı oluşturulur.
3. Doğrulama **rapor sunucusu durumu** olduğu **başlatıldı**. Windows Sunucu Yöneticisi'nde hizmet doğrulamak istiyorsanız, hizmetidir **SQL Server Reporting Services** Windows hizmeti.
4. Tıklayın **hizmet hesabı** ve hesap gerektiği gibi değiştirin. Sanal makine bir etki alanı olmayan birleştirilmiş ortamında, yerleşik kullanılıyorsa **ReportServer** hesabıdır yeterli. Hizmet hesabı hakkında daha fazla bilgi için bkz. [hizmet hesabı](https://msdn.microsoft.com/library/ms189964.aspx).
5. Tıklayın **Web hizmeti URL'si** sol bölmesinde.
6. Tıklayın **Uygula** varsayılan değerlerle yapılandırılır.
7. Not **rapor sunucusu Web hizmeti URL'leri**. Varsayılan TCP bağlantı noktası 80'dir ve URL parçası unutmayın. Bir sonraki adımda Microsoft Azure sanal makine uç noktası için bağlantı noktası oluşturun.
8. İçinde **sonuçları** bölmesinde başarıyla tamamlanan Eylemler doğrulayın.

**Veritabanı:**

1. Tıklayın **veritabanı** sol bölmesinde.
2. Tıklayın **değiştirme veritabanı**.
3. Doğrulama **yeni bir rapor sunucusu veritabanı oluşturmanız** seçilir ve İleri'ye tıklayın.
4. Doğrulama **sunucu adı** tıklatıp **Bağlantıyı Sına**.
5. Sonuç ise **sınama bağlantısı başarılı oldu**, tıklayın **Tamam** ve ardından **sonraki**.
6. Veritabanı adını Not **ReportServer** ve **rapor sunucusu modu** olduğu **yerel** ardından **sonraki**.
7. Tıklayın **sonraki** üzerinde **kimlik bilgilerini** sayfası.
8. Tıklayın **sonraki** üzerinde **özeti** sayfası.
9. Tıklayın **sonraki** üzerinde **ilerleme ve son** sayfası.

**2012 ve 2014 için web portalı URL'si veya Rapor Yöneticisi URL'si:**

1. Tıklayın **Web portalı URL'niz**, veya **Rapor Yöneticisi URL'si** 2014 ve sol bölmede 2012.
2. **Uygula**'ya tıklayın.
3. İçinde **sonuçları** bölmesinde başarıyla tamamlanan Eylemler doğrulayın.
4. **Çıkış**'a tıklayın.

Rapor sunucusu izinleri hakkında daha fazla bilgi için bkz: [yerel mod rapor sunucusu izinleri verme](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Yerel Rapor Yöneticisi için Gözat
Yapılandırmayı doğrulamak için VM üzerinde Rapor Yöneticisi göz atın.

1. VM üzerinde yönetici ayrıcalıklarına sahip Internet Explorer'ı başlatın.
2. HTTP için göz atın: \/ /localhost/VM'de bildirir.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Uzak web portalı veya 2014 ve 2012 için Rapor Yöneticisi için Bağlan
2014'e ve uzak bir bilgisayardan sanal makinede 2012 için web portalı veya Rapor Yöneticisi için bağlanmak isterseniz yeni bir sanal makine TCP uç noktası oluşturun. Varsayılan olarak, rapor sunucusu HTTP isteklerini dinler. **bağlantı noktası 80**. Rapor sunucusu URL'leri, farklı bir bağlantı noktasını kullanacak şekilde yapılandırırsanız aşağıdaki yönergelerde yer bağlantı noktası numarasını belirtmeniz gerekir.

1. Sanal makine için TCP bağlantı noktası 80'in bir uç nokta oluşturun. Daha fazla bilgi için [sanal makine uç noktalarına ve güvenlik duvarı bağlantı noktalarının](#virtual-machine-endpoints-and-firewall-ports) bu belgenin bölüm.
2. Sanal makinenin Güvenlik Duvarı'nda 80 numaralı bağlantı noktasını açın.
3. Web Portalı'na göz atın veya Rapor Yöneticisi kullanarak Azure sanal makine, **DNS adı** URL'deki sunucu adı olarak. Örneğin:
   
    **Rapor sunucusu**: http://uebi.cloudapp.net/reportserver  **Web portalı**: http://uebi.cloudapp.net/reports
   
    [Rapor sunucusu erişimi için bir güvenlik duvarı yapılandırma](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Oluşturma ve raporları Azure sanal makine yayımlama
Aşağıdaki tabloda bazı Microsoft Azure sanal makinede barındırılan bir rapor sunucusu için bir şirket içi bilgisayardan varolan raporları yayımlamak kullanılabilir seçenekler özetlenmektedir:

* **Rapor Oluşturucu**: Tıklayarak sanal makineyi içeren-SQL 2014 ve 2012 için Microsoft SQL Server Rapor Oluşturucusu sürümünü bir kez. Sanal makinedeki SQL 2016 Rapor Oluşturucusu'nu ilk zaman başlatmak için:
  
  1. Tarayıcınız, yönetici ayrıcalıklarıyla başlatın.
  2. Sanal makinedeki web portalında göz atın ve seçim **indirin** sağ üst köşedeki bir simge.
  3. Seçin **Rapor Oluşturucu**.
     
     Daha fazla bilgi için [Rapor Oluşturucusu'nu başlatın](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server veri Araçları**: VM:  SQL Server veri araçları sanal makineye yüklenir ve oluşturmak için kullanılan **rapor sunucusu projeleri** ve sanal makine üzerindeki raporlar. SQL Server veri araçları, sanal makinede rapor sunucusu raporları yayımlayabilirsiniz.
* **SQL Server veri araçları: Uzak**:  Yerel bilgisayarınızda SQL Server veri araçları Raporlama Hizmetleri raporlarını içeren bir Reporting Services projesi oluşturun. Projenin web hizmetinin URL'SİYLE bağlanmak için yapılandırın.
  
    ![SSRS projesi için SSDT proje özellikleri](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Oluşturma bir. Raporlarını içerir ve karşıya yükleyin ve sürücüsü ekleme VHD'yi sabit sürücü.
  
  1. Oluşturma bir. VHD'yi sabit sürücü yerel bilgisayarınızda, raporları içerir.
  2. Oluşturun ve bir yönetim sertifikası yükleyin.
  3. Add-AzureVHD cmdlet'ini kullanarak Azure'a VHD dosyası yükleme [oluşturup yükleme azure'a bir Windows Server VHD](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. Sanal makineye disk ekleyin.

## <a name="install-other-sql-server-services-and-features"></a>Diğer SQL Server Hizmetleri ve Özellikler yükleme
Analysis Services tabular modunda gibi ek SQL Server hizmetlerini yüklemek için SQL server Kurulum Sihirbazı'nı çalıştırın. Kurulum, sanal makinenin yerel diskte dosyalarıdır.

1. Tıklayın **Başlat** ve ardından **tüm programlar**.
2. Tıklayın **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** veya **Microsoft SQL Server 2012** ve ardından **yapılandırma araçları** .
3. Tıklayın **SQL Server Yükleme Merkezi'ni**.

Veya C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe veya C:\SQLServer_11.0_full\setup.exe çalıştırın

> [!NOTE]
> SQL Server kurulumunu çalıştırdığınız ilk kez daha fazla Kurulum dosyaları yüklenebilir ve bir sanal makinenin yeniden başlatılması ve SQL Server kurulumunu yeniden başlatılması gerekir.
> 
> Görüntü Microsoft Azure sanal makine seçildi art arda özelleştirmek gerekiyorsa, kendi SQL Server görüntüsü oluşturma göz önünde bulundurun. Analysis Services SysPrep işlevselliği ile SQL Server 2012 SP1 CU2 etkinleştirildi. Daha fazla bilgi için [için SQL Server kullanarak SysPrep yükleme konuları](https://msdn.microsoft.com/library/ee210754.aspx) ve [sunucu rolleri için Sysprep Destek](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="to-install-analysis-services-tabular-mode"></a>Analysis Services Tabular modunda yüklemek için
Bu bölümdeki adımları **özetlemek** Analysis Services tabular modunda yükleme. Daha fazla bilgi için, aşağıdakilere bakın:

* [Analysis Services Tabular modunda yükleme](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tablosal modelleme (Adventure Works Öğreticisi)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services Tabular modunda yüklemek için:**

1. SQL Server Yükleme Sihirbazı'nda tıklatın **yükleme** sol bölmesi ve ardından **yeni SQL server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**.
   
   * Görürseniz **klasöre Gözat**c:\SQLServer_13.0_full, c:\SQLServer_12.0_full veya c:\SQLServer_11.0_full göz atın ve ardından **Tamam**.
2. Tıklayın **sonraki** ürün güncelleştirmeleri sayfasında.
3. Üzerinde **yükleme türünü** sayfasında **SQL Server'ın yeni yüklemenin gerçekleştirilmesi** tıklatıp **sonraki**.
4. Üzerinde **Kurulum rolü** sayfasında **SQL Server özellik yüklemesi**.
5. Üzerinde **özellik seçimi** sayfasında **Analysis Services**.
6. Üzerinde **örnek Yapılandırması** gibi açıklayıcı bir ad yazın, sayfa **sekmeli** içine **adlı örnek** ve **örnek kimliği** metin kutuları .
7. Üzerinde **Çözümleme Hizmetleri Yapılandırması** sayfasında **Tabular modda**. Geçerli kullanıcının yönetimsel izinler listesine ekleyin.
8. Tamamlayın ve SQL Server Yükleme Sihirbazı'nı kapatın.

## <a name="analysis-services-configuration"></a>Çözümleme Hizmetleri Yapılandırması
### <a name="remote-access-to-analysis-services-server"></a>Analysis Services sunucusu için uzaktan erişim
Analysis Services sunucusu, yalnızca windows kimlik doğrulamasını destekler. Analysis Services, SQL Server Management Studio veya SQL Server veri araçları gibi istemci uygulamalarından uzaktan erişmek için sanal makineyi Azure sanal ağ'ı kullanarak, yerel etki alanına katılması gerekir. Daha fazla bilgi edinmek, [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md).

A **varsayılan örnek** Analysis Services'ın TCP bağlantı noktasında dinler **2383**. Bağlantı noktası, sanal makineler Güvenlik Duvarı'nda açın. Kümelenmiş bir Analysis Services örneğinin de bağlantı noktasını dinler adlı **2383**.

İçin bir **adlandırılmış örnek** Analysis Services SQL Server Browser hizmeti bağlantı noktası erişimi yönetmek için gereklidir. Bağlantı noktası SQL Server Browser varsayılan yapılandırmadır **2382**.

Sanal makineler güvenlik duvarında bağlantı noktası açma **2382** ve statik bir Analysis Services örneğinin bağlantı noktası adlı oluşturun.

1. VM ve bağlantı noktalarını hangi işlemin kullandığını zaten kullanılan bağlantı noktaları doğrulamak için yönetici ayrıcalıklarıyla aşağıdaki komutu çalıştırın:
   
        netstat /ao
2. Statik bir Analysis Services'ı oluşturmak için kullanım SQL Server Management Studio adlı örnek bağlantı noktası 'Bağlantı noktası' güncelleştirerek tablo AS değerinde örneğinin genel özellikler. Daha fazla bilgi için "sabit bir bağlantı noktası kullanmak için bir varsayılan veya adlandırılmış örneğine" bkz [izin Analysis Services erişimi için Windows Güvenlik Duvarı'nı yapılandırma](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Analysis Services hizmetinin tablo örneğini yeniden başlatın.

Daha fazla bilgi için **sanal makine uç noktalarına ve güvenlik duvarı bağlantı noktalarının** bu belgenin bölüm.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Sanal makine uç noktalarına ve güvenlik duvarı bağlantı noktaları
Bu bölümde, Microsoft Azure sanal makine oluşturmak için uç ve sanal makine güvenlik duvarlarında açmanız bağlantı noktaları özetlenmektedir. Aşağıdaki tabloda özetlenmiştir **TCP** için uç noktalar oluşturmak için ve sanal makineler Güvenlik Duvarı'nda bağlantı noktaları.

* Tek bir VM kullanıyorsanız ve aşağıdaki iki öğeyi true ise, VM'nin uç noktalar oluşturmak ihtiyacınız yoktur ve VM üzerindeki güvenlik duvarı bağlantı noktalarını açmanız gerekmez.
  
  * Uzaktan VM üzerinde SQL Server özelliklerini bağlandığınız değil. VM'ye Uzak Masaüstü bağlantısı kurma ve yerel olarak VM üzerinde SQL Server özelliklerine erişme uzak bir SQL Server özelliklerini bağlantı olarak kabul edilmez.
  * VM, Azure sanal ağ veya başka bir VPN tünel çözüm aracılığıyla şirket içi etki alanına katılma.
* Sanal makine bir etki alanına katılmamış ancak uzaktan istiyorsanız VM üzerinde SQL Server özelliklerini bağlanın:
  
  * VM üzerindeki güvenlik duvarında bağlantı noktaları açın.
  * Sanal makine uç noktalarına belirtilenler bağlantı noktaları (*) oluşturun.
* Sanal makine Azure sanal ağ gibi bir VPN tüneli ile bir etki alanına katılmışsa, uç noktaları gerekli değildir. Ancak, sanal makinedeki güvenlik duvarı bağlantı noktalarını açın.
  
  | Bağlantı noktası | Tür | Açıklama |
  | --- | --- | --- |
  | **80** |TCP |Rapor sunucusu uzaktan erişim (*). |
  | **1433** |TCP |SQL Server Management Studio'yu (*). |
  | **1434** |UDP |SQL Server Tarayıcısı. Bu VM'yi bir etki alanına katıldığında gereklidir. |
  | **2382** |TCP |SQL Server Tarayıcısı. |
  | **2383** |TCP |SQL Server Analysis Services varsayılan örneği ve kümelenmiş adlandırılmış örnekler. |
  | **Kullanıcı tanımlı** |TCP |Seçtiğiniz bir bağlantı noktası numarası için örnek bağlantı noktası adlı statik bir Analysis Services oluşturun ve ardından güvenlik duvarı bağlantı noktası numarasını engelini kaldırma. |

Uç noktaları oluşturma hakkında daha fazla bilgi için aşağıdakilere bakın:

* Uç noktaları oluşturun:[bir sanal makineye uç noktaları ayarlama](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL sunucusu: "Kullanarak SQL Server Management Studio sanal makineye bağlanmak için tam yapılandırma adımları" bölümüne bakın [azure'da bir SQL Server sanal makinesi sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Aşağıdaki diyagram, VM özelliklerinin ve bileşenlerinin uzaktan erişime izin vermek için VM Güvenlik Duvarı'nda açmak için bağlantı noktalarını gösterir.

![Azure vm'lerinde bi uygulamaları için açılacak bağlantı noktaları](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Kaynaklar
* Microsoft sunucu yazılımı Azure sanal makine ortamında kullanılan destekleme ilkesini inceleyin. Aşağıdaki konuda, BitLocker, Yük Devretme Kümelemesi ve Ağ Yükü Dengeleme gibi özelliklere yönelik desteği özetler. [Microsoft sunucu yazılımı desteği Azure sanal makineleri için](https://support.microsoft.com/kb/2721672).
* [SQL Server Azure sanal makinelerine genel bakış](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Azure'da bir SQL Server sanal makinesi sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Bir sanal makineye bir veri diski ekleme](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Bir veritabanını Azure VM'deki SQL Server'a geçirme](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Sunucu modu bir Analysis Services örneğinin belirleme](https://msdn.microsoft.com/library/gg471594.aspx)
* [Çok boyutlu modelleme (Adventure Works Öğreticisi)](https://technet.microsoft.com/library/ms170208.aspx)
* [Azure Belge Merkezi](https://azure.microsoft.com/documentation/)
* [Power BI hibrit bir ortamda kullanma](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Geri bildirim ve iletişim bilgileri aracılığıyla Microsoft SQL Server ile bağlantı gönder](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Topluluk içeriği
* [PowerShell ile Azure SQL veritabanı yönetimi](https://azure.microsoft.com/blog/windows-azure-sql-database-management-with-powershell/)

