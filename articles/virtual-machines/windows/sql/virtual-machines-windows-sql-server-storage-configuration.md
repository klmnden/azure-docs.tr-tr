---
title: SQL Server Vm'leri için depolama yapılandırması | Microsoft Docs
description: Bu konuda, nasıl Azure depolama için SQL Server Vm'leri (Resource Manager dağıtım modeli) sağlama sırasında yapılandırır açıklanmaktadır. Ayrıca, depolama, mevcut SQL Server Vm'leri için nasıl yapılandırabileceğinizi açıklar.
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
ms.openlocfilehash: 360ffb3d2c682d6bd2344cb3ae95447ff3df278d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076851"
---
# <a name="storage-configuration-for-sql-server-vms"></a>SQL Server Vm'leri için depolama yapılandırması

Azure'da bir SQL Server sanal makine görüntüsü yapılandırdığınızda, portalı kullanarak depolama yapılandırmasını otomatik hale getirmek için yardımcı olur. Bu, belirli bir performans gereksinimleriniz için en iyi duruma getirmek için SQL Server, depolama üstlenir ve yapılandırarak VM, depolama eklemeyi içerir.

Bu konuda, nasıl Azure depolama, SQL Server Vm'leri için sağlama sırasında ve var olan VM'ler için yapılandırır açıklanmaktadır. Bu yapılandırma dayanır [en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md) Azure SQL Server çalıştıran VM'ler için.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar

Otomatik depolama yapılandırması ayarlarını kullanmak için sanal makine aşağıdaki özelliklere gerektirir:

* İle sağlanan bir [SQL Server galeri görüntüsü](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).
* Kullanan [Resource Manager dağıtım modeli](../../../azure-resource-manager/resource-manager-deployment-model.md).
* Kullanan [premium SSD](../disks-types.md).

## <a name="new-vms"></a>Yeni VM'ler

Aşağıdaki bölümlerde, yeni SQL Server sanal makineleri için depolamayı yapılandırma açıklanmaktadır.

### <a name="azure-portal"></a>Azure portal

Bir SQL Server galeri görüntüsü kullanarak bir Azure VM'si sağlanırken depolama yeni VM'niz için otomatik olarak yapılandırmak seçebilirsiniz. Depolama boyutu, performans sınırlarını ve iş yükü türü belirtin. Aşağıdaki ekran görüntüsünde, sağlama sırasında SQL VM kullanılan depolama yapılandırma dikey pencerede gösterilir.

![Sağlama sırasında SQL Server VM depolama yapılandırması](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Yaptığınız seçimlere göre bağlı olarak, VM oluşturduktan sonra Azure aşağıdaki depolama yapılandırması görevleri gerçekleştirir:

* Oluşturur ve premium SSD sanal makineye ekler.
* Veri diskleri için SQL Server erişilebilir olacak şekilde yapılandırır.
* (Belirtilen boyut ve performans IOPS ve aktarım hızı) gereksinimlerine bağlı olarak bir depolama havuzu halinde veri disklerini yapılandırır.
* Depolama havuzu, sanal makinede yeni bir sürücüye ilişkilendirir.
* (Veri ambarı, işlem işleme veya genel), belirtilen iş yükü türüne bağlı olarak bu yeni bir sürücüye iyileştirir.

Azure depolama ayarlarını nasıl yapılandırdığını hakkında daha fazla bilgi için bkz [depolama yapılandırma bölümü](#storage-configuration). Azure portalında bir SQL Server VM'si oluşturma tam bir kılavuza bakın [sağlama işlemi öğretici](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Kaynak yönetme şablonları

Aşağıdaki Resource Manager şablonları kullanıyorsanız, varsayılan olarak hiçbir depolama havuzu yapılandırması ile iki premium veri diskleri eklenir. Ancak, sanal makineye bağlı premium diskleri sayısını değiştirmek için bu şablonları özelleştirebilirsiniz.

* [Otomatik yedekleme ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Otomatik düzeltme eki uygulama ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [AKV tümleştirme ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Varolan Vm'leri

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server Vm'leri için Azure portalında bazı depolama ayarlarını değiştirebilirsiniz. Açık, [SQL sanal makineleri kaynak](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource)seçip **genel bakış**. SQL Server genel bakış sayfasında, sanal makinenizin geçerli depolama kullanımını gösterir. Vm'nizdeki mevcut tüm sürücüleri bu grafikte görüntülenir. Her bir sürücü için depolama alanı dört bölümde görüntüler:

* SQL verileri
* SQL günlüğü
* Diğer (SQL olmayan depolama)
* Kullanılabilir

Depolama ayarlarını değiştirmek için seçin **yapılandırma** altında **ayarları**. 

![Mevcut SQL Server sanal makinesi için depolama alanını yapılandırma](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Bağlı olarak değişir gördüğünüz yapılandırma seçenekleri bu özelliği önce kullandınız. İlk kez kullanırken, depolama gereksinimleriniz için yeni bir sürücü belirtebilirsiniz. Bir sürücü oluşturmak için bu özelliği daha önce kullandıysanız, sürücünün depolama genişletmek seçebilirsiniz.

### <a name="use-for-the-first-time"></a>İlk kez kullanın

Bu özellik, ilk kez kullanıyorsanız, depolama boyutu ve performansı sınırları için yeni bir sürücü belirtebilirsiniz. Bu deneyim, ne zaman sağlama sırasında gördüklerinizle için benzerdir. İkisi arasındaki temel fark, iş yükü türünü belirtmek izin verilmeyen ' dir. Bu kısıtlama, sanal makinedeki tüm mevcut SQL Server yapılandırmaları kesintiye engeller.

Azure, belirtimlerinden yola çıkarak yeni bir sürücüye oluşturur. Bu senaryoda, Azure depolama yapılandırması aşağıdaki görevleri gerçekleştirir:

* Oluşturur ve premium depolama diskleri sanal makineye ekler.
* Veri diskleri için SQL Server erişilebilir olacak şekilde yapılandırır.
* (Belirtilen boyut ve performans IOPS ve aktarım hızı) gereksinimlerine bağlı olarak bir depolama havuzu halinde veri disklerini yapılandırır.
* Depolama havuzu, sanal makinede yeni bir sürücüye ilişkilendirir.

Azure depolama ayarlarını nasıl yapılandırdığını hakkında daha fazla bilgi için bkz [depolama yapılandırma bölümü](#storage-configuration).

### <a name="add-a-new-drive"></a>Yeni sürücü Ekle

SQL Server sanal makinenizde depolama zaten yapılandırdıysanız, depolama alanını genişletmeyi iki yeni seçeneklerini sunar. İlk seçenek, sanal makinenizin performans düzeyini artırabilir yeni bir sürücüye eklemektir.

Ancak sürücüsü ekledikten sonra performans artışı elde etmek için bazı ek el ile yapılandırma gerçekleştirmelidir.

### <a name="extend-the-drive"></a>Sürücüyü Genişlet

Depolama alanını genişletmeyi için diğer bir seçenek var olan sürücüyü Genişlet oluşturmaktır. Kullanılabilir depolama alanına sürücünüz için bu seçeneği artırır, ancak performans artırmaz. Depolama havuzu oluşturulduktan sonra depolama havuzları ile sütun sayısı değiştirilemiyor. Sütun sayısı, veri disklere yayılarak paralel yazma sayısını belirler. Bu nedenle, eklenen veri diskleri performans artıramıyor. Bunlar yalnızca yazılan veriler için daha fazla depolama alanı sağlayabilir. Bu sınırlama sürücü genişletirken, sütun sayısı ekleyebileceğiniz veri diskleri en az sayısını belirler, anlamına gelir. Dört veri diskine bir depolama havuzu oluşturursanız, bu nedenle sütun sayısını da dörttür. Depolama genişletmek her zaman en az dört veri diskine eklemeniz gerekir.

![Bir SQL VM için bir sürücüyü Genişlet](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Depolama yapılandırması

Bu bölümde, Azure SQL VM sağlama veya Azure portalında yapılandırma sırasında otomatik olarak gerçekleştirir. depolama yapılandırma değişikliklerinin bir başvuru sağlar.

* Azure, sanal makinenizin ikiden az TB'a kadar depolama seçtiyseniz, bir depolama havuzu oluşturmaz.
* Azure, sanal Makineniz için en az iki TB'a kadar depolama seçtiyseniz, bir depolama havuzu yapılandırır. Bu konunun sonraki bölümünde, depolama havuzu yapılandırma ayrıntılarını sağlar.
* Her zaman otomatik depolama yapılandırması kullanan [premium SSD](../disks-types.md) P30 veri diskleri. Sonuç olarak, seçili terabayt sayısı, VM'ye veri diski sayısı arasında bir 1:1 eşleme vardır.

Fiyatlandırma bilgileri için bkz: [depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage) sayfasında **Disk Depolama** sekmesi.

### <a name="creation-of-the-storage-pool"></a>Depolama havuzu oluşturma

Azure, SQL Server Vm'leri üzerinde depolama havuzu oluşturmak için aşağıdaki ayarları kullanır.

| Ayar | Değer |
| --- | --- |
| Stripe boyutu |(Veri ambarlama); 256 KB 64 KB (işlem) |
| Disk boyutları |1 TB |
| Önbellek |Okuma |
| Ayırma boyutu |64 KB NTFS ayırma birimi boyutu |
| Anında dosya başlatma |Enabled |
| Sayfaları bellekte Kilitle |Enabled |
| Kurtarma |Basit kurtarma (esnekliği yok) |
| Sütun sayısı |Veri diski sayısı<sup>1</sup> |
| TempDB konumu |Veri disklerinde depolanan<sup>2</sup> |

<sup>1</sup> depolama havuzu oluşturulduktan sonra depolama havuzundaki sütun sayısı değiştirilemiyor.

<sup>2</sup> bu ayarı yalnızca depolama yapılandırma özelliğini kullanarak oluşturduğunuz ilk sürücü için geçerlidir.

## <a name="workload-optimization-settings"></a>İş yükü iyileştirme ayarları

Aşağıdaki tabloda kullanılabilir üç iş yükü türü seçenekleri ve bunların karşılık gelen iyileştirmeler açıklanmaktadır:

| İş yükü türü | Açıklama | En iyi duruma getirme |
| --- | --- | --- |
| **Genel** |Varsayılan ayar, iş yüklerinin çoğu destekler |None |
| **İşlem tabanlı işleme** |Depolama, geleneksel veritabanı OLTP iş yükleri için en iyi duruma getirir |İzleme bayrağı 1117<br/>İzleme bayrağı 1118 |
| **Veri ambarı** |Depolama, analiz ve raporlama iş yükleri için en iyi duruma getirir |İzleme bayrağı 610<br/>İzleme bayrağı 1117 |

> [!NOTE]
> Depolama yapılandırması adımda seçerek SQL sanal makinesi sağladığınızda, yalnızca iş yükü türünü belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
