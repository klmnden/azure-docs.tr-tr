---
title: "Görüntü CNTK Azure Machine Learning çalışma ekranı içinde kullanarak sınıflandırma | Microsoft Docs"
description: "Eğitim, değerlendirmek ve Azure ML çalışma ekranı kullanarak özel görüntü sınıflandırma modeli dağıtın."
services: machine-learning
documentationcenter: 
author: PatrickBue
ms.author: pabuehle
ms.reviewer: mawah, marhamil, mldocs
ms.service: machine-learning
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: 64a035c216e4d7aa4c14baf1812b9a25e27b3e19
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Azure Machine Learning çalışma ekranı kullanarak görüntü sınıflandırma

Görüntü sınıflandırma yaklaşım, çok sayıda bilgisayar görme sorunları çözmek için kullanılabilir.
Bunlar gibi soruları yanıtlamak model oluşturmaya içerir: *görüntüde mevcut bir nesne?* nereye nesne örneğin olabilir *köpek*, *araba*, veya  *Sevk*. Veya gibi daha karmaşık soruları: *hangi sınıfın göz Hastalık önem derecesine göre bu hasta'nın retinal tarama evinced?*.

Bu tür problemleri çözmek Bu öğretici giderir. Eğitim, değerlendirmek ve kendi görüntü sınıflandırma modelini kullanarak dağıtma gösteriyoruz [Microsoft Bilişsel Araç Seti (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) ayrıntılı bilgi.
Sağlanan örnek görüntüler, ancak Okuyucu, ayrıca kendi veri kümesi getirin ve kendi özel modelleri eğitme.

Çözümleri geleneksel gerekli el ile tanımlamak ve sözde uygulamak için uzman bilgisayar görme *özellikleri*, istenen bilgileri görüntülerinde vurgulayın.
El ile bu yaklaşım değiştirilen 2012 ünlü [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) kağıt ve şu anda öğrenme [1] ayrıntılı derin sinir ağları (DNN) otomatik olarak bu özellikleri bulmak için kullanılır.
Yalnızca görüntü Sınıflandırma, ancak aynı zamanda nesne algılama ve görüntü benzerlik gibi diğer bilgisayar görme sorunları alanında büyük bir geliştirme için DNNs gerektiriyordu.


## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Genel Bakış

Bu öğretici, üç bölüme ayrılır:

- 1. kısım, eğitme, değerlendirmek ve çıktısını üzerinde bir SVM eğitim ve önceden eğitilen DNN featurizer kullanarak bir görüntü sınıflandırma sistemi dağıtma gösterilmektedir.
- 2. kısım daha sonra doğruluğunu artırmak nasıl Örneğin, sabit featurizer kullanmak yerine DNN daraltmayı gösterir.
- Sağlanan örnek görüntünüzü yerine kendi veri kümesi kullanma bölüm 3 kapsayan ve gerekirse, net değiştirilene kendi veri kümesi oluşturmak nasıl görüntüler.

Machine learning ve CNTK önceki deneyimiyle gerekli olmamasına karşın, temel alınan ilkeleri anlamak için faydalıdır. Eğitim zaman, öğreticide bildirilen vb. doğruluğu, yalnızca başvuru numaralarıdır ve kod çalıştırırken gerçek değerler hemen kesinlikle farklılık gösterir.


## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

1. Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
2. [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.  
3. Windows makine. Windows işletim sistemi, çalışma ekranı yalnızca Windows ve Microsoft Bilişsel Araç Seti sırasında MacOS desteklediğinden gereklidir (derin öğrenme kitaplık olarak kullanırız) yalnızca destekleyen Windows ve Linux.
4. 2 bölümünde açıklanan DNN iyileştirme için gerekli ancak adanmış bir GPU SVM eğitim bölümü 1, yürütmek için gerekli değildir. Güçlü bir GPU olmadığı, üzerinde birden çok GPU eğitmek istediğiniz ya da bir Windows makinesine sahip değil, daha sonra Azure'nın derin öğrenme sanal makine Windows işletim sistemiyle birlikte kullanmayı düşünün. Bkz: [burada](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning) 1-tıklatma dağıtım kılavuzu. Dağıtıldığında, bir Uzak Masaüstü bağlantısı üzerinden VM bağlanmak, çalışma ekranı var. yüklemek ve kod sanal makineden yerel olarak çalıştırmak.
5. OpenCV gibi çeşitli Python kitaplıkları yüklü olması gerekir. Tıklatın *komut istemini açın* gelen *dosya* menüde çalışma ekranı ve bu bağımlılıklar yüklemek için aşağıdaki komutları çalıştırın:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.2-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.1-cp35-cp35m-win_amd64.whl`(tam dosya adı ve sürümü değiştirebilirsiniz) http://www.lfd.uci.edu/~gohlke/pythonlibs/ OpenCV Tekerlek indirdikten sonra
    - `conda install pillow`
    - `pip install -U numpy`
    - `pip install bqplot`
    - `jupyter nbextension enable --py --sys-prefix bqplot`

### <a name="troubleshooting--known-bugs"></a>Sorun giderme / bilinen hatalar
- Bir GPU 2. bölüm için gereklidir ve aksi takdirde hata "toplu normalleştirme eğitim CPU üzerinde uygulanmadı" DNN iyileştirmek çalışırken atılır.
- Bellek hataları DNN eğitim sırasında minibatch boyutunu azaltarak kaçınılabilir (değişken `cntk_mb_size` içinde `PARAMETERS.py`).
- Kod CNTK 2.2 kullanılarak test edilmiştir ve çalıştırılacak de eski (yukarı) v2.0 için ve daha yeni sürümleri kalmaksızın veya yalnızca küçük değiştirir.
- Yazma zaman Azure Machine Learning çalışma ekranı not defterlerini 5 MB büyük gösteren sorunlarla karşılaştı. Not Defteri ile kaydedilirse, bu büyük boyuttaki not defterlerini oluşabilir tüm görüntülenen çıktı hücre. Bu hatayla karşılaşırsanız, ardından çalışma ekranı içinde Dosya menüsünden komut istemini açın, yürütmeyi `jupyter notebook`, Not Defteri, Temizle tüm çıktı açıp not defteri kaydedin. Bu adımları gerçekleştirdikten sonra dizüstü bilgisayar düzgün bir şekilde Azure Machine Learning çalışma ekranı içinde yeniden açılır.
- Bu örnekte sağlanan tüm komut dosyaları yerel olarak yürütülmesi sahip ve örneğin docker uzak ortamda üzerinde değil. Tüm not defterlerini ada sahip yerel proje çekirdeğe ayarlamak çekirdek yürütülmesi gerekir "<projectname> yerel" (örneğin "myImgClassUsingCNTK yerel").

    
## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturmak için:
1.  Azure Machine Learning Workbench’i açın.
2.  Üzerinde **projeleri** sayfasında,  **+**  oturum ve seçin **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **arama proje şablonları** arama kutusu, "sınıflandırma görüntü" yazın ve şablonu seçin.
5.  **Oluştur**'a tıklayın.

Bu adımları gerçekleştiren aşağıda gösterilen Proje yapısı oluşturur. Proje dizinine, Azure Machine Learning çalışma ekranı (çalıştırma geçmişini etkinleştirmek için) her çalıştırmayı sonra bu klasör bir kopyasını oluşturduğundan MBayt'ı 25'ten az olacak şekilde sınırlıdır. Bu nedenle, tüm görüntü ve geçici dosyalar için ve dizinden kaydedilmiş *~/Desktop/imgClassificationUsingCntk_data* (olarak adlandırılan *DATA_DIR* bu belgedeki).

  Klasör| Açıklama
  ---|---
  aml_config /|                           Azure Machine Learning çalışma ekranı yapılandırma dosyalarını içeren dizini
  kitaplıkları /|                              Tüm Python ve Jupyter yardımcı işlevleri içeren dizin
  not defterlerini /|                              Tüm not defterlerini içeren dizin
  kaynakları /|                              Tüm kaynaklar (örneğin URL'sini şekilde görüntülerinin) içeren dizin
  komut dosyalarını /|                              Tüm komut dosyaları içeren dizini
  PARAMETERS.py|                       Python betiği tüm parametreleri belirtme
  Readme.MD|                           Bu Benioku belgesine


## <a name="data-description"></a>Veri açıklaması

Bu öğretici, en fazla 428 görüntülerini oluşan bir üst gövde giysisinin doku dataset örnek çalışıyor olarak kullanır. Her görüntü üç farklı dokuları (noktalı, şeritli, leopard) biri olarak işaretiyle gösterilir. Biz, görüntü sayısı küçük tutulur, böylece Bu öğretici hızlı yürütülebilir. Bununla birlikte, kod iyi test edilmiş ve binlerce görüntü veya daha fazla ile çalışır. Tüm görüntüleri scraped Bing görüntü arama'yı kullanarak ve el-açıklama başlığında açıklandığı gibi [bölümü 3](#using-a-custom-dataset). İlgili öznitelikleriyle URL'lerle içinde listelenen görüntü */resources/fashionTextureUrls.tsv* dosya.

Komut dosyası `0_downloadData.py` tüm görüntülere indirmeleri *görüntüleri/DATA_DIR/fashionTexture/* dizin. Büyük olasılıkla bozuk 428 URL'leri bazılarıdır. Bu bir sorun değildir ve yalnızca eğitim ve test için biraz daha az resimler sağlanıncaya anlamına gelir. Bu örnekte sağlanan tüm komut dosyaları yerel olarak yürütülmesi sahip ve örneğin docker uzak ortamda üzerinde değil.

Aşağıdaki şekilde şeritli (Orta) ve (sağdaki) leopard (soldaki), noktalı öznitelikleri için örnekler gösterilmektedir. Ek açıklamalar üst gövde giysisinin öğesi göre yapıldığını.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_all.jpg"  alt="alt text" width="700">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Bölüm 1 - Model eğitimi ve değerlendirmesi

Bu öğreticinin ilk bölümü şu kullanır, ancak değişiklik, önceden eğitilen derin sinir ağı bir sistem eğitim. Bu önceden eğitilen DNN featurizer kullanılır ve doğrusal bir SVM özniteliği tahmin etmek için eğitildi (noktalı, şeritli veya leopard) belirli bir görüntünün.

Biz şimdi ayrıntılı, adım adım ve hangi betiklerinin yürütülmesi gerek Göster bu yaklaşımı açıklanmaktadır. Hangi dosyaların yazılır ve için burada yazılan incelemek için her adımdan sonra öneririz.

Tüm önemli parametreleri belirtilir ve kısa bir açıklama sağlanan, tek bir yerde: `PARAMETERS.py` dosya.




### <a name="step-1-data-preparation"></a>1. adım: Verileri hazırlama
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

Not Defteri `showImages.ipynb` görüntüleri görselleştirmek için ve gerektiğinde, ek açıklama düzeltmek için kullanılabilir. Not Defteri çalıştırmak için Azure Machine Learning çalışma ekranı, üzerinde "Başlat not defteri sunucusunu" Bu seçeneği gösteriliyorsa, değiştirme ada sahip yerel proje çekirdeğe tıklayın açın "<projectname> yerel" (örneğin "myImgClassUsingCNTK yerel") ve ardından tüm hücreleri yürütme dizüstü bilgisayar. Not Defteri görüntülenecek büyük olduğunu şikayetçi bir hata alırsanız, bu belgedeki sorun giderme bölümüne bakın.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Şimdi adlı komut dosyası yürütme `1_prepareData.py`, hangi atar ya da eğitim tüm görüntüleri ayarlama ya da test ayarlama. Bu atama birbirini dışlayan - hiçbir eğitim resim test veya tersi de kullanılır. Varsayılan olarak, her öznitelik sınıfı görüntülerden birini rastgele %75 eğitim için atanan ve test için kalan % 25 atanır. Komut dosyası tarafından oluşturulan tüm verileri kaydedilir *proc/DATA_DIR/fashionTexture/* klasörü.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>2. adım: derin sinir ağı iyileştirme
`Script: 2_refineDNN.py`

Biz bu öğreticinin 1 bölümünde açıklandığı gibi önceden eğitilen DNN sabit tutulur (diğer bir deyişle, Gelişmiş olmadığı). Ancak, komut dosyası adlı `2_refineDNN.py` bölüm 1, önceden eğitilen yüklerken hala yürütülür [ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) [2] modeli ve onu, örneğin, daha yüksek giriş görüntü çözünürlüğü için izin verecek şekilde değiştirir. Bu adım (saniye) hızlı ve bir GPU gerektirmez.

Bölümünde öğreticinin 2 PARAMETERS.py değişiklik dosya nedenler `2_refineDNN.py` de önceden eğitilen DNN iyileştirmek için komut dosyası. Varsayılan olarak, iyileştirme sırasında 45 eğitim dönemlerinde çalıştırın.

Her iki durumda da, son model sonra dosyasına yazılır *DATA_DIR/proc/fashionTexture/cntk_fixed.model*.

### <a name="step-3-evaluate-dnn-for-all-images"></a>3. adım: DNN tüm görüntüleri için değerlendirme
`Script: 3_runDNN.py`

Son adım (büyük olasılıkla Gelişmiş) DNN featurize için artık bizim görüntüleri kullanabilirsiniz. Bir görüntü DNN giriş olarak verilen, çıktı modelinin sondan katmandan 512 float vektör türüdür. Bu vektör görüntüden daha çok daha küçük boyutlu türüdür. Bununla birlikte, bu içeren (ve bile vurgulayın) giysisinin öğesi noktalı, varsa, yayılarak, görüntünün özniteliği tanımak ilgili görüntü veya leopard doku tüm bilgileri.

Tüm DNN görüntü Beyanları dosyasına kaydedilir *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>4. adım: Destek vektör makinesi eğitim
`Script: 4_trainSVM.py`

512 float son adımda hesaplanan Beyanları SVM sınıflandırıcı eğitmek için kullanılan şimdi: bir görüntü giriş olarak verilen, SVM bulunması her bir öznitelik için bir puan çıkarır. Örnek kümemize bu bir puan 'şeritli', 'noktalı' ve 'leopard' için anlamına gelir.

Komut dosyası `4_trainSVM.py` eğitim görüntüleri yükler, bir SVM C regularization (kayma) parametresinin farklı değerler için eğitir ve en yüksek doğruluk ile SVM tutar. Sınıflandırma doğruluğu konsolda yazdırılabilir ve çalışma ekranı çizilen. Sağlanan doku veriler için bu değerleri yaklaşık %100 ve % 88 sırasıyla olmalıdır. Son olarak, eğitilen SVM dosyasına yazılır *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>5. adım: Değerlendirme ve görselleştirme
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

Komut dosyası kullanılarak eğitilmiş görüntü sınıflandırıcı doğruluğunu ölçülebilir `5_evaluate.py`. Komut dosyası puanları eğitilen SVM sınıflandırıcı kullanarak tüm test görüntüleri yüksek puan ve karşılaştırır başından başlayarak gerçekte ek açıklamaları tahmin edilen özniteliklerle özniteliğiyle atar her görüntü.

Komut dosyası çıkışını `5_evaluate.py` aşağıda gösterilmiştir. Tek tek her sınıf sınıflandırma doğruluğu, tam sınama kümesi ('genel doğruluğu') ve ortalama kesinliğini yanı sıra tek tek accuracies ('genel sınıf ortalaması doğruluğu') hesaplanır. % 100 en kötü % 0 ve en iyi olası doğruluğunu karşılık gelir. Rastgele tahmin ortalama üretmek 1 sınıfı ortalaması doğruluğunu öznitelik sayısı: Örneğimizde, bu doğruluğu %33.33 olacaktır. Bu sonuçları önemli ölçüde daha yüksek bir giriş çözünürlüğü gibi kullanırken artırmak `rf_inputResoluton = 1000`, ancak daha uzun DNN hesaplama süreleri ödün verme pahasına.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Doğruluk ek olarak, ilgili alanı altında-eğrisini ile (soldaki); ROC eğrisi çizilir ve karışıklığı matris (sağdaki) gösterilir:

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Son olarak, Not Defteri `showResults.py` test resimler arasında gezinmek ve ilgili sınıflandırma puanlarını görselleştirmek için sağlanmıştır. Bu örnekteki her dizüstü Adım1 içinde açıklandığı gibi yerel proje çekirdek adıyla kullanması gereken "<projectname> yerel":
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>6. adım: dağıtım
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

Eğitilmiş sistem artık bir REST API yayımlanabilir. Dağıtım not defterinde açıklandığı `deploy.ipynb`ve Azure Machine Learning çalışma ekranının içinden işlevselliği temel (ada sahip yerel proje çekirdek çekirdek ayarlamayı unutmayın "<projectname> yerel"). Ayrıca mükemmel dağıtımı bölümüne bakın [IRIS öğretici](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-3) daha fazla dağıtım için ilgili bilgiler.

Uygulama dağıtıldıktan sonra web hizmeti komut dosyası kullanılarak çağrılabilir `6_callWebservice.py`. Web hizmeti IP adresi (yerel veya Bulut üzerinde) ilk komut dosyasında ayarlanan gerektiğini unutmayın. Not Defteri `deploy.ipynb` bu IP adresini bulmak açıklanmaktadır.








## <a name="part-2---accuracy-improvements"></a>Bölüm 2 - doğruluğu geliştirmeleri

Bölüm 1'de, biz derin sinir ağı 512 float çıktısını doğrusal bir destek vektör makinede eğitim tarafından görüntüyü sınıflandırmak nasıl oluşturulacağını gösterir. Bu DNN görüntüleri milyonlarca üzerinde önceden eğitilmiş ve sondan katmanı özellik vektörü olarak döndürdü. Bu yaklaşım, DNN olarak kullanıldığından hızlıdır- olmakla birlikte, yine de genellikle iyi sonuçlar verir.

Biz şimdi bölüm 1 modelden doğruluğunu artırmak için çeşitli yollar sunar. Özellikle, sabit tutmak yerine DNN daraltın.

### <a name="dnn-refinement"></a>DNN iyileştirme

Bir SVM yerine bir sinir ağı sınıflandırmasında doğrudan yapabilirsiniz. Bu, yeni bir son katman giriş olarak sondan katmandan 512 float geçen önceden eğitilen DNN ekleyerek sağlanır. Tam ağ retrained backpropagation artık DNN sınıflandırmasında yapmanın avantajı olmasıdır. Bu yaklaşım genellikle önceden eğitilen DNN olarak kullanmaya kıyasla çok daha iyi sınıflandırma accuracies doğurur-olduğu, ancak daha uzun eğitim saatiyle (hatta GPU) ödün verme pahasına.

Bir SVM yerine sinir ağı eğitiliyor yapılır değişkeni değiştirerek `classifier` içinde `PARAMETERS.py` gelen `svm` için `dnn`. Ardından, 1 bölümünde açıklandığı gibi verileri hazırlama (1. adım) ve SVM eğitim (adım 3) dışında tüm betikler yeniden yürütülmesi gerekir. DNN iyileştirme, bir GPU gerektirir. hiçbir GPU bulunduysa veya GPU (örneğin bir önceki CNTK çalıştırma tarafından) kilitliyse sonra komut dosyası `2_refineDNN.py` bir hata oluşturur. DNN eğitim throw bellek yetersiz hatası minibatch boyutunu azaltarak önlenebilir bazı GPU üzerinde (değişken `cntk_mb_size` içinde `PARAMETERS.py`).

Eğitim tamamlandıktan sonra Gelişmiş modeli kaydedilir *DATA_DIR/proc/fashionTexture/cntk_refined.model*, ve eğitim ve test sınıflandırma hataları eğitim sırasında nasıl değiştiğini gösteren bir çizim çizilmiştir. Eğitim kümesi hatasında test kümesinde çok daha küçük olan bu çizim unutmayın. Bu sözde aşırı sığdırma davranışı, örneğin, düşme oranı daha yüksek bir değer kullanılarak azaltılabilir `rf_dropoutRate`.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Aşağıdaki çizim görüldüğü gibi DNN iyileştirme sağlanan veri kümesi üzerinde kullanarak doğruluğu %92.35 %88.92 (Kısım 1) önce karşı olur. Özellikle, 'noktalı' görüntüleri, bir ROC alanı-altında-eğrisini iyileştirme vs ile 0,98 ile büyük ölçüde artırır. 0,94 önce. Küçük bir veri kümesini kullanıyoruz ve bu nedenle bir kod çalıştırmasını gerçek accuracies farklıdır. Bu uyuşmazlık eğitim ve test kümeleri içine görüntülerinin rastgele bölünmüş gibi stokastik etkileri kaynaklanır.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Geçmiş izlemeyi Çalıştır

Her geçmişini çalıştırmak olan iki veya daha fazla çalıştığında karşılaştırması izin Azure Azure Machine Learning çalışma ekranı depoları bile parçalayın hafta. Bu ayrıntılı olarak anlatılmıştır [Iris Öğreticisi](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-2). Aşağıdaki ekran görüntülerinde komut dosyasının iki çalışması biz karşılaştırmak burada gösterilmiştir `5_evaluate.py`, her iki DNN iyileştirme kullanarak diğer bir deyişle, `classifier = "dnn"`(çalışma sayı 148) veya SVM eğitim diğer bir deyişle, `classifier = "svm"` (çalışma sayı 150).

İlk ekran görüntüsünde, DNN iyileştirme SVM eğitim tüm sınıflar için daha iyi accuracies neden olmaktadır. İkinci ekran sınıflandırıcı neydi dahil olmak üzere izlendiğini tüm ölçümlerini gösterir. Bu izleme komut dosyasındaki yapılır `5_evaluate.py` Azure Machine Learning çalışma ekranı Günlükçü çağırarak. Ayrıca, komut dosyası ROC eğrisi ve karışıklığı matris de kaydeder *çıkarır* klasör. Bu *çıkarır* klasördür özel içeriği da çalışma ekranı geçmişi özelliği tarafından izlenir ve çıktı dosyaları herhangi bir zamanda olup yerel kopyaları üzerine bağımsız olarak, bu nedenle erişilebilir.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>  
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Parametre ayarlama
Projeleri öğrenme çoğu makine için doğru olduğu gibi yeni bir veri kümesi için iyi sonuçları elde ayarlama yanı sıra farklı tasarım kararlarına değerlendirme dikkatli parametresi gerektirir. Bu görevleri yardımcı olmak için tüm önemli parametreleri belirtilir ve kısa bir açıklama sağlanan, tek bir yerde: `PARAMETERS.py` dosya.

Bazı iyileştirmeler için en taahhüdü ihlaline şunlardır:

- Veri Kalitesi: eğitim ve test kümelerine sahip yüksek kaliteli emin olun. Diğer bir deyişle, görüntüleri kaldırılan doğru açıklamalı, belirsiz görüntüleri (örneğin giysisinin öğeleriyle çizgiler ve noktalar) bağımsızdır ve öznitelikleri karşılıklı olarak birbirini dışlar (diğer bir deyişle, her görüntü için tek bir özniteliği ait olacağı şekilde seçilir).
- Görüntüde ilgi, nesne küçükse görüntü sınıflandırma yaklaşımlar düzgün çalışmıyor bilinmektedir. Bu konuda açıklandığı gibi bir nesne algılama yaklaşım kullanarak bu gibi durumlarda göz önünde bulundurun [Öğreticisi](https://github.com/Azure/ObjectDetectionUsingCntk).
- DNN iyileştirme: sağ almak için tartışmaya açık bir şekilde en önemli öğrenme oranı parametredir `rf_lrPerMb`. Eğitim doğruluğu (Kısım 2 ilk şekilde) ayarlarsanız, 0-5 yakın değil %, öğrenme oranı nedeniyle bir sorun olduğu büyük olasılıkla. İle başlayarak diğer parametreler `rf_` daha az önemlidir. Genellikle, eğitim hata katlanarak azaltma ve bu eğitim sonra %0 yakın olması gerekir.
- Giriş çözünürlük: varsayılan görüntü çözünürlüğü 224 x 224 pikseldir. Daha yüksek görüntü çözünürlüğü (parametre: `rf_inputResoluton`), örneğin, 448 x 448 veya 896 x 896 piksel genellikle önemli doğruluğu artırır ancak DNN iyileştirme yavaşlatır. **Daha yüksek görüntü çözünürlüğü neredeyse serbest Yemeği olduğu ve neredeyse her zaman doğruluğu artırır**.
- DNN atlayarak sığdırma: DNN iyileştirme sırasında eğitim ve test doğruluğu arasında büyük bir boşluk kaçının (ilk şekil bölüm 2). Bu aralık çıkarma oranları kullanılarak azaltılabilir `rf_dropoutRate` 0,5 ya da daha fazla bilgi ve regularizer ağırlık artırarak `rf_l2RegWeight`. Bir yüksek çıkarma kuru kullanılarak DNN giriş resim çözünürlüğü yüksekse, özellikle yararlı olabilir.
- Daha derin DNNs değiştirerek kullanmayı deneyin `rf_pretrainedModelFilename` gelen `ResNet_18.model` ya da `ResNet_34.model` veya `ResNet_50.model`. Resnet 50 model yalnızca daha derin değil, ancak çıktısını sondan katmanın boyutu 2048 float (vs. 512 float ResNet 18 ve ResNet 34 modellerin). Bu artan boyut SVM sınıflandırıcı eğitimindeki özellikle yararlı olabilir.

## <a name="part-3---custom-dataset"></a>Bölüm 3 - özel veri kümesi

1 ve 2 bölümünde, eğitilmiş ve sağlanan üst gövde giysisinin dokuları görüntüleri kullanarak bir görüntü sınıflandırma modeli değerlendirilir. Bunun yerine özel bir kullanıcı tarafından sağlanan dataset kullanma şimdi gösteriyoruz. Veya yoksa, nasıl oluşturup gibi Bing kullanarak bir veri kümesi açıklama görüntü arayın.

### <a name="using-a-custom-dataset"></a>Özel bir veri kümesi kullanma

İlk olarak, şirketinizdeki giysisinin doku veri klasör yapısını göz sahip. Not nasıl tüm görüntüler için farklı öznitelikler ilgili klasörlerdeki *noktalı*, * leopard, ve *şeritli* adresindeki *görüntüleri/DATA_DIR/fashionTexture/*. Ayrıca nasıl görüntü klasörü adı da oluşuyor unutmayın `PARAMETERS.py` dosyası:
```python
datasetName = "fashionTexture"
```

Özel bir veri kümesini kullanarak bu klasör yapısını yeniden olarak basit kendi özniteliği göre ve yeni bir kullanıcı tarafından belirtilen dizin için bu alt klasörleri kopyalamak için alt klasörlerdeki tüm görüntüleri nerede *görüntüleri/DATA_DIR/newDataSetName/*. Gerekli yalnızca kod değişikliği ayarlamaktır `datasetName` değişkenini *newDataSetName*. Komut dosyaları 1-5 sonra yürütülebilir sırayla ve tüm ara dosyaları yazılır *proc/DATA_DIR/newDataSetName/*. Diğer bir kod değişiklikleri gereklidir.

Her görüntü için tek bir özniteliği atanabilir önemlidir. Örneğin, bir 'leopard' görüntüsü 'hayvan' da ait olduğundan 'hayvan' ve 'leopard' özniteliklerine sahip yanlış olur. Ayrıca, ek açıklama eklemek bu nedenle zor ve belirsiz görüntüleri kaldırmak en iyisidir.



### <a name="image-scraping-and-annotation"></a>Görüntü değiştirilene ve ek açıklaması

Eğitim ve test için ek açıklama görüntüleri yeterince çok sayıda toplama zor olabilir. Bu sorunu çözmek için bir Internet görüntülerden scrape yoldur. Örneğin, Bing görüntü arama sonuçları sorgu için aşağıya bakın *şeritli ısı*. Beklendiği gibi çoğu görüntü gerçekten şeritli tişörtler. Birkaç yanlış ya da belirsiz görüntüleri (örneğin, sütun 1, 1; satır veya sütun 3, satır 2) tanımlanır ve kolayca kaldırıldı:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>

Büyük ve farklı bir veri kümesi oluşturmak için birden çok sorgu kullanılmalıdır. Örneğin, 7\*3 = 21 sorgular oluşturulan tüm bileşimleri giysisinin öğeleri {blouse, hoodie, pullover, sweater, shirt, ısı, vest} ve {şeritli, noktalı, leopard} öznitelikleri kullanarak otomatik olarak. Sorgu başına ilk 50 resimler karşıdan sonra sağlama 21 * 50 = 1050 maksimum kadar görüntüler.

Görüntüleri Bing görüntü aramadan el ile indirmek yerine, bunun yerine kullanmayı daha kolay olmasından [Bilişsel Hizmetleri Bing görüntü arama API](https://www.microsoft.com/cognitive-services/bing-image-search-api) görüntü URL'leri bir sorgu dizesi belirtilen bir dizi döndürür.

İndirilen resmi tam ya da çoğaltmaları bazıları (örneğin, yalnızca resim çözünürlüğü veya jpg yapıları tarafından farklı). Eğitim ve test bölünmüş içermez aynı görüntüleri bu yinelemeleri kaldırılması. Yinelenen görüntüleri kaldırma elde edilebilir iki adımda çalışan bir karma tabanlı yaklaşımı kullanma: (i) ilk olarak, tüm görüntüler için; karma dize hesaplanır (II) görüntüleri üzerinden ikinci geçişi, yalnızca bu görüntüleri değil henüz görüldü karma dize tutulur. Diğer tüm görüntüleri atılır. Bulduk `dhash` Python Kitaplığı'nda bir yaklaşım `imagehash` ve bu konuda açıklanan [blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) de parametresiyle gerçekleştirmek için `hash_size` 16 olarak ayarlayın. Gerçek çoğaltmaların çoğunluğu kaldırılan sürece yanlış bazı yinelenmeyen görüntüleri kaldırmak için Tamam değildir.





## <a name="conclusion"></a>Sonuç

Bu örnekte anahtar bazı önemli şunlardır:
- Eğitim, değerlendirmek ve görüntü sınıflandırma modelleri dağıtmak için kodu.
- Sağlanan gösterim görüntüler, ancak (kendi görüntü veri kümesini kullanmak için kolayca uyarlanabilir tek bir çizgi değiştirme).
- Resim durumu Uzman özellikleri aktarım öğrenmeyi göre yüksek doğruluk modelleri eğitmek için uygulanması.
- Azure Machine Learning çalışma ekranı ve Jupyter not defteri ile etkileşimli modeli geliştirme.


## <a name="references"></a>Başvurular

[1] Alex Krizhevsky, Ilya Sutskever ve Geoffrey E. Hinton [ _ImageNet sınıflandırma Convolutional derin sinir ağları ile_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf). NIPS 2012.  
[2] Kaiming He, Xiangyu Zhang, Shaoqing Ren ve Jian Sun, [ _derin fazlalık görüntü tanıma için öğrenme_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf). CVPR 2016.
