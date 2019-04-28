---
title: Azure Resource Manager Vm'leri için yüksek oranda kullanılabilirliği ayarlayın | Microsoft Docs
description: Bu öğreticide, Azure sanal makineleri Azure Resource Manager modunda bir Always On kullanılabilirlik grubu oluşturulacağını gösterir.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bddc83d55c8909412f7f935a4324a6f316a82cd7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129562"
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Always On kullanılabilirlik grupları, Azure sanal Makineler'de otomatik olarak yapılandırın: Resource Manager

Bu öğretici, Azure Resource Manager sanal makineler kullanan bir SQL Server kullanılabilirlik grubu oluşturma işlemi gösterilmektedir. Öğreticide, Azure dikey penceresi bir şablonu yapılandırmak için kullanılır. Varsayılan ayarları gözden geçirin, gerekli ayarları yazın ve bu öğreticiyi adım portalındaki dikey pencereleri güncelleştirin.

Tam öğretici, Azure sanal makineler üzerinde aşağıdaki öğeleri içeren bir SQL Server kullanılabilirlik grubu oluşturur:

* Bir ön uç ve arka uç alt ağı dahil olmak üzere birden çok alt ağa sahip bir sanal ağ
* Bir Active Directory etki alanına sahip iki etki alanı denetleyicisi
* SQL Server çalıştıran ve arka uç alt ağına dağıtılır ve Active Directory etki alanına katılmış iki sanal makine
* Düğüm çoğunluğu çekirdek modeli ile bir üç düğümlü yük devretme kümesi
* Bir kullanılabilirlik veritabanının iki synchronous-commit çoğaltmalarından olan bir kullanılabilirlik grubuna

Aşağıdaki çizim, çözümü temsil eder.

![Azure'da kullanılabilirlik grupları için test laboratuvarı mimarisi](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Bu çözümdeki tüm kaynakları tek bir kaynak grubuna ait.

Bu öğreticiye başlamadan önce aşağıdakileri doğrulayın:

* Bir Azure hesabı zaten var. Biri yoksa [bir deneme hesabı için kaydolun](https://azure.microsoft.com/pricing/free-trial/).
* Sanal makine galerisinden bir SQL Server sanal makine sağlamak için GUI'yi kullanmak nasıl biliyorsunuzdur. Daha fazla bilgi için [azure'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).
* Kullanılabilirlik grupları düz bir anlayış zaten var. Daha fazla bilgi için [Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Ayrıca SharePoint ile kullanılabilirlik gruplarını kullanarak ilgileniyorsanız bkz [yapılandırma SQL Server 2012 Always On kullanılabilirlik grupları SharePoint 2013 için](https://technet.microsoft.com/library/jj715261.aspx).
>
>

Bu öğreticide Azure portalını kullanarak:

* Always On şablonu portal'ı seçin.
* Şablon ayarları gözden geçirin ve ortamınız için birkaç yapılandırma ayarlarını güncelleştirin.
* Azure, tüm ortamı oluştururken izleyin.
* Etki alanı denetleyicisi ve ardından SQL Server çalıştıran bir sunucuya bağlanın.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Galeriden kümesi sağlama
Azure, tüm çözüm için bir galeri görüntüsü sağlar. Şablonu bulmak için:

1. Hesabınızı kullanarak Azure portalında oturum açın.
2. Azure portalında **kaynak Oluştur** açmak için **yeni** bölmesi.
3. Üzerinde **yeni** bölmesinde, arama **AlwaysOn**.
   ![AlwaysOn şablonunu bulun](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Arama sonuçlarında bulun **SQL Server AlwaysOn küme**.
   ![AlwaysOn şablonu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Üzerinde **dağıtım modeli seçin**, seçin **Resource Manager**.

### <a name="basics"></a>Temel Bilgiler
Tıklayın **Temelleri** ve aşağıdaki ayarları yapılandırın:

* **Yönetici kullanıcı adı** , etki alanı yönetici izinlerine sahip bir kullanıcı hesabı ve her iki SQL Server örnekleri üzerinde SQL Server sysadmin sabit sunucu rolünün bir üyesidir. Bu öğretici için kullanmak **DomainAdmin**.
* **Parola** etki alanı yönetici hesabı için parola. Karmaşık bir parola kullanın. Parolayı onaylayın.
* **Abonelik** kullanılabilirlik grubu için tüm dağıtılan kaynakları çalıştırmak için bu Azure faturaları aboneliktir. Hesabınızda birden fazla aboneliğiniz varsa, farklı bir abonelik belirtebilirsiniz.
* **Kaynak grubu** bu şablonla oluşturulan tüm Azure kaynaklarını ait olduğu grubu adıdır. Bu öğretici için kullanmak **SQL-HA-RG**. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Konum** burada öğretici oluşturur kaynakları Azure bölgesidir. Bir Azure bölgesi seçin.

Aşağıdaki ekran görüntüsünde bir tamamlanmış olduğu **Temelleri** dikey penceresinde:

![Temel Bilgiler](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

**Tamam** düğmesine tıklayın.

### <a name="domain-and-network-settings"></a>Etki alanı ve ağ ayarları
Bu Azure galeri şablonunun bir etki alanı ve etki alanı denetleyicileri oluşturur. Ayrıca, bir ağ ve iki alt ağ oluşturur. Şablonu sunucuları bir var olan etki alanı ya da sanal ağ oluşturamazsınız. Sonraki adım etki alanı ve ağ ayarlarını yapılandırır.

Üzerinde **etki alanı ve ağ ayarlarına** dikey penceresinde, etki alanı için önceden ayarlanmış değerleri inceleyin ve ağ ayarları:

* **Orman kök etki alanı adı** kümeyi barındıran Active Directory etki alanı için etki alanı adıdır. Öğretici için kullanmak **contoso.com**.
* **Sanal ağ adı** Azure sanal ağı için ağ adı. Öğretici için kullanmak **autohaVNET**.
* **Etki alanı denetleyicisi alt ağ adı** etki alanı denetleyicisini barındıran sanal ağı bir kısmını adıdır. Kullanım **alt ağ-1**. Bu alt ağ adresi ön ekini **10.0.0.0/24**.
* **SQL Server alt ağ adı** çalışma SQL Server ve dosya paylaşım tanığı sunucularını barındıran sanal ağı bir kısmını adıdır. Kullanım **alt ağı 2**. Bu alt ağ adresi ön ekini **10.0.1.0/26**.

Azure'daki sanal ağlar hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md).  

**Etki alanı ve ağ ayarlarına** aşağıdaki ekran görüntüsüne benzer görünmelidir:

![Etki alanı ve ağ ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğreticide, önceden ayarlanmış değerleri kullanın.

Ayarları gözden geçirin ve ardından **Tamam**.

### <a name="availability-group-settings"></a>Kullanılabilirlik grubu ayarları
Üzerinde **kullanılabilirlik grubu ayarları**, kullanılabilirlik grubu ve dinleyici için önceden ayarlanmış değerleri gözden geçirin.

* **Kullanılabilirlik grubu adının** kullanılabilirlik grubu için kümelenmiş bir kaynak adı. Bu öğretici için kullanmak **Contoso-ag**.
* **Kullanılabilirlik grubu dinleyici adıyla** küme ve iç yük dengeleyici tarafından kullanılır. SQL Server'a bağlanan istemciler bu ad, uygun veritabanı çoğaltmasına bağlanmak için kullanabilirsiniz. Bu öğretici için kullanmak **Contoso-listener**.
* **Kullanılabilirlik grubu dinleyicisinin bağlantı noktası** SQL Server dinleyicisi, TCP bağlantı noktasını belirtir. Bu öğretici için varsayılan bağlantı noktası kullanmak **1433**.

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğreticide, önceden ayarlanmış değerleri kullanın.  

![Kullanılabilirlik grubu ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

**Tamam** düğmesine tıklayın.

### <a name="virtual-machine-size-storage-settings"></a>Sanal makine boyutu, depolama ayarları
Üzerinde **VM boyutu, depolama ayarlarını**, bir SQL Server sanal makine boyutu seçin ve diğer ayarları gözden geçirin.

* **SQL Server sanal makine boyutu** için SQL Server çalıştıran her iki sanal makine boyutudur. Bir iş yükünüz için uygun sanal makine boyutu seçin. Bu ortam oluşturuyorsanız öğretici için kullanmak **DS2**. Üretim iş yükleri için iş yükü destekleyen bir sanal makine boyutu seçin. Çoğu üretim iş yükleri gerektirir **DS4** ya da daha büyük. Şablon, bu boyuttaki iki sanal makine oluşturur ve her birinde SQL Server'ı yükler. Daha fazla bilgi için [sanal makine boyutları](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure SQL Server Enterprise Edition'ı yükler. Maliyet, sürüm ve sanal makine boyutu bağlıdır. Geçerli ücretleri hakkında ayrıntılı bilgi için bkz. [sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Etki alanı denetleyicisi sanal makine boyutu** etki alanı denetleyicileri sanal makine boyutu. Bu öğretici kullanılmak **D2**.
* **Dosya Paylaşımı Tanığını sanal makine boyutu** dosya paylaşım tanığı için sanal makine boyutudur. Bu öğretici için kullanmak **A1**.
* **SQL depolama hesabı** işletim sistemi diskleri ve SQL Server verilerini tutan depolama hesabının adıdır. Bu öğretici için kullanmak **alwaysonsql01**.
* **DC depolama hesabı** depolama hesabı etki alanı denetleyicileri adıdır. Bu öğretici için kullanmak **alwaysondc01**.
* **SQL Server veri disk boyutu** TB SQL Server veri disk boyutu TB'dir. 1 ile 4 arasında bir sayı belirtin. Bu öğretici için kullanmak **1**.
* **Depolama İyileştirme** iş yükü türüne göre SQL Server sanal makineleri için özel depolama yapılandırma ayarlarını belirler. Bu senaryoda tüm SQL Server sanal makineleri premium depolama ile Azure disk konak önbelleği salt okunur ayarlamak kullanın. Ayrıca, bu üç ayarlarından birini seçerek iş yükü için SQL Server ayarları iyileştirebilirsiniz:

  * **Genel iş yükü** hiçbir özel yapılandırma ayarları ayarlar.
  * **İşlem tabanlı işleme** izleme bayrağı 1117 ve 1118 ayarlar.
  * **Veri ambarlama** kümeleri izleme bayrağı 1117 ve 610.

Bu öğretici için kullanmak **genel iş yükü**.

![VM boyutu depolama ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Ayarları gözden geçirin ve ardından **Tamam**.

#### <a name="a-note-about-storage"></a>Depolama hakkında bir Not
Ek iyileştirmeler, SQL Server veri diskleri boyutuna bağlıdır. Azure, her veri diski terabayt için ek 1 TB premium depolama alanı ekler. Bir sunucu 2 TB veya daha fazla gerektirdiğinde, şablon her SQL Server sanal makinede bir depolama havuzu oluşturur. Bir depolama havuzu bir depolama sanallaştırma daha yüksek kapasite, dayanıklılık ve performans sağlamak için birden çok disk yapılandırıldığı biçimidir.  Şablon depolama havuzunda bir depolama alanı oluşturur ve işletim sisteminin tek bir veri diski sayısını gösterir. Şablon, bu disk için SQL Server veri diski olarak belirler. Şablon, aşağıdaki ayarları kullanarak, SQL Server için depolama havuzunu ayarlar:

* Stripe, sanal disk ayırma ayarını boyutudur. İşlem tabanlı iş yüklerinizi 64 KB'ı kullanın. Veri ambarı iş yükleriniz 256 KB'ı kullanın.
* Dayanıklılık basit (esnekliği yok).

> [!NOTE]
> Azure premium depolama, yerel olarak yedekli ve ek dayanıklılık depolama havuzu, gerekli olmamasını sağlayacak tek bir bölgede verilerin üç kopyasını tutar.
>
>

* Sütun sayısı depolama havuzundaki disklerin sayısına eşittir.

Depolama alanı ve depolama havuzları hakkında ek bilgi için bkz:

* [Depolama alanlarına genel bakış](https://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server Yedekleme ve depolama havuzları](https://technet.microsoft.com/library/dn390929.aspx)

SQL Server yapılandırma en iyi uygulamalar hakkında daha fazla bilgi için bkz. [performans Azure sanal makineler'de SQL Server için en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server ayarları
Üzerinde **SQL Server ayarları**gözden geçirin ve SQL Server sanal makine adı ön eki, SQL Server sürümü, SQL Server hizmet hesabı ve parola ve SQL otomatik düzeltme eki uygulama bakım zamanlamasını değiştirin.

* **SQL Server adı öneki** her SQL Server sanal makinesi için bir ad oluşturmak için kullanılır. Bu öğretici için kullanmak **sqlserver**. SQL Server sanal makineleri şablon adları *sqlserver-0* ve *sqlserver-1*.
* **SQL Server sürümü** SQL Server sürümüdür. Bu öğretici kullanılmak **SQL Server 2014**. Ayrıca seçebilirsiniz **SQL Server 2012** veya **SQL Server 2016**.
* **SQL Server hizmet hesabının kullanıcı adı** SQL Server hizmeti için etki alanı hesap adı. Bu öğretici için kullanmak **alındığını belirtir**.
* **Parola** SQL Server hizmet hesabı için parola.  Karmaşık bir parola kullanın. Parolayı onaylayın.
* **SQL otomatik düzeltme eki uygulama bakım zamanlaması** Azure SQL sunucuları otomatik olarak düzeltme ekleri haftanın gününü belirtir. Bu öğreticide, yazın **Pazar**.
* **SQL otomatik düzeltme eki uygulama bakım başlangıç saati** otomatik düzeltme eki uygulama başladığında Azure bölgesi için günün saatini olduğu.

> [!NOTE]
> Düzeltme eki uygulama penceresi her bir sanal makine için bir saat aşamalı. Hizmetleri kesintiye uğramasını engellemek için bir defada yalnızca bir sanal makine yama.
>
>

![SQL Server ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Ayarları gözden geçirin ve ardından **Tamam**.

### <a name="summary"></a>Özet
Özet sayfasında, ayarları Azure doğrular. Şablon da indirebilirsiniz. Özeti gözden geçirin. **Tamam** düğmesine tıklayın.

### <a name="buy"></a>Satın Al
Son bu dikey pencereyi içeren **kullanım koşullarını**, ve **gizlilik ilkesi**. Bu bilgileri gözden geçirin. Azure için hazır olduğunuzda, kullanılabilirlik grubu için gerekli kaynaklar sanal makineler ve diğer tüm oluşturmaya başlamak için tıklayın **Oluştur**.

Azure portalında kaynak grubunu ve tüm kaynakları oluşturur.

## <a name="monitor-deployment"></a>İzleyici dağıtım
Azure portalında dağıtımın ilerleme durumunu izleyin. Dağıtım temsil eden bir simge otomatik olarak Azure portalı panosuna sabitlenir.

![Azure Panosu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>SQL Server'a bağlanma
Yeni SQL Server örneklerini İnternet'e bağlı IP adreslerine sahip sanal makineler üzerinde çalışıyor. Doğrudan her SQL Server sanal makine için Uzak Masaüstü (RDP) yapabilirsiniz.

SQL Server için RDP için şu adımları izleyin:

1. Azure portal panosunda, dağıtım işleminin başarılı olduğunu doğrulayın.
2. Tıklayın **kaynakları**.
3. İçinde **kaynakları** dikey penceresinde tıklayın **sqlserver-0**, sanal makinelerin SQL Server çalıştıran bir bilgisayar adı.
4. Dikey penceresinde **sqlserver-0**, tıklayın **Connect**. Tarayıcınızı açın veya uzak bağlantı nesnesi kaydetmek isteyip istemediğinizi sorar. **Aç**'a tıklayın.
5. **Uzak Masaüstü Bağlantısı** bu uzak bağlantının yayımcısının tanımlanamıyor uyarabilir. **Bağlan**'a tıklayın.
6. Windows Güvenlik birincil etki alanı denetleyicisinin IP adresine bağlanmak için kimlik bilgilerinizi girmenizi ister. Tıklayın **başka bir hesap kullan**. İçin **kullanıcı adı**, türü **contoso\DomainAdmin**. Yönetici kullanıcı adı şablonda ayarlarken bu hesabı yapılandırılmış. Şablonu yapılandırırken seçtiğiniz karmaşık bir parola kullanın.
7. **Uzak Masaüstü** uzak bilgisayarda güvenlik sertifikasındaki sorunlar nedeniyle doğrulanamadı, uyarabilir. Güvenlik sertifikası adı gösterir. Öğreticiyi izlediyseniz, addır **sqlserver 0.contoso.com**. **Evet**'e tıklayın.

Şimdi, SQL Server sanal makinesi için RDP ile bağlanır. SQL Server Management Studio'yu açın, SQL Server'ın varsayılan örneğine bağlanın ve kullanılabilirlik grubu yapılandırıldığını doğrulayın.
