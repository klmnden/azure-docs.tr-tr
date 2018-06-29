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
ms.date: 05/11/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: 8d9337053c8905886ed4429d64f8ef5b4e2c7d14
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060456"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure yığında desteklenen konuk işletim sistemleri

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="windows"></a>Windows

Azure yığını aşağıdaki tabloda listelenen Windows konuk işletim sistemlerini destekler:

| İşletim sistemi | Açıklama | Market kullanılabilir |
| --- | --- | --- | --- | --- | --- |
| Windows Server, sürüm 1709 | 64 bit | Kapsayıcılarla çekirdek |
| Windows Server 2016 | 64 bit |  Datacenter, veri merkezi çekirdek kapsayıcıları ile veri merkezi |
| Windows Server 2012 R2 | 64 bit |  Veri merkezi |
| Windows Server 2012 | 64 bit |  Veri merkezi |
| Windows Server 2008 R2 SP1 | 64 bit |  Veri merkezi |
| Windows Server 2008 SP2 | 64 bit |  Kendi görüntünüzü Getir |
| Windows 10 *(bkz. Not 1)* | 64-bit, Pro ve Enterprise | Kendi görüntünüzü Getir |

***Not 1:*** *Azure yığında Windows 10 istemci işletim sistemlerini dağıtmak için bilmeniz gereken [Windows kullanıcı başına lisans](https://www.microsoft.com/en-us/Licensing/product-licensing/windows10.aspx) veya satın alma tam bir çok kullanıcılı barındırma ([QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)).*

Market görüntülerini ödeme olarak,-kullanımlı ya da KLG (EA/SPLA) lisans için kullanılabilir. Hem tek bir Azure yığın örneğinde kullanımı desteklenmiyor. Dağıtım sırasında Azure yığın Konuk Aracısı uygun bir sürümünü görüntüye yerleştirir.

 Datacenter sürümleri, indirme Market'te; Müşteriler diğer sürümleri de dahil olmak üzere, kendi sunucu görüntülerinin kullanıma sunabilirsiniz. Windows istemci görüntüleri markette kullanılamaz.

## <a name="linux"></a>Linux

Linux dağıtımları markette kullanılabilir olarak listelenen gerekli Windows Azure Linux Aracısı (WALA) içerir. Azure yığınına kendi görüntünüzü getirirseniz, yönergeleri izleyin [Azure yığın ekleme Linux görüntülere](azure-stack-linux.md).

> [!NOTE]
> Özel resimler en son ortak WALA sürümüyle oluşturulmalıdır. 2.2.18 eski sürümleri Azure yığın üzerinde düzgün çalışmayabilir.
>
> [Bulut init](https://cloud-init.io/) Azure yığın üzerinde şu anda desteklenmiyor.

| Dağıtım | Açıklama | Yayımcı | Market |
| --- | --- | --- | --- | --- | --- |
| CentOS tabanlı 6.9 | 64 bit | Yanlış Wave | Evet |
| 7.4 centOS tabanlı | 64 bit | Yanlış Wave | Evet |
| ClearLinux | 64 bit | ClearLinux.org | Evet |
| Kapsayıcı Linux |  64 bit | CoreOS | Dengeli |
| Debian 8 "Jessie" | 64 bit | credativ |  Evet |
| Debian 9 "Esnetme" | 64 bit | credativ | Evet |
| Red Hat Enterprise Linux 7.x | 64 bit | Red Hat |Kendi görüntünüzü Getir |
| SLES 11SP4 | 64 bit | SUSE | Evet |
| SLES 12SP3 | 64 bit | SUSE | Evet |
| Ubuntu 14.04 LTS | 64 bit | Canonical | Evet |
| Ubuntu 16.04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 18.04-LTS | 64 bit | Canonical | Evet |

Diğer Linux dağıtımları gelecekte desteklenmiyor olabilir.

Red Hat Enterprise Linux destek bilgileri için lütfen [Red Hat ve Azure yığın: sık sorulan sorular](https://access.redhat.com/articles/3413531).
