---
title: Visual Studio'yu yükleyin ve Azure Stack'e bağlanma | Microsoft Docs
description: Visual Studio'yu yükleyin ve Azure Stack'e bağlanmak için gereken adımları öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/08/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: da17d114c1ffb920fbaae85a6cdcbc35a66631a4
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258003"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio'yu yükleyin ve Azure Stack'e bağlanma

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Visual Studio yazma ve Azure Resource Manager'ı dağıtmak için kullanabileceğiniz [şablonları](azure-stack-arm-templates.md) Azure Stack için. Bu makaledeki adımlarda Visual Studio yükleme açıklayan [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya Azure Stack ile kullanmayı planlıyorsanız bir dış bilgisayara [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Visual Studio yükleme

1. İndirme ve çalıştırma [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Açık **Microsoft Web Platformu yükleyicisi**.

3. Arama **- 2.9.6 Microsoft Azure SDK'sı ile Visual Studio Community 2015**. Tıklayın **ekleme**, ve **yükleme**.

4. Kaldırma **Microsoft Azure PowerShell** Azure SDK'ın bir parçası olarak yüklenir.

    ![Ekran görüntüsü, Webpı yükleme adımları](./media/azure-stack-install-visual-studio/image1.png)

5. [Azure Stack için PowerShell'i yükleme](azure-stack-powershell-install.md)

6. Yükleme tamamlandıktan sonra işletim sistemi yeniden başlatın.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Azure AD ile Azure stack'e bağlanma

1. Visual Studio'yu başlatın.

2. Gelen **görünümü** menüsünde **Cloud Explorer**.

3. Yeni bölmesinde seçin **hesabı Ekle** ve Azure Active Directory (Azure AD) kimlik bilgilerinizle oturum açın.  

    ![Bir kez oturum açıp Azure Stack'e bağlı bulut Gezgini'nin ekran görüntüsü](./media/azure-stack-install-visual-studio/image2.png)

Oturum açtıktan sonra [şablonları dağıtma](azure-stack-deploy-template-visual-studio.md) veya kendi şablonlarınızı oluşturmak için kullanılabilir kaynak türleri ve kaynak gruplarını bulun.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>AD FS ile Azure stack'e bağlanma

1. Visual Studio'yu başlatın.

2. Gelen **Araçları**seçin **seçenekleri**.

3. Genişletin **ortam** içinde **Gezinti bölmesinde** seçip **hesapları**.

4. Seçin **Ekle**ve kullanıcı Azure Resource Manager uç noktasını girin. Azure Stack Geliştirme Seti için URL'dir: `https://management.local.azurestack/external`.  Azure Stack tümleşik sistemleri için URL'si şudur: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. **Add (Ekle)** seçeneğini belirleyin.  

    Visual Studio Azure Kaynak Yöneticisi çağırır ve kimlik doğrulama uç noktası Azure Directory Federasyon Hizmetleri için (AD FS) dahil olmak üzere, uç noktaları bulur.

    ![Bir kez oturum açıp Azure Stack'e bağlı bulut Gezgini'nin ekran görüntüsü](./media/azure-stack-install-visual-studio/image6.png)

6. Seçin **Cloud Explorer** gelen **görünümü** menüsü.

7. Seçin **hesabı Ekle** ve AD FS kimlik bilgilerinizle oturum açın.  

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image7.png)

    Kullanılabilir abonelikler cloud Explorer sorgular. Yönetmek için mevcut bir aboneliği seçebilirsiniz.

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image8.png)

8. Var olan kaynakları, kaynak grupları göz atın veya şablonları dağıtma.

## <a name="next-steps"></a>Sonraki adımlar

- Visual Studio hakkında daha fazla bilgiyi [yan yana](/visualstudio/install/install-visual-studio-versions-side-by-side) diğer Visual Studio sürümleriyle birlikte.
- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md).