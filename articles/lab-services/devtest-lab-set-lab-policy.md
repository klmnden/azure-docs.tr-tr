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
ms.openlocfilehash: aa0ffbd69e73ddbef72e0eabf79f2736079c3d23
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60636528"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara yönelik tüm ilkeleri yönetme

Maliyet denetlemek ve en aza indiren azure DevTest Labs sağlar, her bir laboratuvar ilkelerini (ayarlar) yöneterek, Labs'de boşa. Bu makalede her İlkesi ayarlama konusunda adım adım ayrıntılı olarak açıklanmaktadır.  

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
İlkeyi **kullanıcı başına sanal makine** bireysel bir kullanıcı tarafından oluşturulan VM'lerin sayısını belirtmenizi sağlar. Bir kullanıcı oluşturma veya bir sanal kullanıcı sınırı sağlandığında talep çalışırsa, bir VM oluşturulur ve talep olamaz bir hata iletisi gösterir. 

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **kullanıcı başına sanal makine**.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Seçin **Evet** kullanıcı başına VM'lerin sayısını sınırlamak için. Kullanıcı başına VM'lerin sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturulan veya bir kullanıcı tarafından istenen VM sayısını belirten bir sayısal değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanan VM'ler sayısını sınırlamak için. SSD kullanın, seçin VM sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM'lerin sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-lab"></a>Laboratuvar başına sanal makineler kümesi
İlkeyi **Laboratuvar başına sanal makine** geçerli Laboratuvar için oluşturulan VM'lerin sayısını belirtmenizi sağlar. Laboratuvar sınırına bir VM oluşturmak bir kullanıcı çalışırsa, VM oluşturulan bir hata iletisi gösterir. 

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **Laboratuvar başına sanal makine**.
   
    ![Laboratuvar başına sanal makine](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Seçin **Evet** Laboratuvar başına sanal makinelerin sayısını sınırlamak için. Laboratuvar başına sanal makinelerin sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturulan veya bir kullanıcı tarafından istenen VM sayısını belirten bir sayısal değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanan VM'ler sayısını sınırlamak için. SSD kullanın, seçin VM sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM'lerin sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Küme otomatik kapatma
Otomatik kapatma ilkesi bu Laboratuvar Vm'leri kapatmak süreyi belirtmenize imkan vererek Laboratuvar boşa harcamayı yardımcı olur.

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik kapatma**.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

1. Bu ilkeyi etkinleştirmek, geçerli bir laboratuvar içindeki tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.

1. Belirtin **Evet** veya **Hayır** seçeneği 15 dakika önce belirtilen otomatik kapatma saatinin bildirim göndermek için. Seçerseniz **Evet**, bir Web kancası URL uç noktası veya gönderilen veya gönderilmesi bildirim istediğiniz belirten bir e-posta adresi girin. Kullanıcı bildirimi alır ve kapatmayı ertelemeniz seçeneği de verilir.

   Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, geçerli Laboratuvardaki tüm sanal makineler için bu ilke etkinleştirildikten sonra uygulanır. Bu ayar, belirli bir sanal makineden kaldırmak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **otomatik kapatma** ayarı.

## <a name="set-auto-shutdown-policy"></a>Otomatik kapatma ilkesi ayarlama
Laboratuvar sahibi olarak, tüm VM'ler için laboratuvarda bir kapanma zamanlaması yapılandırabilirsiniz. Bunu yaptığınızda, kullanılmayan makineler çalışıyor (boşta öğesinden) maliyetlerinden tasarruf edebilirler. Laboratuvarınız sanal makineleri merkezi olarak aynı zamanda Kaydet Laboratuvar kullanıcılarınızın çaba, tek tek makineler için bir zamanlama ayarından tüm bir kapatma ilke uygulayabilir. Bu özellik, Laboratuvar kullanıcılarınıza tam denetim için bir denetim yok sunmasını başlangıç Laboratuvar zamanlamanızı ilkesi ayarlamanıza olanak sağlar. Laboratuvar sahibi olarak, aşağıdaki adımları izleyerek bu ilke yapılandırabilirsiniz:

1. Laboratuvarınız için giriş sayfasında, seçin **yapılandırması ve ilkelerini**.
2. Seçin **otomatik kapatma ilke** içinde **zamanlamaları** soldaki menüden bölümü.
3. Seçeneklerden birini seçin. Aşağıdaki bölümlerde, bu seçenekler hakkında daha fazla ayrıntı sağlar: İlkesi ayarlama, yalnızca laboratuvar'da oluşturulan yeni vm'lere ve zaten var olan VM'ler için geçerlidir. 

    ![Otomatik kapatma ilkesi seçenekleri](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve iptal edebilir
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları geçersiz kılabilir veya Laboratuvar zamanlama dışında iyileştirilmiş. Bu seçenek Laboratuvar kullanıcıları kendi sanal makinelerinin otomatik kapatma zamanlamasını üzerinde tam denetim verir. Laboratuvar kullanıcıları herhangi bir değişiklik, VM otomatik kapanma zamanlaması sayfasında bakın.

![Otomatik kapatma ilkesi seçeneği - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve çıkma olamaz
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları Labs zamanlaması geçersiz kılabilirsiniz. Ancak, bunlar otomatik kapatma ilke dışı bırakamazlar. Bu seçenek laboratuvarınızda her makineye bir otomatik kapatma zamanlamasını altında olduğundan emin olur. Laboratuvar kullanıcıları kendi sanal makinelerinin otomatik kapatma zamanlamasını güncelleştirme ve kapatma bildirimlerini ayarlama.

![Otomatik kapatma ilkesi seçeneği - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcı Laboratuvar Yöneticisi tarafından ayarladığı zamanlamayı üzerinde denetimi yoktur
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları geçersiz kılabilir veya Laboratuvar zamanlama dışında iyileştirilmiş. Bu seçenek, Laboratuvar Yönetim zamanlamaya Laboratuvardaki her makine için tam denetim sunar. Laboratuvar kullanıcıları yalnızca kendi sanal makineleri için otomatik kapatma bildirimler ayarlayabilirsiniz.

![Otomatik kapatma ilkesi seçeneği - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="set-autostart"></a>Küme otomatik başlatma
Autostart ilke geçerli Laboratuvar VM'lerin yeniden başlatıldığında belirtmenize olanak sağlar.  

1. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **Autostart**.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.

3. Bu ilkeyi etkinleştirmek, zamanlanmış başlangıç saati, saat dilimi ve saati geçerli olduğu için haftanın günlerini belirtin. 

4. **Kaydet**’i seçin.

Etkinleştirildikten sonra bu ilke geçerli Laboratuvardaki herhangi bir VM için otomatik olarak uygulanmaz. Bu ayarı belirli bir VM'ye uygulamak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **Autostart** ayarı.

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

