---
title: Azure üzerinde Jupyter notebook paketleri yükleme
description: Python, R, yükleme ve F# paketleri gelen içinde Azure üzerinde çalışan bir Jupyter not defteri.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: azure
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: d18a33b7eeb1e3708b4e657180421bd97dc268b3
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57764346"
---
# <a name="install-packages-from-within-a-notebook"></a>İçinde bir not defteri paketleri yükleme

Yapılandırmanız mümkün olmakla birlikte [ortamı için dizüstü bilgisayarınızda proje düzeyinde](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), doğrudan tek bir not defteri içindeki paketleri yüklemek isteyebilirsiniz.

Not defterinden yüklü paketleri yalnızca geçerli sunucu oturumu için geçerlidir. Sunucu kapatıldığında paket yüklemeleri kalıcı değildir.

## <a name="python"></a>Python

Python paketleri pip ya da kod hücreleri komutlarını kullanarak conda kullanarak yüklenebilir:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Komut çıktısı olduğunu gösteriyorsa gereksinim uyulmuş olur ve ardından Azure not defterleri, varsayılan olarak paket içerebilir. Paket yoluyla da yüklenebilir bir [proje ortamı Kurulum adımı](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

R paketleri CRAN veya GitHub kullanarak yüklenebilir `install.packages` işlevi bir kod hücresi içinde:

```r
install.packages("package_name")
```

Yayın öncesi sürümler ve diğer geliştirme paketleri devtools kitaplığını kullanarak Github'dan yükleyebilirsiniz:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

İçindeki paketleri F# yüklenebilir [nuget.org](https://www.nuget.org) Paket bağımlılık Yöneticisi'nden kod hücreleri çağırarak. İlk olarak, Paket Yöneticisi'ni Yükle:

```fsharp
#load "Paket.fsx"
```

Ardından, paketleri yükleyin:

```fsharp
Paket.Package
[ "MathNet.Numerics"
"MathNet.Numerics.FSharp"
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: Yapılandırma ve projeleri yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: Mevcut bir slayt gösterisi](present-jupyter-notebooks-slideshow.md)
