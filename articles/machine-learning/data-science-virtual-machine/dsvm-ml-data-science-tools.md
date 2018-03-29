---
title: Machine Learning ve veri bilimi araçları - Azure | Microsoft Docs
description: Machine learning ve veri bilimi araçları
keywords: Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: f8d7fff30d5f5289c362d78ad89027b8141bbbe6
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="machine-learning-and-data-science-tools"></a>Machine learning ve veri bilimi araçları
Veri bilimi sanal makine (DSVM) zengin birtakım Araçlar ve Python, R, Jale gibi popüler dilde kullanılabilir machine learning için kitaplıkları sahiptir. 

Makine öğrenimi araçları ve kitaplıkları DSVM'bazıları aşağıda verilmiştir. 

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Nedir?   |    Hızlı, taşınabilir ve dağıtılmış gradyan artırma (GBDT, GBRT veya GBM) kitaplığı, Python, R, Java, Scala, C++ ve daha fazla bilgi için. Tek makinede Hadoop, Spark çalıştırır    |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      |  GPU desteğiyle yüklü   |
| Kullanın / çalıştırmak için nasıl?      | Python kitaplığı (2.7 ve 3.5) olarak R paketi ve yol komut satırı aracı (`C:\dsvm\tools\xgboost\bin\xgboost.exe` Windows, `/dsvm/tools/xgboost/xgboost` Linux için)    |
| Örnekleri bağlantılar      | Örnekleri dahil edilmiştir VM üzerinde `/dsvm/tools/xgboost/demo` Linux'ta ve `C:\dsvm\tools\xgboost\demo` Windows   |
| DSVM ilgili araçları      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Nedir?   |   Bir açık kaynak hızlı çıkış-in-sistem kitaplığı öğrenme core Vowpal Wabbit (olarak da bilinen "VW") değil    |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      |  Windows - msi installer, Linux - apt-get |
| Kullanın / çalıştırmak için nasıl?      | Açık bir yol komut satırı aracı olarak (`C:\Program Files\VowpalWabbit\vw.exe` , Windows'da `/usr/bin/vw` Linux)    |
| Örnekleri bağlantılar      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| DSVM ilgili araçları      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Weka, veri araştırma görevleri için machine learning algoritmaları koleksiyonudur. Algoritmalar doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka verileri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme için araçları içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | Genel ML aracı     |
| Kullanın / çalıştırmak için nasıl?      | Windows, Başlat menüsündeki Weka arayın. Linux üzerinde oturum X2Go oturum açın, sonra gidin uygulamaları geliştirme -> Weka ->. |
| Örnekleri bağlantılar      | [Weka örnekleri](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM ilgili araçları      |LightGBM, Rattle, XGBooost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri araştırma için bir grafik kullanıcı arabirimi   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | R için genel UI veri araştırma aracı    |
| Kullanın / çalıştırmak için nasıl?      | Kullanıcı Arabirimi aracıdır. Windows, bir komut istemi başlatın, ardından Çalıştır R içinde R çalıştırmak `rattle()`. Linux üzerinde X2Go ile bağlanmak, bir terminal başlatın, ardından Çalıştır R içinde R çalıştırmak `rattle()`. |
| Örnekleri bağlantılar      | [Rattle](https://togaware.com/onepager/) |
| DSVM ilgili araçları      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Nedir?   | Derecelendirme, Sınıflandırma ve görevleri öğrenme birçok makine için kullanılan karar ağacı algoritmalarını göre (GBDT, GBRT, GBM veya MART) framework artırmanın hızlı, dağıtılmış, yüksek performanslı gradyan.    |
| Desteklenen DSVM sürümleri      | Windows, Linux    |
| Tipik kullanır      | Genel amaçlı gradyan artırma framework      |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      | Windows üzerinde LightGBM Python paket olarak yüklenir. Komut satırı yürütülebilir dosyasını bulunduğu Linux üzerinde `/opt/LightGBM/lightgbm`R paketi yüklenir ve Python paketlerini yüklenir.     |
| Örnekleri bağlantılar      | [LightGBM Guide](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| DSVM ilgili araçları      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Nedir?   | Bellek içi, dağıtılmış, hızlı ve ölçeklenebilir machine learning destekleyen bir açık kaynak AI platformu  |
| Desteklenen DSVM sürümleri      | Linux   |
| Tipik kullanır      | Dağıtılmış, genel amaçlı ölçeklenebilir ML   |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      | H2O yüklü `/dsvm/tools/h2o`.      |
| Kullanın / çalıştırmak için nasıl?      | X2Go kullanarak VM'ye bağlanın. Yeni bir terminal Başlat ve Çalıştır `java -jar /dsvm/tools/h2o/current/h2o.jar`. Daha sonra bir web tarayıcı başlatmak ve bağlanmak `http://localhost:54321`.      |
| Örnekleri bağlantılar      | Örnekleri bulunur altında Jupyter VM'yi `h2o` dizin.      |
| DSVM ilgili araçları      | Apache Spark, MXNet, XGBoost, Sparkling Water, Deep Water    |

Vardır birkaç diğer ML kitaplıkları gibi popüler DSVM `scikit-learn` üzerinde DSVM yüklenmiş Anaconda Python dağıtım bir parçası olarak gelen paket. İlgili paketi yöneticileri çalıştırarak, Python, R ve Jale kullanılabilir olan paketlerin listesini dışarı denetlediğinizden emin olun. 
