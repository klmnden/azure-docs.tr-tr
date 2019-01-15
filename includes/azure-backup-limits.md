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
ms.openlocfilehash: 9ad1208283527f35e04dede2706fd0e424096dc5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54300673"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| **Sınırı** | **Varsayılan** |
| --- | --- |
| Bir kasaya kayıtlı sunucu/makine | Windows Server/Windows istemci/System Center DPM: 50 <br/><br/> Iaas Vm'leri: 1000  |
| Kasa depolama alanındaki bir veri kaynağı boyutu |Maksimum 54400 GB. Iaas VM yedekleme için sınır uygulanmaz |
| Bir Azure aboneliğinde yedekleme kasaları |Bölge başına 500 kasa |
| Günlük yedekleme zamanlama |Windows Server/istemcisi: günde 3<br/> System Center DPM: 2 gün <br/> Iaas Vm'leri: Günde bir kez  |
| Bir Azure VM yedeklemesi için bağlı veri diskleri | 16 |
| Azure yedekleme için VM'ye tek bir veri diski| 4095 GB|
