---
title: include dosyası
description: include dosyası
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 63922eb623576379058c9a8a367d6e52249115f2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109102"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| **Sınırı** | **Varsayılan** |
| --- | --- |
| Bir kasaya kayıtlı sunucu/makine | Windows Server/Windows istemci/System Center DPM: 50 <br/><br/> Iaas Vm'leri: 1000  |
| Kasa depolama alanındaki bir veri kaynağı boyutu |Maksimum 54400 GB. Iaas VM yedekleme için sınır uygulanmaz |
| Bir Azure aboneliğinde yedekleme kasaları |Bölge başına 500 kasa |
| Günlük yedekleme zamanlama |Windows Server/Client: 3 gün<br/> System Center DPM: 2 gün <br/> Iaas Vm'leri: Günde bir kez  |
| Bir Azure VM yedeklemesi için bağlı veri diskleri | 32 |
| Azure yedekleme için VM'ye tek bir veri diski| 4095 GB|



