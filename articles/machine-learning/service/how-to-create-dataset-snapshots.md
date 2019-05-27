---
title: Veri kümesi anlık görüntüleri ile verileri yeniden & Karşılaştır
titleSuffix: Azure Machine Learning service
description: Zaman içinde verileri karşılaştırmak ve veri kümesi anlık görüntülerle yeniden üretilebilirliğini sağlamak hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.date: 05/23/2019
ms.openlocfilehash: 525660be0f38c9458590e52cfcd575acb4cf5444
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66162067"
---
# <a name="compare-data-and-ensure-reproducibility-with-snapshots-preview"></a>Verileri karşılaştırmak ve anlık görüntüleri (Önizleme) ile yeniden üretilebilirliğini emin olun

Bu makalede, şunların nasıl oluşturup anlık görüntülerini yönetmek için [Azure Machine Learning veri kümeleri](how-to-create-register-datasets.md) (veri kümeleri yakalama ya da zaman içinde verileri karşılaştırın). Veri kümeleri erişmek ve çeşitli senaryolarda bulutta, verilerle çalışmayı kolaylaştırır.

**Veri kümesi anlık görüntüleri** oluşturulduğu sırada bir ' % s'profil (Özet istatistikleri) veri depolayın. Ayrıca, anlık görüntü yeniden üretilebilirliğini için verilerin bir kopyasını depolamak seçebilirsiniz.

>[!Important]
> Anlık görüntü depolama ücret yansıtılmaz. Verilerin bir kopyasını depolayarak, anlık görüntüde, daha fazla depolama gerektirir. Kullanım [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) bunlar artık gerekli olmadığında.

## <a name="when-to-use-snapshots"></a>Ne zaman anlık görüntülerini kullanmak üzere

Anlık görüntüler için üç temel kullanım vardır:

+ **Model doğrulama**: Eğitim çalıştırmaları arasında veya karşı üretim verileri farklı anlık görüntü veri profili karşılaştırın.

+ **Modeli yeniden üretilebilirliğini**: Sonuçlarınızı eğitim sırasında verileri içeren bir anlık görüntü çağırarak yeniden oluşturun.

+ **Zaman içinde verileri izleme**: Veri kümesi tarafından nasıl geliştirildiğini bkz [profilleri karşılaştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#compare-profiles-rhs-dataset-snapshot--include-columns-none--exclude-columns-none--histogram-compare-method--histogramcomparemethod-wasserstein--0--)
  
## <a name="prerequisites"></a>Önkoşullar

Veri kümesi anlık görüntüleri oluşturmak için kayıtlı bir Azure Machine Learning veri kümesi gerekir. Biri yoksa bkz [oluşturma ve kaydetme veri kümeleri](how-to-create-register-datasets.md).

## <a name="create-dataset-snapshots"></a>Veri kümesi anlık görüntüleri oluşturma

Bir veri kümesinin bir anlık görüntü oluşturmak için kullanın [ `dataset.create_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?#create-snapshot-snapshot-name--compute-target-none--create-data-snapshot-false--target-datastore-none-) Azure Machine Learning SDK'sının azureml veri kümeleri paketinden.

Varsayılan olarak, anlık görüntü ' % s'profil (Özet istatistikleri) en son verileri depolayan [veri kümesi tanımı](how-to-manage-dataset-definitions.md) uygulanır. Bir veri kümesi tanımı, veriler için tanımlanmış herhangi bir dönüştürme adımı bir kaydını içerir. Tekrarlanabilir iş, veri hazırlığı yapmak için harika bir yoludur.

İsteğe bağlı olarak ayrıca verilerin bir kopyasını, anlık görüntüde ekleyerek ekleyebilirsiniz `create_data_snapshot = True`.  Bu veriler için yeniden üretilebilirliğini yararlı olabilir.

Bu örnekte [örnek suç verileri](https://dprepdata.blob.core.windows.net/dataset-sample-files/crime.csv) ve bir veri kümesi olarak adlandırılan `dataset_crime` makale kullanılarak oluşturulan ["veri kümeleri oluşturma ve kaydetme"](how-to-create-register-datasets.md).

```Python
from azureml.core.workspace import Workspace
from azureml.core.dataset import Dataset
from azureml.data.dataset_snapshot import DatasetSnapshot
import datetime

# get existing workspace
workspace = Workspace.from_config()

# get existing, named dataset:
dataset = workspace.datasets['dataset_crime']

# assign name to snapshot
snapshot_name = 'snapshot_' + datetime.datetime.today().strftime('%Y%m%d%H%M%S')

# create snapshot to store profile from a remote environment
# and include copy of data
snapshot = dataset.create_snapshot(snapshot_name = snapshot_name,
                                   compute_target = remote_compute_target,
                                   create_data_snapshot = True)
```

Anlık görüntüleri zaman uyumsuz olarak oluşturulduğundan, kullanın [ `wait_for_completion()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_snapshot.datasetsnapshot?view=azure-ml-py#wait-for-completion-show-output-true--status-update-frequency-10-) işlemini izlemek için yöntemi.

```Python
# monitor process every 10 seconds
snapshot.wait_for_completion(show_output=True, status_update_frequency=10)

# return snapshot just created
dataset.get_snapshot(snapshot_name)
```

Çıkış:

```
Action status: Running
Action status: Running
Action status: Completed


DatasetSnapshot(Name: snapshot_20190411165420,
Dataset Id: 66cd0830-2f72-41e5-b677-d2d953f54821,
Workspace: "your_workspace_name",
Dataset definition version: 1,
Snapshot created date: 2019-05-11 17:24:00+00:00)
```

Kullanım [ `dataset.delete_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#delete-snapshot-snapshot-name-) bunlar artık gerekli olmadığında.

## <a name="find-snapshots"></a>Anlık görüntüleri bulma

Var olan bir anlık görüntü almak için kullanın [ `get_snapshot()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-snapshot-snapshot-name-).

Belirli bir veri kümesi, kaydedilen anlık görüntülerini listesini almak için kullanın [ `get_all_snapshots()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-all-snapshots--).

```Python
# Get named snapshot for this dataset
dataset.get_snapshot(snapshot_name)

# Get all snapshots for this dataset
dataset.get_all_snapshots()
```

## <a name="get-data-and-profiles-from-snapshots"></a>Verileri ve profilleri anlık görüntülerden Al

Her anlık görüntü Özet istatistikleri içerir ve verilerinizin bir kopyasını Kaydet seçeneği sunar veri kümesinin bir profil oluşturur.

### <a name="get-the-data-profile"></a>Veri profili edinme

Kullanım [ `get_profile()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#get-profile-arguments-none--generate-if-not-exist-true--workspace-none--compute-target-none-) verilerinizin profilinize erişmek için.  Bu profil, eğitim ve üretim verilerini, örneğin karşılaştırmak için kullanabilirsiniz.

```Python
snapshot.get_profile()
```

||Tür|Min|Maks|Sayı|Eksik sayısı|Sayısı eksik|Eksik yüzde|Hata sayısı|Boş sayısı|% 0,1 Quantile|%1 Quantile|%5 Quantile|% 25 Quantile|% 50 Quantile|% 75 Quantile|% 95 Quantile|% 99 Quantile|% 99,9 Quantile|Ortalama|Standart Sapma|Varyans|Komutunu|Basıklık
-|----|---|---|-----|-------------|-----------------|---------------|-----------|-----------|-------------|-----------|-----------|------------|------------|------------|------------|------------|--------------|----|------------------|--------|--------|--------
Kimlik|FieldType.INTEGER|1.04986e + 07|1.05351e + 07|10.0|0.0|10.0|0.0|0.0|0.0|1.04986e + 07|1.04992e + 07|1.04986e + 07|1.05166e + 07|1.05209e + 07|1.05259e + 07|1.05351e + 07|1.05351e + 07|1.05351e + 07|1.05195e + 07|12302.7|1.51358e + 08|-0.495701|-1.02814
Büyük/küçük harf numarası|FieldType.STRING|HZ239907|HZ278872|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Date|FieldType.DATE|2016-04-04 23:56:00+00:00|2016-04-15 17:00:00+00:00|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Engelle|FieldType.STRING|004XX S KILBOURN KAYDET|113XX S PRAIRIE KAYDET|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
IUCR|FieldType.INTEGER|810|1154|10.0|0.0|10.0|0.0|0.0|0.0|810|850|810|890|1136|1153|1154|1154|1154|1058.5|137.285|18847.2|-0.785501|-1.3543
Birincil tür|FieldType.STRING|YANILTICI YÖNTEMİ|HIRSIZLIĞI|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Açıklama|FieldType.STRING|SAHTE DENETİMİ|500 ABD DOLARI|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Konum açıklaması|FieldType.STRING||OKUL, OLUŞTURMA, GENEL|10.0|0.0|10.0|0.0|0.0|1.0||||||||||||||
Arrest|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Yurt içi|FieldType.BOOLEAN|False|False|10.0|0.0|10.0|0.0|0.0|0.0||||||||||||||
Sinyali|FieldType.INTEGER|531|2433|10.0|0.0|10.0|0.0|0.0|0.0|531|531|531|614|1318.5|1911|2433|2433|2433|1371.1|692.094|478994|0.105418|-1.60684
Bölge|FieldType.INTEGER|5|24|10.0|0.0|10.0|0.0|0.0|0.0|5|5|5|6|13|19|24|24|24|13.5|6.94822|48.2778|0.0930109|-1.62325
İleri Git|FieldType.INTEGER|1|48|10.0|0.0|10.0|0.0|0.0|0.0|1|5|1|9|22.5|40|48|48|48|24.5|16.2635|264.5|0.173723|-1.51271
Topluluk alan|FieldType.INTEGER|4|77|10.0|0.0|10.0|0.0|0.0|0.0|4|8.5|4|24|37.5|71|77|77|77|41.2|26.6366|709.511|0.112157|-1.73379

### <a name="get-the-data-from-the-snapshot"></a>Anlık görüntüden veri alma

Bir veri kümesi anlık görüntüde kaydedilen verilerin bir kopyasını almak için bir pandas DataFrame oluşturun ile [ `to_pandas_dataframe()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#to-pandas-dataframe--) yöntemi.

Bu yöntem verilerin bir kopyasını anlık görüntü oluşturma sırasında istenmemiştir başarısız olur.

```Python
snapshot.to_pandas_dataframe().head(3)
```

||Kimlik|Büyük/küçük harf numarası|Tarih|Engelle|IUCR|Birincil tür|Açıklama|Konum açıklaması|Arrest|Yurt içi|...|İleri Git|Topluluk alan|FBI kod|X koordinatı|Y koordinatı|Yıl|Güncelleştirme tarihi|Enlem|Boylam|Konum
-|--|-----------|----|-----|----|------------|-----------|--------------------|------|--------|---|----|--------------|--------|------------|------------|----|----------|--------|---------|--------
0|10498554|HZ239907|2016-04-04 23:56:00|007XX E 111TH ST|1153|YANILTICI YÖNTEMİ|FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI ÜZERİNDEN|DİĞER|False|False|...|9|50|11|1183356.0|1831503.0|2016|2016-05-11 15:48:00|41.692834|-87.604319|(41.692833841, -87.60431945)
1|10516598|HZ258664|2016-04-15 17:00:00|082XX S MARSHFIELD AVE|890|HIRSIZLIĞI|ÖĞESİNDEN YAPI|İKAMET|False|False|...|21|71|6|1166776.0|1850053.0|2016|2016-05-12 15:48:00|41.744107|-87.664494|(41.744106973, -87.664494285)
2|10519196|HZ261252|2016-04-15 10:00:00|104XX S SACRAMENTO AVE|1154|YANILTICI YÖNTEMİ|FİNANSAL KİMLİK HIRSIZLIĞINDAN 300 ABD DOLARI VE ALTINDA|İKAMET|False|False|...|19|74|11|NaN|NaN|2016|2016-05-12 15:50:00|NaN|NaN|

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: SDK [genel bakış](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) tasarım desenleri ve kullanım örnekleri için.

* Veri kümesi anlık görüntülerini kullanarak bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).
