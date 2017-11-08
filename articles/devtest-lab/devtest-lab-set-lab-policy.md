---
title: "Azure DevTest Labs Laboratuvar ilkeleri yönetme | Microsoft Docs"
description: "Laboratuvar ilkeleri VM boyutları, kullanıcı ve kapatma Otomasyon başına en fazla VMs gibi tanımlar öğrenin."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 03cd09e37ff7dd0b7731eee19810ada7aed1a875
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda yönelik tüm ilkeleri yönetme

Azure DevTest Labs maliyet denetlemek ve atık, ortamlarındaki ilkeleri (ayarlar) yöneterek her Laboratuvar için en aza indirmenize olanak sağlar. Bu makalede her ilkesinin nasıl ayarlanacağı hakkında adım adım ayrıntılı olarak açıklanmaktadır.  

## <a name="set-allowed-virtual-machine-sizes"></a>Sanal makine boyutlarını izin kümesi
İzin verilen VM boyutları ayarlamak için ilke Laboratuvar atık hangi VM boyutları laboratuar ortamında izin verildiğini belirtmek sağlayarak en aza indirmek için yardımcı olur. Bu ilke etkinleştirilirse, yalnızca VM boyutları bu listeden VM'ler oluşturmak için kullanılabilir.

1. İçinde [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Laboratuvar seçin ve ardından **yapılandırma ve ilkeleri**.

    ![Laboratuvar ait yapılandırma ve ilkeleri erişim](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **sanal makine boyutları izin**.
   
    ![İzin verilen sanal makine boyutları](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı bırakmak için.

1. Bu ilkeyi etkinleştirirseniz, laboratuvarınızda oluşturulabilmesi için bir veya daha fazla VM boyutları seçin.

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-user"></a>Kullanıcı başına kümesi sanal makineler
İlke için **kullanıcı başına sanal makineler** ayrı bir kullanıcı tarafından oluşturulan VM'ler üst sınırını belirtmenize olanak sağlar. Bir kullanıcı veya Kullanıcı sınırı sağlandığında VM talep oluşturmak üzere çalışırsa, oluşturulan ve istenen VM olamaz bir hata iletisi gösterir. 

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **kullanıcı başına sanal makineler**.
   
    ![Kullanıcı başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Seçin **Evet** kullanıcı başına VM'ler sayısını sınırlamak için. Kullanıcı başına VM'ler sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturduğunuz ya da bir kullanıcı tarafından talep VM'ler maksimum sayısını gösteren sayısal bir değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanabilirsiniz VM'lerin sayısını sınırlamak için. SSD kullanın, seçin VM'lerin sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM maksimum sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-virtual-machines-per-lab"></a>Laboratuvar başına kümesi sanal makineler
İlke için **Laboratuvar başına sanal makine** geçerli Laboratuvar için oluşturulan VM maksimum sayısını belirtmenizi sağlar. Laboratuvar sınırı sağlandığında bir VM oluşturmak bir kullanıcı çalışırsa, VM oluşturulamıyor bir hata iletisi gösterir. 

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **Laboratuvar başına sanal makine**.
   
    ![Laboratuvar başına sanal makineler](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Seçin **Evet** Laboratuvar başına VM'ler sayısını sınırlamak için. Laboratuvar başına VM'ler sayısını sınırlamak istemiyorsanız seçin **Hayır**. Seçerseniz **Evet**, oluşturduğunuz ya da bir kullanıcı tarafından talep VM'ler maksimum sayısını gösteren sayısal bir değer girin. 

1. Seçin **Evet** SSD (katı hal disk) kullanabilirsiniz VM'lerin sayısını sınırlamak için. SSD kullanın, seçin VM'lerin sayısını sınırlamak istemiyorsanız **Hayır**. Seçerseniz **Evet**, SSD kullanılarak oluşturulan VM maksimum sayısını belirten bir değer girin. 

1. **Kaydet**’i seçin.

## <a name="set-auto-shutdown"></a>Set otomatik-kapatma
Otomatik kapatma ilke bu Laboratuvar ait VM'ler kapatma süresi belirtmenize izin vererek Laboratuvar atık en aza indirmenize yardımcı olur.

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **otomatik kapatma**.
   
    ![Otomatik kapatma](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı bırakmak için.

1. Bu ilkeyi etkinleştirirseniz, geçerli laboratuarda tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.

1. Belirtin **Evet** veya **Hayır** belirtilen otomatik kapatma saatten önce 15 dakikada bir bildirim gönderme seçeneği için. Belirtirseniz **Evet**, burada bildirim gönderilen gönderilen veya değiştirilecek için bir Web kancası URL'si uç noktası veya bir e-posta adresi girin. Web kancası hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. **Kaydet**’i seçin.

Varsayılan olarak, bir kez etkinleştirildikten sonra geçerli laboratuarda tüm VM'ler için bu ilke uygulanır. Bu ayarı belirli bir sanal makineden kaldırmak için VM'ın yönetim bölmesini açın ve değiştirmek kendi **otomatik kapatma** ayarı.

## <a name="set-auto-start"></a>Otomatik başlangıcı Ayarla
Otomatik başlatma ilkesi geçerli laboratuarda VM'ler başladığında belirtmenize olanak sağlar.  

1. Laboratuvar 's üzerinde **yapılandırma ve ilkeleri** bölmesinde, **otomatik başlatma**.
   
    ![Otomatik başlatma](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı bırakmak için.

3. Bu ilkeyi etkinleştirirseniz, zamanlanmış başlangıç saati, saat dilimi ve saati geçerli olduğu için haftanın günlerini belirtin. 

4. **Kaydet**’i seçin.

Etkinleştirildikten sonra bu ilkenin geçerli laboratuarda herhangi bir VM için otomatik olarak uygulanmaz. Bu ayar için belirli bir VM'yi uygulamak için VM Yönetimi bölmesini açın ve değiştirin, **otomatik başlatma** ayarı.

## <a name="set-expiration-date"></a>Sona erme tarihi ayarlama
Bir süre sonu ayarlayabilirsiniz ne zaman tarih, [VM oluşturma](devtest-lab-add-vm.md). İçinde **Gelişmiş ayarları**, üzerinde VM otomatik olarak silinir bir tarih belirtmek için takvim simgesini seçin. Varsayılan olarak, VM zaman geçerli olsun.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Tanımlanan ve çeşitli VM ilke ayarları, laboratuvarınız için uygulanan sonra daha sonra deneyin gereken bazı şeyler şunlardır:

* [Paylaşılan IP adreslerini anlamak](devtest-lab-shared-ip.md) -paylaşılan IP açıklar adresleri DevTest Labs'de laboratuvarınız sanal makineleri bağlanmak için gereken genel IP adresleri sayısını en aza indirmek için kullanılır.
* [Maliyet yönetimini yapılandırma](devtest-lab-configure-cost-management.md) -nasıl kullanılacağını anlatan **aylık tahmini maliyet eğilimi** grafiği  
  Geçerli ay görüntülemek için maliyet tarihi tahmini ayın son maliyet tahmini ve.
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - bir VM oluşturduğunuzda, özel bir görüntü veya bir Market görüntüsü olabilecek bir taban belirtin. Bu makalede, bir VHD dosyasından özel bir görüntü oluşturmak nasıl gösterilmektedir.
* [Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) - VMs Azure Market görüntülerini temel oluşturmayı Azure DevTest Labs destekler. Bu makalede, varsa, Azure Market görüntülerini olabilecek belirtmek verilmektedir VM'ler bir laboratuar ortamında oluşturulurken kullanılır.
* [Bir laboratuar ortamında bir VM oluşturma](devtest-lab-add-vm-with-artifacts.md) -temel bir görüntüden bir VM oluşturmak nasıl gösterir (ya da özel veya Market) ve VM yapıları çalışmak nasıl.

