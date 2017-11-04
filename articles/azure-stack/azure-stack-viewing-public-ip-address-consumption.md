---
title: "Ortak IP adresi tüketim Azure yığınında görüntülemek | Microsoft Docs"
description: "Yöneticiler, bir bölgede genel IP adresleri tüketiminin görüntüleyebilir"
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 7651565eebf6272f307a4ce4790ca19b41bfa826
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Ortak IP adresi tüketim Azure yığınında görüntüleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bulut Yöneticisi olarak, kiracılar, ayırma için hala kullanılabilir genel IP adresleri sayısını ve söz konusu konumda ayrılmış genel IP adresleri yüzdesini ayrılmış genel IP adresleri sayısını görebilirsiniz.

**Genel IP havuzları kullanım** döşeme Iaas VM örnekleri, Kiracı doku altyapınız için kullanılmış olup olmadığını doku tüm ortak IP adresi havuzundaki genelinde kullanılan genel IP adresleri toplam sayısını gösterir Hizmetleri veya açıkça kiracılar tarafından oluşturulan ortak IP adresi kaynakları.

Bu kutucuğu amacı, bu konumda tüketilen genel IP adresleri genel sayısını duygusu Azure yığın Yöneticiler vermektir. Bu, bunlar Bu kaynakta düşük çalışıp çalışmadığını belirleme Yöneticiler yardımcı olur.

Üzerinde **kaynak sağlayıcıları**, **ağ** dikey penceresinde **ortak IP adresleri** menü öğesi altında **Kiracı kaynaklarına** yalnızca bu genel listeler Silinmiş IP adreslerini *kiracılar tarafından oluşturulan açıkça*. Şekilde sayısını **kullanılan** genel IP adresleri **genel IP havuzları kullanım** kutucuğu (büyük) öğesinden farklı her zaman üzerinde sayı **genel IP adresleri** döşeme altında **Kiracı kaynaklarını**.

## <a name="view-the-public-ip-address-usage-information"></a>Ortak IP adresi kullanım bilgilerini görüntüleme
Bölgede tüketilen genel IP adresleri toplam sayısını görüntülemek için:

1. Azure yığın Yönetici portalı'nda tıklatın **daha fazla hizmet**altında **yönetim kaynaklarının**, tıklatın **kaynak sağlayıcıları**.
2. Listesinden **kaynak sağlayıcıları**seçin **ağ**.
3. **Ağ** dikey penceresinde görüntüler **genel IP havuzları kullanım** parçasında **genel bakış** bölümü.

![Ağ kaynak sağlayıcısı dikey penceresi](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Aklınızda **kullanılan** numarasını atanan tüm ortak IP adresi havuzlarını o konumda ortak IP adresi sayısı temsil eder. **Serbest** tüm ortak bir IP adresi genel IP sayısı sayı temsil adresi henüz atanmamış ve hala kullanılabilir havuzları. **% Kullanılan** numarasını kullanılan veya bu konumda bulunan tüm genel IP adresi havuzları genel IP adresleri toplam sayısı yüzdesi olarak atanmış adresleri sayısını temsil eder.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Kiracı abonelik tarafından oluşturulan ortak IP adreslerini görüntüleme
Belirli bir bölgede Kiracı abonelikleri tarafından açıkça oluşturulan genel IP adresleri listesini görmek için tıklatın **ortak IP adresleri** altında **Kiracı kaynaklarına**.

![Kiracı genel IP adresleri](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Dinamik olarak ayrılan bazı ortak IP adresleri listede görünür ancak henüz ilişkili bir adresi yoksa fark edebilirsiniz. Adres kaynak ağ kaynak sağlayıcısı, ancak Ağ denetleyicisi henüz oluşturulmamış olmasıdır.

Ağ denetleyicisi, bir arabirim, bir ağ arabirimi kartı (NIC), bir yük dengeleyici veya bir sanal ağ geçidi için gerçekten bağlı olduğu kadar bir adresi bu kaynağa atamaz. Genel IP adresi için bir arabirim bağlanır, ona bir IP adresi ağ denetleyicisi ayırdığında ve görünür **adresi** alan.

## <a name="view-the-public-ip-address-information-summary-table"></a>Ortak IP adresi bilgileri Özet tablosunu görüntüleme
Adres listesi veya başka bir görünür olup olmadığını belirlemek genel IP adresleri atanan farklı durumlarda mevcuttur.

| **Ortak IP adresi ataması durumu** | **Kullanım Özeti görüntülenir** | **Kiracı ortak IP adresleri listesinde görüntülenir** |
| --- | --- | --- |
| Henüz bir NIC veya yük dengeleyiciye (geçici) atanan dinamik genel IP adresi |Hayır |Evet |
| Dinamik genel IP adresi için bir NIC veya yük dengeleyici atanmış. |Evet |Evet |
| Statik genel IP adresi için bir kiracı NIC veya yük dengeleyici atanmış. |Evet |Evet |
| Statik genel IP adresi yapı altyapı Hizmeti uç noktası için atanmış. |Evet |Hayır |
| Genel IP adresi örtük olarak Iaas VM örnekleri için oluşturulur ve sanal ağda giden NAT için kullanılır. Kiracı VM örneği oluşturur ve böylece VM'ler bilgilerini Internet'e gönderebilirsiniz olduğunda bunlar arka planda oluşturulur. |Evet |Hayır |

## <a name="next-steps"></a>Sonraki adımlar
[Depolama hesaplarını Azure yığınında yönetme](azure-stack-manage-storage-accounts.md)