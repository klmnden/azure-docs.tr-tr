---
title: Azure için Avere vFXT için destek alma
description: Azure için destek biletlerini Avere vFXT hakkında açılış açıklaması
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: d621511cbb6983f8ad57ea8305823039475f40d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409685"
---
# <a name="get-help-with-your-system"></a>Sisteminiz ile ilgili yardım alın

İle Avere vFXT yönelik yardım gerekiyorsa, destek almak için çeşitli yollar şunlardır:

* **Avere vFXT sorunu** -açıklandığı gibi Avere vFXT için bir destek bileti açmak için Azure portal'ı kullanmanızı [aşağıda](#open-a-support-ticket-for-your-avere-vfxt).
* **Kota** - kota ilgili bir sorun varsa [bir kota artırım talebinde](#request-a-quota-increase)
* **Belgeler ve örnekler** - bu belgeleri veya örnek, bir sorun bulursanız kullanın ve sorun sayfasının en altına gidin **geri bildirim** mevcut sorunları aramak ve yeni bir tek Eğer dosya bölümü gerekli.  

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Avere vFXT için bir destek bileti açın

Dağıtma veya Avere vFXT kullanarak sırasında sorunlarla karşılaşırsanız, Azure Portalı aracılığıyla yardım isteyin.  

Destek biletiniz kümenizden kaynak ile etiketlendiğinden emin olmak için aşağıdaki adımları izleyin. Bilet etiketleme desteği doğru kaynağa yönlendirmek yardımcı olur. 

1. Gelen [ https://portal.azure.com ](https://portal.azure.com)seçin **kaynak grupları**.

   !["kaynak grupları daire içinde" ile Azure portalında sol menüsünün ekran görüntüsü](media/avere-vfxt-ticket-rg.png)

1. Sorunun oluştuğu vFXT kümesi içeren kaynak grubunu bulun ve Avere sanal makinelerden birini tıklatın.

    ![daire içinde belirli bir VM ile Azure portalında kaynak grubu "genel bakış" bölmesinin ekran görüntüsü](media/avere-vfxt-ticket-vm.png)

1. VM sayfasında, sol bölmenin altındaki aşağı kaydırın ve tıklayın **yeni destek isteği**.

    ![Önceki ekran görüntüsünde gösterilen sanal makine için Azure portal VM sayfasının ekran görüntüsü. Soldaki menünün alt ve "Yeni destek isteği" kaydırılan daire içine alınmıştır.](media/avere-vfxt-ticket-request.png)

1. Sayfasında destek isteğini birini tıklatın **tüm hizmetleri** altına bakın **depolama** seçmek için **Avere vFXT**.

    ![Ekran başlığı "Temel" ve "Hizmet" öğesini bir daire ile Azure portalında yeni destek isteği ekran görüntüsü. "Tüm hizmetler" düğmesi seçilir ve açılan listeden alan "Avere vFXT" değere sahip](media/avere-vfxt-ticket-service.png)

1. İki sayfada, sorununuzu en yakından eşleşen kategorisi ve sorun türünü seçin. Bir kısa bir başlık ve sorunun gerçekleştiği zamana içeren bir açıklama ekleyin. 

   ![üstbilgiyle tamamlanması gereken fazla alan içeriyor "sorun" yeni destek isteği ekranının ekran görüntüsü](media/avere-vfxt-ticket-problem.png)

1. Üzerinde üç sayfasında, iletişim bilgilerinizi doldurun ve tıklayın **Oluştur**. Bir onay ve bilet numarası, e-posta adresinize gönderilir ve destek personelinin üyesi sizinle iletişim kuracağız.

## <a name="request-a-quota-increase"></a>Bir kota artırım talebinde

Okuma [vFXT küme için kota](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) hangi bileşenlerin Avere vFXT Azure'a dağıtmak için gerekli olan öğrenin. Yapabilecekleriniz [bir kota artırım talebinde](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) Azure portalından.