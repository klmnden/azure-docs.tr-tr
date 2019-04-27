---
title: Veri keşfi ve görselleştirme araçları - Azure | Microsoft Docs
description: Veri keşfi ve görselleştirme araçları için veri bilimi sanal makinesi.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 165df03ec06081fe9b2e1ab84ffe7579ac457758
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502271"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Veri keşfi ve görselleştirme araçları üzerinde veri bilimi sanal makinesi

Veri bilimi anahtar bir adımda verileri öğrenmektir. Görselleştirme ve veri İnceleme araçlarının veri anlama artırmanıza yardımcı olur. Bu önemli adım kolaylaştıran DSVM'nin sağlanan bazı araçları şunlardır. 

## <a name="apache-drill"></a>Apache ayrıntıya
|    |           |
| ------------- | ------------- |
| Nedir?   | Büyük veriler üzerinde açık kaynak SQL sorgu altyapısı    |
| Desteklenen DSVM sürümleri      | Windows, Linux  |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?      |  Yüklü `/dsvm/tools/drill*` yalnızca katıştırılmış modunda   |
| Tipik kullanımları      |  ETL gerektirmeden situ veri keşfi. Farklı veri kaynakları ve biçimler CSV, JSON, ilişkisel tabloları, Hadoop dahil olmak üzere sorgu     |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu  <br/> [Ayrıntıya 10 dakika içinde kullanmaya başlayın](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| DSVM ilgili araçları      |   Çıngırağı, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Nedir?   |  Weka veri araştırma görevleri için makine öğrenimi algoritmaları koleksiyonudur. Algoritmalar doğrudan bir veri kümesine uygulanan veya kendi Java koddan çağrılır. Weka veri ön işleme, Sınıflandırma, regresyon, kümeleme, ilişkilendirme kuralları ve görselleştirme araçları içerir. |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | Genel ML aracı     |
| Kullanma / çalıştırın nasıl?      | Windows üzerinde Başlat menüsündeki Weka arayın. Linux üzerinde X2Go bilgilerinizle oturum açın, sonra gidin uygulamaları geliştirme -> Weka ' ->. |
| Örneklere bağlantılar      | [Weka örnekleri](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| DSVM ilgili araçları      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Çıngırağı
|    |           |
| ------------- | ------------- |
| Nedir?   |   R kullanarak veri araştırma için bir grafik kullanıcı arabirimi   |
| Desteklenen DSVM sürümleri     | Windows, Linux     |
| Tipik kullanımları      | R için genel kullanıcı Arabirimi veri araştırma aracı    |
| Kullanma / çalıştırın nasıl?      | Kullanıcı Arabirimi aracıdır. Windows, bir komut istemi açın, R, ardından R çalıştırma içinde çalıştırma `rattle()`. Linux üzerinde X2Go ile bağlanmak, bir terminal Başlat, R, ardından R çalıştırma içinde çalıştırmak `rattle()`. |
| Örneklere bağlantılar      | [Çıngırağı](https://togaware.com/onepager/) |
| DSVM ilgili araçları      |LightGBM, Weka, Xgboost   |

## <a name="powerbi-desktop"></a>PowerBI Masaüstü 
|    |           |
| ------------- | ------------- |
| Nedir?   | Etkileşimli veri görselleştirmesi ve BI aracı    |
| Desteklenen DSVM sürümleri      | Windows  |
| Tipik kullanımları      |  Veri Görselleştirme ve panolar oluşturma   |
| Kullanma / çalıştırın nasıl?      | Masaüstü kısayolu (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| DSVM ilgili araçları      |   Visual Studio 2017, Visual Studio kodu, Juno      |

