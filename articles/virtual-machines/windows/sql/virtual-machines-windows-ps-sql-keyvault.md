---
title: Anahtar kasası (Resource Manager) azure'da Windows sanal makinelerinde SQL Server ile tümleştirme | Microsoft Docs
description: Azure Key Vault ile kullanmak için SQL Server şifreleme yapılandırmasını otomatikleştirme hakkında bilgi edinin. Bu konuda, Resource Manager ile oluşturulan SQL Server sanal makineleri ile Azure anahtar kasası tümleştirmeyi kullanmayı açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 6ad8eea21c10726b2c3eaf1e10bfd5efba4d1e48
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129629"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Azure sanal makinelerinde (Resource Manager) SQL Server için Azure anahtar kasası tümleştirmesini yapılandırma

> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Genel Bakış
Birden çok SQL Server şifreleme özellikleri vardır, örneğin [saydam veri şifrelemesi (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyinde şifrelemeyi (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), ve [yedekleme şifrelemesi](https://msdn.microsoft.com/library/dn449489.aspx). Bu formlar şifreleme, şifreleme için kullandığınız şifreleme anahtarlarını depolamak ve yönetmek gerektirir. Azure Key Vault (AKV) hizmeti, güvenlik ve bu anahtarları güvenli ve yüksek oranda kullanılabilir bir konumda yönetimini iyileştirmek için tasarlanmıştır. [SQL Server Bağlayıcısı](https://www.microsoft.com/download/details.aspx?id=45344) Bu anahtarları Azure Key vault'tan kullanılacak SQL Server'ı etkinleştirir.

SQL Server ile şirket içi makineleri çalıştırıyorsanız, vardır [şirket içi SQL Server makinenizden Azure Key Vault'a erişmesi için izlemeniz adımları](https://msdn.microsoft.com/library/dn198405.aspx). Ancak Azure vm'lerde SQL Server için kullanarak zaman kazanabilirsiniz *Azure anahtar kasası tümleştirmeyi* özelliği.

Bu özellik etkinleştirildiğinde otomatik olarak SQL Server Bağlayıcısı, Azure Key Vault'a erişmesi için EKM sağlayıcısına yapılandırır, yüklenip kasanıza erişmek için izin vermek için kimlik bilgisi oluşturur. Şirket yukarıda belirtilen belgelere adımları sırasında görünüyorsa bu özelliği 2 ve 3. adımları otomatikleştiren görebilirsiniz. Yine de el ile yapmanız gerekir gereken tek şey, anahtarları ve anahtar kasası oluşturmaktır. Burada, tüm kurulum SQL vm'nizin otomatik hale getirilmiştir. Bu özellik, bu kurulum tamamlandıktan sonra veritabanları veya yedekleri normalde yaptığınız gibi şifreleme başlamak için T-SQL deyimleri yürütebilir.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

  >[!NOTE]
  > EKM sağlayıcısı sürümü 1.0.4.0 SQL Server VM üzerinde yüklü [SQL Iaas uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension). SQL Iaas uzantısı yükseltme sağlayıcı sürümü güncelleştirilmez. Lütfen el ile (örneğin, bir SQL yönetilen örneğine geçişi sırasında) gerekirse EKM sağlayıcısı sürüm yükseltme de göz önünde bulundurur.


## <a name="enabling-and-configuring-akv-integration"></a>Etkinleştirme ve yapılandırma AKV tümleştirme
Sağlama sırasında AKV tümleştirme etkinleştirin veya var olan VM'ler için yapılandırın.

### <a name="new-vms"></a>Yeni VM'ler
Yeni bir SQL Server sanal makinesini Resource Manager ile sağlıyorsanız, Azure portalında Azure anahtar kasası tümleştirmeyi etkinleştirmek için bir yol sağlar. Azure Key Vault özelliği yalnızca Enterprise, Developer ve SQL Server'ın değerlendirme sürümleri için kullanılabilir.

![SQL Azure Anahtar Kasası Tümleştirme](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Sağlama ayrıntılı kılavuz için bkz. [Azure Portal'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Varolan Vm'leri
Mevcut SQL Server sanal makineleri için SQL Server sanal makinenizi seçin. Ardından **SQL Server Yapılandırması** bölümünü **ayarları** dikey penceresi.

![Var olan sanal makineler için SQL AKV tümleştirme](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

İçinde **SQL Server Yapılandırması** dikey penceresinde tıklayın **Düzenle** otomatik anahtar kasası tümleştirme bölümünde düğmesini.

![Var olan VM'ler için SQL AKV tümleştirmesini yapılandırma](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

İşiniz bittiğinde tıklayın **Tamam** altındaki düğmesine **SQL Server Yapılandırması** yaptığınız değişiklikleri kaydetmek için dikey.

> [!NOTE]
> Burada oluşturulan kimlik bilgisi adı daha sonra bir SQL oturum açmayla. Bu anahtar kasasına erişmek SQL oturum açma sağlar. 
>
>

> [!NOTE]
> Şablon kullanarak AKV tümleştirme de yapılandırabilirsiniz. Daha fazla bilgi için [Azure anahtar kasası tümleştirme için Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

