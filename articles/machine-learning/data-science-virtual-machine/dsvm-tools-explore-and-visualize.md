---
title: "Veri keşfi ve görselleştirme araçları - Azure | Microsoft Docs"
description: "Veri keşfi ve görselleştirme araçları için veri bilimi sanal makine."
keywords: "Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: gokuma;bradsev
ms.openlocfilehash: 96f8d1bcf5a7e51823ed55a1b924d1397eba9da5
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Veri keşfi ve görselleştirme araçları üzerinde veri bilimi sanal makine

Veri bilimi anahtar bir adımda veri anlamaktır. Görselleştirme ve veri araştırması araçları veri anlama artırmanıza yardımcı olur. Bu anahtar bu faciliate DSVM üzerinde Adım sağlanan bazı araçlar şunlardır. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Nedir?   | Büyük veri açık kaynak SQL sorgu altyapısı    |
| Desteklenen DSVM sürümleri      | Windows, Linux  |
| Nasıl, yapılandırılmış veya DSVM üzerinde yüklü?      |  Yüklü `/dsvm/tools/drill*` yalnızca katıştırılmış modunda   |
| Tipik kullanır      |  ETL gerektirmeden situ veri keşfi. Sorgu farklı veri kaynakları ve ilişkisel tablolar, Hadoop includign CSV, JSON, biçimleri     |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu  <br/> [10 dakika içinde ayrıntıya ile çalışmaya başlama](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM ilgili araçları      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Weka, veri araştırma görevleri için machine learning algoritmaları koleksiyonudur. Algoritmalar doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka verileri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme için araçları içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | Genel ML aracı     |
| Kullanın / çalıştırmak için nasıl?      | Windows, Başlat menüsündeki Weka arayın. Linux üzerinde oturum X2Go oturum açın, sonra gidin uygulamaları geliştirme -> Weka ->. |
| Örnekleri bağlantılar      | [Weka örnekleri](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM ilgili araçları      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri araştırma için bir grafik kullanıcı arabirimi   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanır      | R için genel UI veri araştırma aracı    |
| Kullanın / çalıştırmak için nasıl?      | Kullanıcı Arabirimi aracıdır. Windows, bir komut istemi başlatın, ardından Çalıştır R içinde R çalıştırmak `rattle()`. Linux üzerinde X2Go ile bağlanmak, bir terminal başlatın, ardından Çalıştır R içinde R çalıştırmak `rattle()`. |
| Örnekleri bağlantılar      | [Rattle](https://togaware.com/onepager/) |
| DSVM ilgili araçları      |LightGBM, Weka, Xgboost   |

## <a name="powerbi-desktop"></a>PowerBI Desktop 
|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri Görselleştirme ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanır      |  Veri Görselleştirme ve panolar oluşturma   |
| Kullanın / çalıştırmak için nasıl?      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio Code, Juno      |

