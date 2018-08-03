---
title: Azure Stack'te teklifler için temsilci seçme | Microsoft Docs
description: Diğer kişilerin sorumlu teklifleri oluşturma ve kullanıcılar, oturum put öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 161a17f2360767dacc4f418a078c695d7cb8daa7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449753"
---
# <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack operatör olarak, genellikle diğer kişilerin sorumlu teklifleri oluşturma ve kullanıcıları imzalama yerleştirmek istediğiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, müşterileri oturum ve sizin adınıza yönetmek için Satıcılar isteyebilirsiniz. Veya merkezi bir BT grubu, bir kuruluşta bir parçası kullanıyorsanız, kullanıcı oturum kadar temsilci isteyebileceğiniz diğer BT personeli.

Temsilci erişmek ve kendi kendinize yapabileceğiniz daha fazla kullanıcı yönetmek kolaylaştırır. Temsilci tek düzey aşağıdaki çizimde, ancak Azure Stack birden fazla düzeyde destekler. Sağlayıcı temsilcisi (yu DPs), beş düzeyleri kadar diğer sağlayıcılar için yetkilendirme yapabilirsiniz.

![Temsilci düzeyleri](media/azure-stack-delegated-provider/image1.png)

## <a name="understand-delegation-roles-and-steps"></a>Temsilci rolleri ve adımlarını anlamak

### <a name="delegation-roles"></a>Temsilci rolleri

Aşağıdaki roller temsilci bir parçasıdır:

* *Azure Stack operatörü* Azure Stack altyapısının yönetir ve bir teklif şablon oluşturur. İşleci, diğerleri kendi kullanıcılara teklifler sunmak için atar.

* Temsil edilen Azure Stack operatörlerinin adlı *sağlayıcıları temsilci*. Bunlar, diğer Azure Active Directory (Azure AD) kullanıcıları gibi diğer kuruluşlara ait olabilir.

* *Kullanıcılar* teklifleri için kaydolun ve benzeri verileri depolamak, Vm'leri oluşturma, iş yüklerini yönetmek için kullanın.

### <a name="delegation-steps"></a>Temsilci adımları

Temsilci seçmeyi ayarlama ayarlamak için iki temel adım vardır:

1. *Bir sağlayıcı temsilcisi oluşturmak* tarafından bir kullanıcı yalnızca abonelikleri hizmetine sahip olan bir planına dayanarak bir teklife abone olma. Kullanıcılar bu teklife abone teklifler genişletmek ve teklifler için kullanıcı.

1. *Sağlayıcı temsilcisi için bir teklifi yetkilendirme*. Bu teklif, ne sağlayıcı temsilcisi sunmak için bir şablondur. Sağlayıcı temsilcisi artık teklif almak ve diğer kullanıcılara sunma.

Sonraki grafikte, temsilci seçmeyi ayarlama adımları gösterir.

![Sağlayıcı temsilcisi oluşturun ve bunları kullanıcılar oturum açmak etkinleştirme](media/azure-stack-delegated-provider/image2.png)

**Sağlayıcı temsilcisi gereksinimleri**

Sağlayıcı temsilcisi çalışmak için bir abonelik oluşturarak ana sağlayıcısı ile bir ilişki kurmak bir kullanıcının gerekir. Bu abonelik sağındaki mevcut teklifleri ana Sağlayıcı adına sahip sağlayıcı temsilcisi tanımlar.

Bu ilişki kurulduktan sonra Azure Stack operatörü bir teklif sağlayıcı temsilcisi için yetkilendirme yapabilirsiniz. Sağlayıcı temsilcisi teklif alır, yeniden adlandırın (ancak özünü değiştirilmemesi), müşterilerine sunar.

## <a name="delegation-walkthrough"></a>Temsilci gözden geçirme

Aşağıdaki bölümler, bir temsilci sağlayıcısını ayarlama, bir teklif için temsilci seçme ve kullanıcıların teklif temsilcisi için kaydolup olmadığını doğrulamak için pratik bir kılavuz sağlar.

### <a name="set-up-roles"></a>Rolleri ayarlama

Bu anlatımda kullanmak için iki Azure AD hesaplarının yanı sıra Azure Stack operatörü hesabınızı gerekir. Bu iki hesap yoksa, bunları oluşturmanız gerekir. Hesapları herhangi bir Azure AD kullanıcı ait olabilir ve sağlayıcı temsilcisi ve kullanıcı için sonuna denir.

| **Rol** | **Kuruluş hakları** |
| --- | --- |
| Sağlayıcı temsilcisi |Kullanıcı |
| Kullanıcı |Kullanıcı |

### <a name="identify-the-delegated-provider"></a>Sağlayıcı temsilcisi tanımlayın

1. Azure Stack operatörü Yönetici portalında oturum açın.

1. Bir kullanıcının bir sağlayıcı temsilcisi olmasını sağlayan bir teklifi oluşturmak için:

   a.  [Bir plan oluşturmanız](azure-stack-create-plan.md).
       Bu plan yalnızca abonelikleri hizmet içermelidir. Bu makalede adlı bir planı kullanan **PlanForDelegation** örnek olarak.

   b.  [Teklif oluşturma](azure-stack-create-offer.md) bu plana göre. Bu makalede adlı bir teklif kullanan **OfferToDP** örnek olarak.

   c.  Sağlayıcı temsilcisi, seçerek bu teklife abone olarak ekleyin **abonelikleri** > **Ekle** > **yeni Kiracı aboneliği**.

   ![Sağlayıcı temsilcisi bir abone Ekle](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Tüm Azure Stack'te teklifler sayesinde, sahip olduğunuz gibi genel ve olanak tanıyarak kullanıcılar teklife yapma seçeneğini kaydolun, gizliliğini korumak ve kaydolma yönetme Azure Stack operatörü izin vermek için. Sağlayıcı temsilcisi genellikle küçük bir grubudur. Bu teklif gizliliğini korumak çoğu durumda mantıklı şekilde kimlerin, kabul edilen istersiniz.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Teklif temsilcisi Azure Stack operatörü oluşturur

Sonraki adım, plan ve teklif temsilci olarak gideceğinizi ve kullanıcılarınızın kullanacağı oluşturmaktır. Sağlayıcı temsilcisi planları ve kotaları içerdiğinden değiştirilemiyor çünkü görmek için kullanıcılar tam olarak istediğiniz gibi bu teklif tanımlamak için iyi bir fikirdir.

1. Azure Stack operatör olarak [bir plan oluşturmanız](azure-stack-create-plan.md) ve [teklif](azure-stack-create-offer.md) planına dayanarak. Bu makalede adlı bir teklif kullanan **DelegatedOffer** örnek olarak.

   > [!NOTE]
   > Bu teklif genel olması gerekmez, ancak isterseniz, genel yapabilirsiniz. Ancak, çoğu zaman yalnızca teklif erişimi için yetkilendirilmiş sağlayıcıları istersiniz. Aşağıdaki adımlarda açıklandığı gibi özel bir teklif temsilci sonra sağlayıcı temsilcisi erişimi vardır.

1. Teklif temsilci. Git **DelegatedOffer**. Altında **ayarları**seçin **temsilci sağlayıcıları** > **Ekle**.

1. Sağlayıcı temsilcisi için abonelik aşağı açılan listeden seçin ve ardından **temsilci**.

   ![Bir sağlayıcı temsilcisi Ekle](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Sağlayıcı temsilcisi teklif özelleştirir

Sağlayıcı temsilcisi olarak kullanıcı portalında oturum açın ve ardından teklif temsilcisini şablon olarak kullanarak yeni bir teklif oluşturun.

1. Seçin **yeni** > **Kiracı sunar + planlar** > **teklif**.

    ![Yeni bir teklif oluşturun](media/azure-stack-delegated-provider/image5.png)

1. Teklif için bir ad atayın. Bu makalede **ResellerOffer** örnek olarak. Teklif temsilcisi, temel ve ardından bağlanacağı seçin **Oluştur**.

   ![Bir ad atayın](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Azure Stack operatörü, bir temel plan ve eklenti planları tekliften bir sağlayıcı temsilcisi oluşturmak değil anlamak önemlidir. Sağlayıcı temsilcisi yalnızca onlara yetki verilen teklifleri seçin, teklifler için değişiklik yapamaz.

1. Teklife genel işaretleyerek **Gözat**, ardından **sunar**. Teklif seçin ve ardından **durumunu değiştir**.

1. Sağlayıcı temsilcisi kendi portal üzerinden bu teklifler sunan URL'si. Bu teklifler yalnızca yetkilendirilmiş portal üzerinden görülebilir. Bulmak ve bu URL'yi değiştirmek için:

    a.  Seçin **Gözat** > **diğer hizmetler** > **abonelikleri**. Ardından, sağlayıcı temsilcisi aboneliğini seçin. Örneğin, **DPSubscription** > **özellikleri**.

    b.  Portaldaki kopyalayın, Not Defteri gibi ayrı bir konuma URL.

    ![Sağlayıcı temsilcisi aboneliğini seçin](media/azure-stack-delegated-provider/dpportaluri.png)  

   Teklif temsilcisi olarak yetkilendirilmiş bir sağlayıcı oluşturma işlemi tamamlandı. Sağlayıcı temsilcisi oturumu kapatın ve kullanmakta olduğunuz tarayıcı penceresini kapatın.

### <a name="sign-up-for-the-offer"></a>Teklif için kaydolun

1. Yeni bir tarayıcı penceresinde temsilci portala gidin önceki adımda kaydettiğiniz URL'si. Portalda bir kullanıcı olarak oturum açın.

   >[!NOTE]
   >Teklif temsilcisi temsilci portalını sürece görünür değildir.

1. Panoda seçin **bir abonelik edinmeniz**. Sağlayıcı temsilcisi tarafından oluşturulan yalnızca teklif temsilcisi kullanıcıya sunulan görürsünüz.

   ![Görüntüleme ve teklifleri seçin](media/azure-stack-delegated-provider/image8.png)

Bir teklif için temsilci seçme işlemi tamamlandı. Artık bir kullanıcı bu teklif için bir abonelik alarak kaydolabilirsiniz.

## <a name="multiple-tier-delegation"></a>Çok katmanlı temsilci seçme

Çok katmanlı temsilci diğer varlıklara teklif temsilci olarak yetkilendirilmiş bir sağlayıcı sağlar. Örneğin, daha derin satıcı oluşturmak için nereye kanallar:

* Azure Stack yöneten sağlayıcı bir dağıtıcı için bir teklif atar.
* Bir satıcı dağıtıcı temsilcileri.

Sağlayıcı temsilcisi birden fazla katman teklif temsilci oluşturmak için sonraki sağlayıcıya teklif atar. Azure Stack işlecin olduğu gibi aynı sağlayıcı temsilcisi için işlemidir. Daha fazla bilgi için [Azure Stack operatörü oluşturur teklif temsilcisi](#cloud-operator-creates-the-delegated-offer).

## <a name="next-steps"></a>Sonraki adımlar

[Bir VM sağlama](azure-stack-provision-vm.md)