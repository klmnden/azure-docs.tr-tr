---
title: 'Veri içeri aktarın: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Verileri içeri aktarma modülü Azure Machine Learning hizmetinde bir machine learning denemesi mevcut bulut veri hizmetlerinden verileri yüklemek için nasıl kullanılacağını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ed51c4e7b6c7d226c7827d1ba00bc96a7be1e6b0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028313"
---
# <a name="import-data-module"></a>Veri modülünü içeri aktarın

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir machine learning denemesi mevcut bulut veri hizmetlerinden verileri yüklemek için bu modülü kullanın.  
Modülü şimdi depolama seçeneğini belirleyin ve hızlı bir şekilde tüm seçeneklerini yapılandırmak için mevcut abonelik ve hesaplar arasından seçin yardımcı olacak bir sihirbaz içerir. Var olan bir veri bağlantısını düzenlemeniz gerekir? Hiç sorun değil; sıfırdan yeniden başlatmak zorunda kalmazsınız tüm önceki yapılandırma ayrıntılarını Sihirbazı'nı yükler. 
  
Veri kaynağına bağlanmak ve istediğiniz tanımladıktan sonra [verileri içeri aktarma](./import-data.md) içerir ve verileri Azure Machine Learning çalışma alanınıza yükler değerlere göre her bir sütunun veri türünü çıkarsar. Çıkışı [verileri içeri aktarma](./import-data.md) herhangi bir deneme ile kullanılabilecek bir veri kümesidir.

  
Kaynak verilerinizin değişmesi halinde veri kümesini yenileyin ve yeniden çalıştırarak yeni veri ekleme [verileri içeri aktarma](./import-data.md). Ancak, yeniden denemeyi her çalıştırdığınızda kaynaktan okumaya istemiyorsanız seçin **kullanın, sonuçları önbelleğe** seçeneği true. Bu seçenek belirlendiğinde, modül denemeyi daha önce aynı kaynak hem de aynı giriş seçenekleri kullanarak dolmadığını denetler. Bir önceki çalıştırma bulunursa varsa, önbellekteki verilerin kullanılır, kaynak verilerin yeniden yüklenmesini yerine.
 

## <a name="data-sources"></a>Veri kaynakları

Verileri içeri aktarma modülü aşağıdaki veri kaynaklarını destekler. Ayrıntılı yönergeler ve her bir veri kaynağı kullanma örnekleri için bağlantıları tıklatın. 
 
Verilerinizi nasıl veya ne nerede depolamanız gerekir emin değilseniz, veri bilimi işlemi ortak veri senaryolarda bu kılavuza bakın:  [Azure Machine learning'de Gelişmiş analiz senaryoları](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Veri kaynağı| İle kullanma|
|-----------|-----------|  
|[HTTP üzerinden Web URL'si](./import-from-web-url-via-http.md)|HTTP kullanan ve CSV, TSV, ARFF'ye veya SvmLight biçimlerde sağlanan bir web URL'si üzerinde barındırılan veri alma|  
|[Azure Blob depolama alanından içeri aktarma](./import-from-azure-blob-storage.md) |Azure blob hizmetinde depolanan veri alma|  

## <a name="how-to-use-import-data"></a>Verileri İçeri Aktar'ı kullanma
 
1. Ekleme **verileri içeri aktarma** denemenizi modülü. Bu modülde bulabilirsiniz **veri giriş ve çıkış** arabiriminde kategorisi.

2. Tıklayın **veri içeri aktarma Sihirbazı'nı başlatma** bir Sihirbazı kullanarak veri kaynağına yapılandırmak için.

    Sihirbaz, hesap adını ve kimlik bilgilerini ve diğer seçenekleri yapılandırma Yardım alır. Mevcut bir yapılandırmayı düzenliyorsanız, öncelikle geçerli değerleri yükler.

3. Sihirbazı kullanmak istemiyorsanız tıklayın **veri kaynağı**, bulut tabanlı depolama, okumasını türünü seçin. 

    Ek ayarlar seçtiğiniz depolama türüne göre değişir ve depolama veya sağlanır. Hesap adı, dosya türü veya kimlik bilgilerini sağlamanız gerekebilir. Bazı kaynakları kimlik doğrulaması gerektirmez; diğerleri için hesap adı, bir anahtar veya kapsayıcı adı bilmeniz gerekebilir.

4. Seçin **kullanın, sonuçları önbelleğe** art arda gelen çalıştırmaları yeniden kullanmak üzere bir veri kümesi önbelleğe almak istiyorsanız seçeneği.

    Modül parametrelerini başka hiçbir değişikliğe olduğunu varsayarsak, denemeyi modülü çalıştırılır ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümü kullanan veri yalnızca ilk zaman yükler.

    Verileri yeniden denemeyi her çalıştırdığınızda gerekiyorsa, bu seçeneğin işaretini kaldırın.

5. Denemeyi çalıştırın.

    Verileri içeri aktarma arabirimine veri yüklediğinde, içerdiği değerlerine göre her bir sütunun veri türünü çıkarsar. sayısal veya kategorik.

    - Bir üstbilgi varsa, üst bilgi çıkış veri kümesi sütunları adlandırmak için kullanılır.

    - Verileri var olan bir sütun üst bilgileri varsa, yeni sütun adlarının biçimi Sütun1, Sütun2 kullanılarak oluşturulmuş... , coln *.

## <a name="results"></a>Sonuçlar

İçeri aktarma işlemi tamamlandığında, çıkış veri kümesi tıklayıp **Görselleştir** verileri başarıyla içeri aktarıldı görmek için.

Yeniden kullanmak için verileri kaydetmek istiyorsanız, deneme çalıştığı her saat yeni bir veri kümesini içeri aktarma yerine, çıkışı sağ tıklayıp **veri kümesi olarak Kaydet**. Veri kümesi için bir ad seçin. Kaydedilmiş veri kümesi verileri kaydetmeden sırasındaki korur ve denemeyi yeniden denemeyi kümesinde değişse bile veri güncelleştirilmez. Bu, veri anlık görüntülerini almak için kullanışlı olabilir.

Verileri içeri aktardıktan sonra modelleme ve analiz için bazı ek hazırlıklar ihtiyaç duyabilirsiniz:


- Kullanım [meta verileri Düzenle](./edit-metadata.md) sütun adlarını, sütun farklı veri türü olarak işlemek için veya bazı sütunları etiketleri veya özellikleri olduğunuzu gösterecek şekilde değiştirmek için.

- Kullanma [kümesindeki sütunları seçme](./select-columns-in-dataset.md) modelleme dönüştürmek veya sütunların bir alt kümesini seçin. Dönüştürülen veya kaldırılan sütunlar kolayca özgün veri kümesine kullanarak katılmak [Sütun Ekle](./add-columns.md) modülü.  

- Kullanım [bölüm ve örnek](./partition-and-sample.md) veri kümesini ayırmak, örnekleme gerçekleştirmek veya üst n satırları Al.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 