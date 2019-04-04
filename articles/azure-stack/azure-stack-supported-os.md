---
title: Azure Stack için desteklenen konuk işletim sistemleri | Microsoft Docs
description: Bu konuk işletim sistemleri, Azure Stack üzerinde kullanılabilir.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/17/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: a271edd710356ff612a4558034e9f7459a17ecaa
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666299"
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure Stack üzerinde desteklenen konuk işletim sistemleri

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="windows"></a>Windows

Azure Stack aşağıdaki tabloda listelenen Windows konuk işletim sistemlerini destekler:

| İşletim sistemi | Açıklama | Market'te kullanılabilir |
| --- | --- | --- |
| Windows Server 1709 sürümü | 64 bit | Kapsayıcılar ile çekirdek |
| Windows Server 2016 | 64 bit |  Veri Merkezi, veri merkezi çekirdek kapsayıcılar ile veri merkezi |
| Windows Server 2012 R2 | 64 bit |  Veri merkezi |
| Windows Server 2012 | 64 bit |  Veri merkezi |
| Windows Server 2008 R2 SP1 | 64 bit |  Veri merkezi |
| Windows Server 2008 SP2 | 64 bit |  Kendi görüntünüzü getirin |
| Windows 10 *(bkz. Not 1)* | 64-bit, Pro ve Enterprise | Kendi görüntünüzü getirin |

> [!NOTE]
> Azure Stack üzerinde Windows 10 istemci işletim sistemlerini dağıtmak için olmalıdır [Windows kullanıcı başına lisans](https://www.microsoft.com/en-us/Licensing/product-licensing/windows10.aspx) veya tam bir çok Kiracılı barındırma sağlayıcı satın alma ([QMTH](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)).

Market görüntüleri,-,-kullandıkça veya KLG (EA/SPLA) lisans için kullanılabilir. Her ikisi de tek bir Azure Stack örneğinde kullanımı desteklenmez. Dağıtım sırasında Azure Stack Konuk Aracısı'nın uygun bir sürüm görüntüye yerleştirir.

Datacenter Edition indirme Market; Müşteriler, diğer sürümleri dahil olmak üzere, kendi sunucu görüntülerini getirebilirsiniz. Windows istemci görüntülerini Market'te kullanıma sunulmamıştır.

## <a name="linux"></a>Linux

Linux dağıtımları kullanılabilir olarak Market'te listelenen gerekli Windows Azure Linux Aracısı (WALA) içerir. Azure Stack için kendi görüntünüzü getirin, yönergeleri izleyin. [ekleme Linux görüntüleri için Azure Stack](azure-stack-linux.md).

> [!NOTE]
> Özel görüntüler, en son genel WALA sürüm veya sürüm 2.2.20 ile oluşturulmalıdır. 2.2.20 2.2.35.1 (dışlamalı) arasında ve 2.2.20 daha eski sürümleri, Azure Stack üzerinde düzgün çalışmayabilir. 
>
> [cloud-init](https://cloud-init.io/) Azure Stack üzerinde şu anda desteklenmiyor.

| Dağıtım | Açıklama | Yayımcı | Market |
| --- | --- | --- | --- |
| CentOS tabanlı 6.9 | 64 bit | Rogue Wave | Evet |
| CentOS tabanlı 7.4 | 64 bit | Rogue Wave | Evet |
| ClearLinux | 64 bit | ClearLinux.org | Evet |
| Container Linux |  64 bit | CoreOS | Dengeli |
| Debian 8 "Jessie" | 64 bit | credativ |  Evet |
| Debian 9 "Uzat" | 64 bit | credativ | Evet |
| Red Hat Enterprise Linux 7.x | 64 bit | Red Hat |Kendi görüntünüzü getirin |
| SLES 11SP4 | 64 bit | SUSE | Evet |
| SLES 12SP3 | 64 bit | SUSE | Evet |
| Ubuntu 14.04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 16.04-LTS | 64 bit | Canonical | Evet |
| Ubuntu 18.04-LTS | 64 bit | Canonical | Evet |

Red Hat Enterprise Linux desteği için bilgi [Red Hat ve Azure Stack: Sık sorulan sorular](https://access.redhat.com/articles/3413531).

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack Marketini hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
- [Bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)
