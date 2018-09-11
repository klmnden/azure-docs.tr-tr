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
ms.date: 09/06/2018
ms.author: alkohli
ms.openlocfilehash: aaa4e4bb24ca42adb9d283e6286dbef879bcb1ea
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299857"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Azure Data Box Disk sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure Data Box Disk çözümünüz için ve Data Box Disk için bağlanan istemciler için önemli sistem gereksinimlerini açıklar. Önce Data Box Disk dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatlice gözden öneririz.

> [!IMPORTANT]
> Data Box Disk Önizleme aşamasındadır. Lütfen inceleyin [Önizleme kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bu çözümü dağıtmadan önce. 

Sistem gereksinimleri, diskler, desteklenen depolama hesapları ve depolama türleri için bağlanan istemciler için desteklenen platformları içerir.


## <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

Diskin kilidini açmak ve istemcileri aracılığıyla veri kopyalama işlemi için Data Box Disk bağlı için desteklenen işletim sistemlerinin bir listesi aşağıdadır.

| **İşletim sistemi** | **Test edilen sürüm** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Windows istemcileri için diğer gerekli yazılımı

Windows İstemcisi için aşağıdakileri de yüklü olması gerekir.

| **Yazılım**| **Sürüm** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework'ün |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Diğer gerekli yazılımı Linux istemcileri

Linux istemcisi için aşağıdaki gerekli yazılımı Data Box Disk araç takımı yükler:

- dislocker
- OpenSSL

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

