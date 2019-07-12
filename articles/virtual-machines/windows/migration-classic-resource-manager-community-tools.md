---
title: Topluluk araçları - Klasik kaynakları Azure Resource Manager'a taşıma | Microsoft Docs
description: Bu makale, Iaas kaynaklarını Klasik modelden Azure Resource Manager dağıtım modeline geçirme yardımcı olmak için topluluk tarafından sağlanan araçları kataloglar.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
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
ms.openlocfilehash: 63e1ad044204bf7695d274fa46f06523fd9d460f
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720272"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarını klasik modelden Azure Resource Manager’a geçirmeye yönelik topluluk araçları
Bu makale, Iaas kaynaklarını Klasik modelden Azure Resource Manager dağıtım modeline geçişi ile yardımcı olmak için topluluk tarafından sağlanan araçları kataloglar.

> [!NOTE]
> Bu araçlar, Microsoft Support resmi olarak desteklenmez. Bu nedenle kaynağı GitHub üzerinde açık olan ve düzeltmeler veya ek senaryolar için çekme isteklerini kabul etmek memnuniyet duyuyoruz. Bir sorunu bildirmek için GitHub sorunları özelliğini kullanın.
> 
> Bu araçlarla geçirmek için Klasik sanal makine kapalı kalma süresi neden olur. Desteklenen platform geçişi için arıyorsanız, ziyaret edin 
> 
>   * [Iaas kaynaklarının Klasik'ten Azure Resource Manager yığını'nın desteklenen platform geçişi](migration-classic-resource-manager-overview.md)
>   * [Geçiş Klasik modelden Azure Resource Manager'a Platform ayrıntılı teknik bakış desteklenir](migration-classic-resource-manager-deep-dive.md)
>   * [Iaas kaynaklarının Klasik'ten Azure PowerShell kullanarak Azure Resource Manager'a geçiş](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Azure Hizmet Yönetimi Kurumsal geçişleri için Azure Resource Manager'ın bir parçası olarak oluşturulan Yardımcısı araçları koleksiyonudur. Bu araç, geçiş ve tüm sorunları Demir üretim aboneliğinizi geçiş çalıştırmadan önce test etmek için kullanılabilen başka bir abonelik içine altyapınızı çoğaltmanıza olanak sağlar.

[Aracı belgeleri için bağlantı](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz için Azure Resource Manager Iaas kaynaklarını Klasik Iaas kaynaklarını eksiksiz bir kümesini geçirmek için ek bir seçenektir. Aynı abonelik içinde veya farklı Aboneliklerde ve abonelik türleriyle arasında geçiş oluşabilir (örn: CSP abonelikleri için).

[Aracı belgeleri için bağlantı](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Sonraki Adımlar

* [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a platform destekli geçişe genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için PowerShell kullanma](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için CLI kullanma](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gözden geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a hakkında sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

