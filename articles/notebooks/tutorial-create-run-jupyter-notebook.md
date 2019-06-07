---
title: Öğretici - oluşturma ve Azure üzerinde Jupyter not defteri çalıştırma
description: Bir çalışma, Azure veri bilimi doğrusal regresyon işlemini gösteren not defterlerinde Jupyter not defteri oluşturma
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 65bbb5fe-9939-4e8e-8f5b-c197d4be142a
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: kraigb
ms.openlocfilehash: 09d4038e705fb3bc4ff2c82daf5dc4c07f346f94
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751762"
---
# <a name="tutorial-create-and-run-a-jupyter-notebook-with-python"></a>Öğretici: oluşturma ve Python ile Jupyter not defteri çalıştırma

Bu öğreticide basit bir doğrusal regresyon gösteren tam bir Jupyter not defteri oluşturmak için Azure not defterleri kullanma işleminde size yol gösterir. Bu öğretici sırasında Jupyter not defteri farklı hücreleri oluşturma, hücre çalıştıran ve bir slayt gösterisi olarak not defterini sunma içeren kullanıcı Arabirimi tanıyın.

Tamamlanan not defterini bulunabilir [GitHub - Azure not defterleri örnekleri](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps). Bu öğreticide, ancak yeni bir proje ve boş bir not defteri ile başlar adım adım oluşturma deneyimi.

## <a name="create-the-project"></a>Proje oluşturma

1. Git [Azure not defterleri](https://notebooks.azure.com) ve oturum açın. (Ayrıntılar için bkz [hızlı başlangıç - Azure not defterleri için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Genel profil sayfanızdan seçin **Projelerim** sayfanın üstündeki:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. Üzerinde **Projelerim** sayfasında **+ yeni proje** (klavye kısayolu: n); yalnızca olarak görüntülenebilir **+** tarayıcı penceresini darsa:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. İçinde **yeni proje oluştur** görünen açılan girin veya aşağıdaki bilgileri ayarlayın ve ardından **Oluştur**:

    - **Proje adı**: Doğrusal regresyon örnek - Cricket cıvıltılarını
    - **Proje kimliği**: gerileme örnek doğrusal
    - **Genel proje**: (Seçili)
    - **Bir Benioku.MD oluşturma**: (Seçili)

1. Birkaç dakika sonra Azure not defterleri yeni projeye gider.

## <a name="create-the-data-file"></a>Veri dosyası oluşturma

Adlı projenize bir dosyadan veri Not Defteri oluşturduğunuz doğrusal regresyon modelinin çizer *cricket_chirps.csv*. Buradan kopyalayarak bu dosyayı oluşturabilirsiniz [GitHub - Azure not defterleri örnekleri](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps), veya verileri doğrudan girerek. Aşağıdaki bölümlerde, her iki yaklaşım açıklanmaktadır.

### <a name="upload-the-data-file"></a>Veri dosyasını karşıya yükle

1. Azure not defterlerinde, proje Panoda seçin **karşıya** > **URL'den**
1. Açılan pencerede aşağıdaki URL'yi girin **dosya URL'si** ve *cricket_chirps.csv* içinde **dosya adı**, ardından **Bitti**.

    ```url
    https://raw.githubusercontent.com/Microsoft/AzureNotebooks/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps/cricket_chirps.csv
    ```

1. *Cricket_chirps.csv* dosya artık projenizin dosya listesinde görünmelidir:

    ![Yeni proje dosya listesinde gösteren CSV dosyası oluşturuldu](media/tutorial/csv-file-in-project.png)

### <a name="create-a-file-from-scratch"></a>Sıfırdan bir dosya oluşturun

1. Azure not defterlerinde, proje Panoda seçin **+ yeni** > **boş dosya**
1. Bir alan projenin dosya listesinde görünür. ENTER *cricket_chirps.csv* ve Enter tuşuna basın.
1. Sağ *cricket_chirps.csv* seçip **dosya Düzenle**.
1. Görüntülenen Düzenleyicisi'nde aşağıdaki verileri girin:

    ```csv
    Chirps/Minute,Temperature
    20,88.6
    16,71.6
    19.8,93.3
    18.4,84.3
    17.1,80.6
    15.5,75.2
    14.7,69.7
    17.1,82
    15.4,69.4
    16.2,83.3
    15,79.6
    17.2,82.6
    16,80.6
    17,83.5
    14.4,76.3
    ```

1. Seçin **dosyayı Kaydet** proje Panosu'na geri dönün ve dosyayı kaydedin.

## <a name="install-project-level-packages"></a>Proje düzeyi paketleri yükleme

Bir not defteri komutları gibi her zaman kullanabilirsiniz `!pip install` gerekli paketleri yüklemek için bir kod hücresi içinde. Ancak, not defterinin kod hücreleri çalıştırın ve önemli ölçüde uzun sürebilir her zaman bu tür komutlar çalıştırılır. Bu nedenle, bunun yerine kullanarak proje düzeyinde paketleri yükleyebileceğiniz bir `requirements.txt` dosya.

1. Açıklanan işlemi kullanın [sıfırdan bir dosya oluşturun](#create-a-file-from-scratch) adlı bir dosya oluşturmak için `requirements.txt` aşağıdaki içeriklerle:

    ```text
    matplotlib==3.0.0
    numpy==1.15.0
    pandas==0.23.4
    scikit-learn==0.20.0
    ```

    Da karşıya bir `requirements.txt` dosya çubuğunda açıklandığı tercih ederseniz, yerel bilgisayarınızdan [veri dosyasını karşıya](#upload-the-data-file).

1. Proje Panosu üzerinde seçin **proje ayarları**.
1. Görünen açılır seçin **ortam** sekmesine ve ardından seçin **+ Ekle**.
1. İlk açılır denetimin (işlem) altında **ortamı Kurulumu adımları**, seçin **Requirements.txt**.
1. (Dosya adı) İkinci açılan denetiminde seçin *requirements.txt* (oluşturduğunuz dosyası).
1. (Python sürümü) Üçüncü açılan denetiminde seçin **Python sürüm 3. 6**.
1. **Kaydet**’i seçin.

![Proje ayarları ortam sekmesi requirements.txt dosyasını belirtme](media/tutorial/tutorial-requirements-txt.png)

Bu kurulum adımı ile yerinde projedeki çalıştırılmaya herhangi bir dizüstü bilgisayar, bir ortamda bu paketleri yüklendiği çalıştırın.

## <a name="create-and-run-a-notebook"></a>Not defteri oluşturma ve çalıştırma

Hazır veri dosyası ve proje ortam kümesi ile artık oluşturabilir ve not defterini açın.

1. Proje Panosu üzerinde seçin **+ yeni** > **not defteri**.
1. Açılan pencerede girin *doğrusal regresyon örnek - Cricket Chirps.ipynb* için **öğe adı**, seçin **Python 3.6** dili, ardından **yeni**.
1. Yeni Not Defteri dosya listesinde göründükten sonra not defterini başlayacak şekilde seçin. Otomatik olarak yeni bir tarayıcı sekmesi açılır.
1. Sahip olduğunuz için bir *requirements.txt* dosya ortamı ayarlarınızda göreceğiniz ileti "kapsayıcınızı tamamlanması beklenirken hazırlanıyor." Seçebileceğiniz **Tamam** iletiyi kapatın ve not defterinde; çalışmaya devam etmek için ortamı tamamen ayarlanana kadar kod hücreleri ancak çalıştıramazsınız.
1. Bir tek boş kodu hücreyi Jupyter arabirimle not defteri varsayılan olarak açılır.

    [![İlk Azure not defterleri yeni bir not defteri görünümü](media/tutorial/tutorial-new-notebook.png)](media/tutorial/tutorial-new-notebook.png#lightbox)

## <a name="tour-the-notebook-interface"></a>Not Defteri arabirimi turuna katılın

Çalışan not defteri ile kod ve Markdown hücreleri hücrelere çalıştırın ve not defteri yönetebilirsiniz ekleyebilirsiniz. İlk olarak, ancak bu arabirimi tanımak için birkaç dakikanızı ayırıp değer olur. Tüm belgeler için seçin **yardımcı** > **not defteri yardımcı** menü komutu.

Pencerenin üst kısmında, aşağıdaki öğeleri görürsünüz:

(A) tıklayarak düzenleyebilirsiniz defterinizin adı.
(B) düğmeler, tarayıcınızda yeni bir sekme açın içerilen projeyi ve projeleri panonuza gidin.
Not Defteri ile çalışmak için komutları (C) A menüsü.
(D) sık kullanılan işlemler için kısayollar bir araç çubuğu.
(E) hücre içeren düzenleme tuvalini.
(F) not defterini güvenilir olup göstergesi (varsayılan değer **güvenilir**).
(G) not defteri etkinliği göstergesini birlikte çalıştırmak için kullanılan çekirdek.

[![Birincil kullanıcı Arabirimi alanları Jupyter arabirimi](media/tutorial/tutorial-notebook-ui.png)](media/tutorial/tutorial-notebook-ui.png#lightbox)

Jupyter, birincil kullanıcı Arabirimi öğeleri yerleşik turu sağlar. Seçerek Turu Başlat **yardımcı** > **kullanıcı arabirimi Turu** komut ve açılan pencereler tıklayarak.

Menü komutları gruplarını aşağıdaki gibidir:

| Menü | Açıklama |
| --- | --- |
| Dosya | Oluşturma ve dizüstü bilgisayarlar, kopyalama komutları dahil olmak üzere not defteri dosyasını yönetmek için komutlar Baskı Önizlemeyi Göster ve çeşitli biçimlerde not defterine indirin. |
| Düzenle | Kesme, kopyalama ve hücre yapıştırın, bulmak ve değerleri değiştirmek için tipik komutları hücre ekleri Yönet ve görüntüleri ekleyin.  |
| Görünüm | Jupyter kullanıcı Arabiriminin farklı bölümlerini görünürlüğünü denetleme komutları. |
| Ekle | Geçerli hücreyi altına veya üstüne yeni bir hücre Ekle komutları. Sıklıkla bir not defteri oluştururken bu komutları kullanın. |
| Hücre | Çeşitli **çalıştırma** komutları bir veya daha fazla hücre farklı bileşimlerde çalıştırın. **Hücresi türü** komutları bir hücre arasında tür değiştirme **kod**, **Markdown**, ve **ham NBConvert** (düz metin). **Geçerli çıkışları** ve **tüm çıkışları** komutlarını denetlemek nasıl çalışma kodu çıktısı görüntülenir ve tüm çıkış temizlemek için bir komut içerir. |
| Çekirdek | Nasıl kod Çekirdeği'nde ile birlikte çalıştırıldığı yönetecek komutlar **değişiklik çekirdek** dil veya not defteri çalıştırmak için kullanılan Python sürümünü değiştirmek için. |
| Veriler | Karşıya yüklemek ve proje veya oturumu dosyaları indirmek için komutlar içerir. Bkz: [proje veri dosyalarıyla çalışma](work-with-project-data-files.md) |
| Pencere Öğeleri | Yönetecek komutlar [Jupyter pencere öğeleri](https://ipywidgets.readthedocs.io/en/stable/examples/Widget%20Basics.html), eşleme ve çizim görselleştirme için ek özellikler sağlar.|
| Yardım | Yardım ve belgeler için Jupyter arabirimi sağlayan komutlar içerir. |

Araç çubuğundaki komutların çoğu eşdeğer menü komutları vardır. Bir özel durum **Enter/Düzenle YÜKSELMEYE slayt gösterisi**, üzerinde ele [paylaşımı ve mevcut not defterleri](present-jupyter-notebooks-slideshow.md).

Not Defteri olarak bölümlerde doldurmak gibi birkaç aşağıdaki komutlardan birini kullanın.

## <a name="create-a-markdown-cell"></a>Bir Markdown hücre oluşturma

1. Not Defteri tuval üzerinde gösterilen ilk boş hücreye tıklayın. Varsayılan olarak, bir hücre olduğunda bir **kod** tasarlanmış seçili çekirdek için çalıştırılabilir kod içerecek şekilde anlamına gelir türü (Python, R veya F#). Geçerli türü açılan araç çubuğunda bulunan tür gösterilmektedir:

    ![Hücre türü araç çubuğu açılır](media/tutorial/tutorial-cell-type-drop-down.png)

1. Hücre türe çeviremezsiniz **Markdown** açılır araç çubuğunu kullanarak; bunun yerine, kullanın **hücre** > **hücresi türü**  >   **Markdown** menü komutu:

    ![Hücre türü menü komutu](media/tutorial/tutorial-cell-type-menu.png)

1. Hücreyi düzenlemeye başlamak için tıklayın ve ardından aşağıdaki Markdown'ı girin:

    ```markdown
    # Example Linear Regression

    This notebook contains a walkthrough of a simple linear regression. The data, obtained from [college.cengage.com](https://college.cengage.com/mathematics/brase/understandable_statistics/7e/students/datasets/slr/frames/frame.html), relates the rate of cricket chirps to temperature from *The Song of Insects*, by Dr. G. W. Pierce, Harvard College Press.

    In this example we're using the count of chirps per minute as the independent varible to then predict the dependent variable, temperature. In short, we're using a little data science to make ourselves a cricket thermometer. (You could also reverse the data and use temperature to predict the number of chirps, but it's more fun to use crickets as the thermometer itself!)

    The methods shown in this example follow what's presented in the Udemy course, [Machine Learning A to Z](https://www.udemy.com/machinelearning/learn/v4/).

    A lovely aspect of Notebooks is that you can use Markdown cells to explain what the code is doing rather than code comments. There are several benefits to doing so:

    - Markdown allows for richer text formatting, like *italics*, **bold**, `inline code`, hyperlinks, and headers.
    - Markdown cells automatically word wrap whereas code cells do not. Code comments typically use explicit line breaks for formatting, but that's not necessary in Markdown.
    - Using Markdown cells makes it easier to run the Notebook as a slide show.
    - Markdown cells help you remove lengthy comments from the code, making the code easier to scan.

    When you run a code cell, Jupyter executes the code; when you run a Markdown cell, Jupyter renders all the formatting into text that's suitable for presentation.
    ```

1. Markdown HTML'e tarayıcısı işlemek için seçin **çalıştırmak** komut araç veya kullanın **hücre** > **çalıştırma hücreleri** komutu. Biçimlendirme ve bağlantıları için Markdown kod, bir tarayıcıda beklediğiniz gibi artık görünür.

1. Son hücreye not defterinde çalıştırdığınızda, Jupyter çalıştırdığınız bir altında yeni bir hücre otomatik olarak oluşturur. Daha fazla Markdown bu hücreye aşağıdaki Markdown ile bu bölümdeki adımları tekrarlayarak yerleştirin:

    ```markdown
    ## Install packages using pip or conda

    Because the code in your notebook likely uses some Python packages, you need to make sure the Notebook environment contains those packages. You can do this directly within the notebook in a code block that contains the appropriate pip or conda commands prefixed by `!`:

    \```
    !pip install <pkg name>

    !conda install <pkg name> -y
    \```
    ```

1. Markdown'ı yeniden düzenlemek için işlenen hücreye çift tıklayın. Değişiklikleri yaptıktan sonra HTML oluşturmak için hücre çalıştırın.

## <a name="create-a-code-cell-with-commands"></a>Bir kod hücresi komutlarla oluşturun

Önceki Markdown hücre açıklandığı gibi komutları doğrudan not defterinde içerebilir. Paketleri yüklemek için curl veya veri veya başka bir şey almak için wget çalıştırma komutlarını kullanabilirsiniz. Jupyter not defterleri, etkili bir şekilde ile çalışmak için tam Linux komut alacak şekilde bir Linux sanal makine içinde çalıştırın.

1. Kullandığınız sonra görüntülenen kod hücresine aşağıdaki komutları girin **çalıştırma** önceki Markdown hücreyi. Yeni bir hücre görmüyorsanız oluşturmak **Ekle** > **hücre aşağıda Ekle** veya **+** araç çubuğunda.

    ```bash
    !pip install numpy
    !pip install matplotlib
    !pip install pandas
    !pip install sklearn
    ```

1. Hücre çalıştırmadan önce yeni bir hücreyle oluşturma **+** araç çubuğunda düğme için Markdown ayarlayabilir ve aşağıdaki açıklama girin:

    ```markdown
    Note that when you run a code block that contains install commands, and also those with `import` statements, it make take the notebooks a little time to complete the task. To the left of the code block you see `In [*]` to indicate that execution is happening. The Notebook's kernel on the upper right also shows a filled-in circle to indicate "busy."
    ```

1. Seçin **hücre** > **tümünü Çalıştır** tüm hücreleri not defteri çalıştırmak için komutu. Markdown hücreleri HTML olarak işlemenizi ve komut çekirdek çalıştırın ve çekirdek göstergesi, Markdown'da açıklandığı gözlemleyin dikkat edin:

    ![Not Defteri çekirdeğin meşgul göstergesi](media/tutorial/tutorial-kernel-busy.png)

1. Ayrıca tüm biraz zaman alır `pip install` komutları çalıştırmak için ve proje ortamında bu paketler zaten yüklü olduğundan (ve onlar da Azure not defterlerinde varsayılan olarak dahil edildiği için), okuma, "gereksinimi çok sayıda ileti gördüğünüz zaten memnun." Bu çıkış olabilir görsel olarak dikkat dağıtıcı, böylece seçin, (tek tıklama kullanarak) satış, ardından kullanmak **hücre** > **hücre çıkışları** > **geçiş**çıkış gizlemek için. Ayrıca **Temizle** çıkış tamamen kaldırmak için aynı alt komut.

    **Geçiş** komutu yalnızca en son çıktı hücreden gizler; hücre yeniden çalıştırırsanız, çıkış ekranı yeniden görünür.

1. Paketler proje ortamında yüklü olmadığından, açıklama `! pip install` komutları kullanarak `#`; bu şekilde, not defterine eğitim malzemesi olarak kalır ancak çalıştırmak için hiçbir zaman kesintiye uğramaz ve gereksiz çıkış üretmesi olmaz. Bu durumda, Not Defteri açıklamalı komutları tutma de not defterinin bağımlılıkları gösterir.

    ```bash
    # !pip install numpy
    # !pip install matplotlib
    # !pip install pandas
    # !pip install sklearn
    ```

## <a name="create-the-remaining-cells"></a>Kalan hücreleri oluşturma

Not defterini geri kalanı doldurmak için sonraki Markdown ve kod hücreleri bir dizi oluşturun. Aşağıda listelenen her bir hücresinde ilk yeni hücreyi oluşturun, ardından türü ayarlayın, sonra içeriği yapıştırın.

Her bir hücresinde oluşturduktan sonra not defteri çalıştırmak için bekleyebilirsiniz olsa da, her bir hücresinde oluşturduğunuz farklı çalıştır ilginçtir. Tüm hücreleri çıktıyı gösterir; hata iletisi göremiyorum, hücre normalde çalıştırdığınız varsayılır.

Önceki hücrelerde çalıştırılan kod her kodu hücreyi bağlıdır ve bir hücre çalışacak şekilde yayımlarsanız, daha sonra hücre hataya neden. Bir hücreyi çalıştırılacak unuttuysanız bulursanız, kullanmayı deneyin **hücre** > **tüm yukarıda çalıştırma** geçerli hücreyi çalıştırmadan önce.

(Bu, büyük olasılıkla olacak!) beklenmeyen sonuçlara görürseniz, her bir hücresinde "Code" veya "Markdown" için gerektiği şekilde ayarlandığından emin olun. Örneğin, "Geçersiz söz dizimi" hata kod hücresine Markdown girdikten sonra genellikle oluşur.

1. Markdown hücre:

    ```markdown
    ## Import packages and prepare the dataset

    In this example we're using numpy, pandas, and matplotlib. Data is in the file cricket_chirps.csv. Because this file is in the same project as this present Notebook, we can just load it using a relative pathname.
    ```

1. Kodu hücreyi; çalıştırdığınızda, çıktı olarak İçindekiler gösterir. Çıkış açıklama satırı yaparak çıkış gizleyebilirsiniz `print` deyimi.

    ```python
    import numpy as np
    import pandas as pd

    dataset = pd.read_csv('cricket_chirps.csv')
    print(dataset)
    X = dataset.iloc[:, :-1].values  # Matrix of independent variables -- remove the last column in this data set
    y = dataset.iloc[:, 1].values    # Matrix of dependent variables -- just the last column (1 == 2nd column)
    ```

    > [!Note]
    > Bu kodu "numpy.dtype boyutu değiştirildi"; hakkında uyarılar görebilirsiniz. Uyarılar güvenle yoksayılabilir.

1. Markdown hücre:

    ```markdown
    Next, split the dataset into a Training set (2/3rds) and Test set (1/3rd). We don't need to do any feature scaling because there is only one column of independent variables, and packages typically do scaling for you.
    ```

1. Kodu hücreyi; çalıştırdığınızda, bu hücrenin hiçbir çıktı yok.

    ```python
    from sklearn.model_selection import train_test_split

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 1/3, random_state = 0)
    ```

1. Markdown hücre:

    ```markdown
    ## Fit the data to the training set

    "Fitting" the data to a training set means making the line that describes the relationship between the independent and the dependent variables. With a simple data set like we're using here, you can visualize the line on a simple x-y plot: the x-axis is the independent variable (chirp count in this example), and the y-axis is the independent variable (temperature). Fitting the data means plotting all the points in the training set, then drawing the best-fit line through that data.

    With two independent variables you can imagine a three-dimensional plot with a line fitted to the data. At three or more independent variables, however, it's no longer easy to visualize the fit, but you get the idea. In the end, it's all just mathematics, which a computer can handle easily without having to form a mental picture!

    The regressor's `fit` method here creates the line, which algebraically is of the form `y = x*b1 + b0`, where b1 is the coefficient or slope of the line (which you can get to through `regressor.coef_`), and b0 is the intercept of the line at x=0 (which you can get to through `regressor.intercept_`).
    ```

1. Kodu hücreyi; çalıştırdığınızda, bu hücreyi çıktıyı gösterir `LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None,normalize=False)`.

    ```python
    from sklearn.linear_model import LinearRegression

    regressor = LinearRegression()    # This object is the regressor, that does the regression
    regressor.fit(X_train, y_train)   # Provide training data so the machine can learn to predict using a learned model.
    ```

1. Markdown hücre:

    ```markdown
    ## Predict the results

    With the regressor in hand, we can predict the test set results using its `predict` method. That method takes a vector of independent variables for which you want predictions.

    Because the regressor is fit to the data by virtue of `coef_` and `intercept_`, a prediction is the result of `coef_ * x + intercept_`. (Indeed, `predict(0)` returns `intercept_` and `predict(1)` returns `intercept_ + coef_`.)

    In the code, the `y_test` matrix (from when we split the set) contains the real observations. `y_pred` assigned here contains the predictions for the same `X_test` inputs. It's not expected that the test or training points exactly fit the regression; the regression is trying to find the model that we can use to make predictions with new observations of the independent variables.
    ```

1. Kodu hücreyi; çalıştırdığınızda, bu hücreyi Sonuçlardan memnun gösterir `[79.49588055 75.98873911 77.87719989 80.03544077 75.17939878]`.

    ```python
    y_pred = regressor.predict(X_test)
    print(y_pred)
    ```

1. Markdown hücre:

    ```markdown
    It's interesting to think that all the "predictions" we use in daily life, like weather forecasts, are just regression models of some sort working with various data sets. Those models are much more complicated than what's shown here, but the idea is the same.

    Knowing how predictions work help us understand that the actual observations we would collect in the moment will always be somewhat off from the predictions: the predictions fit exactly to the model, whereas the observations typically won't.

    Of course, such systems feed new observations back into the dataset to continually improve the model, meaning that predictions should get more accurate over time.

    The challenge is determining what data to actually use. For example, with weather, how far back in time do you go? How have weather patterns been changing decade by decade? In any case, something like weather predictions will be doing things hour by hour, day by day, for things like temperature, precipitation, winds, cloud cover, etc. Radar and other observations are of course fed into the model and the predictions are reduced to mathematics.
    ```

1. Markdown hücre:

    ```markdown
    ## Visualize the results

    The following code generates a plot: green dots are training data, red dots are test data, blue dots are predictions. Gray line is the regression itself. You see that all the blue dots are exactly on the line, as they should be, because the predictions exactly fit the model (the line).
    ```

1. Kodu hücreyi; çalıştırdığınızda, bu hücresi grafik çizim üretir. Yoksa ilk çizim süre bakın (ve bunun yerine "640 x 480 1 eksenlerini şekil boyutu" konusuna bakın), hücre yeniden çalıştırın.

    ```python
    import matplotlib.pyplot as plt

    plt.scatter(X_train, y_train, color = 'green')
    plt.scatter(X_test, y_test, color = 'red')   
    plt.scatter(X_test, y_pred, color = 'blue')  # The predicted temperatures of the same X_test input.
    plt.plot(X_train, regressor.predict(X_train), color = 'gray')
    plt.title('Temperature based on chirp count')
    plt.xlabel('Chirps/minute')
    plt.ylabel('Temperature')
    plt.show()
    ```

    ![Matplotlib kod çizim çıktısı](media/tutorial/tutorial-plot-output.png)

1. Markdown hücre:

    ```markdown
    ## Closing comments

    At the end of the day, when you create a model, you use training data. Then you start feeding test data (real observations) to see how well the model actually works. You may find that the model is a little inaccurate over time, in which case you retrain the model with some new data. Retraining the model means you're creating a new fit line that's used for predictions.

    This regression can be used to examine the variability of the relationship between temperature and chirp count. Of course, if the model proves too inaccurate (that is, the predictions aren't very good), then it suggests that we might need to introduce more independent variables like humidity, time of year, latitude, amount of moonlight, and so on. If you have such data, you can do separate lines regressions for each independent variable, and then do multiple regressions with combinations of independent variables. 

    Again, once you are working with more than one or two independent variables, it's much easier to use machine learning to crunch the numbers than to try to visualize it graphically.
    ```

## <a name="clear-outputs-and-rerun-all-cells"></a>Çıkışlar temizleyin ve tüm hücreleri yeniden çalıştırın

Tüm Not doldurmak için önceki bölümdeki adımları uyguladıktan sonra hem bir parça doğrusal regresyon tam Öğreticisi bağlamında kod çalıştırma oluşturmuş oldunuz. Bu kod ve metin doğrudan birleşimi not defterlerini harika avantajlarından biridir!

Tüm dizüstü bilgisayar'ı şimdi yeniden çalıştırmayı deneyin:

1. Çekirdek kişinin oturum verilerini temizleyin ve tüm çıkış seçerek hücre **çekirdek** > **çıkışı Temizle & yeniden başlatma**. Bu komut, her zaman yalnızca kod hücreler arasındaki garip bağımlılıkları oluşturmadınız emin olmak için bir not defteri tamamladıktan sonra çalıştırmak için iyi bir örnektir.

1. Not Defteri kullanarak yeniden **hücre** > **tümünü Çalıştır**. Kod çalışırken çekirdek göstergesi doldurulur dikkat edin.

    Aksi takdirde takılı ya da çok uzun süre çalışan herhangi bir kod varsa, kullanarak çekirdek durdurabilirsiniz **çekirdek** > **kesme** komutu.

1. Sonuçları incelemek için Not defterini kaydırın. (Yeniden çizimi görüntülenmezse, söz konusu hücrenin yeniden çalıştırın.)

## <a name="save-halt-and-close-the-notebook"></a>Not defterini kapatmak kaydedin ve durdurma

Bir not defteri düzenlediğiniz sırada geçerli durumuyla kaydedebilirsiniz **dosya** > **kaydetme ve denetim noktası** komut veya kaydetme araç çubuğunda. "Denetim noktası" oturumu sırasında herhangi bir zamanda döndürebilirsiniz bir anlık görüntü oluşturur. Kontrol noktaları, bir dizi Deneysel değişikliği yapmanızı sağlar ve bu değişiklikleri işe yaramazsa, yalnızca bir denetim noktası kullanmaya geri dönebilirsiniz **dosya** > **kontrol noktasına geri** komutu. Ek hücre ve açıklama satırı çalıştırmak istemediğiniz herhangi bir kod oluşturmak için alternatif yaklaşım, Her iki şekilde çalışır.

De kullanabilirsiniz **dosya** > **kopyalanmasına** projenizde yeni bir dosyaya not defterinin geçerli durumunun bir kopyasını yapmak için herhangi bir zamanda komutu. Bu kopyayı otomatik olarak yeni bir tarayıcı sekmesinde açılır.

Bir not defteri ile işiniz bittiğinde kullanın **dosya** > **Kapat ve durdurmak** not defterini kapatır ve onu çalıştıran çekirdek kapatan komutu. Azure not defterleri kapanır tarayıcı sekmesinde otomatik olarak.

## <a name="debug-notebooks-using-visual-studio-code"></a>Not defterlerini kullanarak Visual Studio Code hata ayıklama

Kod hücreleri defterinizdeki beklediğiniz şekilde davranır yoksa, kod hataları veya diğer hatalar olabilir. Ancak, dışında kullanarak `print` deyimlerini tipik bir Jupyter ortam olmayan herhangi bir hata ayıklama özellikleri sunan, değişkenlerin değerini gösterir.

Neyse ki, not defterinin indirebileceğiniz *.ipynb* dosya, sonra Python uzantısını kullanarak Visual Studio Code'da açın. Uzantı doğrudan bir not defteri, Markdown hücrelerde açıklamaları koruma tek bir kod dosyası olarak içeri aktarır. Not defterini aktardıktan sonra kodunuzda adım adım ilerleyin, kesme noktaları ayarlayın, durumunu inceleyebilir vb. için Visual Studio Code Hata Ayıklayıcısı'nı kullanabilirsiniz. Kodunuzda düzeltmeler yapıldıktan sonra ardından dışa *.ipynb* dosya Visual Studio Code'dan ve geri Azure not defterlerine yükleyin.

Daha fazla bilgi için [Jupyter not defteri hata ayıklama](https://code.visualstudio.com/docs/python/jupyter-support#debug-a-jupyter-notebook) Visual Studio Code belgelerindeki.

Ayrıca bkz: [Visual Studio Code - Jupyter Destek](https://code.visualstudio.com/docs/python/jupyter-support) Jupyter not defterleri için ek özelliklerini Visual Studio Code için.

## <a name="next-steps"></a>Sonraki adımlar

- [Örnek Not Defterleri keşfedin](azure-notebooks-samples.md)

Nasıl yapılır makaleleri:

- [Oluşturma ve projeleri kopyalama](create-clone-jupyter-notebooks.md)
- [Yapılandırma ve projeleri yönetme](configure-manage-azure-notebooks-projects.md)
- [İçinde bir not defteri paketleri yükleme](install-packages-jupyter-notebook.md)
- [Mevcut bir slayt gösterisi](present-jupyter-notebooks-slideshow.md)
- [Veri dosyaları ile çalışma](work-with-project-data-files.md)
- [Veri kaynaklarına erişim](access-data-resources-jupyter-notebooks.md)
- [Azure Machine Learning Hizmetleri kullanma](use-machine-learning-services-jupyter-notebooks.md)
