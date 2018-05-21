---
title: Beklenmeyen yeniden başlatmalar VM'lerin Azure Windows VM ekli VHD'lerde ile ilgili sorunları giderme | Microsoft Docs
description: Beklenmeyen ile ilgili sorunları giderme Windows Vm'leri yeniden başlatır.
keywords: SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines-windows
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 5c16c2d08a1a317f8f718330ff6c86a452ac1815
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Beklenmeyen yeniden başlatmalar VM'lerin ekli VHD ile ilgili sorunları giderme

Bir Azure sanal makine (VM) çok sayıda aynı depolama hesabında olan ekli VHD'ler varsa, beklenmedik şekilde yeniden VM neden olan bir depolama hesabı ölçeklenebilirlik hedefleri aşabilir. Depolama hesabı için dakika ölçümleri denetleyin (**TotalRequests**/**Totalıngress**/**TotalEgress**) aşan ani için bir depolama hesabı ölçeklenebilirlik hedefleri. Bkz: [ölçümleri Göster artışı içinde PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) azaltma depolama hesabınıza gerçekleşip gerçekleşmediğini belirleme konusunda yardım almak için.

Genel olarak, her tek tek giriş veya çıkış işlemi bir sanal makineden bir VHD çevrilir **Al sayfasında** veya **Put sayfa** temel sayfa blobu işlemleri. Bu nedenle, belirli bir davranışı, uygulamanızın üzerinde tek bir depolama hesabında dayalı kaç VHD'ler ayarlamak için ortamınız için tahmini IOPS kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 veya daha az disk olmasını önerir. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla ayrıntı için özellikle toplam istek oranı ve toplam bant genişliğini depolama hesabı türü için kullanmakta olduğunuz.

Depolama hesabınız için ölçeklenebilirlik hedefleri aşan, etkinliğin ayrı ayrı her hesap azaltmak için birden çok depolama hesaplarındaki Vhd'lerinizi koyun.
