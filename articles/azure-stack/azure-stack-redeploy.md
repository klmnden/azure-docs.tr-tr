---
title: "Azure yığın dağıtmanız | Microsoft Docs"
description: "Azure yığını yeniden dağıtın."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Azure yığını yeniden dağıtın
Azure yığın dağıtımı sırasında bir hata alırsanız, aşağıdaki PowerShell komutunu kullanarak kurulumu yeniden çalıştırabilirsiniz: `.\InstallAzureStackpoc.ps1 -rerun`. Bu komut Azure yığın Kurulum üzerinden baştan başlamanıza gerek olmadan daha önce başarısız bir noktada yeniden başlatılır. Aynı kurulum hatası yeniden alırsanız, adres sorunu için tam bir yeniden dağıtım gerçekleştirmek gerekli olabilir. 

Azure yığını yeniden dağıtmak için üzerinden sıfırdan aşağıda açıklandığı gibi başlatmanız gerekir.

## <a name="steps-to-redeploy-azure-stack"></a>Azure yığını yeniden dağıtmak için adımları
1. Geliştirme Seti ana bilgisayarda, yükseltilmiş bir PowerShell konsolu açın > asdk installer.ps1 komut dosyasına gidin > çalıştırın > tıklatın **yeniden**.
2. Temel işletim sistemi seçin (değil **Azure yığın**) tıklatıp **sonraki**.
3. Geliştirme Seti konak yeniden başlatıldıktan sonra önceki dağıtımının bir parçası kullanılan CloudBuilder.vhdx dosyasını silin.
4. [Geliştirme Seti dağıtmak](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)

