---
title: "Sistem durumu ve Uyarıları Azure yığınında izleme | Microsoft Docs"
description: "Sistem durumu ve Uyarıları Azure yığınında izleme öğrenin."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 69901c7b-4673-4bd8-acf2-8c6bdd9d1546
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: twooley
ms.openlocfilehash: cf454a438f088d8079352ac60ce845185b741327
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Sistem durumu ve Uyarıları Azure yığınında izleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın altyapı izleme durumunu ve bir Azure yığın bölge için uyarıları görüntülemek sağlayan özellikler içerir. **Bölge Yönetimi** varsayılan Yönetici portalı'nda varsayılan sağlayıcı abonelik için sabitlenmiş kutucuğu, Azure yığınının dağıtılan tüm bölgelere listeler. Döşeme her bölge için etkin kritik ve uyarı uyarı sayısını gösterir ve uyarı işlevselliği Azure yığınının ve sistem durumu giriş noktanızdır.

 ![Bölge Yönetimi döşeme](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Sistem durumu Azure yığınında anlama

 Sistem durumu ve Uyarıları sistem durumu kaynak sağlayıcısı tarafından yönetilir. Azure yığın altyapı bileşenlerini Azure yığın dağıtım ve yapılandırma sırasında sistem kaynak sağlayıcısı ile kaydedin. Bu kayıt durumu ve her bileşen için uyarılar görünümünü sağlar. Sistem durumu Azure yığınında basit bir kavramdır. Bir bileşenin kayıtlı bir örneği için uyarıları varsa, bu bileşenin sistem durumu kötü etkin uyarı önem derecesini yansıtır; Uyarı veya kritik.
 
 ## <a name="view-and-manage-component-health-state"></a>Görüntüleme ve bileşenin sağlık durumunu yönetme
 
 Bir Azure yığın operatör olarak, Yönetici portalı'nda ve REST API ve PowerShell aracılığıyla bileşenleri sistem durumunu görüntüleyebilirsiniz.
 
Portalda sistem durumu görüntülemek için görüntülemek istediğiniz bölgeyi tıklatın **bölge Yönetimi** döşeme. Sistem durumu altyapı rollerini ve kaynak sağlayıcıları görüntüleyebilirsiniz.

![Altyapı rollerinin listesi](media/azure-stack-monitor-health/image2.png)

Bir kaynak sağlayıcısı veya altyapı rolü daha ayrıntılı bilgileri görüntülemek için tıklatabilirsiniz.

> [!WARNING]
>Bir altyapı rolü ve rol örneği'ye tıklayın, seçenekleri başlatmak için yeniden başlatma veya kapatma vardır. Tümleşik bir sistem güncelleştirmeleri uyguladığınızda, bu eylemleri kullanmayın. Ayrıca, yapmak **değil** Azure yığın Geliştirme Seti ortamında bu seçenekleri kullanın. Bu seçenekler yalnızca bir tümleşik sistemleri ortamı için tasarlanmıştır altyapı rol başına birden fazla rol örneği burada. Development Kit'te (özellikle AzS-Xrp01) rol örneği yeniden sistem kararsızlığına neden olur. Sorununuz için sorun giderme Yardımı için post [Azure yığın Forumu](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Uyarıları görüntüleme

Her Azure yığın bölge için etkin uyarıların listesi doğrudan kullanılabilir **bölge Yönetimi** dikey. Varsayılan yapılandırmada ilk kutucuğunun **uyarıları** döşeme, kritik özetini ve bölge için uyarı uyarıları görüntüler. Panoya hızlı erişim için bu dikey penceresinde herhangi bir kutucuğu gibi uyarıları kutucuğu sabitleyebilirsiniz.   

![Bir uyarı gösterir döşeme uyarıları](media/azure-stack-monitor-health/image3.png)

Üst kısmında, seçerek **uyarıları** kutucuğu, gidin bölge için tüm etkin uyarıların listesi. Ya da seçerseniz **kritik** veya **uyarı** kalem döşeme içinde uyarıları (kritik veya uyarı), filtrelenmiş listesine gidin. 

![Filtrelenmiş uyarı bildirimleri](media/azure-stack-monitor-health/image4.png)
  
**Uyarıları** dikey durumu (etkin veya kapalı) ve önem derecesi (kritik veya uyarı) üzerinde filtreleme yeteneği destekler. Varsayılan görünüm, tüm etkin uyarıları görüntüler. Tüm kapatılan uyarılar sistemden yedi gün sonra kaldırılır.

![Filtre bölmesi Filtresi tarafından kritik veya uyarı durumu](media/azure-stack-monitor-health/image5.png)

**Görünüm API** eylem liste görünümü oluşturmak için kullanılan REST API görüntüler. Bu eylem sorgu uyarılar için kullanabileceğiniz bir REST API sözdizimiyle öğrenmeye hızlı bir yol sağlar. İzleme, raporlama ve çözümleri raporlama mevcut veri merkeziniz ile Otomasyon veya tümleştirme için bu API'yı kullanabilirsiniz. 

![REST API gösterir görünüm API seçeneği](media/azure-stack-monitor-health/image6.png)

Uyarı ayrıntılarını görüntülemek için özel bir uyarı tıklatabilirsiniz. Uyarı ayrıntıları uyarı ile ilişkili ve uyarının kaynağı ve etkilenen bileşeni Hızlı gezinme etkinleştiren tüm alanları gösterir. Örneğin, bir altyapı rol örneklerinin çevrimdışı veya erişilebilir değil aşağıdaki uyarı oluşur.  

![Uyarı ayrıntıları dikey penceresi](media/azure-stack-monitor-health/image7.png)

Altyapı rol örneğini yeniden çevrimiçi olduktan sonra bu uyarıyı otomatik olarak kapatır. Temel alınan sorun çözüldüğünde birçok, ancak her uyarı otomatik olarak kapatılır. Seçtiğiniz öneririz **Kapat uyarı** düzeltme adımları gerçekleştirdikten sonra. Sorun devam ederse, Azure yığın yeni bir uyarı oluşturur. Sorunu gidermezse, uyarı kapalı kalır ve başka bir işlem gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md)

[Azure yığınında bölge Yönetimi](azure-stack-region-management.md)
