---
title: Azure Machine Learning için geliştirme ortamları | Microsoft Docs
description: Geliştirme ortamlara genel bakış, Azure Machine Learning hizmeti ile kullanabilirsiniz. Python 3 geliştirme ortamınız için tek gereksinim olmasıdır, ancak aynı zamanda Conda ortamları kullanmanızı öneririz. İçin geliştirme araçları, Jupyter Notebook belgeleri, Azure not defterleri ve IDE/kod düzenleyicilerinden öneririz.
services: machine-learning
author: rastala
ms.author: roastala
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 9/24/2018
ms.openlocfilehash: f221d160685dd12fb18a611432911baa60ebc6f7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888165"
---
# <a name="development-environment-for-azure-machine-learning"></a>Azure Machine Learning için geliştirme ortamı 

Azure Machine Learning hizmeti ile kullanabileceğiniz geliştirme ortamları hakkında bilgi edinin. 

Azure Machine Learning hizmeti platformu belirsiz olduğundan ve belirli geliştirme ortamı gerektirmez. Gerektiren __Python 3__, ve kullanmanızı öneririz __Conda__ yalıtılmış ortamlar oluşturmak için. __Bu gereksinimleri karşılayan bir geliştirme ortamı zaten varsa__, bu belgede atlayın ve Git [geliştirme ortamınızı yapılandırma](how-to-configure-environment.md) belge.

Bu belgenin geri kalan öneririz geliştirme ortamları ele alınmıştır:

* __Jupyter Notebook__
* __Azure Not Defterleri__
* __Tümleşik geliştirme ortamında (IDE) ve kod düzenleyiciler__
* __Veri bilimi sanal makinesi__

Not Defterleri hem IDE'ler genişletilmiş olarak bu ortamlar arasında bir karşılaştırma zordur. Örneğin, bazı IDE'ler Jupyter not defterleri için istemcileri olarak kullanılabilir. Genel olarak bakıldığında, __not defterlerini__ için tasarlanmış __etkileşimli deneme__ ve __görselleştirme__. __IDE ve kod düzenleyiciler__ araçlar sağlayan __kod kalitesini geliştirme__ ve __dış sistemlerle tümleştirmek__ sürüm denetimi gibi.

> [!TIP]
> Bir ortamı kullanarak diğer kullanarak dışında kilitlemez. Not defterlerini ve IDE'ler yalnızca araçları ve gerektiğinde karma olabilir. Örneğin, bir not defteri denemeye başlamak sonra düzenleme ve IDE içinde hata ayıklama için bir python betiği verin.
>
> Veri bilimi sanal makinesi hem Jupyter not defterleri hem de birçok popüler Python Ide'leri sağlar nedeni budur.

## <a name="jupyter-notebooks"></a>Jupyter Notebooks

Jupyter not defterleri parçasıdır [Jupyter proje](https://jupyter.org/). Bunlar, Canlı kod anlatım metin ve grafikleri ile karışık belgeleri oluşturmak burada etkileşimli bir kodlama deneyimi sağlamaya odaklanmıştır. Jupyter not defterleri çeşitli platformlarda yükleyebilirsiniz.

Jupyter not defteri yüklemeniz gerektiğinde ortamını yüklemek ve yapılandırmak sağlar. Bununla birlikte sistemin bakımından sorumlu olursunuz.

## <a name="azure-notebooks"></a>Azure Not Defterleri

[Azure not defterleri](https://notebooks.azure.com) (Önizleme) olan Azure bulutundaki bir not defterlerini ortamı. Jupyter üzerinde temel alır ve popüler kitaplıklarla önceden yüklenmiş bir ortam sağlar. Neredeyse hiçbir kurulum ile denemeler başlamak üzere Azure Machine Learning SDK'sı Azure not defterlerinde, zaten yüklü.

Azure not defterleri de not defterlerinizi paylaşma işlemi basitleştirir. Bir URL için not defterlerinizi paylaşın, kitaplığınıza genel hale getirmek veya Azure not defterleri arabiriminden sosyal medyada paylaşın.

Azure not defterleri dezavantajı, ortamı üzerinde tam denetim sahibi olmadığınız ve ihtiyaç duyduğunuz özel yazılım yükleme mümkün olmayabilir ' dir. Not defterlerinizi çalışabilir, ayrıca paylaşılan bir ortamda olduğundan adanmış bir Jupyter not defteri yüklemede daha yavaş.

## <a name="ides-and-code-editors"></a>IDE ve kod düzenleyiciler

Azure Machine Learning ile çalışacak birçok IDE ve kod düzenleyiciler vardır. Azure Machine Learning kullanılarak yüklenebilir SDK yalnızca yazılım gereksinimdir `pip` yardımcı programı.

Öneririz [Visual Studio Code](https://code.visualstudio.com/)gibi Python dil ve Azure Machine Learning ile çalışmaya yönelik araçlar sağlar. Linux, macOS ve Windows platformlarında kullanılabilir.

## <a name="data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi

Veri bilimi sanal makinesi (DSVM), önceki ortamları birleşimidir. Jupyter not defterleri, Visual Studio Code ve Azure Machine Learning önceden yüklenmiş SDK sahip Azure platformunda bir vm'dir. VM oluşturmak, sıfırdan bir makine ayarı daha az karmaşık ancak Azure not defterleri daha karmaşık bir işlemdir. Gerekli yazılımı sanal makine görüntüsünü önceden yüklenmiş olduğundan, VM oluşturulduktan sonra Azure Machine Learning ile hızlı bir şekilde denemeye başlayabilirsiniz.

DSVM CPU, GPU ve bellek gibi gereken bilgi işlem kaynaklarını seçmenizi sağlar. Ayrıca yazılım TensorFlow, Keras ve PyTorch gibi popüler makine yanı sıra, PyCharm gibi diğer düzenleyiciler ile önceden yüklenir. İhtiyacınız olan yazılımları yüklü değilse, kendiniz yükleyebilirsiniz.

Önceden yüklenmiş nedir daha fazla bilgi için bkz: [veri bilimi sanal makinesi nedir](../data-science-virtual-machine/overview.md) belge.

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme ortamları hakkında bilgi edindiniz, bilgi [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md).

