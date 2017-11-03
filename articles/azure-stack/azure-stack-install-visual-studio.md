---
title: "Visual Studio yükleme ve Azure yığınına bağlanma | Microsoft Docs"
description: "Visual Studio'yu yüklemek ve Azure yığınına bağlanmak için gereken adımları öğrenin"
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 5487125095f05b2fbfa9489c5b4733f61c0212d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio'yu yüklemek ve Azure yığınına bağlanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Geliştir ve Dağıt Azure Resource Manager için Visual Studio'yu kullanın [şablonları](user/azure-stack-arm-templates.md) Azure yığınında. Bu makalede açıklanan adımları Visual Studio'yu yüklemek için herhangi birinden kullanabileceğiniz [Azure yığın Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ya da istemciden aracılığıyla bağlıysanız, bir Windows tabanlı dış [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Bu adımları Visual Studio 2015 Community Edition yeni bir yüklemesini gerçekleştirin. Daha fazla bilgi edinin [bir arada bulunma](https://msdn.microsoft.com/library/ms246609.aspx) diğer Visual Studio sürümleri arasında.

## <a name="install-visual-studio"></a>Visual Studio yükleme
1. İndirme ve çalıştırma [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).             
2. Arama **Microsoft Azure SDK - 2.9.6 ile Visual Studio Community 2015**, tıklatın **Ekle**, ve **yükleme**.

    ![Ekran görüntüsü, Webpı yükleme adımları](./media/azure-stack-install-visual-studio/image1.png) 

3. Kaldırma **Microsoft Azure PowerShell** Azure SDK'ın bir parçası olarak yüklenir.

    ![Azure PowerShell Ekle/Kaldır programlar arabiriminin ekran görüntüsü](./media/azure-stack-install-visual-studio/image2.png) 

4. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)

5. Yükleme tamamlandıktan sonra işletim sistemini yeniden başlatın.

## <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. Visual Studio'yu başlatın.

2. Gelen **Görünüm** menüsünde, select **Cloud Explorer**.

3. Yeni bölmesinde seçin **hesabı Ekle** ve Azure Active Directory kimlik bilgileriyle oturum açın.  
    ![Cloud Explorer ekran görüntüsü bir kez oturum açmış ve Azure yığınına bağlı](./media/azure-stack-install-visual-studio/image6.png)

Oturum açtıktan sonra [şablonlarını dağıtma](user/azure-stack-deploy-template-visual-studio.md) veya kendi şablonlarınızı oluşturmak için kullanılabilir kaynak türleri ve kaynak grupları göz atın.  

## <a name="next-steps"></a>Sonraki Adımlar

 - [Şablonları geliştirmek için Azure yığını](user/azure-stack-develop-templates.md)
