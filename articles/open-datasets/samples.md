---
title: NOAA ile açık bir veri kümesini kullanarak örnek Jupyter Not Defterleri
titleSuffix: Azure Open Datasets
description: Açık veri kümeleri yüklenemedi ve tanıtım verileri zenginleştirmek için bunları kullanmayı öğrenmek için örnek Jupyter not defterleri Azure açın veri kümeleri için kullanın. Teknikleri, Spark ve Pandas veriyi işlemek için kullanımını içerir.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: b62a2690e5879e45a14d0b06a38e8c5171dda14e
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442339"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Açık veri kümelerini verilerle zenginleştirerek nasıl örnek Jupyter not defterleri Göster 
Azure açık veri kümeleri için örnek Jupyter not defterlerini açık veri kümelerini yüklemek ve tanıtım verileri zenginleştirmek için bunları kullanmayı gösterilmektedir. Teknikleri, veriyi işlemek için Apache Spark ve Pandas kullanımını içerir.

>[!IMPORTANT]
>Spark olmayan ortamda çalışırken, açık veri kümeleri, büyük veri kümeleriyle MemoryError önlemek için yalnızca bir aylık veri birer birer belirli sınıflarla indirme sağlar.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>NOAA tümleşik Surface veritabanı (ISD) veri yükleme 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Hava durumu verilerine en son bir ayın Pandas dataframe'e yüklemek](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Tarihsel hava verilerine sık kullanılan Pandas dataframe'e yüklemek hakkında bilgi edinin. |
|[Hava durumu verilerine en son bir ayın Spark dataframe'e yüklemek](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Tarihsel hava verilerine sık kullandığınız Spark dataframe'e yüklemek hakkında bilgi edinin.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Tanıtım verileri NOAA ISD verilerle katılın 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Tanıtım verileri ile hava durumu verileri - Pandas katılın](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Algılayıcı konumların hava durumu okumalar ile 1 aylık tanıtım veri kümesi içinde bir Pandas dataframe katılın.  |
|[Tanıtım verileri ile hava durumu verileri Spark katılın](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Hava durumu okumalar algılayıcı konumlarla bir tanıtım veri kümesi içinde bir Spark dataframe katılın. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>NYC taksi verilerini NOAA ISD verileriyle katılın 
|Not Defteri        | Açıklama                                    |
|----------------|------------------------------------------------|
|[Taksi seyahat verileri ile hava durumu verileri - Pandas zenginleştirilmiş](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | NYC yeşil taksi verilerini (1 ay içinde) yüklemek ve hava durumu verileri Pandas dataframe zenginleştirin. Bu örnek yöntemini geçersiz kılan `get_pandas_limit` ve veri miktarı ile veri yükleme performansını dengeler.|
|[Taksi seyahat verileri ile hava durumu verileri – Spark zenginleştirilmiş](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | NYC yeşil taksi verileri yüklemek ve hava durumu verileri, Spark dataframe zenginleştirin.  |

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Regresyon otomatik makine öğrenimi ve açık bir veri kümesi ile modelleme](tutorial-opendatasets-automl.md)
* [Açık veri kümeleri için Python SDK](https://aka.ms/open-datasets-api)
* [Azure açık veri kümeleri Kataloğu](https://azure.microsoft.com/services/open-datasets/catalog/)
