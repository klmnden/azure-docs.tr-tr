---
title: Azure Stack doğrulama en iyi yöntemler | Microsoft Docs
description: Bu makalede, bir hizmet olarak doğrulama kullanmak için en iyi uygulamalar açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: a90ec05fa3224033436830e7666c7eb81ff881f6
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247002"
---
# <a name="create-an-oem-package"></a>OEM paketi oluşturma

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack OEM uzantı paketi, hangi OEM tarafından kullanılmak üzere güncelleştirme genişletme ve alan değiştirme gibi işletimsel işlemlerin yanı sıra, dağıtım, Azure Stack altyapısının belirli içerik eklenir yönelik mekanizmadır.

## <a name="creating-the-package"></a>Paket oluşturuluyor

Oluşturup doğrulanmış OEM uzantı paketi VaaS içinde kullanılabilir.  Devam etmeden önce adımları tamamladığınızdan emin olun [OEM paket oluşturma](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true). Paket, ardından VaaS test sonuçları çözüm doğrulama iş akışı içinde imzalamak için birlikte Microsoft'a gönderilir. Aşağıdaki adımlarda oluşturulan dosyalarının VaaS tüketebileceği bir tek bir ZIP dosyasına nasıl gruplanacağını ayrıntılı olarak açıklanmaktadır.

1. Aşağıdaki içerik paketi için tanımlayın:
    - Adlı bir yürütülebilir dosya `<Publisher>-<Model>-<Version>.exe`
    - Bir veya daha fazla dosyaları ikili dosyalar adlı `<Publisher><Model>-<Version>-#.bin`, # 1'den başlayarak sıralı bir numarası olduğu yer. İkili dosyaların sayısı, toplam paket içeriğini boyutuna bağlıdır.
    - Adlı bir bildirim dosyası `oemMetadata.xml`, olacağı paket içeriğini kökünde metadata.xml dosyaya içerik olarak aynı.

2. İçerik dosyaları seçin ve içeriğini bir zip dosyası oluşturun:

    ![ZIP dosyasının içeriğini](media/vaas-create-oem-package-1.png) ![öğesi içeriği sıkıştır](media/vaas-create-oem-package-2.png)

3. Sonuç dosyası tanımlamak için açıklayıcı kadar olacak şekilde yeniden adlandırın.

## <a name="verifying-the-contents"></a>İçeriği doğrulanıyor

Zip dosyanızın yapısı doğrulamak için bu inceleme ve hiçbir alt klasör olduğunu kontrol edin. Sıkıştırılmış içerik TLD sahiptir. Geçerli paket yapısı aşağıda gösterilmiştir.
> [!IMPORTANT]
> Üst klasör içeriklerini yerine sıkıştırma paketi imzalama başarısız olmasına neden olur.

![Düzgün sıkıştırılmış paket içeriği](media/vaas-create-oem-package-3.png)

Zip dosyası artık VaaS için karşıya yüklenen ve çözüm doğrulama iş akışında Microsoft tarafından imzalanmış.

## <a name="next-steps"></a>Sonraki adımlar

- [OEM paketi doğrulama](azure-stack-vaas-validate-oem-package.md)
