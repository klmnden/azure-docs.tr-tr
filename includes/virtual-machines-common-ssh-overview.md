---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0363b719e2587226282257e19e58e3c4e55c2219
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454480"
---
## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarları'na genel bakış

SSH güvenli olmayan bağlantılara güvenli oturum açma işlemleri izin veren bir şifreli bağlantı protokolüdür. SSH Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. SSH kendisini şifreli bir bağlantı sağlar, ancak SSH bağlantıları ile Parolaları kullanarak hala VM yanılma saldırıları veya parolalar tahmin için savunmasız bırakır. Genel-özel anahtar çifti olarak da bilinen kullanarak SSH kullanarak VM'ye bağlanma daha güvenli ve tercih edilen bir yöntem olan *SSH anahtarları*. 

* *Ortak anahtar* Linux VM'nize veya ortak anahtar şifrelemesi ile kullanmak istediğiniz herhangi bir hizmeti yerleştirilir.

* *Özel anahtarı* kimliğinizi doğrulamak için bir SSH bağlantısı yaptığınızda ne, Linux VM'nize bilgidir. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Kuruluşunuzun güvenlik ilkelerine bağlı olarak, birden çok Azure Vm'lerini ve hizmetlerini erişmek için tek genel-özel anahtar çifti yeniden kullanabilirsiniz. Her VM'ye veya hizmete erişmek istediğiniz için ayrı bir çift anahtar gerekmez. 

Ortak anahtarınız herkesle paylaşılabilir, ancak özel anahtarınız, yalnızca sizin (veya yerel güvenlik altyapınız) sahip olmanız gerekir.