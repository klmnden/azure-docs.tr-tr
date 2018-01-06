---
title: "Azure yığın genel bakış güncelleştirmelerini yönetme | Microsoft Docs"
description: "Güncelleştirme yönetimi için Azure tümleşik yığını sistemleri hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 23b05909bda7785b45aeaeed0bd75a90de9ffe50
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="manage-updates-in-azure-stack-overview"></a>Azure yığın genel bakış güncelleştirmelerini yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Microsoft Azure tümleşik yığını genellikle her ay dördüncü Salı günü döner normal bir tempoyla sistemlerde için güncelleştirme paketleri genel kullanılabilirliğine başlangıç serbest bırakır. Kuruluşunuz güncelleştirme bildirimlerini erişim sağlamak için belirli bildirim işlemi hakkında OEM isteyin veya Overview\Release Notes\Integrated sistemlerinde sürüm notları belirli sürümler hakkında daha fazla bilgi için burayı tıklatın.

Microsoft yazılım güncelleştirmeleri her bir sürümü tek güncelleştirme paketi olarak gelir. Bir Azure yığın işleç olarak kolayca içeri aktarabilirsiniz, güncelleştirme paketleri Yönetici portalı'ndan yükleyin ve bu yükleme ilerleyişini izleyin. 

Özgün donatım üreticisi (OEM) donanım satıcınıza güncelleştirmeler, sürücü ve bellenim güncelleştirmeleri gibi de serbest bırakır. Bu güncelleştirmeler ayrı paketler OEM donanım satıcınız tarafından teslim edilir ve Microsoft güncelleştirmelerini ayrı olarak yönetilir.

Sisteminizi destek kapsamında tutmak için Azure özel sürüm düzeyi için güncelleştirilmiş yığın tutmanız gerekir. Gözden geçirmenizi emin olun [Azure ilke bakım yığını](azure-stack-servicing-policy.md).

> [!NOTE]
> Azure yığın Geliştirme Seti için Azure yığın güncelleştirme paketleri uygulanamıyor. Güncelleştirme paketleri tümleşik sistemler için tasarlanmıştır.

## <a name="the-update-resource-provider"></a>Güncelleştirme kaynak sağlayıcısı

Azure yığın uygulama Microsoft yazılım güncelleştirmeleri düzenler bir güncelleştirme kaynak sağlayıcısı içerir. Bu kaynak sağlayıcısı, güncelleştirmeleri tüm fiziksel ana bilgisayarların, Service Fabric uygulamaları ve çalışma zamanları ve tüm altyapı sanal makineleri ve ilişkili hizmetler arasında uygulanması sağlanır.

Güncelleştirmeleri yüklemek gibi güncelleştirme işlemi hedefleri çeşitli alt sistemleri Azure yığınında (örneğin, fiziksel ana bilgisayarlar ve altyapı sanal makineler) olarak kolayca üst düzey durumunu görüntüleyebilirsiniz.

## <a name="plan-for-updates"></a>Güncelleştirmeleri planlama

Bakım işlemleri kullanıcılara bildirmek ve iş saatleri sırasında mümkün olduğunca normal bakım pencereleri zamanlama öneririz. Bakım işlemleri, Kiracı İş yükleri ve portal işlemlerini etkileyebilir.

## <a name="using-the-update-tile-to-manage-updates"></a>Güncelleştirmelerini yönetmek için güncelleştirme döşeme kullanma
Yönetici portalı'ndan güncelleştirmeleri yönetme basit bir işlemdir. Bir Azure yığın işleç panoya güncelleştirme parçasında gidebilirsiniz:

- geçerli sürümü gibi önemli bilgileri görüntüleyin.
- güncelleştirmeleri yüklemek ve ilerleme durumunu izleyin.
- Güncelleştirme geçmişini daha önce yüklenen güncelleştirmeleri gözden geçirin.
 
## <a name="determine-the-current-version"></a>Geçerli sürüm belirleme

Güncelleştirme döşeme Azure yığın geçerli sürümünü gösterir. Yönetici portalı'nda aşağıdaki yöntemlerden birini kullanarak güncelleştirme döşemenin alabilirsiniz:

- Geçerli sürümde Panoda görüntülemek **güncelleştirme** döşeme.
 
   ![Varsayılan Panoda güncelleştirmeleri döşeme](./media/azure-stack-updates/image1.png)
 
- Üzerinde **bölge Yönetimi** döşeme, bölge adını tıklatın. Geçerli sürümde görüntülemek **güncelleştirme** döşeme.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke bakım azure yığını](azure-stack-servicing-policy.md) 
- [Azure yığınında bölge Yönetimi](azure-stack-region-management.md)     


