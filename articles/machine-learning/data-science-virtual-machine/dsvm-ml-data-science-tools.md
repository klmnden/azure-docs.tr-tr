---
title: Makine öğrenimi ve veri bilimi araçları - Azure | Microsoft Docs
description: Makine öğrenimi araçları ve çerçeveleri veri bilimi sanal makinesi üzerinde önceden yüklenmiş hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: e8876306e4ffbd0fa9a8aafc6d5d757fd3c9c614
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502121"
---
# <a name="machine-learning-and-data-science-tools"></a>Makine öğrenimi ve veri bilimi araçları
Azure veri bilimi sanal makineleri zengin bir araç olan ve kitaplıklar için machine learning (ML), Python, R ve Julia'nın gibi popüler dilde kullanılabilir. 

ML araçları ve kitaplıkları üzerinde veri bilimi sanal makineleri bazıları aşağıda verilmiştir. 

## <a name="azure-machine-learninghttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) SDK'sı
|    |           |
| ------------- | ------------- |
| Nedir?   |   Azure Machine Learning, geliştirmek ve ML modelleri dağıtmak için kullanabileceğiniz bir bulut hizmetidir. Oluşturmak, eğitmek, ölçeklendirme ve Python SDK'sı kullanılarak bunların yönetilmesi, Modellerinizi izleyebilirsiniz. Modelleri kapsayıcıları olarak dağıtma ve bulutta, şirket içinde veya Azure IOT Edge üzerinde çalıştırın.   |
| Desteklenen sürümler     | Windows (conda ortam: AzureML), Linux (conda ortam: py36)    |
| Tipik kullanımları      | Genel ML platformu      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Python SDK'sını ve Azure CLI. Conda ortama etkinleştirme `AzureML` Windows Edition *veya* için `py36` Linux Edition.      |
| Örnekler için bağlantı      | Örnek Jupyter not defterleri dahil edilecek `AzureML` not defterlerini altında dizin.  |
| İlgili araçları      | Visual Studio kodu, Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Nedir?   |    Python, R, Java, Scala, C++ ve daha fazla bilgi için (GBDT, GBRT veya GBM) kitaplığı artırma hızlı, taşınabilir ve dağıtılmış bir gradyan XGBoost olur. İşlem, tek bir makine, Hadoop ve Spark üzerinde çalışır.    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  GPU desteğine sahip yüklü   |
| Kullanma veya çalıştırın      | Python olarak kitaplığı (2.7 ve 3.5), R paketi ve yolu komut satırı aracı (`C:\dsvm\tools\xgboost\bin\xgboost.exe` , Windows için `/dsvm/tools/xgboost/xgboost` Linux)    |
| Örneklere bağlantılar      | Örnekleri dahil edilecek VM'de `/dsvm/tools/xgboost/demo` , Linux'ta ve `C:\dsvm\tools\xgboost\demo` Windows üzerinde.   |
| İlgili araçları      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Nedir?   |   Vowpal Wabbit (diğer adıyla "VW") açık kaynaklı, hızlı, çekirdek giden sistem kitaplığı öğrenme.    |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış yüklü mü?      |  Windows--MSI yükleyici, Linux--apt-get |
| Kullanma veya çalıştırın      | Bir yol komut satırı aracı olarak (`C:\Program Files\VowpalWabbit\vw.exe` , Windows üzerinde `/usr/bin/vw` Linux üzerinde)    |
| Örnekler için bağlantı      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| İlgili araçları      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Weka veri araştırma görevleri için ML algoritmaları koleksiyonudur. Algoritmalar ya da doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | Genel ML aracı     |
| Kullanma veya çalıştırın      | Windows üzerinde Başlat menüsünde Weka arayın. Linux üzerinde X2Go bilgilerinizle oturum açın ve ardından Git **uygulamaları** > **geliştirme** > **Weka**. |
| Örnekler için bağlantı      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| İlgili araçları      |LightGBM, Çıngırağı, XGBoost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak Çıngırağı veri madenciliği için bir grafik kullanıcı arabirimi olan   |
| Desteklenen sürümler     | Windows, Linux     |
| Tipik kullanımları      | R için genel kullanıcı Arabirimi veri araştırma aracı    |
| Kullanma veya çalıştırın      | Kullanıcı Arabirimi aracıdır. Windows üzerinde bir komut istemi açın, R çalıştırın ve ardından R içinde çalıştırın `rattle()`. X2Go ile Linux üzerinde bağlama, bir terminal Başlat, R çalıştırın ve ardından R içinde çalıştırın `rattle()`. |
| Örnekler için bağlantı      | [Çıngırağı](https://togaware.com/onepager/) |
| İlgili araçları      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Nedir?   | Karar ağacı algoritmalarını üzerinde temel (GBDT, GBRT, GBM veya REYONU) framework artırma hızlı, dağıtılmış, yüksek performanslı bir gradyan LightGBM olur. Derecelendirme, Sınıflandırma ve diğer birçok ML görevleri için kullanılır.    |
| Desteklenen sürümler      | Windows, Linux    |
| Tipik kullanımları      | Genel amaçlı gradyan artırırken framework      |
| Nasıl, yapılandırılmış yüklü mü?      | Windows üzerinde LightGBM bir Python paketi olarak yüklenir. Linux üzerinde komut satırı yürütülebilir dosyasını bulunduğu `/opt/LightGBM/lightgbm`R paketinin yüklü olduğu ve Python paketleri yüklenir.     |
| Örnekler için bağlantı      | [LightGBM Kılavuzu](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| İlgili araçları      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Nedir?   | H2O bellek içi, dağıtılmış, hızlı ve ölçeklenebilir ML destekleyen açık kaynaklı AI platformudur.  |
| Desteklenen sürümler      | Linux   |
| Tipik kullanımları      | Genel amaçlı dağıtılmış, ölçeklenebilir ML   |
| Nasıl, yapılandırılmış yüklü mü?      | H2O yüklü `/dsvm/tools/h2o`.      |
| Kullanma veya çalıştırın      | X2Go kullanarak VM'ye bağlanın. Yeni bir terminal başlatın ve çalıştırın `java -jar /dsvm/tools/h2o/current/h2o.jar`. Ardından bir web tarayıcı başlatmak ve bağlanma `http://localhost:54321`.      |
| Örnekler için bağlantı      | Örnekleri vm'sinde Jupyter altında kullanılabilir `h2o` dizin.      |
| İlgili araçları      | Apache Spark, MXNet, XGBoost, Sparkling Water, derin su    |

Vardır diğer birçok ML kitaplıkları veri bilimi sanal makineleri gibi popüler `scikit-learn` veri bilimi sanal makinelerde yüklü Anaconda Python dağıtımının bir parçası olarak gelen paket. Python, R ve Julia'nın kullanılabilir paketler listesini görmek için ilgili paket yöneticileri çalıştırın.
