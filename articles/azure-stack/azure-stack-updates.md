---
title: Azure Stack genel bakış güncelleştirmeleri yönetme | Microsoft Docs
description: Güncelleştirme yönetimi için Azure Stack tümleşik sistemleri hakkında bilgi edinin.
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
ms.date: 01/22/2019
ms.author: mabrigg
ms.lastreviewed: 01/22/2019
ms.openlocfilehash: a5fe2d9b295a72b8e2e3adb7e5cffd5a98c03070
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487524"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Azure Stack genel bakış güncelleştirmelerini yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri*

Azure Stack tümleşik sistemleri genellikle için Microsoft güncelleştirme paketlerini her ayın dördüncü Salı geçici olarak bırakın. OEM güncelleştirme bildirimlerini kuruluşunuz ulaştığından emin olmak için belirli bildirim süreci hakkında isteyin. Bu belge kitaplığındaki altında de göz atabilirsiniz **genel bakış** > **sürüm notları** etkin desteği olan sürümler hakkında daha fazla bilgi için. 

Her bir sürümü Microsoft yazılım güncelleştirmeleri bir tek güncelleştirme paketi olarak gelir. Azure Stack operatör olarak içeri aktarabilirsiniz, yükleme ve yükleme ilerleme durumunu bu İzleyici, yönetici portalından paketleri güncelleştirin. 

Özgün ekipman üreticisi (OEM) donanım satıcınıza güncelleştirmeler, sürücü ve bellenim güncelleştirmeleri gibi da serbest bırakır. OEM donanım satıcınız tarafından ayrı paketler halinde teslim edilen bu güncelleştirmeler, Microsoft tarafından yayımlanan güncelleştirme paketleri ile aynı şekilde içeri aktarılır, yüklenir ve yönetilir.

Sisteminizi desteği altında tutmak için Azure Stack belirli sürüm düzeyi için güncelleştirilmiş tutmanız gerekir. Gözden geçirmenizi emin [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).

> [!NOTE]
> Azure Stack Geliştirme Seti için Azure Stack güncelleştirme paketleri uygulanamıyor. Güncelleştirme paketlerini tümleşik sistemler için tasarlanmıştır. Bilgi için [ASDK yeniden](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>Güncelleştirme kaynağı sağlayıcısı

Azure yığını, Microsoft yazılım güncelleştirmeleri uygulama düzenleyen bir güncelleştirme kaynak sağlayıcısı içerir. Bu kaynak sağlayıcısını güncelleştirmeleri tüm fiziksel konaklar, Service Fabric uygulamaları ve çalışma zamanları ve tüm altyapı sanal makineleri ve bunların ilişkili hizmetler arasında uygulanmasını sağlar.

Güncelleştirmeleri yüklemek gibi çeşitli alt sistemlerde (örneğin, fiziksel ana bilgisayarlar ve altyapı sanal makinelerine) Azure Stack'te güncelleştirme işlemi hedefleri olarak üst düzey durumunu görüntüleyebilirsiniz.

## <a name="plan-for-updates"></a>Güncelleştirmelerini planlama

Herhangi bir bakım işlemi kullanıcılara bildirmek ve, normal bakım pencereleri çalışma saatleri sırasında mümkünse zamanlamanızı öneririz. Bakım işlemleri hem Kiracı iş yüklerini ve portal işlemlerini etkileyebilir.

- Bu güncelleştirme yüklemesi başlamadan önce çalıştırması [Test AzureStack](azure-stack-diagnostic-test.md) bulunan tüm çalışma sorunlarını çözün ve Azure Stack durumunu doğrulamak için aşağıdaki parametreleri, tüm uyarılar ve hatalar dahil olmak üzere. Ayrıca etkin Uyarıları gözden geçirin ve eylemi gerektiren tüm çözümleyin.  

  ```powershell
  Test-AzureStack -Group UpdateReadiness
  ``` 

## <a name="using-the-update-tile-to-manage-updates"></a>Güncelleştirmeleri yönetmek için kutucuğu Güncelleştir'ı kullanma
Yönetici portalı'ndan güncelleştirmeleri yönettiğiniz. Azure Stack operatörü panoya, kutucuğu güncelleştir kullanabilirsiniz:

- geçerli sürümü gibi önemli bilgileri görüntüleyin.
- güncelleştirmeleri yüklemek ve ilerleme durumunu izleyebilirsiniz.
- Güncelleştirme geçmişini daha önce yüklenen güncelleştirmeleri gözden geçirin.
 
## <a name="determine-the-current-version"></a>Geçerli sürümünü belirleme

Güncelleştirme kutucuğu, Azure yığını'nın geçerli sürümü gösterir. Yönetici portalı'nda aşağıdaki yöntemlerden birini kullanarak güncelleştirme kutucuğu alabilirsiniz:

- Geçerli sürümde Panoda görüntüleme **güncelleştirme** Döşe.
 
   ![Güncelleştirmeleri varsayılan pano kutucuk](./media/azure-stack-updates/image1.png)
 
- Üzerinde **bölge Yönetimi** kutucuğunda, bölge adına tıklayın. Geçerli sürümde görüntülemek **güncelleştirme** Döşe.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack hizmet ilkesi](azure-stack-servicing-policy.md) 
- [Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)     


