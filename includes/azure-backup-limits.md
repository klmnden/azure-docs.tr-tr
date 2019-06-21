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
ms.openlocfilehash: 9e9c09c1825f5c8383a708e8bd343146396f878e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188771"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| **Sınırı** | **Varsayılan** |
| --- | --- |
| Sunucuları veya bir kasaya kaydedilebilir makineler. | Windows Server/Windows istemci/System Center veri koruma Yöneticisi: kullanır. <br/><br/> Iaas Vm'leri: 1,000.  |
| Kasa Depolama bir veri kaynağı boyutu. |Maksimum 54,400 GB. Sınır Iaas VM yedekleme için geçerli değildir. |
| Bir Azure aboneliği yedekleme kasalarında. |Bölge başına 500 Kasa. |
| Günlük yedeklemeler zamanlayın. |Windows Server/istemcisi: Üç gün.<br/> System Center DPM: İki gün. <br/> Iaas Vm'leri: Günde bir kez.  |
| Bir Azure VM yedeklemesi için bağlı veri diskleri. | 16 |
| Azure yedekleme için VM'ye tek bir veri diski.| 4\.095 GB|
