---
title: Örnek Jupyter Not Defterleri
titleSuffix: Azure Machine Learning service
description: Bulup Python Azure Machine Learning hizmetinde keşfetmek için örnek Jupyter not defterleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 0d74f731d0a7eca25238344e36838dc6c806c788
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53434537"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Azure Machine Learning hizmeti keşfetmek için Jupyter not defterleri kullanma


Kolaylık olması için Jupyter Python not defterleri, Azure Machine Learning hizmeti keşfetmek için kullanabileceğiniz bir dizi geliştirdik. 

Bu sitedeki belgelere hizmeti kullanmak ve bunları kendi durumunuza özelleştirmek için bu not defterlerini kullanma hakkında bilgi edinin. 

## <a name="prerequisite"></a>Önkoşul

Tamamlamak [Azure Machine Learning Python hızlı](quickstart-get-started.md) bir çalışma alanı oluşturma ve Azure not defterleri başlatın.

## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Azure not defterleri deneyin: Bulutta ücretsiz Jupyter Not Defterleri

Azure not defterleri ile başlamak kolaydır! [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) zaten yüklü olan ve Azure not defterleri ile ilgili sizin için yapılandırılır. Yükleme ve gelecekteki güncelleştirmelerin otomatik olarak Azure Hizmetleri yönetilir.
  
+ Çalıştırılacak **öğretici not defterlerini çekirdek**:
  1. Git [Azure not defterleri](https://notebooks.azure.com/).
    
  1. Bulma **öğreticiler** klasöründe **Başlarken** önkoşul hızlı başlangıç sırasında oluşturulan kitaplığı.
    
  1. Çalıştırmak istediğiniz not defterini açın.
    
+ Çalıştırılacak **diğer not defterlerini**:

  1. [Örnek Not Defterleri alma](https://aka.ms/aml-clone-azure-notebooks) Azure not defterleri ile.

  1. Bir çalışma alanı yapılandırma dosyası, bu yöntemlerden birini kullanarak kitaplığa ekleyin:
     + Kopyalama **config.json** dosya **Başlarken** kitaplığına yeni kopyalanan kitaplığı.

     + Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
    
  1. Çalıştırmak istediğiniz not defterini açın.     


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Bir veri bilimi sanal makinesi (DSVM) kullanın

[Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) ve not defteri sunucusu zaten yüklü ve sizin için bir DSVM üzerinde yapılandırılır. Not defterlerini çalıştırmak aşağıdaki adımları kullanın.

1. [Bir DSVM oluşturma](how-to-configure-environment.md#dsvm).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

1. Bir çalışma alanı yapılandırma dosyası, bu yöntemlerden birini kullanarak kitaplığa ekleyin:
    * Kopyalama **aml_config\config.json** kopyalanmış dizine önkoşul Hızlı Başlangıç'ı kullanarak oluşturduğunuz dosya.

    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

## <a name="use-your-own-jupyter-notebook-server"></a>Kendi Jupyter notebook sunucusu kullanma

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.

1. Azure Machine Learning SDK'ları yüklediğiniz önkoşul hızlı başlangıcını tamamladınız emin olun.

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

1. Bir çalışma alanı yapılandırma dosyası, bu yöntemlerden birini kullanarak kitaplığa ekleyin:
    * Kopyalama **aml_config\config.json** kopyalanmış dizine önkoşul Hızlı Başlangıç'ı kullanarak oluşturduğunuz dosya.
    
    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

1. Not içeren klasöre gidin.

1. Notebook'u açın.

<a name="auto"></a>

## <a name="automated-ml-setup"></a>Otomatik ML Kurulumu 

**Bu adımları yalnızca not defterlerinde uygulamak `automated-machine-learning` klasör.**

Yukarıdaki seçeneklerden herhangi birini kullanabilirsiniz, ancak ortamını yükleyin ve aşağıdaki yönergeleri ile aynı zamanda bir çalışma alanı oluşturun. 

1. Yükleme [Mini conda](https://conda.io/miniconda.html). 3.7 seçin veya daha yüksek. Yüklemek için istemleri takip edin. 
   >[!NOTE]
   >Mevcut bir conda olarak sürüm 4.4.10 olması veya üzeri kullanabilirsiniz. Kullanım `conda -V` sürümünü görüntülemek için. Conda sürüm komutuyla güncelleştirebilirsiniz: `conda update conda`. Mini conda özellikle yüklemek için gerek yoktur.

1. Örnek Not defterlerinden indirme [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) zip olarak ve içeriğini yerel bir dizine ayıklayın. Otomatik makine öğrenimi not defterlerini bulunan `how-to-use-azureml/automated-machine-learning` klasör.

1. Yeni bir Conda ortamı ayarlayın. 
   1. Yerel makinenizde bir Conda istemi açın.
   
   1. Yerel makinenize ayıklanan dosyaları gidin.
   
   1. Açık `automated-machine-learning` klasör.
   
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

1. Otomatik-makine öğrenimi klasörü açın ve ardından `configuration.ipynb` dizüstü bilgisayar. 

1. Hücreleri Machine Learning Services kaynak sağlayıcısını kaydedin ve bir çalışma alanı oluşturmak için Not defterini yürütün.

Açın ve yerel makinenizde kaydedilen not defterlerini çalıştırmak artık hazırsınız.


## <a name="next-steps"></a>Sonraki adımlar

Keşfedin [Azure Machine Learning hizmeti için GitHub not defterlerini deposu](https://aka.ms/aml-notebooks)

Bu öğreticileri deneyin:
+ [Eğitmek ve bir görüntü sınıflandırma modeli MNIST ile dağıtma](tutorial-train-models-with-aml.md)

+ [Veri hazırlama ve NYC taksi veri kümesiyle bir regresyon modeli eğitmek için otomatik makine öğrenimi kullanıyor](tutorial-data-prep.md)
