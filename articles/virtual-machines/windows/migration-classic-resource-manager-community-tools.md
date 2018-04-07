---
title: Topluluk araçları - Klasik kaynakları taşıma için Azure Resource Manager | Microsoft Docs
description: Bu makalede, Iaas kaynaklarına Klasikten Azure Resource Manager dağıtım modeline geçiş yardımcı olmak için topluluk tarafından sağlanan araçları kataloglar.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: cce1906e75646f2fb9ea30842e968d14830f3497
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarını klasik modelden Azure Resource Manager’a geçirmeye yönelik topluluk araçları
Bu makalede Klasik Iaas kaynaklardan Azure Resource Manager dağıtım modeline geçiş ile yardımcı olmak üzere topluluk tarafından sağlanan araçları kataloglar.

> [!NOTE]
> Bu araçları tarafından Microsoft Support resmi olarak desteklenmez. Bu nedenle Github'da kaynaklanan açık ve düzeltmeleri veya İlave Senaryolar PRs kabul mutluluk çalışıyoruz. Bir sorunu bildirmek için GitHub sorunları özelliğini kullanın.
> 
> Bu araçların geçiş kapalı kalma süresi Klasik sanal makineniz için neden olur. Desteklenen platform geçiş için arıyorsanız, ziyaret edin 
> 
>   * [Klasik Iaas kaynaklardan Azure Resource Manager yığınına desteklenen platform geçişi](migration-classic-resource-manager-overview.md)
>   * [Teknik derinlemesine platformunda geçiş Klasik Azure Resource Manager için desteklenen](migration-classic-resource-manager-deep-dive.md)
>   * [Iaas kaynaklarını Klasikten Azure Resource Manager'da Azure PowerShell kullanarak geçirme](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Bu, Kurumsal geçişleri için Azure Resource Manager Azure Hizmet Yönetimi'nden bir parçası olarak oluşturulan yardımcı Araçlar koleksiyonudur. Bu araç, geçiş ve sorunları çıkışı Demir geçiş üretim aboneliğinizi çalıştırmadan önce test etmek için kullanılan başka bir aboneliğe altyapınızı çoğaltmanıza olanak sağlar.

[Aracı belgeleri için bağlantı](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz eksiksiz classic Iaas kaynakların Azure Resource Manager Iaas kaynaklarına geçirmek için ek bir seçenektir. Geçiş aynı abonelik içindeki ya da farklı Aboneliklerde ve abonelik türleri arasında oluşabilir (örn: CSP abonelikler).

[Aracı belgeleri için bağlantı](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Sonraki Adımlar

* [Platform desteklenen geçişi Iaas Klasik kaynaklardan Azure Resource Manager'a genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için PowerShell kullanma](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için CLI kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gözden geçirme Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi hakkında en sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

