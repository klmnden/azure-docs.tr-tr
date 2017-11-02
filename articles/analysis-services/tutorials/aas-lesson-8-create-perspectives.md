---
title: "Azure Analysis Services öğreticisi - 8. Ders: Perspektif oluşturma | Microsoft Docs"
description: "Azure Analysis Services öğretici projesinde nasıl perspektif oluşturulacağını açıklar."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a8bb8748d4fe39d664055549564d08524cdc80f3
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="lesson-8-create-perspectives"></a>8. Ders: Perspektif oluşturma

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu derste Internet Sales adlı bir perspektif oluşturacaksınız. Perspektif odaklanmış, işe özgü veya uygulamaya özgü bakış açılarını sağlayan bir modelin görüntülenebilir bir alt kümesini tanımlar. Bir kullanıcı, perspektif kullanarak bir modele bağlandığında perspektifte tanımlı alan olarak yalnızca ilgili model nesnelerini (tablolar, sütunlar, ölçüler, hiyerarşiler ve KPI'ler) görür. Daha fazla bilgi için bkz. [Perspektifler](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
Bu derste oluşturacağınız Internet Sales perspektifinde DimCustomer tablo nesnesi dışlanacak. Görünümden belirli nesneleri dışlayan bir perspektif oluşturduğunuzda söz konusu nesne modelde bulunmaya devam eder. Ancak bir raporlama istemcisi alan listesinde görünmez. Perspektife eklenip eklenmemelerinden bağımsız olarak, hesaplanmış sütunlar ve ölçüler, dışlanan nesne verilerinden hesaplama yapmaya devam edebilir.  
  
Bu dersin amacı perspektiflerin nasıl oluşturulacağını açıklamak ve tablosal model yazma araçları ile çalışmaya başlamaktır. Daha sonra başka tablolar ekleyerek bu modeli genişletirseniz modelin farklı bakış açılarını (örneğin, Stok ve Satış) tanımlamak üzere ek perspektifler oluşturabilirsiniz.  
  
Bu dersin tahmini tamamlanma süresi: **Beş dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu konu başlığı, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin parçasıdır. Bu dersteki görevleri gerçekleştirebilmek için bir önceki dersi tamamlamış olmanız gerekir: [7. Ders: Ana Performans Göstergeleri Oluşturma](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Perspektif oluşturma  
  
#### <a name="to-create-an-internet-sales-perspective"></a>Internet Sales adlı bir perspektif oluşturmak için  
  
1.  **Model** menüsü > **Perspektifler** > **Oluştur ve Yönet**'e tıklayın.  
  
2.  **Perspektifler** iletişim kutusunda **Yeni Perspektif**'e tıklayın.  
  
3.  **Yeni Perspektif** sütun başlığına çift tıklayın ve ardından **Internet Sales** olarak yeniden adlandırın.  
  
4.  **DimCustomer** *dışındaki* tüm tabloları seçin.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    Sonraki bir derste bu perspektifi test etmek için Excel'de Çözümle özelliğini kullanacaksınız. Excel PivotTable Alan Listesinde DimCustomer tablosu hariç tüm tablolar bulunmaktadır.  

## <a name="whats-next"></a>Sırada ne var?
[9. Ders: Hiyerarşi oluşturma](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
