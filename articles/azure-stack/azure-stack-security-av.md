---
title: Azure Stack'te güncelleştirme Windows Defender virüsten koruma
description: Nasıl virüsten koruma ilgili ayrıntılar tutulur en güncel Azure Stack üzerinde
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/14/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 7899f235853991160e4d8b2c2c832d9a77171e83
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242213"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Azure Stack'te güncelleştirme Windows Defender virüsten koruma

[Windows Defender virüsten koruma](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) , güvenlik ve virüs koruması sağlayan bir kötü amaçlı yazılımdan koruma çözümüdür. Her Azure Stack Altyapısı bileşeni (Hyper-V konakları ve sanal makineler), Windows Defender virüsten koruma ile korunur. Güncel koruma için Windows Defender virüsten koruma tanımları, altyapı ve platform düzenli güncelleştirmeler gerektirir. Güncelleştirmeleri nasıl uygulandığını, yapılandırmasına bağlıdır.

## <a name="connected-scenario"></a>Bağlı senaryosu

Kötü amaçlı yazılımdan koruma tanım ve altyapı güncelleştirmelerini, Azure Stack için [güncelleştirme kaynak sağlayıcısı](azure-stack-updates.md#the-update-resource-provider) kötü amaçlı yazılım tanımlarını ve altyapı güncelleştirmelerini günde birden çok kez indirir. Her Azure Stack Altyapısı bileşeni güncelleştirme güncelleştirme kaynak Sağlayıcısı'ndan alır ve güncelleştirme otomatik olarak uygulanır.

Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri uygulamak [aylık Azure Stack güncelleştirme](azure-stack-apply-updates.md). Windows Defender virüsten koruma platformu güncelleştirmeleri ay için aylık Azure Stack güncelleştirme içerir.

## <a name="disconnected-scenario"></a>Bağlantısı kesilmiş senaryosu

 Uygulama [aylık Azure Stack güncelleştirme](azure-stack-apply-updates.md). Windows Defender virüsten koruma tanımları, altyapı ve platform güncelleştirmelerinin ay için aylık Azure Stack güncelleştirme içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
