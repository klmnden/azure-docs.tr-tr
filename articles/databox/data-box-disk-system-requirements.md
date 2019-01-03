---
title: Microsoft Azure Data Box Disk sistem gereksinimleri | Microsoft Docs
description: Yazılım ve ağ, Azure Data Box Disk gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 12/27/2018
ms.author: alkohli
ms.openlocfilehash: 5debc14a6a20c42b62b9a7b2c524e36e94302221
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792875"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Azure Data Box Disk sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure Data Box Disk çözümünüz için ve Data Box Disk için bağlanan istemciler için önemli sistem gereksinimlerini açıklar. Önce Data Box Disk dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu bilgileri dikkatlice gözden öneririz.

> [!IMPORTANT]
> Data Box Disk Önizleme aşamasındadır. Bu çözümü dağıtmadan önce lütfen [Önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 

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

>[!NOTE]
> Azure Data Lake depolama Gen 2 hesapları desteklenmez.


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Data Box Disk için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure blok blobu | |
| Azure sayfa blobu  | |


## <a name="next-step"></a>Sonraki adım

* [Azure Data Box Disk dağıtma](data-box-disk-deploy-ordered.md)

