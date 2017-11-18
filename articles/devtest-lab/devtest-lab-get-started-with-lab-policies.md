---
title: "Azure DevTest Labs temel Laboratuvar ilkeleri yönetme | Microsoft Docs"
description: "DevTest Labs'de Laboratuvar için temel ilkeleri (ayarlar) bazıları ayarlamak öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: e87a37b7aafd774fb0176b74968ad0bba0f5cf3b
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda için temel ilkelerini yönetme

Azure DevTest Labs, maliyet denetlemek ve ilkeleri (ayarlar) her Laboratuvar için yöneterek, ortamlarındaki atık en aza indirmenize olanak sağlar. Bu makalede, iki en kritik ilkeleri ayarlamak öğrenme - sanal oluşturduğunuz ya da tek bir kullanıcı tarafından talep makineler (VM) sayısını sınırlama ve Otomatik kapatma yapılandırma ilkeleri ile çalışmaya başlayın. Her Laboratuvar ilkesinin nasıl ayarlanacağı görüntülemek için bkz: [Azure DevTest Labs'de Laboratuvar ilkeleri tanımlar](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Azure DevTest Labs Laboratuvar 's ilkelerinde erişme
Aşağıdaki adımları ilkeleri için Azure DevTest Labs laboratuvarında kurma yol:

(Görüntüleyip değiştirmek için) için bir laboratuvar ilkeleri, şu adımları izleyin:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **More services**’i (Daha fazla hizmet’i) seçip ardından listeden **DevTest Labs**’i seçin.

1. İstenen Laboratuvar labs listesinden seçin.   

1. Seçin **yapılandırma ve ilkeleri**.

    ![İlke ayarları bölmesi](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. **Yapılandırma ve ilkeleri** bölmesi belirleyebileceğiniz ayarlar menüsünü içerir. Bu makalede yalnızca ayarlarını kapsar **kullanıcı başına sanal makineler**, **otomatik kapatma**, ve **otomatik başlatma**. Kalan ayarları hakkında bilgi için bkz: [Azure DevTest Labs laboratuvarda yönelik tüm ilkeleri yönetmek](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına kümesi sanal makineler
İlke için **kullanıcı başına sanal makineler** ayrı bir kullanıcı tarafından oluşturulan VM maksimum sayısını belirtmenizi sağlar. Bir kullanıcı veya Kullanıcı sınırı sağlandığında VM talep oluşturmak üzere çalışırsa, oluşturulan ve istenen VM olamaz bir hata iletisi gösterir. 

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** menüsünde, select **kullanıcı başına sanal makineler**.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Seçin **Evet** kullanıcı başına VM'ler sayısını sınırlamak için. Kullanıcı başına VM'ler sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturduğunuz ya da bir kullanıcı tarafından talep VM'ler maksimum sayısını gösteren sayısal bir değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanabilirsiniz VM'lerin sayısını sınırlamak için. SSD kullanın, seçin VM'lerin sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM maksimum sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Set otomatik-kapatma
Bu Laboratuvar ait VM'ler kapatma süresi belirtmenize olanak tanıyarak Laboratuvar atık en aza indirmek için otomatik kapatma ilkesi yardımcı olur.

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **otomatik kapatma**.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı bırakmak için.

1. Bu ilkeyi etkinleştirirseniz, geçerli laboratuarda tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.

1. Belirtin **Evet** veya **Hayır** belirtilen otomatik kapatma saatten önce 15 dakikada bir bildirim gönderme seçeneği için. Seçerseniz **Evet**, bir Web kancası URL'si uç noktası girin veya e-posta adresi gönderilen veya gönderilmesi bildirim istediğiniz yeri belirtme. Kullanıcı bildirimi alır ve kapatma erteleme seçeneği verilir.

   Web kancası hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, bir kez etkinleştirildikten sonra geçerli laboratuarda tüm VM'ler için bu ilke uygulanır. Bu ayarı belirli bir sanal makineden kaldırmak için VM'ın yönetim bölmesini açın ve değiştirmek kendi **otomatik kapatma** ayarı.

## <a name="set-auto-start"></a>Otomatik başlangıcı Ayarla
Otomatik başlatma ilkesi geçerli laboratuvara sanal makineleri yeniden başlatıldığında belirtmenize olanak tanır.  

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **otomatik başlatma**.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı bırakmak için.

3. Bu ilkeyi etkinleştirirseniz, zamanlanmış başlangıç saati, saat dilimi ve saati geçerli olduğu için haftanın günlerini belirtin. 

4. **Kaydet**’i seçin.

Etkinleştirildikten sonra bu ilkenin geçerli laboratuarda herhangi bir VM için otomatik olarak uygulanmaz. Bu ayar için mevcut bir VM'yi uygulamak için VM Yönetimi bölmesini açın ve değiştirmek kendi **otomatik başlatma** ayarı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs'de Laboratuvar ilkeleri tanımlar](devtest-lab-set-lab-policy.md) -diğer Laboratuvar ilkeleri değiştirmek öğrenin.
