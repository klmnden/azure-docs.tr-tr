---
title: 'Python betiğini yürütün: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Python kodu çalıştırmak için Azure Machine Learning hizmetinde bir Python betiği yürütme modülü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 6e61ed5a18e69b107b78bf2042de21d14cd6beb5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029138"
---
# <a name="execute-python-script-module"></a>Python betik modülü Yürüt

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Python kodu çalıştırmak için bu modülü kullanın. Python mimari ve tasarım prensipleri hakkında daha fazla bilgi için bkz: [makalesinin.](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)

Python ile var olan modülleri tarafından gibi şu anda desteklenmeyen görevleri gerçekleştirebilirsiniz:

+ Kullanarak verileri Görselleştirme `matplotlib`
+ Çalışma alanınızdaki modelleri numaralandırın Python kitaplıkları kullanma
+ Okuma, yükleme ve desteklenmeyen kaynaklardan gelen verileri düzenleme [verileri içeri aktarma](./import-data.md) Modülü
+ Kendi ayrıntılı kod öğrenme çalıştırın 


Azure Machine Learning, veri işleme için birçok genel yardımdı gereksinimleri içeren Python Anaconda dağıtım kullanır. Anaconda sürüm otomatik olarak güncelleştireceğiz. Geçerli sürümdür:
 -  Anaconda 4.5 + dağıtım için Python 3.6 

Önceden yüklenmiş paketler şunlardır:
-  asn1crypto 0.24.0 ==
- attrs 19.1.0 ==
- Azure ortak 1.1.18 ==
- Azure depolama blobu 1.5.0 ==
- azure-storage-common==1.4.0
- certifi==2019.3.9
- cffi 1.12.2 ==
- chardet 3.0.4 ==
- Şifreleme 2.6.1 ==
- distro 1.4.0 ==
- ıdna 2.8 ==
- jsonschema 3.0.1 ==
- lightgbm 2.2.3 ==
- Daha fazla itertools 6.0.0'dan ==
- numpy 1.16.2 ==
- pandas==0.24.2
- Pillow 6.0.0'dan ==
- pip 19.0.3 ==
- pyarrow 0.12.1 ==
- pycparser 2.19 ==
- pycryptodomex 3.7.3 ==
- pyrsistent 0.14.11 ==
- Python dateutil 2.8.0 ==
- pytz 2018.9 ==
- istekleri 2.21.0 ==
- scikit-bilgi 0.20.3 ==
- scipy 1.2.1 ==
- setuptools 40.8.0 ==
- altı 1.12.0 ==
- torch 1.0.1.post2 ==
- torchvision==0.2.2.post3
- urllib3 1.24.1 ==
- Tekerlek 0.33.1 == 

 Örneğin önceden yüklenmiş listede olmayan diğer paketleri yüklemeye *scikit-çeşitli*, komut dosyanıza aşağıdaki kodu ekleyin: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="how-to-use"></a>Nasıl kullanılır

**Python betiği yürütme** modülü, başlangıç noktası olarak kullanabileceğiniz örnek Python kodu içerir. Yapılandırmak için **Python betiği yürütme** modülü, sağladığınız girişleri ve Python kodu içinde yürütülen bir dizi **Python betiğini** metin kutusu.

1. Ekleme **Python betiği yürütme** denemenizi modülü.

2. Ekleme ve bağlanmak **Dataset1** arabiriminden girişi için kullanmak istediğiniz tüm veri kümeleri. Bu veri kümesi Python betiğinizde başvuru **DataFrame1**.

    Python kullanarak verileri oluşturmak istiyorsanız veya verileri doğrudan modülünü içeri aktarmak için Python kodu kullanın. bir veri kümesinin isteğe bağlı, kullanılır.

    Bu modül ikinci bir veri kümesi üzerinde destekler **Dataset2**. İkinci bir veri kümesi olarak DataFrame2 Python betiğinizde başvuru.

    Azure Machine Learning'de depolanan veri kümeleri için otomatik olarak dönüştürülür **pandas** ile bu modül yüklendiğinde data.frames.

    ![Python Giriş eşleme yürütün](media/module/python-module.png)

4. Yeni Python paketlerini veya kod eklemek için bu özel kaynaklar üzerinde içeren sıkıştırılmış dosya ekleme **betik paketi**. Giriş **betik paketi** sıkıştırılmış bir dosya zaten çalışma alanınıza karşıya yüklenmelidir. 

    Karşıya yüklenen sıkıştırılmış Arşiv'de yer alan herhangi bir dosya deneme yürütme sırasında kullanılabilir. Arşiv dizin yapısını içerir, yapısı korunur, ancak adlı bir dizin eklenmelidir **src** yolu.

5. İçinde **Python betiğini** metin kutusu, geçerli Python betiğini yapıştırın veya yazın.

    **Python betiğini** yorumlar ve veri erişimi ve çıkış için örnek kod, bazı yönergelerle önceden doldurulmuş olduğunu metin kutusu. **Düzenlemeli veya bu kodu değiştirin.** Girinti ve büyük/küçük harf ile ilgili Python kurallarını takip ettiğinizden emin olun.

    + Betik adlı bir işlev içermelidir `azureml_main` olarak bu modülü için giriş noktası.
    + En fazla iki giriş bağımsız değişkeni giriş noktası işlevi içerebilir: `Param<dataframe1>` ve `Param<dataframe2>`
    + Sıkıştırılmış dosyalar üçüncü giriş bağlantı noktasına bağlı geçin ve dizinde saklanan `.\Script Bundle`, ayrıca eklendiği için Python `sys.path`. 

    Bu nedenle, zip dosyası içeriyorsa `mymodule.py`, kullanarak içe aktarın `import mymodule`.

    + İki veri kümesi türü bir dizi olmalıdır arabirimi döndürülmesi `pandas.DataFrame`. Python kodunuzun diğer çıktıları oluştur ve bunları doğrudan Azure depolamaya yazma.

6. Denemeyi çalıştırın veya modülünü seçin ve **Seçileni Çalıştır** yalnızca Python betiğini çalıştırılacak.

    Tüm veri ve kod, bir sanal makineye yüklenir ve belirtilen Python ortamını kullanarak çalıştırın.

## <a name="results"></a>Sonuçlar

Katıştırılmış Python kodu tarafından gerçekleştirilen tüm hesaplamalar sonuçlarının bir pandas sağlanmalıdır. Otomatik olarak Azure Machine Learning veri kümesi biçimine dönüştürülür ve böylece sonuçları denemede diğer modüller ile kullanabileceğiniz DataFrame.

Modül, iki veri kümesi döndürür:  
  
+ **Veri kümesi 1 sonuçları**, ilk döndürülen pandas dataframe Python komut dosyasında tanımlanır

+ **Veri kümesi 2 neden**, ikinci döndürülen pandas dataframe Python komut dosyasında tanımlanır


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 