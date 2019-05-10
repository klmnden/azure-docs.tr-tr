---
title: 'Hızlı Başlangıç: Bulutta bir not defteri çalıştırma'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Yönetilen notebook sunucusu bulutta çalışma alanınızı kullanın.  Çalışma alanınızda denemeler, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 1a48f8620fb99f1cf8787dabc738d328a796d093
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510626"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için bir bulut tabanlı bir not defteri sunucusu kullan

Bir bulut tabanlı bir not defteri sunucusu oluşturmak ve ardından bunu kullanın.  Bu hızlı başlangıçta, değer'de oturum açması Python kodunu çalıştırma [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md). Çalışma alanı, denemeler, eğitmek ve Machine Learning ile makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır. 

Bu hızlı başlangıçta Azure Machine Learning çalıştırmak için gereken Python ortamını ile yapılandırılmış bulut kaynağı, Azure Machine Learning çalışma alanı oluşturma işlemi gösterilmektedir. Bunun yerine ortamınızda kullanmak için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için kendi notebook sunucusu kullanmak](quickstart-run-local-notebook.md).  

Bu hızlı başlangıçta, aşağıdaki eylemleri gerçekleştirin:

* Çalışma alanınızda yeni bir bulut tabanlı bir not defteri sunucusu oluşturun.
* Jupyter web arabirimi başlatın.
* Pi ve günlükleri hataları her yinelemede, tahmin kodunu içeren bir not defterini açın.
* Not Defteri çalıştırma.
* Oturum hata değerlerini çalışma alanınızda görüntüleyin. Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanı varsa, atlamak [sonraki bölümde](#create-a-cloud-based-notebook-server). Aksi takdirde, şimdi bir tane oluşturun.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-a-cloud-based-notebook-server"></a>Bulut tabanlı bir not defteri sunucusu oluşturma

 Çalışma alanınızdan, Jupyter not defterleri ile çalışmaya başlamak için bir bulut kaynağı oluşturun. Bu kaynak, önceden yapılandırılmış Azure Machine Learning hizmeti çalıştırmak için gereken her şeyi ile bulut tabanlı bir platform sunar.

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  Çalışma alanınızı portalda bulun bilmiyorsanız, bkz. nasıl [çalışma alanınızı bulmak](how-to-manage-workspace.md#view).

1. Azure portalında çalışma sayfasında, seçin **not defteri Vm'leri** soldaki.

1. Seçin **+ yeni** not defteri VM oluşturmak için.

     ![Yeni sanal Makineyi seçin](./media/quickstart-run-cloud-notebook/add-workstation.png)

1. Sanal Makineniz için bir ad sağlayın. Ardından **Oluştur**’u seçin.

    > [!NOTE]
    > Not Defteri VM adınız 2 ile 16 karakter arasında olmalıdır. Geçerli karakterler: harfler, sayılar ve ve karakter.  Adı, ayrıca Azure aboneliğinizi arasında benzersiz olmalıdır.

    ![Yeni VM oluşturma](media/quickstart-run-cloud-notebook/create-new-workstation.png)

1. Yaklaşık 4-5 durum olana kadar dakika bekleyip **çalıştıran**.

## <a name="launch-jupyter-web-interface"></a>Jupyter web arabirimi başlatın

Sanal makinenizin çalışmaya başladıktan sonra kullanın **not defteri Vm'leri** Jupyter web arabirimini açmak için bölüm.

1. Seçin **Jupyter** içinde **URI** VM'niz için sütun.  

    ![Jupyter notebook sunucusu Başlat](./media/quickstart-run-cloud-notebook/start-server.png)

    Bağlantı, Not Defteri sunucusu başlatır ve Jupyter notebook Web sayfasını yeni bir tarayıcı sekmesinde açılır.  Bu bağlantı, yalnızca sanal Makineyi oluşturan kişi için çalışır.

1. Jupyter not defteri sayfasında üst foldername sizin kullanıcı adınızdır.  Bu klasörü seçin.

1. Bir sürüm numarası, örneğin örnekleri foldername içerir **örnekleri 1.0.33.1**.  Örnekleri klasörü seçin.

1. Seçin **hızlı** dizüstü bilgisayar.

## <a name="run-the-notebook"></a>Not defterini çalıştırma

Pi tahminleri ve çalışma alanınıza Hata günlüklerini bir not defteri çalıştırın.

1. Seçin **01.run experiment.ipynb** not defterini açın.

1. İlk kodu hücreyi tıklatın ve seçin **çalıştırma**.

    > [!NOTE]
    > Kod hücreleri önce bunları ayraçlar var. Köşeli ayraçlar boş olduğunda (__[]__), kod çalıştırılmadı. Kodu çalışırken, bir yıldız işareti görürsünüz (__[*]__). Kod tamamlandıktan sonra bir sayı **[1]** görünür.  Sayı hücreleri çalıştırıldığı sırada söyler.
    >
    > Kullanım **Shift girin** bir hücresini çalıştırmak için bir kısayol olarak.

    ![Birinci kod hücresini çalıştırmak](media/quickstart-run-cloud-notebook/cell1.png)

1. İkinci kod hücresini çalıştırmak. Kimlik doğrulaması yapmak için yönergeleri görürseniz, kodu kopyalayın ve oturum açmak için bağlantıyı izleyin. Oturum açtıktan sonra bu ayar, tarayıcınızın hatırlanır.  

    ![Kimlik Doğrula](media/quickstart-run-cloud-notebook/authenticate.png)

1. Tamamlandığında, hücre sayısını __[2]__ görünür.  Oturum açmak olsaydı, kimlik doğrulaması başarılı durum iletisi görürsünüz.   Oturum açmak zorunda olmadığı, bu hücre için herhangi bir çıktı görmezsiniz, yalnızca sayı hücresi başarıyla çalıştığını göstermek için görünür.

    ![Başarı iletisi](media/quickstart-run-cloud-notebook/success.png)

1. Kod hücreleri geri kalanını çalıştırın.  Her bir hücresinde çalışmayı tamamladıktan gibi görünen hücre sayısı görürsünüz. Yalnızca son hücreye herhangi bir çıkış görüntüler.  

    En büyük kodu hücreyi gördüğünüz `run.log` birden fazla yerde kullanılır. Her `run.log` çalışma alanınıza değeri ekler.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

1. Çıkışı `run` hücre geri çalışma alanınızda deneme sonuçlarını görüntülemek için Azure portalına bir bağlantı içerir.

    ![Denemeleri görüntüleme](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Tıklayın **Azure portalı bağlantısını** çalışma alanınızda çalıştırma hakkında bilgi görüntülemek için.  Bu bağlantı, Azure portalında çalışma alanınızı açar.

1. Çizimler gördüğünüz günlüğe kaydedilen değerler otomatik olarak çalışma alanında oluşturulur. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur.

   ![Geçmişi görüntüleme](./media/quickstart-run-cloud-notebook/web-results.png)

Yaklaşık PI koda rastgele değerler kullandığından, çizimleri farklı değerler gösterilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="stop-the-notebook-vm"></a>Not defterini VM durdurma

Maliyetini azaltmak için kullanmadığınızda, Not defterini VM'yi durdurun.  

1. Çalışma alanınızı seçin **not defteri Vm'leri**.

   ![VM sunucu stop](./media/quickstart-run-cloud-notebook/stop-server.png)

1. Listeden VM’yi seçin.

1. Seçin **Durdur**.

1. Yeniden sunucunun kullanıcı için hazır olduğunuzda, seçin **Başlat**.

### <a name="delete-everything"></a>Her şeyi silin

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu korumakla birlikte tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntülemek ve seçmek **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bu görevleri tamamlandı:

* VM bir not defteri oluşturun.
* Jupyter web arabirimi başlatın.
* Pi ve günlükleri hataları her yinelemede, tahmin kodunu içeren bir not defterini açın.
* Not Defteri çalıştırma.
* Oturum hata değerlerini çalışma alanınızda görüntüleyin.  Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

Jupyter not defteri Web sayfasında, diğer not defterlerinde Azure Machine Learning hizmeti hakkında daha fazla bilgi edinmek için örnekler klasörüne göz atın.

Ayrıntılı iş akışı deneyimi için eğitmek ve model dağıtma için Machine Learning öğreticileri izleyin:  

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)
