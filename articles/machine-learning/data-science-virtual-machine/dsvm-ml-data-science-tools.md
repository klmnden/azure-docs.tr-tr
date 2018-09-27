---
title: Makine öğrenimi ve veri bilimi araçları - Azure | Microsoft Docs
description: Makine öğrenimi ve veri bilimi araçları
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 9a9dc868c4f22f95ca5027e3c95513d176c69eac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392413"
---
# <a name="machine-learning-and-data-science-tools"></a>Makine öğrenimi ve veri bilimi araçları
Veri bilimi sanal makinesi (DSVM) zengin birtakım Araçlar ve kitaplıklar için machine learning Python, R, Julia gibi popüler dilde kullanılabilir sahiptir. 

Makine öğrenimi araçlarını ve kitaplıklarını temel DSVM bazıları aşağıda verilmiştir. 

## <a name="azure-machine-learning-servicehttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>[Azure Machine Learning hizmeti](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) SDK'sı
|    |           |
| ------------- | ------------- |
| Nedir?   |   Azure Machine Learning hizmeti geliştirmek ve makine öğrenimi modelleri dağıtmak için kullanabileceğiniz bir bulut hizmetidir.  Oluşturmak, eğitmek, ölçeklendirme ve Python SDK'sını kullanarak bunları yönetirken, Modellerinizi izleyebilirsiniz. Modelleri kapsayıcıları olarak dağıtma ve bulutta, şirket içi veya IOT Edge üzerinde çalıştırın.   |
| Desteklenen DSVM sürümleri     | Windows (Conda ortam: AzureML), Linux (Conda ortam: py36)    |
| Tipik kullanımları      | Genel ML platformu      |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      |  GPU desteğine sahip yüklü   |
| Kullanma / çalıştırın nasıl?      | Python SDK'sını ve Azure komut satırı aracı (AZ CLI). Conda ortama etkinleştirme `AzureML` Windows sürümü veya çok `py36` Linux Edition.      |
| Örneklere bağlantılar      | Örnekleri Jupyter not defterleri dahil edilecek `AzureML` not defterlerini altında dizin  |
| DSVM ilgili araçları      | Visual Studio kodu, Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| Nedir?   |    Hızlı, taşınabilir ve dağıtılmış gradyan artırma (GBDT, GBRT veya GBM) kitaplığı, Python, R, Java, Scala, C++ ve daha fazlası. Tek makine üzerinde Hadoop, Spark çalışır    |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      |  GPU desteğine sahip yüklü   |
| Kullanma / çalıştırın nasıl?      | Python kitaplığı (2.7 ve 3.5) olarak R paketi ve yolu komut satırı aracı (`C:\dsvm\tools\xgboost\bin\xgboost.exe` , Windows için `/dsvm/tools/xgboost/xgboost` Linux)    |
| Örneklere bağlantılar      | Örnekleri dahil edilecek VM'de `/dsvm/tools/xgboost/demo` Linux'ta ve `C:\dsvm\tools\xgboost\demo` Windows üzerinde   |
| DSVM ilgili araçları      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| Nedir?   |   Vowpal Wabbit (diğer adıyla "VW") olan açık kaynaklı hızlı çıkış,-sistem kitaplığı öğrenme çekirdek    |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel ML kitaplığı      |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      |  Windows - MSI yükleyici, Linux - apt-get |
| Kullanma / çalıştırın nasıl?      | Açık bir yol komut satırı aracı olarak (`C:\Program Files\VowpalWabbit\vw.exe` , Windows üzerinde `/usr/bin/vw` Linux üzerinde)    |
| Örneklere bağlantılar      | [VowPal Wabbit örnekleri](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| DSVM ilgili araçları      |LightGBM, MXNet, XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Weka veri araştırma görevleri için makine öğrenimi algoritmaları koleksiyonudur. Algoritmalar doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel ML aracı     |
| Kullanma / çalıştırın nasıl?      | Windows üzerinde Başlat menüsündeki Weka arayın. Linux üzerinde X2Go bilgilerinizle oturum açın, sonra gidin uygulamaları geliştirme -> Weka ' ->. |
| Örneklere bağlantılar      | [Weka örnekleri](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM ilgili araçları      |LightGBM, Çıngırağı, XGBooost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri araştırma için bir grafik kullanıcı arabirimi   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | R için genel kullanıcı Arabirimi veri araştırma aracı    |
| Kullanma / çalıştırın nasıl?      | Kullanıcı Arabirimi aracıdır. Windows, bir komut istemi açın, R, ardından R çalıştırma içinde çalıştırma `rattle()`. Linux üzerinde X2Go ile bağlanmak, bir terminal Başlat, R, ardından R çalıştırma içinde çalıştırmak `rattle()`. |
| Örneklere bağlantılar      | [Çıngırağı](https://togaware.com/onepager/) |
| DSVM ilgili araçları      |LightGBM, Weka, XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| Nedir?   | Hızlı, dağıtılmış, yüksek performanslı gradyan derecelendirme, Sınıflandırma ve görevleri birçok diğer makine öğrenimi için kullanılan karar ağacı algoritmalarını göre (GBDT, GBRT, GBM veya REYONU) framework artırma.    |
| Desteklenen DSVM sürümleri      | Windows, Linux    |
| Tipik kullanımları      | Genel amaçlı gradyan artırırken framework      |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      | Windows üzerinde LightGBM bir Python paketi olarak yüklenir. Linux üzerinde komut satırı yürütülebilir dosyasını bulunduğu `/opt/LightGBM/lightgbm`R paketinin yüklü olduğu ve Python paketleri yüklenir.     |
| Örneklere bağlantılar      | [LightGBM Kılavuzu](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| DSVM ilgili araçları      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| Nedir?   | Bellek içi, dağıtılmış, hızlı ve ölçeklenebilir makine öğrenimi destekleyen bir açık kaynak yapay ZEKA platformu  |
| Desteklenen DSVM sürümleri      | Linux   |
| Tipik kullanımları      | Genel amaçlı dağıtılmış, ölçeklenebilir ML   |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      | H2O yüklü `/dsvm/tools/h2o`.      |
| Kullanma / çalıştırın nasıl?      | X2Go kullanarak VM'ye bağlanın. Yeni bir terminal Başlat ve Çalıştır `java -jar /dsvm/tools/h2o/current/h2o.jar`. Ardından bir web tarayıcı başlatmak ve bağlanma `http://localhost:54321`.      |
| Örneklere bağlantılar      | Örnekleri vm'sinde Jupyter altında kullanılabilir `h2o` dizin.      |
| DSVM ilgili araçları      | Apache Spark, MXNet, XGBoost, Sparkling Water, derin su    |

Popüler gibi DSVM üzerinde birçok ML kitaplıkları vardır `scikit-learn` DSVM'nin yüklenmiş Anaconda Python dağıtımının bir parçası olarak gelen paket. İlgili paket yöneticileri çalıştırarak, Python, R ve Julia'nın kullanılabilir paketler listesini dışarı denetlediğinizden emin olun. 
