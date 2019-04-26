---
title: Akıllı gruplar
description: Akıllı toplamalar yardımcı uyarı uyarı gürültüsünü azaltmak gruplardır.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: e0bef0fc4f4b61add24c243af0dac64933ad5bab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60346335"
---
# <a name="smart-groups"></a>Akıllı gruplar
Uyarılar uğraşmanızı bulmak için gerçekten önemli şeylere - akıllı grupları bu sorunun çözümü olması amaçlanır gürültü aracılığıyla karıştırmanız genişlettiklerinde karşılaştığı yaygın bir sınama.  

Akıllı grupları, tek bir sorunu temsil eden ilgili uyarıları birleştirmek için makine öğrenme algoritmalarını kullanarak otomatik olarak oluşturulur.  Bir uyarı oluşturulduğunda, algoritma yeni bir akıllı grubu veya benzer yapıya geçmiş desenleri ve benzer özellikleri gibi bilgileri temel alarak varolan akıllı grubuna ekler. Örneğin, birçok ayrı uyarıları için önde gelen ve bu tür uyarılar geçmişte Bu uyarılar büyük olasılıkla tek bir akıllı gruba gruplandırılacak zaman birlikte oluştuysa önerme aynı anda birkaç sanal makineye bir abonelikte % CPU ani bir ortak kök neden olabilir. Birisi için sorun giderme uyarıları, akıllı grupları yalnızca izin verdiğini ilgili uyarıları tek bir toplu birim olarak yöneterek gürültüsünü azaltmak için bunları, bunun anlamı, ayrıca bunları uyarılarını için ortak kök nedenlerini olası doğrultusunda size yol gösterir.

Şu anda algoritması yalnızca bir abonelik içindeki aynı İzleyici hizmeti uyarıları dikkate alır. Akıllı grupları aracılığıyla bu birleştirme uyarı gürültüsünü en fazla %99 azaltabilir. Uyarılar, akıllı Grup Ayrıntıları sayfası grubunda bulunan nedeni görüntüleyebilirsiniz.

Akıllı grupları ayrıntılarını görüntülemek ve Uyarılar için nasıl durumu benzer şekilde ayarlayın. Her uyarı, bir ve yalnızca bir akıllı grubunun bir üyesidir. 

## <a name="smart-group-state"></a>Akıllı grubu durumu
Akıllı Grup durumu, uyarı durumuna akıllı bir grup düzeyinde çözümleme sürecini yönetmenize olanak tanıyan benzer bir kavramdır. Benzer şekilde akıllı bir grup oluşturulduğunda uyarı durumu, sahip **yeni** ya da değiştirilebilir durumu **onaylanan** veya **kapalı**.

Aşağıdaki akıllı Grup durumları desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yalnızca algıladı ve henüz gözden. |
| Onaylanan | Bir yönetici, akıllı Grup inceleme ve üzerinde çalışmaya başladı. |
| Kapatıldı | Sorun çözüldü. Akıllı bir grup kapatıldıktan sonra başka bir duruma değiştirerek yeniden açabilirsiniz. |

[Akıllı grubunuzun durumunu değiştirmeyi öğrenin.](https://aka.ms/managing-alert-smart-group-states)

> [!NOTE]
>  Akıllı bir grubu durumunu değiştirmeyi üyesine uyarıları durumunu değiştirmez.

## <a name="smart-group-details-page"></a>Akıllı Grup Ayrıntıları sayfası

Akıllı bir grubu seçtiğinizde akıllı Grup ayrıntı sayfası görüntülenir. Bu grubu oluşturmak için kullanılan ve durumuna değiştirmenize olanak tanır mantık dahil olmak üzere grubun akıllı hakkında ayrıntılar sağlar.
 
![Akıllı grubu ayrıntısı](media/alerts-smartgroups-overview/smart-group-detail.png)


Akıllı Grup ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Uyarılar | Akıllı gruba dahil bireysel uyarıları listeler. Kendi uyarı ayrıntısı sayfasını açmak için bir uyarı seçin. |
| Geçmiş | Akıllı grup için yapılan tüm değişiklikler tarafından gerçekleştirilen her eylemi listeler. Durum değişikliklerini ve uyarı üyelik değişiklikleri şu anda sınırlı budur. |

## <a name="smart-group-taxonomy"></a>Akıllı grubu sınıflandırma

Akıllı bir grubun adı, ilk uyarı adıdır. Oluşturamaz veya akıllı bir grubu yeniden adlandırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı gruplarını yönetme](https://aka.ms/managing-smart-groups)
- [Uyarı ve akıllı Grup durum değişikliği](https://aka.ms/managing-alert-smart-group-states)

