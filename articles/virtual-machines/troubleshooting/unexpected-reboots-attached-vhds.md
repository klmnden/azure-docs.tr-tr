---
title: Azure sanal makinelerinde ekli VHD'ler ile VM'lerin beklenmeyen yeniden başlatmaları sorunlarını giderme | Microsoft Docs
description: VM'lerin beklenmeyen yeniden başlatmaları giderilir.
keywords: SSH bağlantısını reddetti, ssh hatası, azure, SSH bağlantısı başarısız ssh
services: virtual-machines
author: genlin
manager: cshepard
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: cffc6166ab7d0864646421b35fbecafdd80eb27e
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414762"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Ekli VHD'ler ile VM'lerin beklenmeyen yeniden başlatmaları sorunlarını giderme

Azure sanal makinesi (VM) çok sayıda aynı depolama hesabında bulunan ekli VHD'lerde varsa, VM beklenmedik şekilde yeniden başlatılmasına neden ayrı bir depolama hesabınız için ölçeklenebilirlik hedefleri aşabilir. Depolama hesabı için dakika ölçümlerini denetleyin (**TotalRequests**/**Totalıngress**/**TotalEgress**) aşan ani artışlar için bir depolama hesabı için ölçeklenebilirlik hedefleri. Bkz: [ölçümler Percentthrottlingerror'da artış artış gösteriyor](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) azaltma, depolama hesabınızda gerçekleşip gerçekleşmediğini belirleme konusunda yardım almak için.

Genel olarak, her tek giriş veya çıkış işlem bir VHD'den sanal makine üzerinde çevrilir **Al sayfasında** veya **Put sayfa** temel sayfa blob işlemleri. Bu nedenle, ortamınız için tahmini IOPS kaç VHD'ler, uygulamanızın özel bir davranış tek bir depolama hesabında tabanlı ayarlamak için kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 ya da daha az disklerine sahip önerir. Bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md) depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla ayrıntı için özellikle toplam istek hızı ve depolama hesabı türü için toplam bant genişliği kullanıyorsunuz.

Depolama hesabınız için ölçeklenebilirlik hedefleri aşıyorsa ayrı ayrı her hesap etkinliğinde azaltmak için birden çok depolama hesabında Vhd'lerinizi yerleştirin.
