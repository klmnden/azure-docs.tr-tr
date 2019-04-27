---
title: SQL Server Linux Azure sanal makineler SSS | Microsoft Docs
description: Bu makalede, SQL Server Linux Azure VM üzerinde çalıştırmayla ilgili sık sorulan soruların yanıtlarını sağlar.
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 12/13/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: fb1cafcf9405576749ea91aeea033c6ee783a026
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60739276"
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Linux Azure sanal Makineler'de sık sorulan sorular için SQL Server

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

Bu makalede çalıştırma hakkında en yaygın soruların yanıtları sağlanır [Linux Azure sanal Makineler'de SQL Server](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Bu makalede Linux sanal makinelerinde SQL Server'a özgü sorunları ele alınmaktadır. Windows sanal makinelerinde SQL Server çalıştırıyorsanız bkz [Windows SSS](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Görüntüleri

1. **Hangi SQL Server sanal makine galeri görüntüleri kullanılabilir mi?**

   Azure sanal makine görüntüleri tüm sürümleri üzerinde SQL Server'ın tüm desteklenen ana sürümler için hem Linux hem de Windows için korur. Daha fazla bilgi için tam listesi görmek [Linux VM görüntüleri](sql-server-linux-virtual-machines-overview.md#create) ve [Windows VM görüntüleri](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

1. **Mevcut SQL Server sanal makine galeri görüntüleri güncelleştirilir mi?**

   Her iki ayda, SQL Server görüntüleri sanal makine galerisindeki en son Linux ile güncelleştirilir ve Windows güncelleştirir. Linux görüntüleri için bu sistem güncelleştirmeleri içerir. Windows görüntüleri için bu Windows güncelleştirme önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri dahil olmak üzere, önemli olarak işaretlenmiş tüm güncelleştirmeleri içerir. SQL Server toplu güncelleştirmeler, Linux ve Windows için farklı şekilde ele alınır. Linux için SQL Server toplu güncelleştirmeleri ayrıca yenileme dahil edilir. Ancak şu anda SQL Server veya Windows Server toplu güncelleştirmeleri ile Windows Vm'leri güncelleştirilmez.

1. **Hangi SQL Server paketleri de yüklü ilgili?**

   SQL Server Linux Vm'leri varsayılan olarak yüklü olan SQL Server paketleri için bkz [yüklü paketleri](sql-server-linux-virtual-machines-overview.md#packages).

1. **Galeriden bir SQL Server sanal makine görüntüleri kaldırılmaları?**

   Evet. Azure, yalnızca tek bir görüntü ana sürüm ve sürüm bazında tutar. Örneğin, yeni bir SQL Server hizmet paketi yayımlandığında, Azure galeri için hizmet paketi için yeni bir görüntü ekler. SQL Server görüntüsü önceki hizmet paketi için hemen Azure portalından kaldırılır. Ancak, sonraki üç ay için Powershell'den sağlamak için kullanılabilir durumda kalır. Üç ay sonra önceki hizmet paketi görüntü artık kullanılamıyor. Yaşam döngüsü sonuna ulaştığında bir SQL Server sürümü desteklenmeyen hale gelirse, bu kaldırma İlkesi de geçerli.

## <a name="creation"></a>Oluşturma

1. **Bir Linux Azure sanal makinesinde SQL Server ile nasıl oluşturabilirim?**

   SQL Server içeren bir Linux sanal makine oluşturmak için en kolay çözümdür bakın. Azure'a kaydolma ve Portalı'ndan bir SQL VM oluşturmaya ilişkin öğretici için bkz: [Azure portalında bir Linux SQL Server sanal makinesi sağlama](provision-sql-server-linux-virtual-machine.md). Ayrıca, el ile SQL Server üzerinde bir VM ile ücretsiz lisanslı bir sürüme (Geliştirici veya Express) yükleme ya da şirket içi lisans yeniden kullanma seçeneğiniz de vardır. Kendi lisansınızı getirirseniz, olmalıdır [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility).

1. **Neden bir RHEL veya SLES SQL Server VM'si bir harcama sınırına sahip bir Azure aboneliği ile sağlayabilirim olamaz?**

   RHEL ve SLES sanal makineleri, harcama limiti olmayan ve doğrulanmış ödeme yöntemi (genellikle bir kredi kartı) abonelikle ilişkili olan bir abonelik gerektirir. Harcama limitini kaldırmadan RHEL ya da SLES VM sağlama, aboneliğiniz devre dışı ve tüm VM'ler/Hizmetleri durduruldu. Bu duruma aboneliği yeniden etkinleştirmek için çalıştırırsanız [harcama limitini Kaldır](https://account.windowsazure.com/subscriptions). Kalan kredileriniz geçerli fatura dönemi için geri yüklenir, ancak yeniden başlatın ve çalıştırmaya devam seçerseniz bir RHEL veya SLES VM görüntüsü ek maliyeti kredi kartınızdan geçer.

## <a name="licensing"></a>Lisanslama

1. **Lisanslı SQL Server kopyamı bir Azure sanal makinesine nasıl yükleyebilirim?**

   İlk olarak, bir Linux işletim sistemi yalnızca sanal makine oluşturun. Ardından çalıştırın [SQL Server yükleme adımlarını](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) Linux dağıtımınız için. Ayrıca ücretsiz lisanslı SQL Server sürümleri birini yüklemekte olduğunuz sürece, bir SQL Server Lisansı olmalıdır ve [azure'de Yazılım Güvencesiyle lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/).

1. **Getir-kendi lisansını (KLG) Linux sanal makine görüntüleri için SQL Server vardır?**

   Şu anda hiçbir BYOL Linux sanal makine görüntüleri için SQL Server vardır. Ancak, el ile SQL Server yalnızca Linux VM üzerinde önceki sorulara anlatıldığı gibi yükleyebilirsiniz.

1. **Kullandıkça ödeme galeri görüntülerinden biri kullanılarak oluşturulan sanal makineyi kendi SQL Server lisansımı kullanmak için değiştirebilir miyim?**

   Hayır. Kendi lisansınızı kullanarak için saniye başına ödeme lisanslama geçiş yapamazsınız. Yeni bir Linux VM oluşturma, SQL Server yükleme ve verilerinizi taşımanız gerekir. Kendi lisansınızı getirmek hakkında daha fazla ayrıntı için önceki soruda bakın.

## <a name="administration"></a>Yönetim

1. **Bir Linux SQL Server sanal makinesi SQL Server Management Studio (SSMS) yönetebilirim?**

   Evet, ancak SSMS şu anda yalnızca Windows aracı. SSMS ile Linux SQL Server Vm'leri kullanmak için bir Windows makineden uzaktan bağlanmanız gerekir. Yerel olarak Linux'ta yeni [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) aracı, çoğu yönetim görevlerini gerçekleştirebilir. Platformlar arası veritabanı yönetim aracı için bkz: [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is).

1. **SQL Server'ı SQL VM'sinden tamamen kaldırabilir miyim?**

   Evet, ancak bölümünde anlatıldığı gibi SQL sanal Makineniz için ödeme yapmaya devam edecek [SQL Server Azure Vm'leri için fiyatlandırma Kılavuzu](../../windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md?toc=%2fazure%2fvirtual-machines%2flinux%2fsql%2ftoc.json). Artık SQL Server'a ihtiyacınız kalmadıysa yeni bir sanal makine dağıtabilir ve verilerle uygulamaları yeni sanal makineye geçirebilirsiniz. Ardından SQL Server sanal makinesini kaldırabilirsiniz.

## <a name="updating-and-patching"></a>Güncelleştirme ve düzeltme eki uygulama

1. **Azure VM'de SQL Server'ın yeni bir sürümü/yayını için nasıl yükseltebilirim?**

   Şu anda bir Azure sanal makinesinde çalışan SQL Server için yerinde yükseltme mevcut değildir. İstediğiniz SQL Server sürümü ile yeni bir Azure sanal makinesi oluşturun ve ardından [standart veri geçirme tekniklerini](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview) kullanarak veritabanlarınızı yeni sunucuya geçirin.

## <a name="general"></a>Genel

1. **Azure Vm'lerinde SQL Server yüksek kullanılabilirlik çözümleri destekleniyor mu?**

   Şu anda değil. Always On kullanılabilirlik grupları hem de Yük Devretme Kümelemesi Pacemaker gibi Linux'ta bir küme çözümü gerektirir. SQL Server için desteklenen Linux dağıtımları, bulutta, yüksek kullanılabilirlik eklentileri desteklemez.

## <a name="resources"></a>Kaynaklar

**Linux Vm'leri**:

* [Bir Linux sanal makinesinde SQL Server'a genel bakış](sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux VM'si sağlama](provision-sql-server-linux-virtual-machine.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows Vm'leri**:

* [Bir Windows VM üzerinde SQL Server'a genel bakış](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Bir SQL Server Windows VM sağlama](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [SSS (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)