---
title: Windows sanal masaüstü Önizleme Yük Dengeleme yöntemi - Azure'ı yapılandırma
description: Yük Dengeleme yöntemi için bir Windows sanal masaüstü ortamında yapılandırılır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 0c4702dada17e759d89c33be99b3155f4b15ad9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60328893"
---
# <a name="configure-the-windows-virtual-desktop-preview-load-balancing-method"></a>Windows sanal masaüstü Önizleme yük dengeleme yöntemini yapılandırma

Bir konak havuz için Yük Dengeleme yöntemi yapılandırma gereksinimlerinize daha iyi uyacak şekilde Windows sanal masaüstü Önizleme ortamı ayarlamak sağlar.

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
