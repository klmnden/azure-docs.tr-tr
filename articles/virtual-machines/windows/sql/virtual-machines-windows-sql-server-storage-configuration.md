---
title: SQL Server VM'ler için depolama yapılandırması | Microsoft Docs
description: Bu konu, nasıl Azure depolama için SQL Server Vm'lerinin (Resource Manager dağıtım modeli) sağlama sırasında yapılandırır açıklar. Ayrıca, depolama, var olan SQL Server VM'ler için nasıl yapılandırabileceğiniz açıklanmaktadır.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: craigg
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/05/2017
ms.author: ninarn
ms.openlocfilehash: 21c8b955d48da03559097db93b2cb66029a203ec
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29399092"
---
# <a name="storage-configuration-for-sql-server-vms"></a>SQL Server VM'ler için depolama yapılandırması
Azure'da bir SQL Server sanal makine görüntüsü yapılandırdığınızda, Portal depolama yapılandırmanızı otomatikleştirmek için yardımcı olur. Bu, belirli bir performans gereksinimlerinizi en iyi duruma getirmek için bu depolama SQL Server için erişilebilir hale getirme ve bu yapılandırma VM, depolama ekleme içerir.

Bu konu, nasıl Azure depolama, SQL Server VM'ler için sağlama sırasında hem var olan VM'ler için yapılandırır açıklar. Bu yapılandırma dayanır [performansı en iyi uygulamaları](virtual-machines-windows-sql-performance.md) Azure SQL Server çalıştıran VM'ler için.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Otomatik depolama yapılandırma ayarlarını kullanmak için sanal makine aşağıdaki özelliklere gerektirir:

* İle sağlanan bir [SQL Server galeri görüntüsü](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).
* Kullanan [Resource Manager dağıtım modeli](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Kullanan [Premium depolama](../premium-storage.md).

## <a name="new-vms"></a>Yeni sanal makineleri
Aşağıdaki bölümlerde, yeni SQL Server sanal makineler için depolama alanının nasıl yapılandırılacağını açıklar.

### <a name="azure-portal"></a>Azure Portal
Bir SQL Server galeri görüntüsü kullanarak bir Azure VM sağlanırken, depolama, yeni VM için otomatik olarak yapılandırmak seçebilirsiniz. Depolama boyutu, performans sınırlarını ve iş yükü türünü belirtin. Aşağıdaki ekran görüntüsünde, sağlama SQL VM sırasında kullanılan depolama yapılandırma dikey gösterir.

![Sağlama sırasında SQL Server VM depolama yapılandırması](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Üzerinde yaptığınız seçimlere dayanarak, VM oluşturduktan sonra Azure aşağıdaki depolama yapılandırma görevleri gerçekleştirir:

* Oluşturur ve premium depolama veri diski sanal makineye iliştirir.
* SQL Server tarafından erişilebilir olması için veri diski yapılandırır.
* Belirtilen boyutu ve performans (IOPS ve üretilen iş) gereksinimlerine bağlı olarak bir depolama havuzu halinde veri diskleri yapılandırır.
* Depolama havuzu, sanal makinede yeni bir sürücü ilişkilendirir.
* (Veri ambarı hareket işleme veya genel), belirtilen iş yükü türüne bağlı olarak bu yeni bir sürücüye en iyi duruma getirir.

Azure depolama ayarlarını nasıl yapılandırır hakkında daha fazla ayrıntı için bkz: [depolama yapılandırma bölümü](#storage-configuration). Azure Portalı'nda bir SQL Server VM oluşturmak nasıl tam bir anlatım için bkz: [sağlama işlemi öğretici](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Kaynak Yönet şablonları
Aşağıdaki Resource Manager şablonları kullanırsanız, iki premium diskleri varsayılan olarak, hiçbir depolama havuzu yapılandırması ile ilişkilendirilir. Ancak, sanal makineye bağlı premium veri diski sayısı değiştirmek için bu şablonları özelleştirebilirsiniz.

* [Otomatik yedekleme ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Otomatik düzeltme eki uygulama ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [AKV tümleştirme ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Var olan sanal makineleri
Var olan SQL Server VM'ler için Azure Portalı'ndaki bazı depolama ayarlarını değiştirebilirsiniz. VM'yi seçin, Ayarlar alanına gidin ve ardından SQL Server yapılandırma seçin. SQL Server yapılandırma dikey penceresinde, VM geçerli depolama kullanımını gösterir. VM'ın mevcut tüm sürücülerde Bu grafikte görüntülenir. Her bir sürücü için depolama alanı dört bölümlerde görüntüler:

* SQL veri
* SQL günlüğü
* Diğer (SQL dışı depolama)
* Kullanılabilir

![Var olan SQL Server VM için depolama yapılandırma](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Yeni sürücü eklemek veya var olan sürücüsüne genişletmek için depolama yapılandırmak için grafiğin düzenleme bağlantısını tıklatın.

Bağlı olarak değişir gördüğünüz yapılandırma seçenekleri bu özelliği önce kullandınız. İlk kez kullanırken, depolama gereksinimleriniz için yeni bir sürücü belirtebilirsiniz. Bir sürücü oluşturmak için bu özelliği daha önce kullandıysanız, sürücünün depolama genişletmek seçebilirsiniz.

### <a name="use-for-the-first-time"></a>İlk kez kullanın
Bu özellik, ilk kez kullanıyorsanız, depolama boyutu ve performans sınırları için yeni bir sürücü belirtebilirsiniz. Bu deneyim, ne zaman sağlama sırasında gördüğünüz için benzer. İkisi arasındaki temel fark, iş yükü türünü belirtin izin verilmeyen ' dir. Bu kısıtlama, sanal makinede mevcut tüm SQL Server yapılandırmaları kesintiye engeller.

![SQL Server depolama kaydırıcılar yapılandırın](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure, belirtimlere bağlı yeni bir sürücüye oluşturur. Bu senaryoda, Azure aşağıdaki depolama yapılandırma görevleri gerçekleştirir:

* Oluşturur ve premium depolama veri diski sanal makineye iliştirir.
* SQL Server tarafından erişilebilir olması için veri diski yapılandırır.
* Belirtilen boyutu ve performans (IOPS ve üretilen iş) gereksinimlerine bağlı olarak bir depolama havuzu halinde veri diskleri yapılandırır.
* Depolama havuzu, sanal makinede yeni bir sürücü ilişkilendirir.

Azure depolama ayarlarını nasıl yapılandırır hakkında daha fazla ayrıntı için bkz: [depolama yapılandırma bölümü](#storage-configuration).

### <a name="add-a-new-drive"></a>Yeni bir sürücü ekleyin
SQL Server VM'nize depolama zaten yapılandırdıysanız, depolama alanını genişletmeyi iki yeni seçeneklerini getirir. İlk seçenek, VM performans düzeyini artırabilirsiniz yeni bir sürücüye eklemektir.

![Yeni bir sürücü için bir SQL VM ekleme](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Ancak, sürücü eklendikten sonra performans artışı elde etmek için bazı ek el ile yapılandırma gerçekleştirmelidir.

### <a name="extend-the-drive"></a>Sürücü genişletme
Depolama alanını genişletmeyi için diğer varolan sürücü genişletmek için seçenektir. Bu seçenek kullanılabilir depolama alanı sürücünüzün artırır ancak performans artırmaz. Depolama havuzu oluşturulduktan sonra depolama havuzlarıyla sütun sayısı değiştirilemiyor. Sütun sayısı bu veri disklere yayılarak paralel yazma sayısını belirler. Bu nedenle, olan ek veri disklerinin performans artırılamıyor. Bunlar yalnızca yazılan veriler için daha fazla depolama alanı sağlayabilir. Bu sınırlama sürücü genişletirken sütun sayısı ekleyebileceğiniz veri diskleri minimum sayısını belirler, anlamına gelir. Dört veri diskleri ile bir depolama havuzunda oluşturursanız, bu nedenle sütunları da dört sayısıdır. Depolama, genişletmek istediğiniz zaman, en az dört veri diskleri eklemeniz gerekir.

![Bir sürücü için bir SQL VM genişletme](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Depolama yapılandırması
Bu bölümde, Azure SQL VM sağlama veya Azure Portal'da yapılandırması sırasında otomatik olarak gerçekleştirir. depolama yapılandırma değişiklikleri için bir başvuru sağlar.

* Azure VM için depolama ikiden TBs seçtiyseniz, depolama havuzu oluşturmaz.
* VM için en az iki TBs depolama seçtiyseniz, Azure depolama havuzu yapılandırır. Bu konunun sonraki bölümlerinde, depolama havuzu yapılandırma ayrıntılarını sağlar.
* Her zaman otomatik depolama yapılandırmasını kullanan [premium depolama](../premium-storage.md) P30 veri diski. Sonuç olarak, seçili numaranızı terabayt ve, VM'ye bağlı veri diski sayısı 1:1 eşlemesini yoktur.

Fiyatlandırma bilgileri için bkz: [depolama fiyatlandırma](https://azure.microsoft.com/pricing/details/storage) sayfasında **Disk Depolama** sekmesi.

### <a name="creation-of-the-storage-pool"></a>Depolama havuzu oluşturma
Azure, SQL Server sanal makinelerin depolama havuzu oluşturmak için aşağıdaki ayarları kullanır.

| Ayar | Değer |
| --- | --- |
| Şerit boyutu |256 KB (veri ambarı); 64 KB (işlem) |
| Disk boyutları |1 TB |
| Önbellek |Okuma |
| Ayırma boyutu |64 KB NTFS ayırma birimi boyutu |
| Anında dosya başlatma |Etkin |
| Bellekteki sayfaları kilitle |Etkin |
| Kurtarma |Basit kurtarma (esnekliği yok) |
| Sütun sayısı |Veri diski sayısı<sup>1</sup> |
| TempDB konumu |Veri disklerinde depolanan<sup>2</sup> |

<sup>1</sup> depolama havuzu oluşturulduktan sonra depolama havuzundaki sütun sayısı değiştirilemiyor.

<sup>2</sup> Bu ayar yalnızca depolama yapılandırma özelliğini kullanarak oluşturduğunuz ilk sürücü için geçerlidir.

## <a name="workload-optimization-settings"></a>İş yükü iyileştirme ayarları
Aşağıdaki tabloda kullanılabilir üç iş yükü türü seçenekleri ve bunların karşılık gelen iyileştirmeler açıklanmaktadır:

| İş yükü türü | Açıklama | En iyi duruma getirme |
| --- | --- | --- |
| Genel |Çoğu iş yükünü destekler varsayılan ayarı |Hiçbiri |
| **İşlem işleme** |Depolama alanı geleneksel veritabanı OLTP iş yükleri için en iyi duruma getirir |İzleme bayrağı 1117<br/>İzleme bayrağı 1118 |
| **Veri ambarı** |Çözümleme ve raporlama iş yükleri için depolama en iyi duruma getirir |İzleme bayrağı 610<br/>İzleme bayrağı 1117 |

> [!NOTE]
> SQL sanal makinesi depolama yapılandırması adımda seçerek sağladığınızda, yalnızca iş yükü türünü belirtebilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
