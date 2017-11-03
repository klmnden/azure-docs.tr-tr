---
title: "Azure yığın dağıtmanız | Microsoft Docs"
description: "Azure yığını yeniden dağıtın."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: erikje
ms.openlocfilehash: 891cde9b16bbbb51729129b6ad7a0f3794307baa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="redeploy-azure-stack"></a>Azure yığını yeniden dağıtın
Azure yığını yeniden dağıtmak için üzerinden sıfırdan aşağıda açıklandığı gibi başlatmanız gerekir.

## <a name="steps-to-redeploy-azure-stack"></a>Azure yığını yeniden dağıtmak için adımları
1. Geliştirme Seti ana bilgisayarda, yükseltilmiş bir PowerShell konsolu açın > asdk installer.ps1 komut dosyasına gidin > çalıştırın > tıklatın **yeniden**.
2. Temel işletim sistemi seçin (değil **Azure yığın**) tıklatıp **sonraki**.
3. Geliştirme Seti konak yeniden başlatıldıktan sonra önceki dağıtımının bir parçası kullanılan CloudBuilder.vhdx dosyasını silin.
4. [Geliştirme Seti dağıtmak](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

