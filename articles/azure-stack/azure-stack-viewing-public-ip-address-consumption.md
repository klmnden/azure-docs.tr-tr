---
title: Azure Stack'te genel IP adresi kullanımını görüntüleme | Microsoft Docs
description: Yöneticiler, bir bölgede genel IP adresi kullanımını görüntüleyebilir
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/14/2019
ms.author: mabrigg
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: d97674940f0af91bf50af87cfe96fda9644d469b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242061"
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Azure Stack'te genel IP adresi kullanımını görüntüleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bulut Yöneticisi olarak, görüntüleyebilirsiniz:
 - Kiracılar için ayrılmış genel IP adresleri sayısı.
 - Ayırma için hala kullanılabilir genel IP adresi sayısı.
 - Bu konumda ayrılmış genel IP adresleri yüzdesi.

**Genel IP havuzlarını kullanım** kutucuk genel IP adresi havuzları arasında kullanılan genel IP adresleri sayısını gösterir. Her IP adresi için kutucuğun örnekleri, doku altyapı hizmetleri ve kiracılar tarafından açıkça oluşturulan genel IP adresi kaynakları Kiracı Iaas VM için kullanımını gösterir.

Kutucuk amacı, Azure Stack operatörlerinin bu konumda kullanılan genel IP adresleri sayısını bir fikir vermektir. Sayı, bu kaynak üzerinde düşük çalışıp çalışmadığını belirleme yöneticilere yardımcı olur.

**Genel IP adresleri** menü öğesi altında **Kiracı kaynaklarını** olan yalnızca bu genel IP adresleri listelenir *kiracılar tarafından açıkça oluşturulan*. Menü öğesi bulabilirsiniz **kaynak sağlayıcıları**, **ağ** bölmesi. Sayısı **kullanılan** genel IP adresleri üzerindeki **genel IP havuzlarını kullanım** kutucuk (büyük) öğesinden farklı her zaman şirket sayısı **genel IP adresleri** kutucuğuna altında **Kiracı kaynaklarını**.

## <a name="view-the-public-ip-address-usage-information"></a>Genel IP adresi kullanım bilgilerini görüntüleme

Bölgede kullanılan genel IP adresleri toplam sayısını görüntülemek için:

1. Azure Stack Yönetici portalında **tüm hizmetleri**. Ardından, altında **Yönetim** kategorisi seçin **ağ**.
1. **Ağ** bölmesini görüntüler **genel IP havuzlarını kullanım** kutucuğu **genel bakış** bölümü.

![Ağ kaynak sağlayıcısı bölmesi](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

**Kullanılan** numarası, genel IP adresi havuzlarını atanmış genel IP adresleri sayısını temsil eder. **Ücretsiz** sayı temsil genel bir IP adresi genel IP sayısı adres henüz atanmamış ve hala kullanılabilir havuzları. **% Kullanılan** numarası kullanılan veya bir genel IP adresi havuzları o konumda genel IP adresleri toplam sayısının yüzdesi olarak atanmış adresleri sayısını temsil eder.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Kiracı abonelik tarafından oluşturulan genel IP adreslerini görüntüle

Seçin **genel IP adresleri** altında **Kiracı kaynaklarını**. Belirli bir bölgede Kiracı abonelikler tarafından açıkça oluşturulan genel IP adresleri listesini gözden geçirin.

![Kiracı genel IP adresleri](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Dinamik olarak ayrılan bazı genel IP adresleri listesinde göründüğünü fark edebilirsiniz. Ancak, bir adresi henüz kendileriyle ilişkili olmamıştır. Ağ kaynak sağlayıcısı ancak henüz Ağ denetleyicisi adresi kaynağı oluşturuldu.

Ağ denetleyicisi bir arabirim, bir ağ arabirimi kartı (NIC), bir yük dengeleyici veya bir sanal ağ geçidi bağlar kadar bir adresi kaynağa atamaz. Bir arabirim için genel IP adresine bağlar, Ağ denetleyicisi, bir IP adresi ayırır. Adres görünür **adresi** alan.

## <a name="view-the-public-ip-address-information-summary-table"></a>Genel IP adresi bilgileri Özet tablosunu görüntüleme

Farklı durumlarda adres bir liste veya başka bir görünür olup olmadığını belirleyen genel IP adresleri atanır.

| **Genel IP adresi ataması durumu** | **Kullanım Özeti görüntülenir.** | **Kiracı genel IP adresi listesinde görünür.** |
| --- | --- | --- |
| Henüz bir NIC veya yük dengeleyiciye (geçici) atanan dinamik genel IP adresi |Hayır |Evet |
| Bir NIC veya yük dengeleyiciye atanan dinamik genel IP adresidir. |Evet |Evet |
| Bir kiracı NIC veya yük dengeleyiciye atanan statik genel IP adresidir. |Evet |Evet |
| Statik genel IP adresi doku altyapısı hizmet uç noktası atanmış. |Evet |Hayır |
| Genel IP adresi örtük olarak Iaas sanal makine örnekleri için oluşturulan ve giden NAT sanal ağ üzerinde kullanılır. Her bir kiracı bir sanal makine örneği oluşturur ve böylece VM'ler bilgilerini Internet'e gönderebilirsiniz bunlar Sahne arkasında oluşturulur. |Evet |Hayır |

## <a name="next-steps"></a>Sonraki adımlar

[Azure stack'teki depolama hesapları yönetme](azure-stack-manage-storage-accounts.md)