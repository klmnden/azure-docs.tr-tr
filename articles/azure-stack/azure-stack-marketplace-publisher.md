---
title: "Oluşturma ve yayımlama Market öğesi Market araç kullanma | Microsoft Docs"
description: "Market öğesi ile Araç Seti yayımlama hızlı bir şekilde oluşturmayı öğrenin"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/14/2017
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 61ee3296429f9641643f1c9268ae89e3691fcfa1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>Yayımlama aracını kullanarak Market öğesi ekleme
İçeriğinizi ekleme [Azure yığın Market](azure-stack-marketplace.md) çözümlerinizi sizin ve kiracılarınızın dağıtımı için kullanılabilir yapar.  Market araç seti, Iaas Azure Resource Manager şablonları veya VM uzantıları göre Azure Market paketleri (.azpkg) dosyaları oluşturur.  Market Araç Seti .azpkg dosyaları, aracı ile oluşturulmuş veya kullanarak yayımlamak için de kullanabilirsiniz [el ile](azure-stack-create-and-publish-marketplace-item.md) adımları.  Bu konu, aracı yükleme, bir VM şablonunu temel alan bir Market öğesi oluşturma ve ardından bu öğeyi Azure yığın Marketinde yayımlama kılavuzluk eder.     


## <a name="prerequisites"></a>Önkoşullar
 - Araç Seti Azure yığın ana bilgisayarda çalıştırmak veya sahip [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) aracını çalıştırdığınız makineden bağlantısını.

 - Karşıdan [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) ve ayıklayın.

 - Karşıdan [Azure Galerisi paketleme aracı](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe). 

 - Market yayımlamayı simgelerini ve küçük resim dosyası gerektirir.  Kendi kullanmak veya kaydetmek [örnek](azure-stack-marketplace-publisher.md#support-files) dosyaları yerel olarak bu örneğin.

## <a name="download-the-tool"></a>Aracı'nı indirme
Market Araç Seti olabilir [Azure yığın araçları depodaki indirilen](azure-stack-powershell-download.md).


##  <a name="create-marketplace-items"></a>Market öğesi oluşturma
Bu bölümde, Market Araç Seti .azpkg biçiminde bir Market öğesi paketi oluşturmak için kullanın.  

### <a name="provide-marketplace-information-with-wizard"></a>Sihirbazı ile Market bilgileri sağlayın
1. Market Araç Seti PowerShell oturumu çalıştırın:
```PowerShell
    .\MarketplaceToolkit.ps1
```

2. Tıklatın **çözüm** sekmesi.  Bu ekran, Market öğesi hakkında bilgi kabul eder. Market görünmesini istediğiniz şekilde öğenizi hakkında bilgi girin.  Ayrıca belirtebilirsiniz bir [parametreler dosyası](azure-stack-marketplace-publisher.md#use-a-parameters-file) formun önceden doldurmak için.  
    
    ![Market Araç Seti ilk ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image7.png)
3. Tıklatın **Gözat** ve her bir simge ve ekran alan için bir görüntü dosyası seçin.  Kendi simgeler veya örnek simgeleri kullanabilirsiniz [destek dosyalarını](azure-stack-marketplace-publisher.md#support-files) bölümü.
4. Tüm alanlar doldurulduktan sonra "Önizleme çözümü" Market çözümünüzde önizlemesi için seçin.  Gözden geçirme ve metin, görüntüler ve ekran'ı tıklatmadan önce Düzenle **sonraki**.  

### <a name="import-template-and-create-package"></a>Şablonu içeri aktarın ve paketi oluşturma
Bu bölümde, şablonu içeri aktarın ve çözümünüz için giriş ile çalışır.

1.  Tıklatın **Gözat** seçip *azuredeploy.json* indirilen şablonlarındaki 101-basit-Windows-VM klasöründen.

    ![Market Araç Seti ikinci ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image8.png)
2.  Dağıtım Sihirbazı'nı doldurulur bir *temel* şablonda belirtilen her parametre için adım ve giriş öğeleri.  Başka adımlar da eklemek ve girişleri adımları arasında taşıyın.  Örnek olarak, çözümünüz için "Ön uç yapılandırma" ve "Arka uç yapılandırması" adımlarını isteyebilirsiniz.
3.  AzureGalleryPackager.exe yolunu belirtin.  
4.  Tıklatın **oluşturma** ve Market araç seti, çözümünüzün bir .azpkg dosyasında paketler.  Tamamlandıktan sonra sihirbaz, çözüm dosya yolunu görüntüler ve paketinizin Azure yığınına yayımlamaya devam seçeneği sunar.


## <a name="publish-marketplace-items"></a>Market öğesi yayımlama
Bu bölümde, Market öğesi, Azure yığın Marketinde yayımlama.

![Market Araç Seti ilk ekranının ekran görüntüsü](./media/azure-stack-marketplace-publisher/image9.png)

1.  Sihirbaz, çözüm yayımlamaya bilgileri gerektirir:
    
    |Alan|Açıklama|
    |-----|-----|
    | Hizmet Yöneticisi adı | Hizmet yöneticisi hesabı.  Örnek:  ServiceAdmin@mydomain.onmicrosoft.com |
    | Parola | Hizmet yönetici hesabının parolası. |
    | API Endpoint | Azure yığın Azure Resource Manager uç noktası.  Örnek: management.local.azurestack.external |
2.  Tıklatın **Yayımla** ve yayımlama günlük görüntülenir.
3.  Artık Azure yığın Portalı aracılığıyla yayımlanan öğenizi dağıtamaz.


## <a name="use-a-parameters-file"></a>Bir parametre dosyası kullanın
Market öğesi bilgisi tamamlamak için bir parametre dosyası da kullanabilirsiniz.  

Market Araç Seti içeren bir *solution.parameters.ps1* kendi parametreler dosyası oluşturmak için kullanabilirsiniz.


## <a name="support-files"></a>Destek dosyaları
| Açıklama | Örnek |
| ----- | ----- |
| 40x40 .png icon | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| 90 x 90 .png simgesi | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| 115x115 .png icon | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| 255x115 .png icon | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| 533 x 324 .png küçük resim | ![](./media/azure-stack-marketplace-publisher/image5.png) |


