---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188319"
---
Herhangi bir küme sunucusunda, Windows Server 2008 R2 veya Windows Server 2012 çalıştırıyorsanız, ardından, doğrulamanız gerekir düzeltmeyi [KB2854082](https://support.microsoft.com/kb/2854082) her şirket içi sunucular veya kümenin parçası olan Azure Vm'lerinin yüklenir. Ayrıca bu düzeltme herhangi bir sunucu veya kümedeki ancak kullanılabilirlik grubu değil, VM olması gerekir.

Küme düğümlerinin her biri için Uzak Masaüstü oturumu içinde indirme [KB2854082](https://support.microsoft.com/kb/2854082) yerel bir dizin. Ardından, düzeltmeyi her küme düğümünde sırayla yükleyin. Küme hizmetinin küme düğümünde şu anda çalışıyorsa sunucusu düzeltme yüklemesini sonunda başlatılır.

> [!WARNING]
> Küme hizmetinin durdurulması veya sunucunun yeniden başlatılmasını çekirdek sistem kümenizi ve kullanılabilirlik grubunun durumunu etkiler ve kümenizi çevrimdışı çalışmasına neden olabilir. Yükleme sırasında kümenizin yüksek kullanılabilirliği sürdürmek için emin olun:
> 
> * En uygun çekirdek durumunu kümedir. 
> * Herhangi bir düğümde düzeltmeyi yüklemeden önce tüm küme düğümleri çevrimiçi değil.
> * Kümedeki herhangi bir düğümde düzeltmeyi yüklemeden önce tam olarak sunucuyu yeniden başlatmayı da dahil olmak üzere, bir düğümde tamamlanmak üzere çalıştırılmasını düzeltme yüklemesini sağlar.
> 
> 

