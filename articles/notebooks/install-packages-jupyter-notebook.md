---
title: Azure üzerinde Jupyter notebook paketleri yükleyin | Microsoft Docs
description: Python, R, yükleme ve F# içinde bir not defteri paketleri öğesinden.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 8444855336b97c8bb71afa3953e81b701a5ff41f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856707"
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

R paketleri CRAN veya Github kullanarak yüklenebilir `install.packages` işlevi bir kod hücresi içinde:

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

- [Nasıl yapılır: projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: bir slayt gösterisi sunar](present-jupyter-notebooks-slideshow.md)
