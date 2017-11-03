---
title: "Otomatik ölçeklendirme bir bulut hizmeti klasik portalda | Microsoft Docs"
description: "(Klasik) Azure'da bulut hizmeti web rolü veya çalışan rolü için otomatik ölçeklendirme kurallarını yapılandırmak için Klasik Portalı'nı kullanmayı öğrenin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a>Klasik Portalı'ndaki bir bulut hizmeti için ölçeklendirme otomatik yapılandırma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-how-to-scale-portal.md)
> * [Klasik Azure Portalı](cloud-services-how-to-scale.md)

Klasik Azure Portalı'nın ölçek sayfasında web rolü veya çalışan rolü için otomatik ölçeklendirme ayarlarını yapılandırabilirsiniz. Alternatif olarak, el ile yerine otomatik ölçeklendirmeyi kural tabanlı ölçeklendirme yapılandırabilirsiniz.

> [!NOTE]
> Bu makalede bulut hizmeti web ve çalışan rolleri odaklanır. Bir sanal makine (Klasik) doğrudan oluşturduğunuzda, bir bulut hizmetinde barındırılır. Bu bilgilerin bazıları, bu tür sanal makineler için geçerlidir. Sanal makinelerin bir kullanılabilirlik kümesi ölçeklendirme yalnızca bunları açma ve kapatma yapılandırdığınız ölçek kurallara göre kapatılıyor. Sanal makineler ve kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [sanal makinelerin kullanılabilirliğini yönetme](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Uygulamanız için ölçeklendirme yapılandırmadan önce aşağıdaki bilgileri dikkate alın:

* Ölçeklendirme Çekirdek kullanımını etkilenir.

    Daha fazla sayıda çekirdek büyük rol örneği kullanın. Aboneliğiniz için bir uygulama yalnızca çekirdek sınırının içinde ölçeklendirebilirsiniz. Örneğin, aboneliğiniz 20 olarak belirlenen çekirdek sınırına sahip söyleyin. Bir uygulamayı iki orta ölçekli bulut hizmetleriyle (4 çekirdeğe toplamı) çalıştırırsanız, kalan 16 çekirdek tarafından aboneliğinizde diğer bulut hizmet dağıtımları yalnızca ölçeklendirebilirsiniz. Boyutları hakkında daha fazla bilgi için bkz: [bulut hizmeti boyutları](cloud-services-sizes-specs.md).

* Bir kuyruk oluşturun ve bir ileti eşiğine dayalı bir uygulama ölçeklendirebilirsiniz önce bir rolü ile ilişkilendir. Daha fazla bilgi için bkz: [kuyruk depolama hizmetini kullanmayı](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Bu bulut hizmetinize bağlı kaynaklar ölçeklendirebilirsiniz. Kaynakları bağlama hakkında daha fazla bilgi için bkz: [nasıl yapılır: bir bulut hizmeti için bir kaynak bağlantı](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* Yüksek kullanılabilirlik, uygulamanızın etkinleştirmek için iki veya daha fazla rol örneği ile dağıtılan emin olmalısınız. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Zamanlama ölçeklendirme
Varsayılan olarak, tüm rolleri belirli bir zamanlama izlemeyin. Bu nedenle, değiştirilen tüm ayarlar tüm saatler ve tüm günler yıl geçerlidir. İsterseniz, aşağıdaki modlarından birini el ile veya otomatik ölçeklendirme ayarlayabilirsiniz:

* Haftanın günü
* Hafta sonları
* Hafta gece
* Hafta mornings
* Belirli tarihleri
* Belirli bir tarih aralığı

Zamanlama ayarı yapılandırılan [Klasik Azure portalı](https://manage.windowsazure.com/) üzerinde  
**Bulut Hizmetleri** > **\[bulut hizmetiniz\]** > **ölçek**  >   **\[Üretim ve hazırlama\]**  sayfası.

Tıklatın **zamanlama süreleri ayarlamanız** düğmesi değiştirmek istediğiniz her rol için.

![Bulut hizmeti bir zamanlamaya göre otomatik ölçeklendirme][scale_schedules]

## <a name="manual-scale"></a>El ile ölçek
Üzerinde **ölçek** sayfasında, el ile artırabilir veya örnekleri bir bulut hizmetinde çalışan sayısını azaltın. Bir zamanlama oluşturmadıysanız, bu ayar, oluşturduğunuz her zamanlama veya her zaman için yapılandırılır.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**ve ardından panoyu açmak için bulut hizmeti adını tıklatın.
   
   > [!TIP]
   > Bulut hizmetinizi görmüyorsanız, gelen değiştirmeniz gerekebilir **üretim** için **hazırlama** veya tam tersi.

2. Tıklatın **ölçek**.
3. Ölçeklendirme seçeneklerini değiştirmek için istediğiniz zamanlamayı seçin. *Hayır programlanan zamanlarda* tanımlı bir zamanlama varsa varsayılandır.
4. Bul **ölçek ölçüm tarafından** bölümünde ve seçin **NONE**. Tüm rolleri için varsayılan ayardır.
5. Her rol bulut hizmetinde kullanılacak örneklerinin sayısını değiştirmek için kaydırıcıyı sahiptir.
   
    ![Bir bulut hizmet rolü elle ölçeklendirme][manual_scale]
   
    Daha fazla örnekleri gerekiyorsa, değiştirmeniz gerekebilir [bulut hizmeti sanal makine boyutu](cloud-services-sizes-specs.md).
6. **Kaydet** düğmesine tıklayın.  
   Rol örnekleri eklenir veya seçimlerinizi tabanlı kaldırılır.

> [!TIP]
> Her gördüğünüz ![][tip_icon] için fareyi hareket ettirin ve hangi belirli bir ayarı yaptığı hakkında Yardım alabilirsiniz.

## <a name="automatic-scale---cpu"></a>Otomatik ölçek - CPU
Ortalama CPU kullanım yüzdesini üstünde veya altında belirtilen eşikler kalırsa Bu mod ölçeklendirir. Rol örnekleri oluşturulur veya böyle bir durumda silindi.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**ve ardından panoyu açmak için bulut hizmeti adını tıklatın.
   
   > [!TIP]
   > Bulut hizmetinizi görmüyorsanız, gelen değiştirmeniz gerekebilir **üretim** için **hazırlama** veya tam tersi.

2. Tıklatın **ölçek**.
3. Ölçeklendirme seçeneklerini değiştirmek için istediğiniz zamanlamayı seçin. *Hayır programlanan zamanlarda* tanımlı bir zamanlama varsa varsayılandır.
4. Bul **ölçek ölçüm tarafından** bölümünde ve seçin **CPU**.
5. Artık rol örnekleri, hedef CPU kullanımı (yukarı ölçek tetiklemek için) ve tarafından yukarı ve aşağı ölçeklendirmek için kaç tane minimum ve maksimum aralığını yapılandırabilirsiniz.

![Bir bulut hizmet rolü tarafından cpu yükünü ölçeklendirme][cpu_scale]

> [!TIP]
> Her gördüğünüz ![][tip_icon] için fareyi hareket ettirin ve hangi belirli bir ayarı yaptığı hakkında Yardım alabilirsiniz.

## <a name="automatic-scale---queue"></a>Otomatik ölçek - sırası
Bu mod, bir kuyruktaki ileti sayısı üzerinde veya belirtilen eşiğin altında kalırsa otomatik olarak ölçeklendirir. Rol örnekleri oluşturulur veya böyle bir durumda silindi.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**ve ardından panoyu açmak için bulut hizmeti adını tıklatın.
   
   > [!TIP]
   > Bulut hizmetinizi görmüyorsanız, gelen değiştirmeniz gerekebilir **üretim** için **hazırlama** veya tam tersi.

2. Tıklatın **ölçek**.
3. Bul **ölçek ölçüm tarafından** bölümünde ve seçin **SIRA**.
4. Artık rol örnekleri, kuyruk ve her bir örneği ve tarafından yukarı ve aşağı ölçeklendirmek için kaç tane örnekleri için işlemek için sıraya ileti sayısı minimum ve maksimum aralığını yapılandırabilirsiniz.

![İleti sırası tarafından bir bulut hizmet rolü ölçeklendirme][queue_scale]

> [!TIP]
> Her gördüğünüz ![][tip_icon] için fareyi hareket ettirin ve hangi belirli bir ayarı yaptığı hakkında Yardım alabilirsiniz.

## <a name="scale-linked-resources"></a>Bağlı kaynaklar ölçeklendirme
Genellikle bir rolü ölçeklendirdiğinizde uygulamayı da kullanarak veritabanı ölçeklendirmek faydalıdır. Bulut hizmeti için veritabanı bağlantı varsa, uygun bağlantıyı tıklatarak o kaynak için ölçeklendirme ayarlarını erişebilir.

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), tıklatın **bulut Hizmetleri**ve ardından panoyu açmak için bulut hizmeti adını tıklatın.
   
   > [!TIP]
   > Bulut hizmetinizi görmüyorsanız, gelen değiştirmeniz gerekebilir **üretim** için **hazırlama** veya tam tersi.

2. Tıklatın **ölçek**.
3. Bul **bağlı kaynakları** 'ye tıklayın **bu veritabanı için ölçek yönetmek**.
   
   > [!NOTE]
   > Görmüyorsanız, bir **bağlı kaynakları** bölümünde, büyük olasılıkla herhangi bağlı kaynaklar yok.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
