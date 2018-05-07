---
title: Machine Learning Python istemci kitaplığı veri kümeleriyle erişim | Microsoft Docs
description: Yükleyin ve Python istemci kitaplığı erişmek ve Azure Machine Learning veri bir yerel Python ortamından güvenli bir şekilde yönetmek için kullanın.
services: machine-learning
documentationcenter: python
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: f97fbb76ddf48fb3c7ec79b6b2ed8cee3e0ceabb
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Azure Machine Learning Python istemci kitaplığını kullanarak Python ile veri kümelerine erişim
Microsoft Azure Machine Learning Python istemci kitaplığı önizlemesini güvenli erişim Azure Machine Learning veri kümeleriniz için bir yerel Python ortamından etkinleştirebilir ve oluşturulması ve bir çalışma alanı kümelerinde yönetimi sağlar.

Bu konu hakkında yönergeler sağlar:

* Machine Learning Python istemci Kitaplığı'nı yüklemek 
* erişim ve Azure Machine Learning veri kümelerini yerel Python ortamınızdan erişme yetkisi almak yönergeler de dahil olmak üzere veri kümeleri, karşıya yükleme
* denemeler ara veri kümeleri erişim
* veri kümeleri listeleme, meta verilerine erişmek, bir veri kümesi içeriğini okumak, yeni veri kümeleri oluşturma ve mevcut veri kümelerini güncelleştirmek için Python istemci kitaplığını kullanma

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Önkoşullar
Python istemci kitaplığı altında aşağıdaki ortamları test edilmiştir:

* Windows, Mac ve Linux
* Python 2.7, 3.3 ve 3.4

Bunu, aşağıdaki paketleri bir bağımlılığa sahiptir:

* istek
* Python dateutil
* pandas

Bir Python dağıtımı gibi kullanmanızı öneririz [Anaconda](http://continuum.io/downloads#all) veya [Kanopi](https://store.enthought.com/downloads/), Python, IPython gelen ve yukarıda listelenen üç paketleri yüklü. IPython kesinlikle gerekli olmamakla birlikte, düzenleme ve etkileşimli olarak verileri görselleştirmek için harika bir ortamıdır.

### <a name="installation"></a>Azure Machine Learning Python istemci kitaplığı yükleme
Bu konuda açıklanan görevleri tamamlamak için ayrıca Azure Machine Learning Python istemci kitaplığı yüklenmesi gerekir. Kullanılabilir [Python paket dizini](https://pypi.python.org/pypi/azureml). Python ortamınızda yüklemek için yerel Python ortamınızdan aşağıdaki komutu çalıştırın:

    pip install azureml

Alternatif olarak, indirin ve kaynaklardan yüklemek [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Makinenizde git varsa, doğrudan git deposundan yüklemek için PIP kullanabilirsiniz:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Veri kümeleri erişmek için Studio kod parçacıklarını kullanma
Python istemci kitaplığı programlı erişim çalıştırılmış denemeler, var olan veri kümelerine sağlar.

Studio web arabiriminden indirmek ve veri kümeleri konumu makinenizde Pandas DataFrame nesne olarak seri durumdan için gerekli tüm bilgileri içeren kod parçacıkları oluşturabilir.

### <a name="security"></a>Veri erişimi için güvenlik
Python istemci kitaplığı ile kullanmak üzere çalışma alanı kimliği ve yetkilendirme içerir Studio tarafından sağlanan kod parçacıkları belirteci. Bu çalışma alanınıza tam erişim sağlamak ve parola gibi korunmalıdır.

Güvenlik nedenleriyle, kod parçacığı işlevleri yalnızca yap rollerine sahip kullanıcılar için kullanılabilir **sahibi** çalışma alanı için. Azure Machine Learning Studio'da gösterilir rolünüze **kullanıcılar** altında sayfa **ayarları**.

![Güvenlik][security]

Rolünüze olarak ayarlanmamışsa **sahibi**, ya da istek sahibi olarak ortamına yeniden davet edilme veya kod parçacığı ile sağlamak için çalışma alanının sahibi sormak için kullanabilirsiniz.

Yetkilendirme belirteci edinmek için aşağıdakilerden birini yapabilirsiniz:

* Bir belirteci için bir sahibinden isteyin. Sahipleri kendi yetkilendirme belirteçleri Studio'da kendi çalışma alanı ayarları sayfasından erişebilirsiniz. Seçin **ayarları** tıklatın ve sol bölmede **YETKİLENDİRME BELİRTEÇLERİ** birincil ve ikincil belirteçleri görmek için.  Birincil veya ikincil yetkilendirme belirteçleri kod parçacığında kullanılabilse de sahipleri yalnızca ikincil yetkilendirme belirteçleri paylaşıma önerilir.

![Yetkilendirme belirteçleri](./media/python-data-access/ml-python-access-settings-tokens.png)

* Sahibi rolüne Yükseltilecek isteyin.  Bunu yapmak için geçerli bir çalışma alanı sahibi ilk çalışma alanından kaldırın sonra ona bir sahibi olarak yeniden davet gerekir.

Geliştiriciler çalışma alanı kimliği ve yetkilendirme aldıktan sonra belirteç, bunlar bağımsız olarak kendi rolleri kod parçacığını kullanarak çalışma erişebilir.

Yetkilendirme belirteçleri yönetilir **YETKİLENDİRME BELİRTEÇLERİ** altında sayfa **ayarları**. Bunları yeniden oluşturabilirsiniz, ancak bu yordamı olan önceki belirtece erişimi iptal eder.

### <a name="accessingDatasets"></a>Yerel bir Python uygulama erişim veri kümeleri
1. Machine Learning Studio'da tıklatın **veri KÜMELERİ** sol gezinti çubuğunda.
2. Erişmek istediğiniz veri kümesini seçin. Veri kümeleri birini seçebilirsiniz **MY veri KÜMELERİ** listesi veya **örnekleri** listesi.
3. Alt araç çubuğundan tıklatın **veri erişim kodu oluştur**. Verileri Python istemci kitaplığı ile uyumlu bir biçimde ise, bu düğmesi devre dışıdır.
   
    ![Veri kümeleri][datasets]
4. Kod parçacığı görünür ve panonuza kopyalayın penceresinden seçin.
   
    ![Erişim kodu][dataset-access-code]
5. Kodu yerel Python uygulamanızı not defterinize yapıştırın.
   
    ![Not Defteri][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Machine Learning denemelerini ara veri kümeleri erişim
Bir denemeyi Machine Learning Studio'da çalıştırıldıktan sonra Ara veri kümeleri modülleri çıkış düğümlerdeki erişmek mümkündür. Ara veri kümeleri oluşturulan ve model aracı çalıştırdığınızda ara adımlar için kullanılan verilerdir.

Veri biçimi Python istemci kitaplığı ile uyumlu olduğu sürece ara veri kümeleri erişilebilir.

Aşağıdaki biçimler desteklenir (Bu sabittir içinde `azureml.DataTypeIds` sınıfı):

* Düz metin
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Modül çıkış düğüm üzerinde gelerek biçimi belirleyebilirsiniz. Düğüm adı bir araç ipucu ile birlikte görüntülenir.

Bazı modüller gibi [bölünmüş] [ split] adlı bir biçimde çıktı modülü `Dataset`, Python istemci kitaplığı tarafından desteklenmiyor.

![Veri kümesi biçimi][dataset-format]

Dönüştürme modülü gibi kullanmanıza gerek [CSV'ye Dönüştür][convert-to-csv], bir çıktı biçimi desteklenen bir biçime almak için.

![GenericCSV biçimi][csv-format]

Aşağıdaki adımlar, bir deneme oluşturur, çalıştırır ve Ara dataset erişen bir örnek gösterir.

1. Yeni bir deneme oluşturun.
2. INSERT bir **yetişkin Census gelir ikili sınıflandırma dataset** modülü.
3. INSERT bir [bölünmüş] [ split] modülü ve kendi giriş veri kümesi modülü çıktıya bağlanın.
4. INSERT bir [CSV'ye Dönüştür] [ convert-to-csv] modülü ve kendi giriş birine bağlanın [bölünmüş] [ split] modülü çıkarır.
5. Denemeyi kaydedin, çalıştırmak ve çalışan bitmesini bekleyin.
6. Çıktı düğümü tıklatın [CSV'ye Dönüştür] [ convert-to-csv] modülü.
7. Bağlam menüsü görüntülendiğinde seçin **veri erişim kodu oluştur**.
   
    ![Bağlam menüsü][experiment]
8. Kod parçacığı seçin ve görüntülenen penceresinden panonuza kopyalayın.
   
    ![Erişim kodu][intermediate-dataset-access-code]
9. Kodu defterinizde yapıştırın.
   
    ![Not Defteri][ipython-intermediate-dataset]
10. Matplotlib kullanarak verileri Görselleştir. Bu yaş sütunu için bir histogram görüntüler:
    
    ![Çubuk grafik][ipython-histogram]

## <a name="clientApis"></a>Erişim, okuma, oluşturma ve veri kümelerini yönetmek için makine öğrenme Python istemci kitaplığını kullanma
### <a name="workspace"></a>Çalışma alanı
Çalışma alanı Python istemci kitaplığı için giriş noktasıdır. Sağlamak `Workspace` sınıfı çalışma alanı kimliği ve yetkilendirme belirteci örnek oluşturmak için:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Veri kümeleri listeleme
Belirli bir çalışma alanı tüm veri kümelerinin numaralandırmak için:

    for ds in ws.datasets:
        print(ds.name)

Yalnızca kullanıcı tarafından oluşturulan veri kümelerinin numaralandırmak için:

    for ds in ws.user_datasets:
        print(ds.name)

Yalnızca örnek veri kümeleri numaralandırmak için:

    for ds in ws.example_datasets:
        print(ds.name)

Bir veri kümesi (büyük küçük harfe duyarlı) adı tarafından erişebilirsiniz:

    ds = ws.datasets['my dataset name']

Ya da dizin tarafından erişebilirsiniz:

    ds = ws.datasets[0]


### <a name="metadata"></a>Meta Veriler
Veri kümeleri içerik yanı sıra meta veriler bulunur. (Ara veri kümeleri bu kural için bir özel durumdur ve meta verileri yok.)

Bazı meta veri değerleri, oluşturma sırasında kullanıcı tarafından atanır:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Başkaları tarafından Azure ML atanan değerler şunlardır:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Bkz: `SourceDataset` kullanılabilir meta veriler hakkında daha fazla bilgi için sınıf.

### <a name="read-contents"></a>İçeriğini okuma
Machine Learning Studio tarafından otomatik olarak sağlanan kod parçacıkları indirin ve Pandas DataFrame nesne kümesine seri durumdan. Bu gerçekleştirilir `to_dataframe` yöntemi:

    frame = ds.to_dataframe()

Ham verileri indirmek ve seri durumundan kendiniz gerçekleştirmek tercih ederseniz, bir seçenektir. Şu anda Python istemci kitaplığı seri durumdan çıkarılamıyor'ARFF ' gibi biçimler için tek seçenek budur.

Metin olarak içeriği okunamıyor:

    text_data = ds.read_as_text()

İçeriği ikili olarak okumak için:

    binary_data = ds.read_as_binary()

Ayrıca bir akış içeriğine açabilirsiniz:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Yeni bir veri kümesi oluşturma
Python istemci kitaplığı veri kümeleri Python programınızdan yüklemenize olanak sağlar. Bu veri kümeleri sonra çalışma alanınızda kullanılabilir.

Bir Pandas DataFrame verileriniz varsa, aşağıdaki kodu kullanın:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Verilerinizi zaten serileştirilmiş, kullanabilirsiniz:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python istemci kitaplığı Pandas DataFrame aşağıdaki biçimlerden için seri hale getiremiyor (Bu sabittir içinde `azureml.DataTypeIds` sınıfı):

* Düz metin
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Mevcut bir veri kümesini güncelleştir
Var olan bir dataset eşleşen bir ada sahip yeni bir veri kümesi yüklemeye çalışırsanız bir çakışma hatası almanız gerekir.

Mevcut bir veri kümesini güncelleştirmek için önce mevcut veri kümesini başvuru almanız gerekir:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Ardından `update_from_dataframe` seri hale getirmek ve Azure veri kümesine içeriğini değiştirmek için:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Farklı bir biçime verilerini seri hale getirmek istiyorsanız, isteğe bağlı için bir değer belirtin `data_type_id` parametresi.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

İsteğe bağlı olarak yeni bir açıklama için bir değer belirterek ayarlayabileceğiniz `description` parametresi.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

İsteğe bağlı olarak yeni bir ad için bir değer belirterek ayarlayabileceğiniz `name` parametresi. Şu andan itibaren yalnızca yeni bir ad kullanarak dataset almak. Aşağıdaki kod, verileri, ad ve açıklama güncelleştirir.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`, `name` Ve `description` parametreler isteğe bağlıdır ve varsayılan önceki değerlerine. `dataframe` Parametresi gereklidir her zaman.

Verilerinizi zaten serileştirilmiş kullanırsanız `update_from_raw_data` yerine `update_from_dataframe`. Yalnızca içinde geçirirseniz `raw_data` yerine `dataframe`, benzer şekilde çalışır.

<!-- Images -->
[security]:./media/python-data-access/security.png
[dataset-format]:./media/python-data-access/dataset-format.png
[csv-format]:./media/python-data-access/csv-format.png
[datasets]:./media/python-data-access/datasets.png
[dataset-access-code]:./media/python-data-access/dataset-access-code.png
[ipython-dataset]:./media/python-data-access/ipython-dataset.png
[experiment]:./media/python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

