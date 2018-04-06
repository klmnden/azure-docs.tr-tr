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
ms.openlocfilehash: b345283f87c446ff3b583b0c5dd8a4be222303ae
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
Aşağıdaki sınırları Azure yedekleme hizmeti için geçerlidir.

| Sınır tanımlayıcısı | Varsayılan Sınır |
| --- | --- |
| Her kasa için kaydedilebilen kayıtlı sunucuları/makine sayısı |Windows Server/istemci/SCDPM için 50 <br/> Iaas VM'ler için 200 |
| Azure kasası depolamada depolanan veriler için bir veri kaynağı boyutu |54400 GB max<sup>1</sup> |
| Her Azure aboneliğinizde oluşturduğunuz yedek kasalarını sayısı |Her bölge 25 kurtarma Hizmetleri kasaları |
| Yedekleme günde zamanlanabilir sayısı |Windows Server/istemcisi için günde 3 <br/> SCDPM için gün başına 2 <br/> Iaas VM'ler için günde bir kez |
| Yedekleme için bir Azure sanal makinesine bağlı veri diskleri |16 |
| Yedekleme için bir Azure sanal makineye bağlı tek bir veri diski boyutu| 4095 GB <sup>2</sup>|

* <sup>1</sup>54400 GB sınırını Iaas VM yedekleme için geçerli değildir.
 

