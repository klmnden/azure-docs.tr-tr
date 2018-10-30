---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d656d756759c997972eb034e194355185be93e1a
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227155"
---
Herhangi bir küme sunucusunda, Windows Server 2008 R2 veya Windows Server 2012 çalıştırıyorsanız, ardından, doğrulamanız gerekir düzeltmeyi [KB2854082](http://support.microsoft.com/kb/2854082) her şirket içi sunucular veya kümenin parçası olan Azure Vm'lerinin yüklenir. Ayrıca bu düzeltme herhangi bir sunucu veya kümedeki ancak kullanılabilirlik grubu değil, VM olması gerekir.

Küme düğümlerinin her biri için Uzak Masaüstü oturumu içinde indirme [KB2854082](http://support.microsoft.com/kb/2854082) yerel bir dizin. Ardından, düzeltmeyi her küme düğümünde sırayla yükleyin. Küme hizmetinin küme düğümünde şu anda çalışıyorsa sunucusu düzeltme yüklemesini sonunda başlatılır.

> [!WARNING]
> Küme hizmetinin durdurulması veya sunucunun yeniden başlatılmasını çekirdek sistem kümenizi ve kullanılabilirlik grubunun durumunu etkiler ve kümenizi çevrimdışı çalışmasına neden olabilir. Yükleme sırasında kümenizin yüksek kullanılabilirliği sürdürmek için emin olun:
> 
> * En uygun çekirdek durumunu kümedir. 
> * Herhangi bir düğümde düzeltmeyi yüklemeden önce tüm küme düğümleri çevrimiçi değil.
> * Kümedeki herhangi bir düğümde düzeltmeyi yüklemeden önce tam olarak sunucuyu yeniden başlatmayı da dahil olmak üzere, bir düğümde tamamlanmak üzere çalıştırılmasını düzeltme yüklemesini sağlar.
> 
> 

