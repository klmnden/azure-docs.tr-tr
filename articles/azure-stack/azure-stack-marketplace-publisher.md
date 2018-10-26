---
title: Oluşturma ve Market öğesi yayımlama Market araç setini kullanma | Microsoft Docs
description: Market öğesi yayımlama araç seti ile hızlı bir şekilde oluşturmayı öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 98a7ae31aeeae29b81a1053b38992d3bc4e0e03f
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085318"
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>Market öğesi yayımlama aracını kullanarak Ekle

İçeriklerinizi ekleme [Azure Stack Marketini](azure-stack-marketplace.md) çözümlerinizi siz ve kiracılarınız için dağıtım yapar. Market araç seti, Iaas Azure Resource Manager şablonları veya VM uzantılarını temel alan Azure Marketi'nde paket (.azpkg) dosyaları oluşturur. Market Araç Seti .azpkg dosyaları aracıyla oluşturulan ya da kullanarak yayımlamak için de kullanabilirsiniz [el ile](azure-stack-create-and-publish-marketplace-item.md) adımları. Bu konuda, ardından bu öğeyi Azure Stack Market'te yayımlama aracını indirme ve bir VM şablonu temel alan bir Market öğesi oluşturma açıklanmaktadır.     

## <a name="prerequisites"></a>Önkoşullar

 - Azure Stack ana bilgisayarda Araç Seti çalıştırma veya sahip [VPN](.\asdk\asdk-connect.md#connect-with-vpn) ASDK konağa bağlantısı Aracı çalıştırdığınız makine.

 - İndirme [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) ve ayıklayın.

 - İndirme [Azure Galerisi paketleme aracı](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe). 

 - Market'te yayımlama simgeleri ve küçük resim dosyası gerektirir. Kendi kullanmak veya kaydetmek [örnek](azure-stack-marketplace-publisher.md#support-files) Bu örnek için yerel dosyaları.

## <a name="download-the-tool"></a>Aracı'nı indirme

Market araç setini karşıdan yükleyebileceğiniz [Azure Stack araçları deposundan](azure-stack-powershell-download.md).

##  <a name="create-marketplace-items"></a>Market öğesi oluşturma

Bu bölümde, Market Araç Seti .azpkg biçiminde bir Market öğesi paketi oluşturmak için kullanın.  

### <a name="provide-marketplace-information-with-wizard"></a>Sihirbazı ile Market bilgileri sağlayın

1. Market araç seti, bir PowerShell oturumundan çalıştırın:
   ```PowerShell
   .\MarketplaceToolkit.ps1
   ```

2. Tıklayın **çözüm** sekmesi. Bu ekran, Market öğesi hakkında bilgi kabul eder. Market'te görünmesini istediğiniz şekilde öğeniz hakkında bilgi girin. Ayrıca belirtebileceğiniz bir [parametre dosyasını](azure-stack-marketplace-publisher.md#use-a-parameters-file) form önceden doldurmak için.  
    
    ![Market Araç Seti ilk ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image7.png)
3. Tıklayın **Gözat** ve her bir simge ve ekran alan için bir resim dosyası seçin. Kendi simgeleri veya örnek simgeleri kullanabilirsiniz [destek dosyalarını](azure-stack-marketplace-publisher.md#support-files) bölümü.
4. Tüm alanları doldurulduktan sonra Market dahilindeki çözüm önizlemesi için "Önizleme Çözüm"'ı seçin. Gözden geçirin ve metin, resimler ve ekran tıklamadan önce Düzenle **sonraki**.  

### <a name="import-template-and-create-package"></a>Şablonu içeri aktarma ve paket oluşturma

Bu bölümde, şablonu içeri aktarma ve çözümünüz için giriş ile çalışır.

1.  Tıklayın **Gözat** seçip *azuredeploy.json* indirilen şablonları basit Windows VM 101 klasöründeki.

    ![Market Araç Seti ikinci ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image8.png)
2.  Dağıtım Sihirbazı ile doldurulan bir *temel* adım ve giriş öğelerini şablonunda belirtilen her parametre için. Başka adımlar da eklemek ve girişleri adımlar arasında taşıyın. Örneğin, çözümünüz için "Ön uç yapılandırması" ve "Arka uç yapılandırması" adımları isteyebilirsiniz.
3.  AzureGalleryPackager.exe yolunu belirtin.  
4.  **Oluştur**’a tıklayın. Market araç seti, çözümünüzün bir .azpkg dosyasında paketler. Tamamlandığında, Sihirbazı, çözüm dosyanızı yolunu görüntüler ve Azure Stack'e paketinizi yayımlamaya devam seçeneği sunar.

## <a name="publish-marketplace-items"></a>Market öğesi yayımlama

Bu bölümde, Azure Stack Marketini için Market öğesi yayımlama.

![Market Araç Seti ilk ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image9.png)

1.  Sihirbaz, çözümünüzü yayımlamak için gerekli bilgiyi gerektirir:
    
    |Alan|Açıklama|
    |-----|-----|
    | Hizmet Yöneticisi adı | Hizmet Yöneticisi hesabını.  Örnek: ServiceAdmin@mydomain.onmicrosoft.com |
    | Parola | Hizmet Yöneticisi hesabının parolası. |
    | API Uç Noktası | Azure Stack Azure Resource Manager uç noktası. Örnek: management.local.azurestack.external |
2.  Tıklayın **Yayımla** ve yayımlama günlük görüntülenir.
3.  Şimdi Azure Stack portal aracılığıyla yayımlanan öğeniz dağıtabilir.

## <a name="use-a-parameters-file"></a>Bir parametre dosyası kullanma

Market öğesi bilgisini tamamlamak için bir parametre dosyası da kullanabilirsiniz.  

Market Araç Seti içeren bir *solution.parameters.ps1* kendi parametre dosyasını oluşturmak için kullanabilirsiniz.

## <a name="support-files"></a>Destek dosyaları

| Açıklama | Örnek |
| ----- | ----- |
| 40 x 40 .png simgesi | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| 90 x 90 .png simgesi | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| 115 x 115 .png simgesi | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| 255 x 115 .png simgesi | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| 533 x 324 .png küçük resmi | ![](./media/azure-stack-marketplace-publisher/image5.png) |

## <a name="next-steps"></a>Sonraki adımlar

[Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
[Bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)