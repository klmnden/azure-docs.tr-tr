---
title: Azure Güvenlik Merkezi'nde güvenlik uyarılarını işleme | Microsoft Belgeleri
description: Bu belge, güvenlik olaylarını işlemek için Azure Güvenlik Merkezi özelliklerini kullanmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2018
ms.author: rkarlin
ms.openlocfilehash: 68bcd2b1916ccdf68eaa31ed251661a6b7e1bca0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60704126"
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Azure Güvenlik Merkezi’nde Güvenlik Olaylarını İşleme
Güvenlik uyarılarının önceliklendirilmesi ve araştırılması en nitelikli güvenlik analiz uzmanları için bile vakit harcayıcı olabilir ve birçoğu için nereden başlanacağını bilmek bile zordur. Farklı [güvenlik uyarıları](security-center-managing-and-responding-alerts.md) arasındaki bilgileri bağlamak için [Analiz](security-center-detection-capabilities.md) özelliğini kullanan Güvenlik Merkezi, bir saldırı kampanyasını ve tüm ilgili uyarıları tek bir yerde görmenizi sağlayabilir; saldırganın hangi işlemleri yaptığını ve hangi kaynakların etkilendiğini hızlıca görebilirsiniz.

Bu belge Güvenlik Merkezi’ndeki güvenlik uyarısı özelliğinin güvenlik olaylarını işlemenize nasıl yardımcı olduğunu ele almaktadır.

## <a name="what-is-a-security-incident"></a>Güvenlik olayı nedir?
Güvenlik Merkezi'nde bir güvenlik olayı, bir kaynağın [sonlandırma zinciri](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) desenleri ile hizalanan tüm uyarılarının toplamıdır. Olaylar [Güvenlik Uyarıları](security-center-managing-and-responding-alerts.md) kutucuğunda ve dikey penceresinde görüntülenir. Bir Olay, her olay hakkında daha fazla bilgi almanızı sağlayan ilgili uyarıların listesini ortaya çıkarır.

## <a name="managing-security-incidents"></a>Güvenlik olaylarını yönetme
Geçerli güvenlik olaylarınızı güvenlik uyarıları kutucuğuna bakarak gözden geçirebilirsiniz. Azure Portal’a erişin ve aşağıdaki adımları izleyerek her bir güvenlik olayına ilişkin daha fazla ayrıntı görüntüleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-incident/security-center-incident-fig1.png)

2. Bu kutucuğa tıklayarak kutucuğu genişletin; bir güvenlik olayı algılanırsa aşağıda gösterildiği gibi güvenlik uyarıları grafiğinin altında görünür:

    ![Güvenlik olayı](./media/security-center-incident/security-center-incident-fig2.png)

3. Güvenlik olayı açıklamasının diğer uyarılardan farklı bir simgeye sahip olduğuna dikkat edin. Bu olay hakkında daha fazla ayrıntı görüntülemek için üzerine tıklayın.

    ![Güvenlik olayı](./media/security-center-incident/security-center-incident-fig3.png)

4. **Olay** dikey penceresinde, bu güvenlik olayının tam açıklaması, önem düzeyi (bu durumda yüksektir), olayın geçerli durumu (bu durumda hala *etkindir* ve kullanıcının uyarı için henüz bir eylem gerçekleştirmediğini gösterir; bu işlem, **Güvenlik uyarıları** dikey penceresinde olaya tıklanarak gerçekleştirilebilir), saldırılan kaynak (bu durumda *VM1*), ve düzeltme adımları dahil olmak üzere olayla ilgili daha fazla ayrıntıyı görürsünüz ve alt bölmede bu olaya dahil edilen uyarılar yer alır. Her uyarı hakkında daha fazla bilgi edinmek istiyorsanız, uyarıya tıklayarak aşağıda gösterildiği gibi başka bir dikey pencere açılmasını sağlayın:

    ![Güvenlik olayı](./media/security-center-incident/security-center-incident-fig4.png)

Bu dikey penceredeki bilgiler uyarıya göre farklılık gösterir. Bu uyarıların nasıl yönetileceği hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) konusunu okuyun. Bu özellik ile ilgili bazı önemli noktalar:

* Yeni bir filtre, görünümünüzü Yalnızca olay, Yalnızca uyarılar veya her ikisi için özelleştirmenizi sağlar.
* Aynı uyarı bir Olayın (varsa) parçası olabilir ve aynı zamanda tek başına uyarı olarak görünebilir.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Güvenlik Merkezi'nde güvenlik olayı özelliğini nasıl kullanacağınız hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz
