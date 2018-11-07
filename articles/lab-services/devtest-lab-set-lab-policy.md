---
title: Azure DevTest Labs'de Laboratuvar ilkelerini yönetme | Microsoft Docs
description: VM boyutları, kullanıcı ve kapatma Otomasyon başına en fazla VM gibi Laboratuvar ilkelerini tanımlamayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 378eb8c1f2070e8f4b28c221369938e2ff04e2f3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255191"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara yönelik tüm ilkeleri yönetme

Azure DevTest Labs, maliyet denetlemek ve laboratuvarlarınızı içinde boşa harcamayı için her bir laboratuvar ilkeleri (ayarlar) yöneterek sağlar. Bu makalede her İlkesi ayarlama konusunda adım adım ayrıntılı olarak açıklanmaktadır.  

## <a name="set-allowed-virtual-machine-sizes"></a>Sanal makine boyutları izin kümesi
Hangi VM boyutlarının laboratuvarda izin verildiğini belirtmek sağlayarak Laboratuvar israfı en aza indirmek için izin verilen VM boyutları ayarlama İlkesi yardımcı olur. Bu ilke etkinleştirilirse, yalnızca bu listedeki VM boyutları, sanal makineler oluşturmak için kullanılabilir.

1. İçinde [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin ve ardından **yapılandırması ve ilkelerini**.

    ![Laboratuvar yapılandırması ve ilkelerini erişim](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **sanal makine boyutları izin**.
   
    ![İzin verilen sanal makine boyutları](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

1. Bu ilkeyi etkinleştirmek, laboratuvarınızda oluşturulabilmesi için bir veya daha fazla VM boyutları seçin.

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına sanal makineler kümesi
İlkeyi **kullanıcı başına sanal makine** bireysel bir kullanıcı tarafından oluşturulan VM'ler en fazla sayısını belirtmenizi sağlar. Bir kullanıcı oluşturma veya Kullanıcı sınırı sağlandığında bir VM talep çalışırsa, bir VM oluşturulur ve talep olamaz bir hata iletisi gösterir. 

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **kullanıcı başına sanal makine**.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Seçin **Evet** kullanıcı başına VM'lerin sayısını sınırlamak için. Kullanıcı başına VM'lerin sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturulan veya bir kullanıcı tarafından talep VM'ler en fazla sayısını gösteren bir sayısal değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanan VM'ler sayısını sınırlamak için. SSD kullanın, seçin VM sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM'ler en fazla sayısını gösteren bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-lab"></a>Laboratuvar başına sanal makineler kümesi
İlkeyi **Laboratuvar başına sanal makine** geçerli Laboratuvar için oluşturulan VM'ler en fazla sayısını belirtmenizi sağlar. Laboratuvar sınırına bir VM oluşturmak bir kullanıcı çalışırsa, VM oluşturulan bir hata iletisi gösterir. 

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **Laboratuvar başına sanal makine**.
   
    ![Laboratuvar başına sanal makine](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Seçin **Evet** Laboratuvar başına sanal makinelerin sayısını sınırlamak için. Laboratuvar başına sanal makinelerin sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturulan veya bir kullanıcı tarafından talep VM'ler en fazla sayısını gösteren bir sayısal değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanan VM'ler sayısını sınırlamak için. SSD kullanın, seçin VM sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM'ler en fazla sayısını gösteren bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Küme otomatik kapatma
Otomatik kapatma ilkesi bu Laboratuvar Vm'leri kapatmak süreyi belirtmenize imkan vererek Laboratuvar boşa harcamayı yardımcı olur.

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik kapatma**.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

1. Bu ilkeyi etkinleştirmek, geçerli bir laboratuvar içindeki tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.

1. Belirtin **Evet** veya **Hayır** seçeneği 15 dakika önce belirtilen otomatik kapatma saatinin bildirim göndermek için. Seçerseniz **Evet**, bir Web kancası URL uç noktası veya gönderilen veya gönderilmesi bildirim istediğiniz belirten bir e-posta adresi girin. Kullanıcı bildirimi alır ve kapatma erteleme seçeneği verilir.

   Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, geçerli Laboratuvardaki tüm sanal makineler için bu ilke etkinleştirildikten sonra uygulanır. Bu ayar, belirli bir sanal makineden kaldırmak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **otomatik kapatma** ayarı.

## <a name="set-auto-start"></a>Otomatik başlangıcı Ayarla
Otomatik başlatma ilke geçerli Laboratuvardaki VM'ler başlatıldığında belirtmenize olanak sağlar.  

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik başlatma**.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

3. Bu ilkeyi etkinleştirmek, zamanlanmış başlangıç saati, saat dilimi ve saati geçerli olduğu için haftanın günlerini belirtin. 

4. **Kaydet**’i seçin.

Etkinleştirildikten sonra bu ilke geçerli Laboratuvardaki herhangi bir VM için otomatik olarak uygulanmaz. Bu ayarı belirli bir VM'ye uygulamak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **otomatik başlatma** ayarı.

## <a name="set-expiration-date"></a>Sona erme tarihini ayarlayın
Ayarlayabileceğiniz bir sona erme tarihi ne zaman, [VM oluşturma](devtest-lab-add-vm.md). İçinde **Gelişmiş ayarlar**, üzerinde VM otomatik olarak silinir bir tarih belirtmek için takvim simgesini seçin. Varsayılan olarak, sanal Makinenin her zaman geçerli olsun.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Tanımlanan ve laboratuvarınız için çeşitli VM ilke ayarlarının uygulandığı sonra sonraki denemek için bazı işlemler aşağıda verilmiştir:

* [Paylaşılan IP adreslerini anlama](devtest-lab-shared-ip.md) -açıklar nasıl paylaşılan IP adresleri DevTest Labs Laboratuvarınızı VM bağlanmak için gereken genel IP adresleri sayısını en aza indirmek için kullanılır.
* [Maliyet yönetimini yapılandırma](devtest-lab-configure-cost-management.md) -nasıl kullanılacağı gösterilmektedir **aylık tahmini maliyet eğilimi** grafiği  
  Geçerli ay görüntülemek için maliyet tarihi ve tahmini aylık son maliyet tahmini.
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, belirttiğiniz bir temel veya özel bir görüntü, hem de bir Market görüntüsü olabilir. Bu makalede, bir VHD dosyasından bir özel görüntü oluşturma işlemini göstermektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - Azure Market görüntüleri temel alan VM oluşturmayı Azure DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntüleri olabilecek belirtmek verilmektedir bir laboratuvarda VM oluşturulurken kullanılan.
* [Bir laboratuvarda VM oluşturma](devtest-lab-add-vm.md) -bir temel görüntüden VM oluşturma işlemini gösterir (ya da özel veya Market) ve vm'nizde yapıları ile çalışma.

