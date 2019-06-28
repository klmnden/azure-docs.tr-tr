---
title: Eğitme ve modeli - Azure IOT Edge üzerinde Machine Learning dağıtma | Microsoft Docs
description: Azure Machine Learning kullanarak makine öğrenme modeli eğitmek ve sonra modeli bir Azure IOT Edge modülü dağıtılabilir bir kapsayıcı görüntüsü olarak paketi.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: acf0b1984eb3e68858be6ed68731612448e672f4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67432701"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Öğretici: Bir Azure Machine Learning modelini eğitme ve dağıtma

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Bu makalede, Azure not defterleri önce bir machine learning kullanarak Azure Machine Learning modeli eğitmek ve sonra bu modeli bir Azure IOT Edge modülü dağıtılabilir bir kapsayıcı görüntüsü olarak paketlemek için kullanırız. Azure not defterleri, denemeler, eğitmek ve makine öğrenimi modelleri dağıtmak için kullanılan bir temel Taşı olan bir Azure Machine Learning hizmeti çalışma alanında, avantajlarından yararlanın.

Öğreticinin bu bölümünde etkinlikler arasında iki not defteri ayrılır.

* **01 turbofan\_regression.ipynb:** Bu not defteri, eğitim ve Azure Machine Learning kullanarak bir modeli yayımlamak için adımlarda size yol gösterir. Genel anlamıyla adımlar şunlardır:

  1. İndirme, hazırlamak ve eğitim verilerini keşfedin
  2. Hizmet çalışma alanını oluşturmak ve bir machine learning denemesinden çalıştırmak için kullanın.
  3. Denemeyi modeli sonuçlarından değerlendir
  4. En iyi modeli hizmeti çalışma alanında yayımlayabilirsiniz.

* **02 turbofan\_dağıtma\_model.ipynb:** Bu not defteri, önceki not defteri oluşturduğunuz modeli alır ve Azure IOT Edge cihazına dağıtılmaya hazır bir kapsayıcı görüntüsü oluşturmak için kullanır.

  1. Model için Puanlama betiğine oluşturma
  2. Görüntüyü yayımlamanıza ve oluşturma
  3. Görüntüyü Azure Container Instance üzerinde bir web hizmeti olarak dağıtma
  4. Model ve beklendiği gibi görüntü iş doğrulamak için web hizmetini kullanın

Bu makaledeki adımlarda genellikle veri uzmanları tarafından gerçekleştirilmesi.

## <a name="set-up-azure-notebooks"></a>Azure Not Defterleri ' ayarlayın

İki Jupyter Not defterlerinden ve Destek dosyalarını barındırmak için Azure not defterleri kullanın. Burada oluşturun ve bir Azure not defterleri projeyi yapılandırın. Jupyter ve/veya Azure not defterleri kullanmadıysanız, tanıtım belgelerinin birkaç şunlardır:

* **Hızlı Başlangıç:** [Oluşturma ve bir not defteri paylaşma](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Öğretici:** [Oluşturma ve Python ile Jupyter not defteri çalıştırma](../notebooks/tutorial-create-run-jupyter-notebook.md)

Olarak daha önce Geliştirici sanal makineyle Azure not defterlerini kullanarak alıştırma için tutarlı bir ortam sağlar.

> [!NOTE]
> Ayarladıktan Azure not defterleri hizmeti herhangi bir makineden erişilebilir. Kurulum sırasında tüm gereken dosyaları geliştirme sanal makineye kullanmanız gerekir.

### <a name="create-an-azure-notebooks-account"></a>Azure not defterleri hesap oluşturma

Azure not defteri hesapları Azure aboneliklerinden bağımsızdır. Azure not defterleri kullanılacak bir hesabı oluşturmanız gerekir.

1. Gidin [Azure not defterleri](https://notebooks.azure.com).

2. Tıklayın **oturum** sayfanın üst, sağ üst köşesinde.

3. Ya da iş veya Okul hesabı (Azure Active Directory) veya kişisel hesabınızla (Microsoft Account) oturum açın.

4. Azure not defterleri önce kullanmadıysanız, Azure not defterleri uygulama için erişim vermek için istenir.

5. Bir kullanıcı kimliği, Azure not defterleri oluşturun.

### <a name="upload-jupyter-notebooks-files"></a>Jupyter not defterleri dosyaları karşıya yükleme

Bu adımda, yeni bir Azure not defterleri projesi oluşturun ve dosyalarını yükleyin. Özellikle, biz karşıya yükleme dosyaları şunlardır:

* **01-turbofan\_regression.ipynb**: Azure depolama hesabından cihaz bandı tarafından oluşturulan verileri yükleme işleminde size Jupyter not defteri dosyası; keşfetmek ve sınıflandırıcı eğitim için verileri hazırlama; modeli eğitmek; test veri kümesini kullanarak verileri test bulunan Test\_FD003.txt dosya; ve son olarak sınıflandırıcı kaydetme Machine Learning hizmeti çalışma alanında model.

* **02 turbofan\_dağıtma\_model.ipynb:** Bir kapsayıcı görüntüsü oluşturmak için Machine Learning hizmeti çalışma alanına kaydedilir sınıflandırıcı modelini kullanma işleminde size kılavuzluk Jupyter not defteri. Görüntü oluşturulduktan sonra böylece doğrulamak için görüntü bir web hizmeti olarak dağıtma işlemi boyunca size yol olarak çalıştığını not defteri Yürüyüşü bekleniyor. Bizim Edge cihazının doğrulandı resim dağıtılacağı [oluştur ve özel IOT Edge modüllerini dağıtmak](tutorial-machine-learning-edge-06-custom-modules.md) Bu öğretici bölümü.

* **Test\_FD003.txt:** Bu dosya, bizim eğitilen sınıflandırıcı doğrularken ayarlamak bizim test olarak kullanacağız verileri içerir. Örneğin kolaylık olması için ayarlanmış bizim test olarak özgün yarışması için sağlanan test verilerini kullanmak seçtik.

* **RUL\_FD003.txt:** Bu dosya, test her bir cihazın son döngüsü için RUL içerir\_FD003.txt dosya. Bkz: **readme.txt** ve **zarar yayma Modeling.pdf** C: dosyalarında\\kaynak\\IoTEdgeAndMlSample\\veri\\Turbofan için bir verileri ayrıntılı bir açıklama.

* **Utils.PY:** Verilerle çalışmak için Python yardımcı işlevler kümesi içerir. İlk not defterini işlevlerini ayrıntılı bir açıklama içerir.

* **Benioku.MD:** Not defterlerini kullanımını açıklayan Benioku.

Yeni bir proje oluşturun ve dosyaları, not defterine yükleyin.

1. Seçin **Projelerim** üst menü çubuğundan.

1. Seçin **+ yeni proje**. Bir ad ve bir kimliğini girin Genel veya Benioku sahip proje için gerek yoktur.

1. Seçin **karşıya** ve **bilgisayarı**.

1. Seçin **seçin dosyaları**.

1. Gidin **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Tüm dosyaları seçin ve tıklayın **açık**.

1. Seçin **karşıya** karşıya yükleme başlar ve ardından **Bitti** işlemi tamamlandıktan sonra.

## <a name="run-azure-notebooks"></a>Azure not defterleri çalıştırın

Proje oluşturulduktan sonra çalıştırma **01 turbofan\_regression.ipynb** dizüstü bilgisayar.

1. Turbofan proje sayfasından seçin **01 turbofan\_regression.ipynb**.

    ![İlk notebook to run seçin](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

2. İstenirse, iletişim ve select Python 3.6 çekirdek seçin **ayarlamak çekirdek**.

3. Not Defteri olarak listeleniyorsa **güvenilmeyen**, tıklayın **güvenilmeyen** üst pencere öğesi sağ not defterinin. İletişim kutusu göründüğünde, seçersiniz **güven**.

4. Not defterindeki yönergeleri izleyin.

    * `Ctrl + Enter` bir hücreyi çalıştırır.
    * `Shift + Enter` bir hücreyi çalıştırır ve sonraki hücreye gider.
    * Bir hücreyi çalışırken köşeli ayraçların arasına bir yıldız işareti gibi sahip **[\*]** . Aşağıda bu tamamlandığında, yıldız işareti, bir sayı ile değiştirilecek ve ilgili çıkış olabilir görünür. Hücreleri öncekilerle çalışmaları sık derleme olduğundan, yalnızca tek bir hücrede aynı anda çalıştırabilirsiniz.

5. Çalışan tamamladığınızda **01 turbofan\_regression.ipynb** Not Defteri, proje sayfaya dönün.

6. Açık **02 turbofan\_dağıtma\_model.ipynb** ve ikinci not defteri çalıştırmak için bu bölümdeki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure not defterlerinde çalıştıran iki Jupyter Notebook turbofan cihazlardan gelen veriler sınıflandırıcı bir kapsayıcı görüntüsü oluşturma ve dağıtma ve görüntü olarak web uygulamaları test etmek için bir model olarak kaydetmek için kalan faydalı ömrü (RUL) sınıflandırıcı, eğitmek için kullanılacak kullandık izmet.

IOT Edge cihazı oluşturmak için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [IOT Edge cihazı yapılandırma](tutorial-machine-learning-edge-05-configure-edge-device.md)
