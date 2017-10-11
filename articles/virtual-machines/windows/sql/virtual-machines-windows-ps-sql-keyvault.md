---
title: "Anahtar kasası (Resource Manager) azure'da Windows sanal makineleri üzerinde SQL Server ile tümleştirme | Microsoft Docs"
description: "Azure anahtar kasası ile kullanmak için SQL Server şifrelemesi yapılandırmasını öğrenin. Bu konu, Resource Manager ile oluşturulan SQL Server sanal makineleri ile Azure anahtar kasası tümleştirmeyi kullanımı açıklanmaktadır."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 32b9564fa5c9ca6864ade343fda309b2c3edf123
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>SQL Server için Azure anahtar kasası tümleştirme Azure sanal makinelerde (Kaynak Yöneticisi) yapılandırın
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Birden çok SQL Server şifreleme özellikleri vardır, gibi [saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyinde şifreleme (Temizle)](https://msdn.microsoft.com/library/ms173744.aspx), ve [yedek şifreleme](https://msdn.microsoft.com/library/dn449489.aspx). Bu formlar şifreleme, şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek gerektirir. Azure anahtar kasası (AKV) hizmeti bu anahtarların güvenli ve yüksek oranda kullanılabilir bir konumda yönetim ve güvenlik artırmak için tasarlanmıştır. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) SQL Server'ın bu anahtarları Azure anahtar kasası kullanmaya sağlar.

Şirket içi SQL Server çalıştıran, var. makineleri durumunda olan [Azure anahtar kasası, şirket içi SQL Server makineden erişmek için izlemeniz adımları](https://msdn.microsoft.com/library/dn198405.aspx). Ancak Azure vm'lerinde SQL Server için kullanarak zaman kazanabilirsiniz *Azure anahtar kasası tümleştirmeyi* özelliği.

Bu özellik etkinleştirildiğinde, otomatik olarak SQL Server Bağlayıcısı'nı yükler, Azure anahtar kasası erişmek için EKM sağlayıcısına yapılandırır ve kasanızı erişmesine izin vermek için kimlik bilgisi oluşturur. Yukarıda açıklanan şirket içi belgelerindeki adımları sırasında görünüyorsa, bu özellik adım 2 ve 3 otomatikleştirir görebilirsiniz. Anahtar kasasını ve anahtarları hala el ile yapmanız gerekir tek şey oluşturmaktır. Buradan, tüm Kurulum, SQL VM otomatik hale getirilmiştir. Bu özellik, bu kurulum tamamlandıktan sonra veritabanları veya yedeklemeler normal olarak şifreleme başlamak için T-SQL deyimlerini yürütebilir.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Etkinleştirme ve yapılandırma AKV tümleştirme
Sağlama sırasında AKV tümleştirme etkinleştirmek veya var olan VM'ler için yapılandırın.

### <a name="new-vms"></a>Yeni sanal makineleri
Yeni bir SQL Server sanal makine Resource Manager ile sağlıyorsanız, Azure portalında Azure anahtar kasası tümleştirmeyi etkinleştirmek için bir adım sağlar. Azure anahtar kasası özelliği yalnızca Enterprise, Developer ve değerlendirme sürümleri SQL Server için kullanılabilir.

![SQL Azure Anahtar Kasası Tümleştirme](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Sağlama ayrıntılı bilgi için bkz [Azure Portal'da bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Var olan sanal makineleri
Var olan SQL Server sanal makineler için SQL Server sanal makine seçin. Ardından **SQL Server yapılandırma** bölümünü **ayarları** dikey.

![Var olan VM'ler için SQL AKV tümleştirme](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

İçinde **SQL Server yapılandırma** dikey penceresinde tıklatın **Düzenle** otomatik anahtar kasası tümleştirme bölümdeki düğmesi.

![SQL AKV tümleştirme var olan VM'ler için yapılandırma](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Tamamlandığında, tıklatın **Tamam** alt düğmesinde **SQL Server yapılandırma** yaptığınız değişiklikleri kaydetmek için dikey.

> [!NOTE]
> Bir şablon kullanarak AKV tümleştirme de yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Azure anahtar kasası tümleştirme için Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

