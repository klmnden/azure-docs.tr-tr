---
title: Microsoft Azure Data Box Disk sistem gereksinimleri | Microsoft Docs
description: Yazılım ve ağ, Azure Data Box Disk gereksinimleri hakkında bilgi edinin
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
ms.openlocfilehash: 2fdd574adf3587f11984bee2a2549d9bcd0c4c0d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126014"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Azure Data Box Disk sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure Data Box Disk çözümünüz için ve Data Box Disk için bağlanan istemciler için önemli sistem gereksinimlerini açıklar. Önce Data Box Disk dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatlice gözden öneririz.

> [!IMPORTANT]
> Data Box Disk Önizleme aşamasındadır. Lütfen inceleyin [Önizleme kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bu çözümü dağıtmadan önce. 

Sistem gereksinimleri, diskler, desteklenen depolama hesapları ve depolama türleri için bağlanan istemciler için desteklenen platformları içerir.


## <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

Diskin kilidini açmak ve istemcileri aracılığıyla veri kopyalama işlemi için Data Box Disk bağlı için desteklenen işletim sistemlerinin bir listesi aşağıdadır.

| **İşletim sistemi/platform** | **Sürümleri** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
| Windows PowerShell |4.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework'ün |4.0|

> [!NOTE] 
> Disk çalıştıran istemcilerde etkinleştirilecek BitLocker gereksinimlerini aracı kilidini açmak ve verileri kopyalamak için kullanılır.


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Data Box Disk için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Depolama hesabı** | **Notlar** |
| --- | --- |
| Klasik | Standart |
| Genel Amaçlı  |Standart; V1 ve V2 desteklenir. Sık ve seyrek erişimli katmanları desteklenir. |


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Data Box Disk için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure blok blobu | |
| Azure sayfa blobu  | |


## <a name="next-step"></a>Sonraki adım

* [Azure Data Box Disk dağıtma](data-box-disk-deploy-ordered.md)

