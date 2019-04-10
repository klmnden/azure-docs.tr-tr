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
ms.date: 04/04/2019
ms.author: mabrigg
ms.lastreviewed: 04/04/2019
ms.reviewer: justini
ms.openlocfilehash: bf797404b173d6febe133eacbb9d36310160aff6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59281582"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Azure Stack genel bakış güncelleştirmelerini yönetme

*Şunlara uygulanır Azure Stack tümleşik sistemleri*

Azure Stack tümleşik sistemleri genellikle için Microsoft güncelleştirme paketlerini her ayın dördüncü Salı geçici olarak bırakın. Özgün ekipman üreticisi (OEM) güncelleştirme bildirimlerini kuruluşunuz ulaştığından emin olmak için belirli bildirim süreci hakkında isteyin. Bu belge kitaplığındaki altında de göz atabilirsiniz **genel bakış** > **sürüm notları** etkin desteği olan sürümler hakkında daha fazla bilgi için. 

Her bir sürümü Microsoft yazılım güncelleştirmeleri bir tek güncelleştirme paketi olarak gelir. Azure Stack operatör olarak içeri aktarabilirsiniz, yükleme ve yükleme ilerleme durumunu bu İzleyici, yönetici portalından paketleri güncelleştirin. 

Ayrıca, OEM satıcısı güncelleştirmeler, sürücü ve bellenim güncelleştirmeleri gibi serbest bırakır. Bu güncelleştirmeleri satıcısı tarafından ayrı paketler halinde dağıtılır, ancak bunlar içeri aktarıldı, yüklü ve güncelleştirme paketlerini Microsoft gelen aynı şekilde yönetilir.

Sisteminizi desteği altında tutmak için Azure Stack belirli sürüm düzeyi için güncelleştirilmiş tutmanız gerekir. Gözden geçirmenizi emin [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md).

> [!NOTE]
> Azure Stack Geliştirme Seti için Azure Stack güncelleştirme paketleri uygulanamıyor. Güncelleştirme paketlerini tümleşik sistemler için tasarlanmıştır. Bilgi için [ASDK yeniden](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>Güncelleştirme kaynağı sağlayıcısı

Azure yığını, Microsoft yazılım güncelleştirmeleri uygulama işleyen bir güncelleştirme kaynak sağlayıcısı içerir. Bu sağlayıcı güncelleştirmeleri, tüm fiziksel konaklar, Service Fabric uygulamaları ve çalışma zamanları ve tüm altyapı sanal makineleri ve bunların ilişkili hizmetler arasında uygulandığını denetler.

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
- Bulutun geçerli OEM Paket sürümü görüntüle
 
## <a name="determine-the-current-version"></a>Geçerli sürümünü belirleme

Azure yığını'nın geçerli sürümü güncelleştirme kutucuğunda görüntüleyebilirsiniz. Kutucuğu açmak için:

1. Azure Stack Yönetim Portalı'nı açın.
2. Seçin **Pano**. İçinde **güncelleştirme** kutucuğu, geçerli sürümü listelenir. 

    ![Güncelleştirmeleri varsayılan pano kutucuk](./media/azure-stack-updates/image1.png)

    Örneğin, ekranda 1.1903.0.35 sürümüdür.

## <a name="install-updates-and-monitor-progress"></a>Güncelleştirmeleri yüklemek ve ilerleme durumunu izleyin


1. Azure Stack Yönetim Portalı'nı açın.
2. Seçin **Pano**. Güncelleştirme kutucuğu seçin.
3. Seçin **Şimdi Güncelleştir**.

    ![Azure Stack güncelleştirme çalıştırma ayrıntıları](media/azure-stack-updates/azure-stack-update-button.png)

4.  Çeşitli alt sistemlerde Azure Stack'te güncelleştirme işlemini gezinir gibi üst düzey durumunu görüntüleyebilirsiniz. Alt sistemlerin fiziksel ana bilgisayarları, Service Fabric, altyapı sanal makineleri içerir ve Hizmetleri örneği yönetici ve kullanıcı portalı sağlar. Güncelleştirme işlemi boyunca güncelleştirme kaynak sağlayıcısı başarılı olduğunda adım sayısı yanı sıra, devam eden sayısı gibi güncelleştirme hakkında ayrıntılı raporlar.

5. Seçin **tam günlükleri indirmek** tam günlükleri indirmek için ayrıntıları dikey penceresinde güncelleştirmenin çalıştırın.

    ![Azure Stack güncelleştirme çalıştırma ayrıntıları](media/azure-stack-updates/update-run-details.png)

6. Tamamlandığında, güncelleştirmeyi kaynak sağlayıcısı sağlar bir **başarılı** güncelleştirme işleminin tamamlandığını bildirmek için onay ve ne kadar sürdüğünü. Burada, tüm güncelleştirmeleri, kullanılabilir güncelleştirmeleri veya filtreyi kullanarak yüklü güncelleştirmeler hakkında bilgi görüntüleyebilirsiniz.

    ![Azure Stack Güncelleştirme ayrıntıları başarılı çalıştırma](media/azure-stack-updates/update-success.png)

   Güncelleştirme başarısız olursa, raporları kutucuk güncelleştirme **dikkat etmeniz gereken**. Kullanım **tam günlükleri indirmek** burada güncelleştirme başarısız olmuş bir üst düzey durumunu almak için. Azure Stack günlük toplama, tanılama ve sorun giderme kolaylaştırmak yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack hizmet İlkesi](azure-stack-servicing-policy.md) 
- [Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)
