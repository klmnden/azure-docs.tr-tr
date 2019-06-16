---
title: Azure DevTest labs'deki temel Laboratuvar ilkelerini yönetme | Microsoft Docs
description: DevTest Labs'de bir laboratuvar için temel ilkeleri (ayarlar) bazıları ayarlamayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 8cc529fbf9b24335be1bec07f81c732ced7a2b72
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60773851"
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara yönelik temel ilkeleri yönetme

Azure DevTest Labs, maliyet denetlemek ve her bir laboratuvar ilkelerini (ayarlar) yöneterek, Labs boşa harcamayı sağlar. Bu makalede, iki en önemli ilkeleri ayarlamak öğrenme - oluşturulan veya tek bir kullanıcı tarafından istenen sanal makinelerin (VM) sayısının sınırlanması ve Otomatik kapatma yapılandırma ilkelerini kullanmaya başlama. Her laboratuar ilkesini ayarlama görüntülemek için bkz [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Azure DevTest Labs'de bir laboratuvar ilkelerini erişme
Aşağıdaki adımlar, Azure DevTest labs'deki bir laboratuvara yönelik ilkeleri ayarlama işleminde size kılavuzluk:

(Görüntüleyip değiştirmek için) bir laboratuvara yönelik ilkeleri, aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.   

1. Seçin **yapılandırması ve ilkelerini**.

    ![İlke ayarları bölmesi](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. **Yapılandırması ve ilkelerini** bölmesi belirtebileceğiniz ayarları içeren bir menü içerir. Bu makale yalnızca ayarlarını kapsar **kullanıcı başına sanal makine**, **otomatik kapatma**, ve **otomatik başlatma**. Kalan ayarlar hakkında bilgi edinmek için [Azure DevTest labs'deki bir laboratuvara yönelik tüm ilkeleri yönetme](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına sanal makineler kümesi
İlkeyi **kullanıcı başına sanal makine** bireysel bir kullanıcı tarafından oluşturulan VM'ler en fazla sayısını belirtmenizi sağlar. Bir kullanıcı oluşturma veya Kullanıcı sınırı sağlandığında bir VM talep çalışırsa, bir VM oluşturulur ve talep olamaz bir hata iletisi gösterir. 

1. Laboratuvar'ın **yapılandırması ve ilkelerini** menüsünde **kullanıcı başına sanal makine**.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Seçin **Evet** kullanıcı başına VM'lerin sayısını sınırlamak için. Kullanıcı başına VM'lerin sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturulan veya bir kullanıcı tarafından talep VM'ler en fazla sayısını gösteren bir sayısal değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanan VM'ler sayısını sınırlamak için. SSD kullanın, seçin VM sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM'ler en fazla sayısını gösteren bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Küme otomatik kapatma
Bu Laboratuvar Vm'leri kapatmak süreyi belirtmenize olanak tanıyarak Laboratuvar israfı en aza indirmek için otomatik kapatmayı ilke yardımcı olur.

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik kapatma**.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

1. Bu ilkeyi etkinleştirmek, geçerli bir laboratuvar içindeki tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.

1. Belirtin **Evet** veya **Hayır** seçeneği 15 dakika önce belirtilen otomatik kapatma saatinin bildirim göndermek için. Seçerseniz **Evet**, bir Web kancası URL uç noktasını girin veya e-posta adresi, gönderilen veya gönderilmesi bildirim istediğiniz belirtme. Kullanıcı bildirimi alır ve kapatma erteleme seçeneği verilir.

   Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, geçerli Laboratuvardaki tüm sanal makineler için bu ilke etkinleştirildikten sonra uygulanır. Bu ayar, belirli bir sanal makineden kaldırmak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **otomatik kapatma** ayarı.

## <a name="set-auto-start"></a>Otomatik başlangıcı Ayarla
Otomatik başlatma ilke geçerli Laboratuvar VM'lerin yeniden başlatıldığında belirtmenize olanak sağlar.  

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik başlatma**.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

3. Bu ilkeyi etkinleştirmek, zamanlanmış başlangıç saati, saat dilimi ve saati geçerli olduğu için haftanın günlerini belirtin. 

4. **Kaydet**’i seçin.

Etkinleştirildikten sonra bu ilke geçerli Laboratuvardaki herhangi bir VM için otomatik olarak uygulanmaz. Bu ayar, mevcut bir VM'ye uygulamak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **otomatik başlatma** ayarı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) -diğer Laboratuvar ilkelerini değiştirme hakkında bilgi edinin.
