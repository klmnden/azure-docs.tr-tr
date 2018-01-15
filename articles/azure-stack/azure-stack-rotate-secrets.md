---
title: "Gizli anahtarları Azure yığınında döndürme | Microsoft Docs"
description: "Gizli anahtarlarınız Azure yığınında döndürme öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: 0a4118a8927e4261fafa307af5b9c29623ce5c3f
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Gizli anahtarları Azure yığınında Döndür

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Parolalarınızı normal bir tempoyla Azure yığın bileşenler için güncelleştirin.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Temel kart yönetim denetleyicisi (BMC) parolalarını güncelleştiriliyor

Temel Kart Yönetim denetleyicileri (BMC) sunucularınız fiziksel durumunu izleyin. BMC parola güncelleştirme hakkında yönergeler ve belirtimleri özgün donanım üreticisi (OEM) donanım satıcınıza göre farklılık gösterir.

1. BMC Azure yığın fiziksel sunucularda, OEM yönergelerini izleyerek güncelleştirin. Ortamınızdaki her BMC için parola aynı olmalıdır.
2. Ayrıcalıklı bir uç nokta Azure yığın oturumu açın. Yönergeler için bkz: [Azure yığınında kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md).
3. Sonra PowerShell istemi olarak değiştirildi **[IP adresi veya ERCS VM name]: PS >** veya **[azs-ercs01]: PS >**çalıştırabileceğiniz bir ortam, bağlı olarak `Set-BmcPassword` çalıştırarak `invoke-command`. Ayrıcalıklı endpoint oturum değişkeni bir parametre olarak geçirin.  
Örneğin:
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Sonraki adımlar

Azure yığını ve güvenlik hakkında daha fazla bilgi için bkz: [Azure yığın altyapı güvenlik tutumunu](azure-stack-security-foundations.md).