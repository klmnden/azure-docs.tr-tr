---
title: include dosyası
description: include dosyası
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 1f41567f7d5dcc9b6536b47ef639ba53abff8ec4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756019"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| Sınır tanımlayıcı | Varsayılan Sınır |
| --- | --- |
| Her kasaya yönelik olarak kaydedilebilen sunucu/makine sayısı |Windows Server/istemci/SCDPM için 50 <br/> Iaas Vm'leri için 1000 |
| Kasa Azure Depolama'da depolanan veriler için bir veri kaynağı boyutu |54400 GB maksimum<sup>1</sup> |
| Her bir Azure aboneliği için oluşturulan yedekleme kasalarının sayısı |Bölge başına 500 kurtarma Hizmetleri kasası |
| Günlük yedekleme zamanlanabilir sayısı |Windows Server/istemcisi için günde 3 <br/> SCDPM için günde 2 <br/> Iaas VM'ler için günde bir kez |
| Bir Azure sanal makinesine bağlı veri diskleri |16 |
| Bir Azure sanal makinesine bağlı tek tek veri disk boyutu| 4095 GB <sup>2</sup>|

* <sup>1</sup>54400 GB sınırına Iaas VM yedekleme için geçerli değildir.
 

