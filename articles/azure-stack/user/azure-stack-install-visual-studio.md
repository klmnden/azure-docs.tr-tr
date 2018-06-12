---
title: Visual Studio yükleme ve Azure yığınına bağlanma | Microsoft Docs
description: Visual Studio'yu yüklemek ve Azure yığınına bağlanmak için gereken adımları öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: unknown
ms.openlocfilehash: cbd5e5dbcdd2565e8066b0721f45863569bfd90a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295039"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio'yu yüklemek ve Azure yığınına bağlanın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Visual Studio yazma ve Azure Resource Manager dağıtmak için kullanabileceğiniz [şablonları](azure-stack-arm-templates.md) Azure yığınına. Bu makaledeki adımları, Visual Studio yüklenmesinde size yol [Azure yığın](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), veya Azure yığınından planlıyorsanız, dış bir bilgisayarda [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Visual Studio yükleme

1. İndirme ve çalıştırma [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Açık **Microsoft Web Platformu yükleyicisi**.

3. Arama **Microsoft Azure SDK - 2.9.6 ile Visual Studio Community 2015**. Tıklatın **ekleme**, ve **yükleme**.

4. Kaldırma **Microsoft Azure PowerShell** Azure SDK'ın bir parçası olarak yüklenir.

    ![Ekran görüntüsü, Webpı yükleme adımları](./media/azure-stack-install-visual-studio/image1.png) 

5. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)

6. Yükleme tamamlandıktan sonra işletim sistemini yeniden başlatın.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Azure AD ile Azure yığın bağlanın

1. Visual Studio'yu başlatın.

2. Gelen **Görünüm** menüsünde, select **Cloud Explorer**.

3. Yeni bölmesinde seçin **hesabı Ekle** ve Azure Active Directory (Azure AD) kimlik bilgileriyle oturum açın.  

    ![Cloud Explorer ekran görüntüsü bir kez oturum açmış ve Azure yığınına bağlı](./media/azure-stack-install-visual-studio/image2.png)

Oturum açtıktan sonra [şablonlarını dağıtma](azure-stack-deploy-template-visual-studio.md) veya kendi şablonlarınızı oluşturmak için kullanılabilir kaynak türleri ve kaynak grupları göz atın.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>AD FS ile Azure yığın bağlanın

1. Visual Studio'yu başlatın.

2. Gelen **Araçları**seçin **seçenekleri**.

3. Genişletme **ortam** içinde **Gezinti Bölmesi** seçip **hesapları**.

4. Seçin **Ekle**ve kullanıcı Azure Kaynak Yöneticisi uç noktası girin.  
  Azure yığın Geliştirme Seti için URL'si şöyledir: `https://management.local.azurestack/external`.  
  Azure yığın sistemleri URL tümleşik aranır: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. **Add (Ekle)** seçeneğini belirleyin.  

    Visual Studio Azure Kaynak Yöneticisi çağırır ve Azure Directory Federasyon hizmetlerini (AD FS) kimlik doğrulama uç noktası dahil olmak üzere uç noktaları bulur.

    ![Cloud Explorer ekran görüntüsü bir kez oturum açmış ve Azure yığınına bağlı](./media/azure-stack-install-visual-studio/image6.png)

6. Seçin **Cloud Explorer** gelen **Görünüm** menüsü.
7. Seçin **hesabı Ekle** ve AD FS kimlik bilgileriyle oturum açın.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer kullanılabilir abonelikleri sorgular. Yönetmek için kullanılabilir bir aboneliği seçebilirsiniz.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Gözatma mevcut kaynakları, kaynak grupları veya şablonları dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

 - Daha fazla bilgi edinin [bir arada bulunma](https://msdn.microsoft.com/library/ms246609.aspx) diğer Visual Studio sürümleriyle.
 - [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)