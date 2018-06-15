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
ms.openlocfilehash: 919582745d166cf8b3f3937f9bac4fc0dc1fe64f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31613415"
---
## <a name="overview-of-ssh-and-keys"></a>SSH ve anahtarları genel bakış

SSH güvenli olmayan bağlantıları üzerinden güvenli oturum açma bilgileri veren bir şifreli bir bağlantı protokolüdür. Bu Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. SSH kendisini şifreli bir bağlantı sağlasa da, SSH bağlantılarla parolalarla hala VM yanılma saldırılarını veya parolalarını tahmin açık halde kalır. SSH kullanarak bir VM bağlayan daha güvenli ve tercih edilen bir yöntem olarak da bilinen SSH anahtarları bir genel-özel anahtar çifti kullanmaktır. 

* *Ortak anahtar* Linux VM veya ortak anahtar şifrelemesi ile kullanmak istediğiniz başka bir hizmet yerleştirilir.

* *Özel anahtarı* kimliğinizi doğrulamak için bir SSH bağlantısı yaptığınızda ne, Linux VM'NİZDE mevcut değil. Bu özel anahtarı koruyun. Özel anahtarı paylaşmayın.

Kuruluşunuzun güvenlik ilkelerine bağlı olarak, birden çok Azure Vm'leri ve hizmetlerine erişmek için bir tek genel-özel anahtar çifti yeniden kullanabilirsiniz. Her VM veya erişmek istediğiniz hizmet için ayrı bir çift anahtar gerekmez. 

Ortak anahtarınız herkesle paylaşılabilir; ancak, özel anahtarınıza yalnızca siz (veya yerel güvenlik altyapınız) sahip olursunuz.