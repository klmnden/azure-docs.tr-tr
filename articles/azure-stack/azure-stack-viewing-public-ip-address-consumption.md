---
title: Ortak IP adresi tüketim Azure yığınında görüntülemek | Microsoft Docs
description: Yöneticiler, bir bölgede genel IP adresleri tüketiminin görüntüleyebilir
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 50bf01d6de6105d3041c6bb88e803f3d110f751d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29742467"
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Ortak IP adresi tüketim Azure yığınında görüntüleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bulut yönetici olarak, görüntüleyebilirsiniz:
 - Kiracılar için ayrılan ortak IP adresi sayısı.
 - Ayırma için hala kullanılabilir ortak IP adresi sayısı.
 - Bu konumda ayrılmış genel IP adresleri yüzdesi.

**Genel IP havuzları kullanım** döşeme ortak IP adresi havuzu genelinde kullanılan genel IP adresleri sayısını gösterir. Her IP adresi için döşeme Kiracı Iaas VM için kullanım örnekleri ve yapı altyapı hizmetleri açıkça kiracılar tarafından oluşturulan ortak IP adresi kaynakları gösterir.

Döşeme amacı, bu konumda kullanılan ortak IP adresi sayısı duygusu Azure yığın işleçleri vermektir. Sayı, bu kaynaktaki düşük çalışıp çalışmadığını belirleme yöneticilerin yardımcı olur.

**Ortak IP adresleri** menü öğesi altında **Kiracı kaynaklarına** henüz yalnızca bu ortak IP adreslerini listeler *kiracılar tarafından oluşturulan açıkça*. Menü öğesi bulabilirsiniz **kaynak sağlayıcıları**, **ağ** bölmesi. Sayısı **kullanılan** genel IP adresleri **genel IP havuzları kullanım** kutucuğu (büyük) öğesinden farklı her zaman üzerinde sayı **genel IP adresleri** döşeme altında **Kiracı kaynaklarını**.

## <a name="view-the-public-ip-address-usage-information"></a>Ortak IP adresi kullanım bilgilerini görüntüleme
Bölgede tüketilen genel IP adresleri toplam sayısını görüntülemek için:

1. Azure yığın Yönetici portalı'nda seçin **daha fazla hizmet**altında **yönetim kaynaklarının**seçin **kaynak sağlayıcıları**.
2. Listesinden **kaynak sağlayıcıları**seçin **ağ**.
3. **Ağ** bölmesi görüntüler **genel IP havuzları kullanım** parçasında **genel bakış** bölümü.

![Ağ kaynak sağlayıcısı bölmesi](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

**Kullanılan** numarası, ortak IP adresi havuzlarını atanmış ortak IP adresleri sayısını temsil eder. **Serbest** ortak bir IP adresi genel IP sayısı sayı temsil adresi henüz atanmamış ve hala kullanılabilir havuzları. **% Kullanılan** numarasını kullanılan veya ortak IP adresi havuzları o konumda genel IP adresleri toplam sayısı yüzdesi olarak atanmış adresleri sayısını temsil eder.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Kiracı abonelik tarafından oluşturulan ortak IP adreslerini görüntüleme
Seçin **ortak IP adresleri** altında **Kiracı kaynaklarına**. Belirli bir bölgede Kiracı abonelikleri tarafından açıkça oluşturulan genel IP adresleri listesini gözden geçirin.

![Kiracı genel IP adresleri](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Dinamik olarak ayrılan bazı ortak IP adresleri listesinde göründüğünü fark edebilirsiniz. Ancak, bir adresi henüz kendileriyle ilişkili olmamıştır. Ağ kaynak sağlayıcısı, ancak henüz Ağ denetleyicisi adresi kaynağı oluşturuldu.

Arabirim, bir ağ arabirimi kartı (NIC), bir yük dengeleyici veya bir sanal ağ geçidi bağlar kadar Ağ denetleyicisi için kaynak adres atamaz. Genel IP adresi için bir arabirim bağladığında, Ağ denetleyicisi bir IP adresi ayırır. Adres görünür **adresi** alan.

## <a name="view-the-public-ip-address-information-summary-table"></a>Ortak IP adresi bilgileri Özet tablosunu görüntüleme
Farklı durumlarda ortak IP adresleri adres listesi veya başka bir görünür olup olmadığını belirleyen atanır.

| **Ortak IP adresi ataması durumu** | **Kullanım Özeti görüntülenir** | **Kiracı ortak IP adresleri listesinde görüntülenir** |
| --- | --- | --- |
| Henüz bir NIC veya yük dengeleyiciye (geçici) atanan dinamik genel IP adresi |Hayır |Evet |
| Dinamik genel IP adresi için bir NIC veya yük dengeleyici atanmış. |Evet |Evet |
| Statik genel IP adresi için bir kiracı NIC veya yük dengeleyici atanmış. |Evet |Evet |
| Statik genel IP adresi yapı altyapı Hizmeti uç noktası için atanmış. |Evet |Hayır |
| Genel IP adresi örtük olarak Iaas VM örnekleri için oluşturulur ve sanal ağda giden NAT için kullanılır. Kiracı VM örneği oluşturur ve böylece VM'ler bilgilerini Internet'e gönderebilirsiniz olduğunda bunlar arka planda oluşturulur. |Evet |Hayır |

## <a name="next-steps"></a>Sonraki adımlar
[Depolama hesaplarını Azure yığınında yönetme](azure-stack-manage-storage-accounts.md)