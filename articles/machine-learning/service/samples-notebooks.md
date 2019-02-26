---
title: Örnek Jupyter Not Defterleri
titleSuffix: Azure Machine Learning service
description: Bulup Python Azure Machine Learning hizmetinde keşfetmek için örnek Jupyter not defterleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 12da1b20c5e4e6299445b8ec8ec90eeec6711e2c
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805527"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Azure Machine Learning hizmeti keşfetmek için Jupyter not defterleri kullanma

Kolaylık olması için Jupyter Python not defterleri, Azure Machine Learning hizmeti keşfetmek için kullanabileceğiniz bir dizi geliştirdik. 

Bu sitedeki belgelere hizmeti kullanmak ve bunları kendi durumunuza özelleştirmek için bu not defterlerini kullanma hakkında bilgi edinin. 

Bu örnek not defterleri ile bir not defteri sunucusu çalıştırmak için aşağıdaki yollardan birini kullanın.  Sunucu çalıştırıldıktan sonra öğretici not defterlerinde Bul **öğreticiler** klasöründe veya farklı özellikleri keşfedin **Yardım-How-to-kullanın-azureml** klasör.


## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Azure not defterleri deneyin: Bulutta ücretsiz Jupyter Not Defterleri

Azure not defterleri ile başlamak kolaydır! [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) zaten yüklü olan ve sizin için yapılandırılmış [Azure not defterleri](https://notebooks.azure.com/). Yükleme ve gelecekteki güncelleştirmelerin otomatik olarak Azure Hizmetleri yönetilir.
  
[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Bir veri bilimi sanal makinesi (DSVM) kullanın

[Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) ve not defteri sunucusu zaten yüklü ve sizin için bir DSVM üzerinde yapılandırılır. 

Çalıştırdıktan sonra [bir DSVM oluşturma](how-to-configure-environment.md#dsvm), not defterlerini çalıştırmak için DSVM üzerinde aşağıdaki adımları kullanın.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]


## <a name="use-your-own-jupyter-notebook-server"></a>Kendi Jupyter notebook sunucusu kullanma

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Hızlı Başlangıç yönergeleri, hızlı ve öğretici not defterlerini çalıştırmak için gereken paketleri yükler.  Diğer örnek not defterleri, ek bileşen yüklenmesini gerektirebilir.  Bu bileşenler hakkında daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK'sını yükleme](https://docs.microsoft.com/python/api/overview/azure/ml/install).

<a name="automated-ml-setup"></a>

## <a name="automated-machine-learning-setup"></a>Otomatik makine öğrenimi Kurulumu 

_Bu adımları yalnızca not defterlerinde uygulamak **how-to-use-azureml/automated-machine-learning** klasör._

Yukarıdaki seçeneklerden herhangi birini kullanabilirsiniz, ancak ortamını yükleyin ve aşağıdaki yönergeleri ile aynı zamanda bir çalışma alanı oluşturun. 

1. Yükleme [Mini conda](https://conda.io/miniconda.html). 3.7 seçin veya daha yüksek. Yüklemek için istemleri takip edin. 
   >[!NOTE]
   >Mevcut bir conda olarak sürüm 4.4.10 olması veya üzeri kullanabilirsiniz. Kullanım `conda -V` sürümünü görüntülemek için. Conda sürüm komutuyla güncelleştirebilirsiniz: `conda update conda`. Mini conda özellikle yüklemek için gerek yoktur.

1. Örnek Not defterlerinden indirme [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) zip olarak ve içeriğini yerel bir dizine ayıklayın. Otomatik makine öğrenimi not defterlerini bulunan `how-to-use-azureml/automated-machine-learning` klasör.

1. Yeni bir Conda ortamı ayarlayın. 
   1. Yerel makinenizde bir Conda istemi açın.
   
   1. Yerel makinenize ayıklanan dosyaları gidin.
   
   1. Açık **otomatik-makine öğrenimi** klasör.
   
   1. Yürütme `automl_setup.cmd` conda istemi, Windows veya `.sh` işletim sisteminiz için dosya. Yürütme yaklaşık 10 dakika sürebilir.

      Kurulum betiği:
      + Yeni bir conda ortamı oluşturur
      + Gerekli paketleri yükler
      + Pencere öğesini yapılandırır
      + Jupyter not defteri başlatır
      
   >[!NOTE]
   > Komut dosyası conda ortam adı, isteğe bağlı bir parametre olarak alır. Varsayılan conda ortam adı `azure_automl`. Tam komut, işletim sistemine bağlıdır. Bu, yeni bir ortam oluşturuyorsanız veya yeni bir sürümüne yükseltmeniz yararlı olur. Örneğin, bir ortamından adı azure_automl_sandbox oluşturmak için 'automl_setup.cmd azure_automl_sandbox' kullanabilirsiniz. 
      
1. Betik tamamlandıktan sonra tarayıcınızda bir Jupyter not defteri giriş sayfasını görürsünüz.

1. Not defterlerini kaydettiğiniz yoluna gidin. 

1. Otomatik-makine öğrenimi klasörü açın ve ardından **configuration.ipynb** dizüstü bilgisayar. 

1. Hücreleri Machine Learning Services kaynak sağlayıcısını kaydedin ve bir çalışma alanı oluşturmak için Not defterini yürütün.

Açın ve yerel makinenizde kaydedilen not defterlerini çalıştırmak artık hazırsınız.


## <a name="next-steps"></a>Sonraki adımlar

Keşfedin [Azure Machine Learning hizmeti için GitHub not defterlerini deposu](https://aka.ms/aml-notebooks)

Bu öğreticileri deneyin:
+ [Eğitmek ve bir görüntü sınıflandırma modeli MNIST ile dağıtma](tutorial-train-models-with-aml.md)

+ [Veri hazırlama ve NYC taksi veri kümesiyle bir regresyon modeli eğitmek için otomatik makine öğrenimi kullanıyor](tutorial-data-prep.md)
