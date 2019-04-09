---
title: Hızlı Başlangıç bulutta bir not defteri çalıştırma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Yönetilen notebook sunucusu bulutta çalışma alanınızı kullanın.  Çalışma alanınızda denemeler, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 9cd643185fb4647b19082980edfd333c507aab8a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266265"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için bir bulut tabanlı bir not defteri sunucusu kullan

Bir bulut tabanlı bir not defteri sunucusu oluşturmak ve ardından bu değerleri Azure Machine Learning hizmetinde oturum kodu çalıştırmak için kullanın [çalışma](concept-azure-machine-learning-architecture.md). Çalışma alanınızda denemeler, eğitmek ve Machine Learning ile makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır. 

Bu hızlı başlangıçta Azure Machine Learning çalıştırmak için gereken Python ortamını ile yapılandırılmış bulut kaynağı, Azure Machine Learning çalışma alanı oluşturma işlemi gösterilmektedir. Bunun yerine ortamınızda kullanmak için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için kendi notebook sunucusu kullanmak](quickstart-run-local-notebook.md).  
 
Bu hızlı başlangıçta, aşağıdaki eylemleri gerçekleştirin:

* İş istasyonu oluşturma
* İş istasyonunuzda bir Jupyter not defteri sunucusu Başlat
* Pi ve günlükleri hataları her yinelemede, tahmin kodunu içeren bir not defterini açın.
* Not Defteri çalıştırma.
* Oturum hata değerlerini çalışma alanınızda görüntüleyin.  Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="prerequisites"></a>Önkoşullar

1. [Bir Azure Machine Learning çalışma alanı oluşturma](setup-create-workspace.md#portal) tane yoksa.

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  Çalışma alanınızı portalda bulun bilmiyorsanız, bkz. nasıl [çalışma alanınızı bulmak](how-to-manage-workspace.md#view).

## <a name="create-a-workstation"></a>İş istasyonu oluşturma 

Bir not defteri iş istasyonu, Azure Machine Learning hizmeti çalıştırmak için gereken her şeyi ile önceden yapılandırılmış Jupyter Notebook için bulut tabanlı bir platform sunar. Çalışma alanınızdan, Jupyter not defterleri ile çalışmaya başlamak için bu platform oluşturabilirsiniz.

1. Azure portalında çalışma sayfasında, seçin **not defteri iş istasyonu** soldaki.

1. Seçin **not defterlerine bir Azure Machine Learning iş istasyonu (Önizleme) oluşturma**

   ![Çalışma alanını keşfedin](./media/quickstart-run-cloud-notebook/explore-aml.png)

1. **Not defteri iş istasyonları** bölüm çalışma alanınızda kullanılabilir tüm bulut tabanlı bir not defteri sunucularının bir listesini gösterir.  Buradan bu kaynakları ve artık gerekli değilse silin. 

1. Seçin **iş istasyonu eklemek** bir not defteri iş istasyonu oluşturmak için.

     ![İş istasyonu Ekle'yi seçin](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Not Defteri iş istasyonu eklemek bölümünü istasyonunuzu vermek bir **işlem adı** seçip bir **işlem türü**. Ardından **Oluştur**’u seçin.

    ![Yeni iş istasyonu oluşturma](media/quickstart-run-cloud-notebook/create-new-workstation.png)

    > [!NOTE]
    > İş istasyonunuzu oluşturmak için yaklaşık iki dakika sürer. İşiniz bittiğinde durumu "Çalışıyor" güncelleştirmelerini ve Jupyter ve JupyterLab bağlantılar görüntülenir.

## <a name="launch-jupyter-web-interface"></a>Jupyter web arabirimi başlatın

İş istasyonunuzu oluşturulduktan sonra Jupyter web arabirimini açmak için Not Defteri iş istasyonları bölümü kullanın.

* Seçin **Jupyter** veya **Jupyter Laboratuvar** içinde **başlatma** istasyonunuzu için sütun.

    ![Jupyter notebook sunucusu Başlat](./media/quickstart-run-cloud-notebook/start-notebook-server.png)

    Bu, Not Defteri sunucusu başlatır ve yeni bir tarayıcı sekmesinde sunucusu giriş sayfası açılır.  Sunucunuz, Azure Machine Learning hizmeti ile kullanmaya başlamak için kullanabileceğiniz örnek not defterleri gösterir.

### <a name="run-the-notebook"></a>Not defterini çalıştırma

Pi tahminleri ve çalışma alanınıza Hata günlüklerini bir not defteri çalıştırın.

1. Seçin **01.run experiment.ipynb** not defterini açın.

1. Durum alanı çekirdek başlatıldı kadar beklenecek söyler.  Çekirdek hazır olduğunda ileti kaybolur.

    ![Çekirdek başlatmak bekle](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1. Çekirdek başlatıldıktan sonra bir hücre, bir zaman kullanarak çalıştırın **Shift + Enter**. Veya **hücreleri** > **tümünü Çalıştır** tüm not defterlerini çalıştırmak için. Bir yıldız işareti gördüğünüzde (__*__) hücrede yanında hücre hala çalışıyor. Hücredeki kodun çalışması tamamlandığında bir sayı görünür.  

Not defterinde çalıştıran tüm hücreleri bitirdikten sonra çalışma alanınızda günlüğe kaydedilen değerleri görüntüleyebilirsiniz.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

1. Çıkışı `run` hücre geri çalışma alanınızda deneme sonuçlarını görüntülemek için Azure portalına bir bağlantı içerir. 

    ![Denemeleri görüntüleme](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Tıklayın **Azure portalı bağlantısını** çalışma alanınızda çalıştırma hakkında bilgi görüntülemek için.  Bu bağlantı, Azure portalında çalışma alanınızı açar.

1. Çizimler gördüğünüz günlüğe kaydedilen değerler otomatik olarak çalışma alanında oluşturulur. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur.

   ![Geçmişi görüntüleme](./media/quickstart-run-cloud-notebook/web-results.png)

Yaklaşık PI koda rastgele değerler kullandığından, çizimleri farklı değerler gösterilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu korumakla birlikte tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntülemek ve seçmek **Sil**.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, aşağıdaki tamamlandı:

* İş istasyonu oluşturma
* İş istasyonunuzda bir Jupyter not defteri sunucusu Başlat
* Pi ve günlükleri hataları her yinelemede, tahmin kodunu içeren bir not defterini açın.
* Not Defteri çalıştırma.
* Oturum hata değerlerini çalışma alanınızda görüntüleyin.  Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

Ayrıntılı iş akışı deneyimi için eğitmek ve model dağıtma için Machine Learning öğreticileri izleyin:  

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)
