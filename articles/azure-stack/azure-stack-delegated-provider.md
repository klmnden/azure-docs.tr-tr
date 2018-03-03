---
title: "Azure yığınında teklifleri için temsilci seçme | Microsoft Docs"
description: "Teklifleri oluşturma ve kullanıcılar için imzalama sorumlu diğer kişilerin put öğrenin."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 287bc04660664facbe99d2cb80ae6c92e41c4111
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın operatör olarak genellikle teklifleri oluşturma ve kullanıcıları imzalama sorumlu diğer kişilerin yerleştirmek istediğiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, satıcılar müşterileri oturum ve bunları sizin adınıza yönetmek isteyebilirsiniz. Ya da bir kuruluşta merkezi BT grubunun bir parçası değilseniz, kullanıcıları, müdahalesi olmadan oturum yan kuruluşlarının isteyebilirsiniz.

Temsilci seçme, bu görevleri erişmek ve doğrudan daha fazla kullanıcı yönetmek sağlayarak yardımcı olur. Aşağıdaki çizimde bir temsilci düzeyini gösterir, ancak Azure yığını birden çok düzeyi destekler. Temsilci sağlayıcıları (DPs) sırayla beş düzeye kadar diğer sağlayıcıları için temsilci seçebilirsiniz.

![Temsilci düzeyleri](media/azure-stack-delegated-provider/image1.png)

Azure yığın işleçleri oluşturulmasını sunar ve diğer kullanıcıların kullanıcılara temsilci işlevini kullanarak temsilci seçebilirsiniz.

## <a name="roles-and-steps-in-delegation"></a>Rolleri ve temsilci adımlar
Temsilci anlamak için üç rol söz konusu olduğunu aklınızda bulundurun:

* *Azure yığın işleci* Azure yığın altyapı yönetir, bir teklif şablonu oluşturur ve bunların kullanıcılara sunmak başkalarına atar.

* Temsilci Azure yığın işleçleri adlı *sağlayıcıları temsilci*. Diğer Azure Active Directory (Azure AD) kullanıcıları gibi diğer kuruluşlara ait.

* *Kullanıcıların* teklifleri için kaydolun ve sanal makineleri, veri saklama oluşturma, iş yüklerini yönetmek için kullanın ve benzeri.

Aşağıdaki grafikte gösterildiği gibi temsilci seçmeyi ayarlama ayarlamak için iki adımı vardır.

1. *Temsilci sağlayıcıları tanımlamak*. Bunun için bunları yalnızca abonelik hizmeti içeren bir plana göre bir teklif abone tarafından. Bu teklif için abone kullanıcılar bazı teklifleri genişletmek ve bunları kaydınızı kullanıcılar oturum özelliği de dahil olmak üzere Azure yığın işlecin özelliklerini alın.

2. *Bir teklif temsilci sağlayıcıya temsilci*. Bu teklif ne temsilci sağlayıcısının sunabileceği için bir şablon olarak çalışır. Temsilci sağlayıcısı teklif artık devam edebilir. Bunun için bir ad seçin (ancak, hizmet ve kotalar değiştirmeyin) ve müşterilere sunar.

![Temsilci sağlayıcısı oluşturma ve bunları kullanıcıları imzalamak etkinleştirme](media/azure-stack-delegated-provider/image2.png)

Temsilci sağlayıcıları olarak davranacak şekilde kullanıcıların ana sağlayıcı ile bir ilişkisi oluşturmanız gerekir. Diğer bir deyişle, bir abonelik oluşturmak ihtiyaç duyar. Bu senaryoda, bu abonelik mevcut teklifleri ana sağlayıcıyı adına sağa sahip olarak yetkilendirilmiş sağlayıcıları tanımlar.

Bu ilişki kurulduktan sonra Azure yığın işleci temsilci sağlayıcıya bir teklif devredebilirsiniz. Temsilci sağlayıcısı şimdi Teklif alın, yeniden adlandırın (ancak özünü değiştirmemeniz) yapabiliyor ve müşterilerine sunar.

Aşağıdaki bölümlerde, bir temsilci sağlayıcısı oluşturmak için bir teklif temsilci açıklar ve kullanıcılar için kaydolabilirsiniz olduğunu doğrulayın.

## <a name="set-up-roles"></a>Rolleri ayarlamanız

Ek Azure ihtiyacınız iş temsilci sağlayıcısındaki görmek için Azure yığın işleci hesabınızı yanı sıra AD hesaplar. Bu iki hesap yoksa, bunları oluşturun. Hesapları için herhangi bir Azure AD kullanıcı ait olabilir ve temsilci sağlayıcı ve kullanıcı olarak sonuna denir.

| **Rol** | **Kuruluş hakları** |
| --- | --- |
| Temsilci sağlayıcısı |Kullanıcı |
| Kullanıcı |Kullanıcı |

## <a name="identify-the-delegated-providers"></a>Temsilci sağlayıcıları tanımlayın
1. Bir Azure yığın operatör olarak oturum açın.

2. Kullanıcıların temsilci sağlayıcıları olmasını sağlayan teklif oluşturun:
   
   a.  [Bir plan oluşturmak](azure-stack-create-plan.md).
       Bu plan yalnızca abonelik hizmeti içermelidir. Bu makalede adlı bir planı kullanan **PlanForDelegation**.
   
   b.  [Bir teklif oluşturmak](azure-stack-create-offer.md) bu plana göre. Bu makalede adı verilen bir teklif kullanan **OfferToDP**.
   
   c.  Teklif oluşturma işlemi tamamlandıktan sonra temsilci sağlayıcısı bu teklif için bir abone olarak ekleyin. Seçerek bunu **abonelikleri** > **Ekle** > **yeni Kiracı aboneliği**.
   
   ![Bir abone olarak yetkilendirilmiş Sağlayıcısı Ekle](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> Tüm Azure yığın teklifleri ile yüklü olduğu genel ve olanak tanıyarak kullanıcı teklif yapma seçeneğini kaydolun, veya gizliliğini korumak ve kaydolma yönetmek Azure yığın işleci izin vermek için. Temsilci genellikle küçük bir grup sağlayıcılarıdır. Bu teklif gizliliğini korumak çoğu durumda mantıklı şekilde, kabul edilen kimin istiyorsunuz.
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Temsilci teklif Azure yığın işleci oluşturur

Temsilci sağlayıcınız şimdi kurduğunuz. Sonraki adım, plan ve temsilci seçmek için uygulayacağınız ve müşterileriniz kullanacağı teklif oluşturmaktır. Bu teklif temsilci sağlayıcısı planları ve içerdiği kotaları değiştiremediğinizden görmek için müşteriler tam olarak istediğiniz olarak tanımlamak için iyi bir fikirdir.

1. Bir Azure yığın işleç olarak [bir plan oluşturmak](azure-stack-create-plan.md) ve [bir teklif](azure-stack-create-offer.md) dayalı. Bu makalede adı verilen bir teklif kullanan **DelegatedOffer.**
   
   > [!NOTE]
   > Bu teklif ortak olmak zorunda değildir. Seçerseniz, ortak yapabilirsiniz. Çoğu durumda, ancak yalnızca erişmesi için temsilci sağlayıcıları istiyor. Aşağıdaki adımlarda açıklandığı gibi özel bir tekliftir temsilci sonra temsilci sağlayıcısı erişimi vardır.
   > 
   > 
1. Teklif temsilci. Git **DelegatedOffer.** Ardından **ayarları** bölmesinde, **temsilci sağlayıcıları** > **Ekle**.

2. Aşağı açılan liste kutusundan temsilci sağlayıcısı için aboneliği seçin ve ardından **temsilci**.

> ![Temsilci Sağlayıcısı Ekle](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Teklif temsilci sağlayıcısı özelleştirir

Kullanıcı Portalı temsilci sağlayıcısı olarak oturum açın ve sonra bir şablon olarak yetkilendirilmiş teklif kullanarak yeni bir teklif oluşturun.

1. Seçin **yeni** > **Kiracı sunar + planları** > **teklif**.

    ![Yeni bir teklif oluşturma](media/azure-stack-delegated-provider/image5.png)


1. Teklif için bir ad atayın. Bu makalede kullanan **ResellerOffer**. Temsilci teklif, temel ve ardından seçin **oluşturma**.
   
   ![Bir ad atayın](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Oluşturma Azure yığın işleci deneyimli olarak sunmak için karşılaştırıldığında fark unutmayın. Temsilci sağlayıcısı, temel plan ve eklenti planları tekliften oluşturmak değil. Kullanıcılar yalnızca kendilerine temsilci olarak nasıl atandığını ve bu teklifleri değişiklik yapamazsınız teklifleri arasından seçim yapabilirsiniz.

1. Teklif ortak işaretleyerek **Gözat**ve ardından **sunar**. Teklif seçin ve ardından **durum değiştirme**.

2. Temsilci sağlayıcı kendi portal üzerinden bu teklifleri sunar URL. Bu teklif yalnızca temsilci Portalı aracılığıyla görülebilir. Bul ve bu URL'yi değiştirmek için:
   
    a.  Seçin **Gözat** > **daha fazla hizmet** > **abonelikleri**. Ardından yetkilendirilmiş sağlayıcısı aboneliğini seçin. Örneğin, **DPSubscription** > **özellikleri**.
   
    b.  Portal kopyalama Not Defteri gibi ayrı bir konuma URL.
   
    ![Temsilci sağlayıcısı aboneliği seçin](media/azure-stack-delegated-provider/dpportaluri.png)  
   
   Atanmış bir teklif yetkilendirilmiş bir sağlayıcı oluşturdunuz. Temsilci sağlayıcısı olarak oturumu kapatın. Kullanmakta olduğunuz tarayıcı penceresini kapatın.

## <a name="sign-up-for-the-offer"></a>Teklif için kaydolun
1. Yeni bir tarayıcı penceresinde temsilci Portalı'na gidin önceki adımda kaydettiğiniz URL. Portal için bir kullanıcı olarak oturum açın. 
   
   >[!NOTE]
   >Temsilci portal kullanmadığınız sürece temsilci teklifleri görünür değildir. 

2. Panoda seçin **bir abonelik edinmeniz**. Temsilci sağlayıcı tarafından oluşturulan temsilci teklifleri kullanıcıya sunulan bakın:

> ![Görüntüleyin ve teklifleri seçin](media/azure-stack-delegated-provider/image8.png)
> 
> 

Teklif temsilci işlemi tamamlanır. Kullanıcı artık bu teklif için bir abonelik alarak kaydolabilir.

## <a name="multiple-tier-delegation"></a>Birden çok katmanlı temsilci seçme

Birden çok katmanlı temsilci teklif diğer varlıklar için temsilciye atanmış sağlayıcıyı etkinleştirir. Bu, örneğin, Azure yığın yönetme sağlayıcı dağıtıcı teklife temsilciler daha derin satıcı kanalları oluşturulmasını sağlar. Bu dağıtıcı sırasıyla bir satıcı atar. Azure yığını temsilci en fazla beş düzeylerini destekler.

Teklif temsilci birden fazla katman oluşturmak için temsilci sağlayıcısı sırayla sonraki sağlayıcısına teklif atar. Azure yığın işleci için olduğu gibi aynı temsilci sağlayıcısı işlemidir (bkz [Azure yığın işleci oluşturur temsilci teklif](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Sonraki adımlar
[Bir VM sağlama](azure-stack-provision-vm.md)

