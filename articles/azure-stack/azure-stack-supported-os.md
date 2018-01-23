---
title: "Azure yığını için desteklenen konuk işletim sistemleri | Microsoft Docs"
description: "Bu konuk işletim sistemleri Azure yığın üzerinde kullanılabilir."
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: c9f5bee38772623fb79fa081be8eaece981cc8ab
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure yığında desteklenen konuk işletim sistemleri

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="windows"></a>Windows
Azure yığın aşağıdaki Windows konuk işletim sistemlerini destekler. Market görüntülerini Azure yığınına yüklenebilir. Windows istemci görüntüleri markette kullanılabilir değil.

Dağıtım sırasında uygun bir konuk Aracısı sürümü görüntüsüne eklenmiş Azure yığın güvence altına alır.

| İşletim sistemi | Açıklama | Yayımcı | İşletim Sistemi Türü | Market |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 bit | Microsoft | Windows | Veri merkezi |
| Windows Server 2012 | 64 bit | Microsoft | Windows | Veri merkezi |
| Windows Server 2012 R2 | 64 bit | Microsoft | Windows | Veri merkezi |
| Windows Server 2016 | 64 bit | Microsoft | Windows | Datacenter, veri merkezi çekirdek kapsayıcıları ile veri merkezi |
| Windows 7 | 64-bit, Pro ve Enterprise | Microsoft | Windows | Hayır |
| Windows 8.1 | 64-bit, Pro ve Enterprise | Microsoft | Windows | Hayır |
| Windows 10 *(bkz. Not 1)* | 64-bit, Pro ve Enterprise | Microsoft | Windows | Hayır |

***Not 1:****Azure yığında Windows 10 istemci işletim sistemlerini dağıtmak için bilmeniz gereken [Windows kullanıcı başına lisans](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) veya satın alma tam bir çok kullanıcılı barındırma ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

Linux dağıtımları burada listelenen gerekli Windows Azure Linux Aracısı (WALA) içerir.

> [!NOTE]   
> 2.2.3 sayısından daha eski WALA sürümleri ile oluşturulmuş görüntülerin *değil* desteklenen ve dağıtmak olası değildir. Bazı WALA Aracı sürümleri not işlevi 2.2.12 ve 2.2.13 sürümleri de dahil olmak üzere Azure yığın vm'lerinde bilinmektedir.


| Dağıtım | Açıklama | Yayımcı | Market |
| --- | --- | --- | --- | --- | --- |
| Kapsayıcı Linux |  64 bit | CoreOS | Dengeli |
| CentOS tabanlı 6.9 | 64 bit | Yanlış Wave | Evet |
| CentOS tabanlı 7.3 | 64 bit | Yanlış Wave | Evet |
| 7.4 centOS tabanlı | 64 bit | Yanlış Wave | Evet |
| Debian 8 "Jessie" | 64 bit | credativ |  Evet |
| Debian 9 "Esnetme" | 64 bit | credativ | Evet |
| Oracle Linux | 64 bit | Oracle | Hayır |
| Red Hat Enterprise Linux 7.x | 64 bit | Red Hat | Hayır |
| SLES 11SP4 | 64 bit | SUSE | Evet |
| SLES 12SP3 | 64 bit | SUSE | Evet |
| Ubuntu 14.04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 16.04-LTS | 64 bit | Canonical | Evet |

Diğer Linux dağıtımları gelecekte desteklenmiyor olabilir.
