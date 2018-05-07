---
title: Beklenmeyen yeniden başlatmalar VM'lerin Azure Linux VM'ler ekli VHD'lerde ile ilgili sorunları giderme | Microsoft Docs
description: Beklenmeyen ile ilgili sorunları giderme Linux VM'ler yeniden başlatır.
keywords: SSH bağlantı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines-linux
author: tamram
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/01/2018
ms.author: tamram
ms.openlocfilehash: 8ccb6b61c8de1599cd3db011d6401c781cefc31a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Beklenmeyen yeniden başlatmalar VM'lerin ekli VHD ile ilgili sorunları giderme

Bir Azure sanal makine (VM) çok sayıda aynı depolama hesabında olan ekli VHD'ler varsa, beklenmedik şekilde yeniden VM neden olan bir depolama hesabı ölçeklenebilirlik hedefleri aşabilir. Depolama hesabı için dakika ölçümleri denetleyin (**TotalRequests**/**Totalıngress**/**TotalEgress**) aşan ani için bir depolama hesabı ölçeklenebilirlik hedefleri. Bkz: [ölçümleri Göster artışı içinde PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) azaltma depolama hesabınıza gerçekleşip gerçekleşmediğini belirleme konusunda yardım almak için.

Genel olarak, her tek tek giriş veya çıkış işlemi bir sanal makineden bir VHD çevrilir **Al sayfasında** veya **Put sayfa** temel sayfa blobu işlemleri. Bu nedenle, belirli bir davranışı, uygulamanızın üzerinde tek bir depolama hesabında dayalı kaç VHD'ler ayarlamak için ortamınız için tahmini IOPS kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 veya daha az disk olmasını önerir. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla ayrıntı için özellikle toplam istek oranı ve toplam bant genişliğini depolama hesabı türü için kullanmakta olduğunuz.

Depolama hesabınız için ölçeklenebilirlik hedefleri aşan, etkinliğin ayrı ayrı her hesap azaltmak için birden çok depolama hesaplarındaki Vhd'lerinizi koyun.
