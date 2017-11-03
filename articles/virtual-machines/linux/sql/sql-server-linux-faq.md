---
title: SQL Server Linux Azure sanal makinelerinde SSS | Microsoft Docs
description: "Bu makalede, Linux Azure Vm'lerinde SQL Server çalıştıran hakkında sık sorulan soruların yanıtlarını sağlar."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 10/05/2017
ms.author: jroth
ms.openlocfilehash: a001ae116e33e0b7be4431b0bc4c8bb319f4e801
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Sık Sorulan Sorular SQL Server için Linux Azure sanal makineleri

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

Bu konuda çalıştırma hakkında en yaygın sorulara yanıtlar sağlayan [Linux Azure Virtual Machines'de SQL Server](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Bu konuda Linux VM'ler üzerindeki SQL Server'a özgü sorunları odaklanır. Windows Vm'lerinde SQL Server çalışıyorsa bkz [Windows SSS](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

1. **SQL Server ile nasıl bir Linux Azure sanal makine oluşturulsun mu?**

   SQL Server içeren bir Linux sanal makine oluşturmak için en kolay çözüm olur. Azure'a kaydolma ve Portalı'ndan bir SQL VM oluşturma bir öğretici için bkz: [Azure portalında Linux SQL Server sanal makine sağlama](provision-sql-server-linux-virtual-machine.md). El ile SQL Server üzerinde bir VM ile ya da serbestçe lisanslı bir sürüm (Geliştirici veya Express) yüklemek veya bir şirket içi lisans yeniden kullanma seçeneğiniz de vardır. Kendi lisansını Getir seçerseniz bulunmalıdır [azure'da Yazılım Güvencesi ile lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility).

1. **Bir Azure VM'de SQL Server'ın yeni bir sürümü/yayını için nasıl yükseltme?**

   Şu anda hiçbir yerinde yükseltme bir Azure VM içinde çalışan SQL Server için yoktur. İstenen SQL Server sürümü/yayını ile yeni bir Azure sanal makine oluşturma ve ardından yeni kullanılarak sunucusuna veritabanlarınızı geçiş [standart veri geçiş teknikleri](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

1. **Azure VM temelinde my lisanslı SQL Server kopyası nasıl yükleyebilirim?**

   İlk olarak, Linux işletim sistemi-yalnızca sanal makine oluşturun. Ardından çalıştırın [SQL Server yükleme adımlarını](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) Linux dağıtımınız için. SQL Server'ın serbestçe lisanslı sürümlerinden birini yüklemiyorsanız, ayrıca bir SQL Server Lisansı olmalıdır ve [azure'da Yazılım Güvencesi ile lisans taşınabilirliği](https://azure.microsoft.com/pricing/license-mobility/).

1. **SQL Server için Getir bilgisayarınızı-kendi-lisans (KLG) Linux sanal makine görüntüleri var mı?**

   Şu anda hiçbir KLG Linux sanal makine görüntü SQL Server için vardır. Ancak, el ile SQL Server üzerinde yalnızca Linux VM önceki sorulara anlatıldığı gibi yükleyebilirsiniz.

1. **Kullandıkça Öde galeri görüntülerden birini oluşturulduysa, kendi SQL Server lisansınızı kullanmak için bir VM değiştirebilir miyim?**

   Hayır. Kendi lisansınızı kullanmak için dakika başına ödeme lisans geçemezsiniz. Yeni bir Linux VM oluşturun, SQL Server yükleyin ve verilerinizi taşımanız gerekir. Kendi lisansını getirme hakkında daha fazla ayrıntı için önceki soruya bakın.

1. **Hangi SQL Server paketleri de yüklü ilgili?**

   SQL Server Linux VM'ler üzerinde varsayılan olarak yüklü olan SQL Server paketleri için bkz [yüklü paketleri](sql-server-linux-virtual-machines-overview.md#packages).

1. **Azure Vm'lerinde SQL Server yüksek kullanılabilirlik çözümleri destekleniyor mu?**

   Şu anda değil. Always On kullanılabilirlik grupları hem de Yük Devretme Kümelemesi Pacemaker gibi Linux kümeleme bir çözümde gerektirir. Desteklenen Linux dağıtımları için SQL Server, yüksek kullanılabilirlik eklentileri bulutta desteklemez.

1. **Neden ı RHEL veya SLES SQL Server VM bir harcama sınırına sahip bir Azure aboneliği ile sağlayamıyor?**

   RHEL ve SLES sanal makineleri bir abonelik harcama sınırı ve abonelikle ilişkili bir doğrulanmış ödeme yöntemi (genellikle bir kredi kartı) gerektirir. Harcama sınırını kaldırmadan bir RHEL veya SLES VM sağlarsanız, aboneliğiniz devre dışı ve tüm VM'ler/Hizmetleri durduruldu. Abonelik yeniden etkinleştirmek için bu duruma, çalıştırırsanız [harcama sınırını kaldırmak](https://account.windowsazure.com/subscriptions). Kalan kredinizi geçerli fatura dönemi için geri yüklenir, ancak yeniden başlatın ve çalıştırmaya devam etmek seçerseniz bir RHEL veya SLES VM görüntü ek ücret kredi kartınız karşı gidin.

## <a name="resources"></a>Kaynaklar

**Linux VM'ler**:

* [Bir Linux VM üzerinde SQL Server'ın genel bakış](sql-server-linux-virtual-machines-overview.md)
* [Bir SQL Server Linux VM sağlama](provision-sql-server-linux-virtual-machine.md)
* [SQL Server'da Linux belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows sanal makineleri**:

* [Bir Windows VM üzerinde SQL Server'ın genel bakış](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server, Windows VM sağlama](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Sık sorulan sorular (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)