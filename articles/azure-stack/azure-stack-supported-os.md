---
title: Azure yığını için desteklenen konuk işletim sistemleri | Microsoft Docs
description: Bu konuk işletim sistemleri Azure yığın üzerinde kullanılabilir.
services: azure-stack
documentationcenter: ''
author: Brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: ff3aea4e449e3d489b0c0f01345ecd9773c7d885
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure yığında desteklenen konuk işletim sistemleri

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="windows"></a>Windows
Azure yığını aşağıdaki tabloda listelenen Windows konuk işletim sistemlerini destekler: görüntüleri Market'te Azure yığınına yüklenebilir. Windows istemci görüntüleri markette kullanılabilir değil.

Dağıtım sırasında Azure yığın Konuk Aracısı uygun bir sürümünü görüntüye yerleştirir.

| İşletim sistemi | Açıklama | Market kullanılabilir |
| --- | --- | --- | --- | --- | --- |
| Windows Server, sürüm 1709 | 64 bit | Kapsayıcılarla çekirdek |
| Windows Server 2016 | 64 bit |  Datacenter, veri merkezi çekirdek kapsayıcıları ile veri merkezi |
| Windows Server 2012 R2 | 64 bit |  Veri merkezi |
| Windows Server 2012 | 64 bit |  Veri merkezi |
| Windows Server 2008 R2 SP1 | 64 bit |  Veri merkezi |
| Windows Server 2008 SP2 | 64 bit |  Kendi görüntünüzü Getir |
| Windows 10 *(bkz. Not 1)* | 64-bit, Pro ve Enterprise | Kendi görüntünüzü Getir |

***Not 1:****Azure yığında Windows 10 istemci işletim sistemlerini dağıtmak için bilmeniz gereken [Windows kullanıcı başına lisans](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) veya satın alma tam bir çok kullanıcılı barındırma ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).*

Market görüntülerini ödeme olarak,-kullanımlı ya da KLG (EA/SPLA) lisans için kullanılabilir. Hem tek bir Azure yığın örneğinde kullanımı desteklenmiyor. 

Yalnızca Datacenter sürümleri Market bulunur; Müşteriler diğer sürümleri de dahil olmak üzere, kendi sunucu görüntülerinin kullanıma sunabilirsiniz.

## <a name="linux"></a>Linux

Linux dağıtımları burada listelenen gerekli Windows Azure Linux Aracısı (WALA) içerir.

> [!NOTE]   
> Özel resimler en son ortak WALA sürümüyle oluşturulmalıdır. 2.2.18 eski sürümleri Azure yığın üzerinde düzgün çalışmayabilir.  
>
> [Bulut init](https://cloud-init.io/) Azure yığın üzerinde şu anda desteklenmiyor.

| Dağıtım | Açıklama | Yayımcı | Market |
| --- | --- | --- | --- | --- | --- |
| Kapsayıcı Linux |  64 bit | CoreOS | Dengeli |
| CentOS tabanlı 6.9 | 64 bit | Yanlış Wave | Evet |
| 7.4 centOS tabanlı | 64 bit | Yanlış Wave | Evet |
| ClearLinux | 64 bit | ClearLinux.org | Evet |
| Debian 8 "Jessie" | 64 bit | credativ |  Evet |
| Debian 9 "Esnetme" | 64 bit | credativ | Evet |
| Red Hat Enterprise Linux (Bekleyen) 7.x | 64 bit | Red Hat |Kendi görüntünüzü Getir |
| SLES 11SP4 | 64 bit | SUSE | Evet |
| SLES 12SP3 | 64 bit | SUSE | Evet |
| Ubuntu 14.04 LTS | 64 bit | Canonical | Evet |
| Ubuntu 16.04-LTS | 64 bit | Canonical | Evet |

Diğer Linux dağıtımları gelecekte desteklenmiyor olabilir.
