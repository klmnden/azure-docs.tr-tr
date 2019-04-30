---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: e128f3c67a41322d9c25a8d6941e937729760bf4
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097702"
---
Bu adımda, yük dengeli uç nokta (daha önce belirtildiği gibi 59999) araştırma bağlantı noktasını açmak için bir güvenlik duvarı kuralı oluşturun ve kullanılabilirlik grubu dinleyicisinin bağlantı noktası açmak için başka bir kural. Kullanılabilirlik grubu çoğaltmalarının içeren sanal makinelere yük dengeli uç nokta oluşturduğundan, yoklama bağlantı noktası ve ilgili vm'lerde dinleyici bağlantı noktasını açmanız gerekir.

1. Çoğaltmaları barındıran sanal makineler üzerinde Başlat **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.

2. Sağ **gelen kuralları**ve ardından **yeni kural**.

3. Üzerinde **kural türü** sayfasında **bağlantı noktası**ve ardından **sonraki**.

4. Üzerinde **protokol ve bağlantı noktaları** sayfasında **TCP**, türü **59999** içinde **belirli yerel bağlantı noktaları** kutusuna ve ardından  **Sonraki**.

5. Üzerinde **eylem** sayfasında, korumak **bağlantıya izin** seçili ve ardından **sonraki**.

6. Üzerinde **profili** sayfasında varsayılan ayarları kabul edin ve ardından **sonraki**.

7. Üzerinde **adı** sayfasında **adı** metin kutusunda, bir kural adı gibi belirtin **her zaman dinleyicisi yoklama bağlantı noktası**ve ardından **son**.

8. Kullanılabilirlik grubu dinleyicisinin bağlantı noktası (daha önce betiğin $EndpointPort parametresinde belirtildiği şekilde) için önceki adımları yineleyin ve ardından bir uygun kural adı gibi belirtin **her zaman dinleyicisi bağlantı noktası**.

<!-- Update_Description: update meta properties -->