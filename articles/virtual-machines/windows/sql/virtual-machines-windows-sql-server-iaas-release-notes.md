---
title: SQL Server Azure VM sürüm notları | Microsoft Docs
description: Azure VM'de SQL Server'ın geliştirmeleri ve yeni özellikleri hakkında bilgi edinin
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: ee3aeb9f44d1b98d6307c6a72d1e4786ea1ec664
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076898"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server Azure sanal makine sürüm notları

Azure SQL Server'ın yerleşik görüntü ile bir sanal makine dağıtmanıza olanak tanır. Bu makalede yeni özellikler ve geliştirmeler son sürümlerinde özetlenir [Azure sanal makineler'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/). Makale ayrıca doğrudan ilgili sürüme ancak aynı zaman çerçevesinde yayımlanan önemli içerik güncelleştirmeleri listelenir. Diğer Azure Hizmetleri için geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates)

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Azure portalında yeni SQL VM Yönetimi** | Artık SQL Server sanal Makinenizi Azure portalında yönetmek için yeni bir yolu yoktur. Daha fazla bilgi için [Azure portalında SQL Server VM yönetme](virtual-machines-windows-sql-manage-portal.md).  | 
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Belgeleri geliştirmeleri

| Belgeler | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM portal Yönetimi** | Yeni SQL VM Yönetim Portalı deneyimi için yaklaşık bir düzine makaleler güncelleştirilmiştir. | 
| &nbsp; | &nbsp; |


## <a name="april-2019"></a>Nisan 2019

### <a name="service-improvements"></a>Hizmet geliştirmeleri

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **SQL Server 2008/2008R2 desteğini genişletme** | [Desteğini genişletmek](virtual-machines-windows-sql-server-2008-eos-extend-support.md) SQL Server 2008 ve SQL Server 2008 R2 geçirerek *olarak-olan* Azure VM'ye. | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>Mart 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Özel görüntü desteklenebilirliği** | Şimdi yükleyebilirsiniz [SQL Iaas uzantısı](virtual-machines-windows-sql-server-agent-extension.md#installation) özel işletim sistemi ve SQL görüntüleri için sunan sınırlı işlevselliğini [esnek lisanslama](virtual-machines-windows-sql-ahb.md). SQL kaynak sağlayıcısı ile özel görüntünüzü kayıt olarak belirttiğinizde lisans türü 'AHUB' Aksi durumda kayıt başarısız olur.  | 
| **Adlandırılmış örnek desteklenebilirliği** | Artık kullanabilir [SQL Iaas uzantısı](virtual-machines-windows-sql-server-agent-extension.md#installation) varsayılan örnek düzgün kaldırılmışsa adlandırılmış bir örnek ile. | 
| **Portal geliştirme** | Kullanılabilirliği iyileştirmek için SQL Server VM'SİNİN dağıtımı için Azure portalı deneyiminde yenilenmiştir. Daha fazla bilgi için bkz: kısa [Hızlı Başlangıç](quickstart-sql-vm-create-portal.md) ve daha kapsamlı [nasıl yapılır](virtual-machines-windows-portal-sql-server-provision.md) kılavuzda bir SQL Server VM'SİNİN dağıtımı için.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Şubat 2019

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Portal geliştirme** | Bir SQL Server getirin-kendi lisansını kullanmaya VM'den Kullandıkça Öde için lisanslama modeli değiştirmek artık mümkündür [Azure portalında](virtual-machines-windows-sql-ahb.md#with-the-azure-portal-1).|
|**Azure SQL VM CLI ile AG dağıtım basitleştirme** | Artık hiç olmadığı kadar bir kullanılabilirlik grubuna azure'da bir SQL Server VM'si dağıtmak için daha kolaydır. [Azure SQL VM CLI](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) WSFC, ILB ve ağ dinleyicisi tüm komut satırından ve kayıt zamanında oluşturmanıza olanak tanır! Daha fazla bilgi için [Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için Azure SQL VM CLI'yı kullanmak](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Aralık 2018

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL kümesine Grup kaynak sağlayıcısı** | Windows Yük devretme kümesi meta verileri tanımlayan yeni bir kaynak sağlayıcısı (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups). Bir SQL Server VM'si için birleştirme *SqlVirtualMachineGroups* Windows Yük devretme kümesi hizmeti bootstraps ve VM kümeye ekler.  |
|**Azure hızlı başlangıç şablonları ile bir kullanılabilirlik grubu dağıtımı ayarlama otomatikleştirin** |Artık, Windows Yük devretme kümesi oluşturma, SQL Server Vm'leri için katılın, dinleyiciyi oluşturun ve iç Load Balancer iki Azure hızlı başlangıç şablonları ile yapılandırmak mümkündür. Daha fazla bilgi için [kullanımı Azure hızlı başlangıç Azure VM'deki SQL Server Always On kullanılabilirlik grubu yapılandırmak için şablonu](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Otomatik SQL VM kaynak Sağlayıcısı kaydı** | SQL Server Vm'leri bu ay otomatik olarak kayıtlı sonra yeni SQL Server Kaynak sağlayıcısı ile dağıtılabilir. SQL Server bu ay önce dağıtılan Vm'leri yine de el ile kayıtlı olması gerekir. Daha fazla bilgi için [mevcut bir SQL VM ile SQL VM kaynak sağlayıcısı kaydetme](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Kasım 2018

| Hizmet geliştirmeleri | Ayrıntılar |
| --- | --- |
| **Yeni SQL VM kaynak sağlayıcısı** |  SQL Server VM'nize daha iyi yönetimine olanak sağlayan SQL Server Vm'leri (Microsoft.SqlVirtualMachine) için yeni bir kaynak sağlayıcısı. VM'nizi kaydetme ile ilgili daha fazla bilgi için bkz: [mevcut bir SQL VM ile yeni kaynak sağlayıcısını kaydetme](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). |
|**Anahtar lisanslama modeli** |Şimdi Azure CLI veya Powershell kullanarak SQL vm'nizin kullanım başına ödeme ve Getir kendi lisans modeli arasında geçiş yapabilirsiniz. Daha fazla bilgi için [bir SQL VM için lisanslama modelini değiştirme](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Ek kaynaklar

**Windows Vm'leri**:

* [Bir Windows VM üzerinde SQL Server'a genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
* [Bir SQL Server Windows VM sağlama](virtual-machines-windows-portal-sql-server-provision.md)
* [Bir veritabanını Azure VM'deki SQL Server'a geçirme](virtual-machines-windows-migrate-sql.md)
* [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makineler'de SQL Server](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure Sanal Makineler’de SQL Server için performansa yönelik en iyi uygulamalar](virtual-machines-windows-sql-performance.md)
* [Azure Sanal Makineler'de SQL Server için Uygulama Desenleri ve Geliştirme Stratejileri](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Linux Vm'leri**:

* [Bir Linux sanal makinesinde SQL Server'a genel bakış](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bir SQL Server Linux sanal makinesi sağlama](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [SSS (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
