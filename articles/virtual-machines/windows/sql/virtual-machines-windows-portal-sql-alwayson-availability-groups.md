---
title: Azure Resource Manager VM'ler için yüksek kullanılabilirliği ayarlamadan ayarlama | Microsoft Docs
description: Bu öğreticide Azure Resource Manager moduna Azure sanal makinelerinde ile Always On kullanılabilirlik grubu oluşturulacağını gösterir.
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
ms.openlocfilehash: a612ffd5a68e34cb0a367a6a883495ef26aeb4bc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29401030"
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Always On kullanılabilirlik grupları Azure sanal makineleri otomatik olarak yapılandırın: Kaynak Yöneticisi

Bu öğreticide Azure Resource Manager sanal makineleri kullanan bir SQL Server kullanılabilirlik grubu oluşturulacağını gösterir. Öğretici Azure dikey penceresi bir şablonu yapılandırmak için kullanır. Varsayılan ayarları gözden geçirin, gerekli ayarları yazın ve Portalı'nda Kanatlar, bu öğreticide yol gibi güncelleştirin.

Tam öğretici Azure sanal makinelerde aşağıdaki öğeleri içeren bir SQL Server kullanılabilirlik grubu oluşturur:

* Bir ön uç ve arka uç alt ağ dahil olmak üzere birden çok alt ağa sahip bir sanal ağ
* Bir Active Directory etki alanına sahip iki etki alanı denetleyicisi
* SQL Server çalıştıran ve arka uç alt ağ için dağıtılmış ve Active Directory etki alanına katılan iki sanal makineler
* Düğüm çoğunluğu çekirdek modeli ile üç düğümlü yük devretme kümesi
* Bir kullanılabilirlik veritabanının iki synchronous-commit çoğaltmalarından sahip bir kullanılabilirlik grubu

Aşağıdaki çizimde eksiksiz çözüm temsil eder.

![Test laboratuvarı mimarisi Azure içindeki kullanılabilirlik grupları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Bu çözümdeki tüm kaynakları tek bir kaynak grubuna ait.

Bu öğretici başlamadan önce aşağıdakileri doğrulayın:

* Bir Azure hesabı zaten var. Biri, yoksa [bir deneme hesabı için kaydolun](http://azure.microsoft.com/pricing/free-trial/).
* Sanal makine Galeriden bir SQL Server sanal makine sağlamak için GUI'yi kullanmak nasıl zaten biliyor. Daha fazla bilgi için bkz: [Azure üzerinde bir SQL Server sanal makinenin sağlanması](virtual-machines-windows-portal-sql-server-provision.md).
* Kullanılabilirlik grupları düz bir anlayış zaten var. Daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Kullanılabilirlik grupları ile SharePoint kullanarak ilgileniyorsanız, ayrıca bkz. [yapılandırmak SQL Server 2012 Always On kullanılabilirlik grupları SharePoint 2013 için](http://technet.microsoft.com/library/jj715261.aspx).
>
>

Bu öğreticide, Azure portalında kullanın:

* Her zaman açık şablonu portaldan seçin.
* Şablon ayarları gözden geçirin ve ortamınız için birkaç yapılandırma ayarlarını güncelleştirin.
* Tüm ortamı oluşturur gibi Azure izleyin.
* Bir etki alanı denetleyicisi ve ardından SQL Server çalıştıran bir sunucuya bağlanın.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Galeri kümeden sağlama
Azure galeri görüntüsü için tüm çözümü sağlar. Şablon bulmak için:

1. Hesabınızı kullanarak Azure portalında oturum açın.
2. Azure portalında tıklatın **kaynak oluşturma** açmak için **yeni** bölmesi.
3. Üzerinde **yeni** bölmesinde, arama **AlwaysOn**.
   ![AlwaysOn şablonunu bulun](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Arama sonuçlarında bulun **SQL Server AlwaysOn kümesi**.
   ![AlwaysOn şablonu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Üzerinde **dağıtım modeli seçin**, seçin **Resource Manager**.

### <a name="basics"></a>Temel Bilgiler
Tıklatın **Temelleri** ve aşağıdaki ayarları yapılandırın:

* **Yönetici kullanıcı adı** etki alanı yönetici izinlerine sahip bir kullanıcı hesabı ve her iki SQL Server örnekleri üzerinde SQL Server sysadmin sabit sunucu rolünün bir üyesidir. Bu öğretici için kullanmak **DomainAdmin**.
* **Parola** etki alanı yöneticisi hesabı için parola. Karmaşık bir parola kullanın. Parolayı onaylayın.
* **Abonelik** aboneliğin kullanılabilirlik grubu için tüm dağıtılan kaynakları çalıştırmak için Azure o fatura olduğu. Hesabınızın birden çok aboneliğiniz varsa, farklı bir abonelik belirtebilirsiniz.
* **Kaynak grubu** bu şablonla oluşturulan tüm Azure kaynaklarına ait olduğu grubu için bir ad değil. Bu öğretici için kullanmak **SQL-HA-RG**. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Konum** öğretici oluşturur kaynakları Azure bölgesi. Bir Azure bölgesi seçin.

Aşağıdaki ekran görüntüsünde bir tamamlanmış olan **Temelleri** dikey penceresinde:

![Temel Bilgiler](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

**Tamam**’a tıklayın.

### <a name="domain-and-network-settings"></a>Etki alanı ve ağ ayarları
Bir etki alanı ve etki alanı denetleyicileri bu Azure galerisinde şablon oluşturur. Ayrıca, bir ağ ve iki alt ağı oluşturur. Şablon sunucuları bir var olan etki alanı ya da sanal ağ oluşturulamıyor. Sonraki adım etki alanı ve ağ ayarlarını yapılandırır.

Üzerinde **etki alanı ve ağ ayarlarını** dikey penceresinde, etki alanı için hazır değerleri inceleyin ve ağ ayarları:

* **Orman kök etki alanı adı** küme barındıran Active Directory etki alanı için etki alanı adıdır. Öğretici için kullanmak **contoso.com**.
* **Sanal ağ adı** Azure sanal ağı için ağ adıdır. Öğretici için kullanmak **autohaVNET**.
* **Etki alanı denetleyicisi alt ağ adı** bir kısmı, etki alanı denetleyicisi barındıran sanal ağın adı. Kullanım **alt ağ 1**. Bu alt ağ adresi öneki kullanan **10.0.0.0/24**.
* **SQL Server alt ağ adı** çalışma SQL Server ve dosya paylaşımı tanığı sunucularını barındıran sanal ağ bir kısmı adıdır. Kullanım **alt ağ-2**. Bu alt ağ adresi öneki kullanan **10.0.1.0/26**.

Azure'da sanal ağlar hakkında daha fazla bilgi için bkz: [sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md).  

**Etki alanı ve ağ ayarlarını** aşağıdaki ekran görüntüsüne görünmelidir:

![Etki alanı ve ağ ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğretici için hazır değerleri kullanın.

Ayarları gözden geçirin ve ardından **Tamam**.

### <a name="availability-group-settings"></a>Kullanılabilirlik grubu ayarları
Üzerinde **kullanılabilirlik grubu ayarlarını**, kullanılabilirlik grubu dinleyicisi için hazır değerleri inceleyin.

* **Kullanılabilirlik grubu adının** kullanılabilirlik grubu için kümelenmiş bir kaynak adı. Bu öğretici için kullanmak **Contoso ag**.
* **Kullanılabilirlik grubu dinleyici adı** küme ve iç yük dengeleyicisi tarafından kullanılır. SQL Server'a bağlanan istemciler, veritabanının uygun çoğaltmaya bağlanmak için bu adı kullanabilirsiniz. Bu öğretici için kullanmak **Contoso dinleyici**.
* **Kullanılabilirlik grubu dinleyicisinin bağlantı noktası** SQL Server dinleyicisi TCP bağlantı noktasını belirtir. Bu öğretici için varsayılan bağlantı noktasını kullanan **1433**.

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğretici için hazır değerleri kullanın.  

![Kullanılabilirlik grubu ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

**Tamam**’a tıklayın.

### <a name="virtual-machine-size-storage-settings"></a>Sanal makine boyutu, depolama ayarları
Üzerinde **VM boyutu, depolama ayarlarını**, bir SQL Server sanal makine boyutu seçin ve diğer ayarları gözden geçirin.

* **SQL Server sanal makine boyutu** SQL Server çalıştıran her iki sanal makine için boyutu. İş yükü için uygun sanal makine boyutu seçin. Bu ortamda öğretici için oluşturuluyorsa, kullanın **DS2**. Üretim iş yükleri için iş yükü destekleyebilir bir sanal makine boyutu seçin. Birçok üretim iş yükleri gerektiren **DS4** veya daha büyük. Şablon bu boyuttaki iki sanal makine oluşturur ve her biri üzerinde SQL Server'ı yükler. Daha fazla bilgi için bkz: [sanal makineler için Boyutlar](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure SQL Server Enterprise sürümü yükler. Maliyet edition ve sanal makine boyutuna bağlıdır. Geçerli maliyetler hakkında ayrıntılı bilgi için bkz: [sanal makineler fiyatlandırma](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Etki alanı denetleyicisi sanal makine boyutu** etki alanı denetleyicileri için sanal makine boyutu. Bu öğretici kullanmak için **D2**.
* **Dosya paylaşımı tanığı sanal makine boyutu** dosya paylaşım tanığı için sanal makine boyutudur. Bu öğretici için kullanmak **A1**.
* **SQL depolama hesabı** işletim sistemi disklerinde ve SQL Server verilerini tutan depolama hesabının adıdır. Bu öğretici için kullanmak **alwaysonsql01**.
* **DC depolama hesabı** depolama hesabı adı etki alanı denetleyicileri. Bu öğretici için kullanmak **alwaysondc01**.
* **SQL Server verilerini disk boyutu** TB TB SQL Server veri diski boyutudur. 1 ile 4 arasında bir sayı belirtin. Bu öğretici için kullanmak **1**.
* **Depolama İyileştirme** belirli depolama yapılandırma ayarları iş yükü türüne göre SQL Server sanal makineler için ayarlar. Bu senaryoda tüm SQL Server sanal makineleri salt okunur ayarlamak Azure disk konak önbelleği premium depolama kullanın. Ayrıca, bu üç ayarlarından birini seçerek iş yükü için SQL Server ayarları iyileştirebilirsiniz:

  * **Genel iş yükü** belirli yapılandırma ayarları yok ayarlar.
  * **İşlem işleme** kümeleri izleme bayrağı 1117 ve 1118.
  * **Veri ambarı** kümeleri izleme bayrağı 1117 ve 610.

Bu öğretici için kullanmak **genel iş yükü**.

![VM boyutu depolama ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Ayarları gözden geçirin ve ardından **Tamam**.

#### <a name="a-note-about-storage"></a>Depolama hakkında bir Not
Ek iyileştirmeler SQL Server veri diskleri boyutuna göre değişir. Her veri diski terabayt için Azure ek 1 TB premium depolama alanı ekler. Bir sunucu 2 TB veya daha fazla gerektirdiğinde, şablon her SQL Server sanal makinede bir depolama havuzu oluşturur. Bir depolama havuzu depolama sanallaştırma birden çok diske daha yüksek kapasite, esneklik ve performans sağlamak üzere yapılandırıldığı biçimidir.  Şablon depolama havuzu üzerinde bir depolama alanı oluşturur ve işletim sistemi için bir tek veri diski gösterir. Şablon bu disk için SQL Server veri diski olarak belirler. Şablon, aşağıdaki ayarları kullanarak, SQL Server için depolama havuzu ayarladığını:

* Şerit boyutu sanal disk için ayırma ayardır. İşlem iş yükleri 64 KB kullanır. Veri ambarı iş yükleri 256 KB kullanın.
* Dayanıklılık basit (esnekliği yok).

> [!NOTE]
> Azure premium storage yerel olarak yedekli ve depolama havuzuna en fazla esneklik gerekli olmamasını sağlayacak şekilde tek bir bölge içinde verileri üç kopyasını tutar.
>
>

* Sütun sayısı depolama havuzundaki disklerin sayısına eşittir.

Depolama alanı ve depolama havuzları hakkında ek bilgi için bkz:

* [Depolama alanlarına genel bakış](http://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server Yedekleme ve depolama havuzları](http://technet.microsoft.com/library/dn390929.aspx)

SQL Server yapılandırma en iyi uygulamalar hakkında daha fazla bilgi için bkz: [performans Azure sanal makinelerde SQL Server için en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server ayarları
Üzerinde **SQL Server ayarları**, gözden geçirin ve SQL Server sanal makine adı ön eki, SQL Server sürümü, SQL Server hizmet hesabı ve parolası ve SQL otomatik düzeltme eki uygulama bakım zamanlaması değiştirin.

* **SQL Server adı öneki** her SQL Server sanal makine için bir ad oluşturmak için kullanılır. Bu öğretici için kullanmak **sqlserver**. SQL Server sanal makineleri şablon adları *sqlserver-0* ve *sqlserver-1*.
* **SQL Server sürümü** SQL Server sürümüdür. Bu öğretici kullanmak için **SQL Server 2014**. Ayrıca seçebilirsiniz **SQL Server 2012** veya **SQL Server 2016**.
* **SQL Server hizmet hesabının kullanıcı adı** SQL Server hizmeti için etki alanı hesap adı. Bu öğretici için kullanmak **SQL hizmet**.
* **Parola** SQL Server hizmet hesabı için parola.  Karmaşık bir parola kullanın. Parolayı onaylayın.
* **SQL otomatik düzeltme eki uygulama bakım zamanlaması** Azure SQL sunucuları otomatik olarak düzeltme ekleri haftanın gününü tanımlar. Bu öğretici için yazın **Pazar**.
* **SQL otomatik düzeltme eki uygulama bakım başlangıç saati** otomatik düzeltme eki uygulama başladığında Azure bölgesi için günün saatini değil.

> [!NOTE]
> Her sanal makine için düzeltme eki uygulama penceresi bir saate göre aşamalı. Yalnızca bir sanal makine, hizmetlerin kesilmesini önlemek için aynı anda düzeltme eki.
>
>

![SQL Server ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Ayarları gözden geçirin ve ardından **Tamam**.

### <a name="summary"></a>Özet
Özet sayfasında, Azure ayarlarını doğrular. Ayrıca, şablonu indirebilirsiniz. Özeti gözden geçirin. **Tamam**’a tıklayın.

### <a name="buy"></a>Satın Al
Bu son dikey içeren **kullanım koşulları**, ve **gizlilik ilkesi**. Bu bilgileri gözden geçirin. Azure için hazır olduğunuzda sanal makinelerin ve diğer tüm oluşturmaya başlamak için kullanılabilirlik grubu için kaynaklar gerekli, tıklatın **oluşturma**.

Azure portalı, kaynak grubunu ve tüm kaynaklar oluşturur.

## <a name="monitor-deployment"></a>İzleyici dağıtım
Azure portalından dağıtımının ilerleme durumunu izleyin. Dağıtım temsil eden bir simge otomatik olarak Azure portal panosuna sabitlendi.

![Azure Panosu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>SQL Server'a bağlanma
Yeni SQL Server örneği internet'e bağlı IP adresine sahip sanal makinelerde çalışmıyor. Her SQL Server sanal makine için doğrudan Uzak Masaüstü (RDP) yapabilirsiniz.

Bir SQL Server için RDP için şu adımları izleyin:

1. Azure portal panosunda, dağıtımın başarılı olduğunu doğrulayın.
2. Tıklatın **kaynakları**.
3. İçinde **kaynakları** dikey penceresinde tıklatın **sqlserver-0**, sanal makinelerin SQL Server çalıştıran bir bilgisayar adı.
4. İçin dikey **sqlserver-0**, tıklatın **Bağlan**. Tarayıcınızı açın veya uzak bağlantı nesnesi kaydetmek isteyip istemediğinizi sorar. Tıklatın **açık**.
5. **Uzak Masaüstü Bağlantısı** bu uzak bağlantının yayımcısının tanımlanamıyor uyarabilir. **Bağlan**'a tıklayın.
6. Windows güvenliği birincil etki alanı denetleyicisinin IP adresine bağlanmak için kimlik bilgilerinizi girmenizi ister. Tıklatın **başka bir hesap kullan**. İçin **kullanıcı adı**, türü **contoso\DomainAdmin**. Şablonda yönetici kullanıcı adı olarak ayarladığınızda bu hesabı yapılandırılmış. Şablon yapılandırdığınızda seçtiğiniz karmaşık parola kullanın.
7. **Uzak Masaüstü** , uzak bilgisayarın kendi güvenlik sertifikasıyla ilgili sorunlar nedeniyle doğrulanamadı, sizi uyarabilir. Güvenlik sertifikası adı gösterir. Öğretici izlediyseniz addır **sqlserver 0.contoso.com**. Tıklatın **Evet**.

Şimdi, SQL Server sanal makine için RDP ile bağlanır. SQL Server Management Studio'yu açın, SQL Server'ın varsayılan örneğine bağlanın ve kullanılabilirlik grubu yapılandırıldığından emin olun.
