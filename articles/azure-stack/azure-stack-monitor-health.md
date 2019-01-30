---
title: Sistem durumu ve Azure stack'teki uyarıları izleme | Microsoft Docs
description: Sistem durumu ve Uyarıları Azure Stack'te izlemeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 3e37b8f45d7068586d20b9c4b3ccdabb017d0416
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242867"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Sistem durumu ve Azure stack'teki uyarıları izleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack içerir altyapı izleme sistem durumu ve uyarıları için bir Azure Stack bölgesi yardımcı özellikleri görüntüleyin. **Bölge Yönetimi** Yönetici portalı'nda varsayılan olarak varsayılan sağlayıcı aboneliği için Sabitlenen kutucuk, Azure Stack dağıtılan tüm bölgeler listelenir. Kutucuğu, her bölge için kritik ve uyarı etkin uyarı sayısını gösterir. Kutucuk, durumunu ve uyarı Azure Stack işlevselliğini giriş noktasıdır.

![Bölge Yönetimi kutucuğu](media/azure-stack-monitor-health/image1.png)

## <a name="understand-health-in-azure-stack"></a>Azure Stack durumunu anlama

Sistem kaynak sağlayıcısı, sistem durumu ve Uyarıları yönetir. Azure Stack altyapısının bileşenleri, Azure Stack dağıtımı ve yapılandırması sırasında ile sistem durumu kaynak sağlayıcısını kaydedin. Bu kayıt, sistem durumu ve uyarıları her bileşeni için görüntülenmesini sağlar. Azure stack'teki sistem basit bir kavramdır. Bir bileşenin kayıtlı bir örneği için uyarıları varsa, söz konusu bileşen sistem durumu kötü etkin uyarı önem derecesi yansıtır: uyarı veya kritik.

## <a name="alert-severity-definition"></a>Uyarı önem derecesi tanımı

Azure Stack'te yalnızca iki önem dereceleri uyarılarla başlatır: **uyarı** ve **kritik**.

- **Uyarı**  
  Bir işleç uyarı zamanlanmış bir biçimde karşılayabilir. Uyarı, kullanıcı iş yükleri genellikle etkilemez.

- **Kritik**  
  Bir işleç kritik uyarı ile aciliyet giderilmelidir. Bunlar şu anda etkileyen veya yakında Azure Stack kullanıcılarını etkiler sorunlardır.


## <a name="view-and-manage-component-health-state"></a>Görüntüleme ve bileşen durumunu yönetme

Yönetici portalı'nda ve REST API ve PowerShell aracılığıyla bileşenleri sistem durumunu görüntüleyebilirsiniz.

Sistem durumu Portalı'nda görüntülemek için görüntülemek istediğiniz bölgeyi tıklayın **bölge Yönetimi** Döşe. Sistem durumu altyapısı rollerinin ve kaynak sağlayıcılarını görüntüleyebilirsiniz.

![Altyapısı rollerinin listesi](media/azure-stack-monitor-health/image2.png)

Daha ayrıntılı bilgileri görüntülemek için bir kaynak sağlayıcısı veya altyapı rolü tıklayabilirsiniz.

> [!WARNING]  
> Bir altyapı rolü ve rol örneği'a tıklayın, seçenekler vardır **Başlat**, **yeniden**, veya **kapatma**. Bu Eylemler, tümleşik bir sistem güncelleştirmeleri uyguladığınızda kullanmayın. Ayrıca, işlemi **değil** Azure Stack geliştirme Seti'ni ortamında bu seçenekleri kullanın. Bu seçenekler yalnızca bir tümleşik sistemler ortam için tasarlanmış altyapı rol başına birden fazla rol örneğini olduğu. Bir rol örneği (özellikle AzS-Xrp01) development Kit'te yeniden sistem kararsızlığına neden olur. Yaşadığınız sorun giderme Yardımı için sonrası [Azure Stack Forumu](https://aka.ms/azurestackforum).
>

## <a name="view-alerts"></a>Uyarıları görüntüleme

Her Azure Stack bölge için etkin uyarıların listesi doğrudan kullanılabilir **bölge Yönetimi** dikey penceresi. Varsayılan yapılandırmada ilk kutucuk **uyarılar** kutucuğunda, kritik bir özetini ve bölge için uyarı bildirimleri görüntüleyen. Uyarılar kutucuğu panoya hızlı erişim için bu dikey penceredeki diğer herhangi bir kutucuğa gibi sabitleyebilirsiniz.

![Bir uyarı görüntüler kutucuk uyarıları](media/azure-stack-monitor-health/image3.png)

Üst kısmında seçerek **uyarılar** kutucuk ulaşmanıza bölge için tüm etkin uyarıların listesi. Ya da seçerseniz **kritik** veya **uyarı** satır öğesi döşeme içindeki uyarılar (kritik veya uyarı) filtrelenmiş bir listesini gidin. 

**Uyarılar** dikey (etkin veya kapalı) durum ve önem derecesi (kritik veya uyarı) filtreleme özelliği destekler. Varsayılan görünüm, tüm etkin uyarıları görüntüler. Tüm kapatılan uyarılar sistemden yedi gün sonra silinir.

![Filtre bölmesini Filtresi tarafından kritik veya uyarı durumu](media/azure-stack-monitor-health/alert-view.png)

**Görünümü API** liste görünümü oluşturmak için kullanılan REST API eylemi görüntüler. Bu eylem, sorgu uyarılar için kullanabileceğiniz REST API söz dizimi hakkında bilgi sahibi olmak için hızlı bir yolunu sağlar. Bu API izleme, raporlama ve bilet oluşturma çözümleri var olan veri merkeziniz ile Otomasyon veya tümleştirme kullanabilirsiniz.

Uyarı ayrıntılarını görüntülemek için belirli bir uyarı tıklayabilirsiniz. Uyarı ayrıntıları uyarı ile ilişkili olan ve etkilenen bileşen ve uyarının kaynağı olan Hızlı gezinmeyi etkinleştirme tüm alanları gösterir. Bir altyapı rol örneği çevrimdışı olması veya erişilemez, örneğin, aşağıdaki uyarı meydana gelir.  

![Uyarı ayrıntıları dikey penceresi](media/azure-stack-monitor-health/alert-detail.png)

## <a name="repair-alerts"></a>Onarım uyarıları

Seçebileceğiniz **onarım** bazı uyarılar.

Bu onay kutusu seçildiğinde, **onarım** eylemi sorunu çözmeyi denemek için uyarıyı belirli adımları gerçekleştirir. Durumu seçildiğinde **onarım** eylemi, portal bildirimi kullanılabilir.

![Onarım devam eden](media/azure-stack-monitor-health/repair-in-progress.png)

**Onarım** eylem başarıyla tamamlandığında veya aynı portal bildirimi dikey penceresinde eylemi tamamlamak için hata rapor eder.  Bir uyarı için Onarma eylemi başarısız olursa yeniden çalıştırabilirsiniz **onarım** uyarı ayrıntısı eylem. Onarım işlemi başarıyla tamamlarsa **olmayan** yeniden **onarım** eylem.

![Onarım başarıyla tamamlandığında](media/azure-stack-monitor-health/repair-completed.png)

Bu uyarı, altyapı rol örneği yeniden çevrimiçi olduktan sonra otomatik olarak kapanır. Her uyarı, ancak çoğu otomatik olarak kapanır temel sorun çözüldüğünde. Azure Stack sorunu çözümlenirse onarım eylem düğmesi sağlayan uyarılar otomatik olarak kapatılacak.  Diğer tüm uyarılar için seçin **Kapat uyarı** düzeltme adımları gerçekleştirdikten sonra. Azure Stack, sorun devam ederse, yeni bir uyarı oluşturur. Sorunu çözün, uyarıyı kapalı kalır ve başka adım gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure stack'teki güncelleştirmelerini yönetme](azure-stack-updates.md)

[Azure stack'teki bölge Yönetimi](azure-stack-region-management.md)
