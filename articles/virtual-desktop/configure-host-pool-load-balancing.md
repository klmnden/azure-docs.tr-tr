---
title: Windows sanal masaüstü Yük Dengeleme yöntemi (Önizleme) - Azure'ı yapılandırma
description: Yük Dengeleme yöntemi için bir Windows sanal masaüstü ortamında yapılandırılır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 10a1066b85b16749fe95e373e696d486b0e7bafa
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318352"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Windows sanal masaüstü yük dengeleme yöntemini yapılandırma

Konak havuz için Yük Dengeleme yöntemi yapılandırma gereksinimlerinize daha iyi uyacak şekilde Windows sanal masaüstü (Önizleme) ortamı ayarlamanıza olanak sağlar.

>[!NOTE]
> Kullanıcıların her zaman ana havuzdaki oturumu ana bilgisayarı 1:1 eşlemeye sahip olduğundan bu bir masaüstü kalıcı ana havuzuna geçerli değildir.

## <a name="configure-breadth-first-load-balancing"></a>Avantajlarına ilk yük dengelemeyi yapılandırma

Avantajlarına ilk Yük Dengeleme yeni kalıcı olmayan konak havuzları için varsayılan yapılandırmasıdır. Yeni kullanıcı oturumları, avantajlarına ilk Yük Dengeleme konaklarda kullanılabilir oturum ana bilgisayarı havuzu arasında dağıtır. Avantajlarına ilk Yük Dengeleme yapılandırırken, konak havuzunda oturumu ana bilgisayarı başına maksimum oturum sınırı ayarlayabilir.

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Maksimum oturum sınırı ayarlama olmadan avantajlarına ilk yük dengelemeyi gerçekleştirmek için bir ana bilgisayar havuzunu yapılandırmak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Yeni maksimum oturum sınırı kullanılacak ve avantajlarına ilk Yük Dengeleme gerçekleştirmek için bir ana bilgisayar havuzunu yapılandırmak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Derinlik ilk yük dengelemeyi yapılandırma

Derinlik ilk Yük Dengeleme bir kullanılabilir oturumu ana bilgisayarı, en yüksek bağlantı sayısı ile yeni kullanıcı oturumlarını dağıtır ancak kendi maksimum oturum sınırı eşiğine değil. Derinlik ilk Yük Dengeleme, yapılandırırken, **gerekir** konak havuzunda oturumu ana bilgisayarı başına maksimum oturum sınırı ayarlayın.

Derinlik ilk Yük Dengeleme gerçekleştirmek için bir ana bilgisayar havuzunu yapılandırmak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
