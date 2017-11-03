---
title: "Birden çok coğrafi Yardım belgeleri | Microsoft Docs"
description: "Çalışma alanı oluşturma ve bir web hizmeti Güney Orta Amerika Birleşik Devletleri (SCUS) öğesinden farklı bir Azure bölgesi yayımlamak öğrenin Azure bölgesi."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: e4941ccf8c6d7a0c77527e9c1d722bc3a770114a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="multi-geo-help-documentation"></a>Çoklu Coğrafi Bölge Yardım belgeleri
Bu makalede, nasıl bir çalışma alanı oluşturmak ve bir web hizmeti farklı Azure bölgelerinde yayımlama açıklanır.  [Azure ürünleri bölge sayfası tarafından](https://azure.microsoft.com/en-us/regions/services/) Azure Machine Learning olduğu kullanılabilir bölgeleri listeler.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Klasik Azure portalında oturum açın.
2. Tıklatın **+ yeni** > **Veri Hizmetleri** > **MACHINE LEARNING** > **hızlı Oluştur**.  Altında **konumu** gibi başka bir bölge seçin **Güneydoğu Asya**.
   ![Birden çok coğrafi yardımcı görüntü 1][1]
3. Çalışma alanını seçin ve ardından **oturum açma ML Studio**.
   ![Birden çok coğrafi yardımcı görüntü 2][2]
4. Diğer çalışma gibi kullanabilir, başka bir bölgede bir çalışma alanı artık sahipsiniz. Alanlarınız arasında geçiş yapmak için ekranınızın sağ üst için bakın. Açılan listeyi tıklatın, bölgeyi seçin ve çalışma alanını seçin. Her şeyi çalışma bölgesine yereldir.  Örneğin, bir çalışma alanından oluşturulan web hizmetlerinizi tümünün çalışma alanında bulunan aynı bölgede olacaktır.
   ![Birden çok coğrafi yardımcı görüntü 3][3]

## <a name="open-an-experiment-from-gallery"></a>Galeriden bir denemeyi açın
Galeriden bir denemeyi açarsanız, denemede kopyalamak istediğiniz hangi bölgede öğesini de seçebilirsiniz.

![Birden çok coğrafi yardımcı görüntü 4][4a]

## <a name="web-service-management"></a>Web Hizmeti Yönetimi
Program aracılığıyla web Hizmetleri gibi yeniden eğitme için yönetmek için bölgeye özgü adresi kullanın:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Dikkat edilecek noktalar
1. Yalnızca bu şekilde aynı bölgeye ait çalışma alanları arasında denemeler kopyalayabilirsiniz. Deneme kopyalamanız gerekiyorsa kullanabileceğiniz farklı bölgelerdeki çalışma alanları arasında [PowerShell](http://aka.ms/amlps) komutunu [ *kopya AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) bunu yapmaya yönelik. Başka bir çözüm listelenmemiş modunda Galerisi içine deneme yayımlamaktır diğer bölgesinden çalışma alanını açın.
2. Bölge Seçici aynı anda yalnızca bir bölge için çalışma alanlarını gösterir.  
3. Ücretsiz çalışma alanında ya da konuk erişimi (anonim) çalışma oluşturulacak ve ABD merkezi Güney içinde barındırılan  
4. Güneydoğu Asya Güneydoğu Asya, bir çalışma alanından dağıtılan web hizmetleri de barındırılacak.  

## <a name="more-information"></a>Daha fazla bilgi
Üzerinde bir soru sorun [Azure Machine Learning Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/multi-geo/multi-geo_1.png
[2]: ./media/multi-geo/multi-geo_2.png
[3]: ./media/multi-geo/multi-geo_3.png
[4a]: ./media/multi-geo/multi-geo_4a.png
