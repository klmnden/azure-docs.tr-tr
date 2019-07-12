---
title: Azure VM'de SQL Server sürümünden yerinde yükseltilmesini gerçekleştirme | Microsoft Docs
description: Azure'da SQL Server VM'nize sürümü değiştirmeyi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 05d2170fe9e6055179bf49d803d4739ddc05be89
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607552"
---
# <a name="how-to-perform-an-in-place-upgrade-of-sql-server-edition-on-azure-vm"></a>Azure VM'de SQL Server sürümünden yerinde yükseltilmesini gerçekleştirme

Bu makalede, azure'da Windows sanal makinesinde mevcut bir SQL Server için SQL Server sürümünü değiştirmek açıklar. 

SQL Server sürümü ürün anahtarı tarafından belirlenir ve yükleme işlemi ile belirtilir. Sürüm ne belirleyen [özellikleri](/sql/sql-server/editions-and-components-of-sql-server-2017) SQL Server ürün içinde kullanılabilir. Yükleme medyasında ve maliyeti azaltın veya daha fazla özellikleri etkinleştirmek için yükseltin ya da Sürüm Düşürme SQL Server sürümüyle değiştirebilirsiniz.

SQL VM kaynak sağlayıcısı ile kaydettikten sonra yükleme medyasını kullanarak SQL Server sürümü güncelleştirdiyseniz, ardından Azure güncelleştirmek için buna uygun olarak fatura SQL Server sürümü özelliği SQL VM kaynağı şu şekilde ayarlamanız gerekir:

1. [Azure portal](https://portal.azure.com) oturum açın. 
1. SQL Server sanal makine kaynağınıza gidin. 
1. Altında **ayarları**seçin **yapılandırma** ve istenen sürümünüz SQL Server'ın açılır listeden seçip **Edition**. 

   ![Değişiklik sürümü meta verileri](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. SQL Server sürümü önce değiştirmeniz gerekir ve bu sürüm özelliğini SQL Server sürümü ile eşleşmelidir bildiren uyarıyı gözden geçirin. 
1. Seçin **Uygula** sürüm meta verileri değişikliklerinizi uygulamak için. 


## <a name="prerequisites"></a>Önkoşullar

SQL Server sürümünün herhangi bir yerinde değişiklik yapmak için aşağıdakiler gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- Bir Windows [SQL Server VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) kayıtlı [SQL VM kaynak sağlayıcısı](virtual-machines-windows-sql-register-with-resource-provider.md).
- İstenen SQL Server sürümü ile medya ayarlayın. Kullanan müşteriler [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) , yükleme medyasından edinebilirsiniz [Toplu Lisanslama Merkezi](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Müşterilerin sahip Yazılım Güvencesi kendi istediğiniz sürüm olan Market SQL Server VM görüntüsü Kurulum medyasından kullanabilirsiniz.


## <a name="upgrade-edition"></a>Sürüm yükseltme

  > [!WARNING]
  > - **SQL Server sürümüne yükseltme, Analysis Services ve R Services gibi ilişkili hizmetlerin yanı sıra, SQL Server için hizmeti yeniden başlatılır.** 

SQL Server sürümüne yükseltmek için istenen SQL Server sürümünün SQL Server Kurulumu medya almak ve sonra aşağıdakileri yapın:

1. SQL Server yükleme medyasından Setup.exe başlatın. 
1. Gidin **Bakım** ve **sürüm yükseltme** seçeneği. 

   ![SQL Server sürümüne yükseltin](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Seçin **sonraki** ulaşana kadar **sürüm yükseltme hazır** sayfasında ve ardından **yükseltme**. Değişiklik etkisi sürüyor ve ardından görürsünüz Kurulum penceresine birkaç dakikalığına sarkabilir bir **tam** yükseltmenizi edition tamamlandığını onaylayan sayfa. 

SQL Server sürümünden yükseltildikten sonra yukarıda gösterildiği gibi Azure portalında Sql sanal makinesi, "Sürüm" özelliğini değiştirmeniz gerekir; Bu meta veriler ve bu VM ile ilişkili faturalandırma güncelleştirir.

## <a name="downgrade-edition"></a>Sürüm Düşürme

  > [!WARNING]
  > - **SQL Server sürümü önceki sürüme indirme gerektirir tamamen ek kapalı kalma süresi kaynaklanabilecek SQL Server'ı kaldırma**. 


SQL Server sürümü indirgemek için tamamen SQL Server'ı kaldırın ve istenen sürümü Kurulum medyası ile yeniden yükleyin gerekecektir. 

SQL Server sürümü, aşağıdaki adımları izleyerek düşürebilir:

1. Sistem veritabanları dahil olmak üzere tüm veritabanlarını yedekleyin. 
1. Sistem veritabanları (master, model ve msdb) yeni bir konuma taşıyın. 
1. SQL Server ve tüm ilişkili hizmetleri tamamen kaldırın. 
1. Sanal makineyi yeniden başlatın. 
1. İstenen SQL Server sürümü ile medya kullanarak SQL Server'ı yükleyin.
1. En son hizmet paketleri ve toplu güncelleştirmeleri yükleyin.  
1. Daha önce farklı bir konuma taşınmış sistem veritabanları ile yükleme sırasında oluşturulan yeni sistem veritabanları değiştirin. 

SQL Server sürümü indirgenen sonra yukarıda gösterildiği gibi Azure portalındaki SQL sanal makinesi 'Sürüm' özelliğini değiştirmeniz; Bu meta veriler ve bu VM ile ilişkili faturalandırma güncelleştirir.

## <a name="remarks"></a>Açıklamalar

 - Edition özellik SQL Server VM'si için tüm SQL sanal PAYG hem KLG lisans türleri dahil olmak üzere makineler için sanal makinede yüklü olan SQL Server sürümü ile eşleşmelidir.
 - SQL Server VM kaynağınızı sürüklerseniz, görüntünün sabit kodlu sürümü ayarına geri geçer.
  - Sürümü değiştirme özelliği, SQL VM kaynak Sağlayıcısı'nın bir özelliğidir. Bir SQL Server VM, Azure portalı üzerinden bir Market görüntüsü otomatik olarak dağıtma ile kaynak sağlayıcısını kaydeder. Bununla birlikte, müşteriler, şirket içinde SQL Server yükleme el ile gerekecektir [kendi SQL Server sanal Makinesini kaydetme](virtual-machines-windows-sql-register-with-resource-provider.md).
- Bir SQL Server VM bir kullanılabilirlik kümesine ekleme, sanal Makineyi yeniden oluşturmayı gerektirir. Kullanılabilirlik eklenmiş gibi tüm Vm'leri olarak kümesi varsayılan sürüm'e geri gider ve sürümü yeniden değiştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server üzerindeki bir Windows VM ile ilgili SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Fiyatlandırma Kılavuzu bir Windows VM'de SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server Windows VM sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


