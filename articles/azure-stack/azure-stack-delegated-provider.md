---
title: Azure Stack'te teklifler için temsilci seçme | Microsoft Docs
description: Diğer kişilerin sorumlu teklifleri oluşturma ve kullanıcılar, oturum put öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: a075fdceaf57625280ef73499d868d5c3724426c
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236490"
---
# <a name="delegate-offers-in-azure-stack"></a>Azure Stack’te teklifleri yetkilendirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack operatör olarak, genellikle diğer kişilerin sorumlu kullanıcılar imzalama ve abonelik oluşturma yerleştirmek istediğiniz. Örneğin, bir hizmet sağlayıcı barındırıyorsanız, müşterileri oturum ve sizin adınıza yönetmek için Satıcılar isteyebilirsiniz. Veya, bir kuruluşta merkezi BT grubunun bir parçası kullanıyorsanız, kullanıcı kaydı diğer BT personeli için temsilci seçmek isteyebilirsiniz.

Temsilci erişmek ve aşağıdaki resimde gösterildiği gibi kendiniz tarafından yapabileceğinden daha fazla kullanıcı yönetmek daha kolay hale getirir:

![Temsilci düzeyleri](media/azure-stack-delegated-provider/image1.png)

Temsilci seçme, bir teklif (teklif temsilcisi) sağlayıcı temsilcisi yönetir ve son müşterilere abonelikleri katılımı olmadan bu teklif altındaki Sistem Yöneticisi'nden alın.

## <a name="understand-delegation-roles-and-steps"></a>Temsilci rolleri ve adımlarını anlamak

### <a name="delegation-roles"></a>Temsilci rolleri

Aşağıdaki roller temsilci bir parçasıdır:

* *Azure Stack operatörü* Azure Stack altyapısının yönetir ve bir teklif şablon oluşturur. İşleci, diğerleri kendi Kiracı tekliflerini sağlamak atar.

* Kullanıcılardır ile temsil edilen Azure Stack operatörlerinin *sahibi* veya *katkıda bulunan* Aboneliklerde hakları adlı *sağlayıcıları temsilci*. Bunlar gibi diğer Azure Active Directory (Azure AD) kiracıların diğer kuruluşlara ait olabilir.

* *Kullanıcılar* teklifleri için kaydolun ve benzeri verileri depolamak, Vm'leri oluşturma, iş yüklerini yönetmek için kullanın.

### <a name="delegation-steps"></a>Temsilci adımları

Temsilci seçmeyi ayarlama ayarlamak için iki temel adım vardır:

1. **Sağlayıcı temsilcisi aboneliğini oluşturmak**: Bir kullanıcı yalnızca abonelikleri hizmeti içeren bir teklif için abone olun. Bu teklife abone kullanıcılar ardından teklif temsilcisi diğer kullanıcılara teklifler için açarak genişletebilirsiniz.

2. **Sağlayıcı temsilcisi için bir teklifi yetkilendirme**: Bu teklif abonelikleri oluşturma veya kullanıcıları için teklif genişletmek için sağlayıcı temsilcisi sağlar. Sağlayıcı temsilcisi artık teklif almak ve diğer kullanıcılara sunma.

Temsilci seçmeyi ayarlama adımları aşağıdaki şekilde gösterilmiştir:

![Sağlayıcı temsilcisi oluşturun ve bunları kullanıcılar oturum açmak etkinleştirme](media/azure-stack-delegated-provider/image2.png)

#### <a name="delegated-provider-requirements"></a>Sağlayıcı temsilcisi gereksinimleri

Sağlayıcı temsilcisi çalışmak için bir kullanıcı bir abonelik oluşturarak bir ilişki ana sağlayıcıyla kurar. Bu abonelikte teklif temsilcisi ana Sağlayıcı adına sunmak hakkı sahip sağlayıcı temsilcisi tanımlar.

Bu ilişki kurulduktan sonra Azure Stack operatörü bir teklif sağlayıcı temsilcisi için yetkilendirme yapabilirsiniz. Sağlayıcı temsilcisi teklif alır, yeniden adlandırın (ancak özünü değiştirilmemesi), müşterilerine sunar.

## <a name="delegation-walkthrough"></a>Temsilci gözden geçirme

Aşağıdaki bölümler, bir temsilci sağlayıcısını ayarlama, bir teklif için temsilci seçme ve kullanıcıların teklif temsilcisi için kaydolup olmadığını doğrulamak için pratik bir kılavuz sağlar.

### <a name="set-up-roles"></a>Rolleri ayarlama

Bu anlatımda kullanmak için iki Azure AD hesaplarının yanı sıra Azure Stack operatörü hesabınızı gerekir. Bu iki hesap yoksa, bunları oluşturmanız gerekir. Hesapları herhangi bir Azure AD kullanıcı ait olabilir ve sağlayıcı temsilcisi ve kullanıcı için sonuna denir.

| **Rol** | **Kuruluş hakları** |
| --- | --- |
| Sağlayıcı temsilcisi |Kullanıcı |
| Kullanıcı |Kullanıcı |

 > [!NOTE]
   > Bir CSP satıcısı söz konusu olduğunda bu sağlayıcı temsilcisi oluşturmak için bu kullanıcılar Kiracı-Directory (AAD kullanıcı) gerektirir. Azure Stack operatörü gerekecektir [yerleşik ilk](https://docs.microsoft.com/azure/azure-stack/azure-stack-enable-multitenancy) izleyerek kullanım ve faturalandırma yapılandırın ve AAD Kiracı [adımları](https://docs.microsoft.com/azure/azure-stack/azure-stack-csp-howto-register-tenants).

### <a name="identify-the-delegated-provider"></a>Sağlayıcı temsilcisi tanımlayın

1. Azure Stack operatörü Yönetici portalında oturum açın.

1. Bir kullanıcının bir sağlayıcı temsilcisi olmasını sağlayan bir teklifi oluşturmak için:

   a.  [Bir plan oluşturmanız](azure-stack-create-plan.md).
       Bu plan, yalnızca abonelik hizmeti içermelidir. Bu makalede adlı bir planı kullanan **PlanForDelegation** örnek olarak.

   b.  [Teklif oluşturma](azure-stack-create-offer.md) bu plana göre. Bu makalede adlı bir teklif kullanan **OfferToDP** örnek olarak.

   c.  Sağlayıcı temsilcisi, seçerek bu teklife abone olarak ekleyin **abonelikleri**, ardından **Ekle**, ardından **yeni Kiracı aboneliği**.

   ![Sağlayıcı temsilcisi bir abone Ekle](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Tüm Azure Stack'te teklifler sayesinde, sahip olduğunuz gibi genel ve olanak tanıyarak kullanıcılar teklife yapma seçeneğini kaydolun, gizliliğini korumak ve kaydolma yönetme Azure Stack operatörü izin vermek için. Sağlayıcı temsilcisi genellikle küçük bir grubudur. Bu teklif gizliliğini korumak çoğu durumda mantıklı şekilde kimlerin, kabul edilen istersiniz.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Teklif temsilcisi Azure Stack operatörü oluşturur

Sonraki adım, plan ve teklif temsilci olarak gideceğinizi ve kullanıcılarınızın kullanacağı oluşturmaktır. Sağlayıcı temsilcisi planları ve kotaları içerdiğinden değiştirilemiyor çünkü tam olarak, kullanıcıların istediğiniz şekilde bu teklif tanımlamak için iyi bir fikirdir.

1. Azure Stack operatör olarak [bir plan oluşturmanız](azure-stack-create-plan.md) ve [teklif](azure-stack-create-offer.md) planına dayanarak. Bu makalede adlı bir teklif kullanan **DelegatedOffer** örnek olarak.

   > [!NOTE]
   > Bu teklif, ortak olmak zorunda değildir, ancak genel yapabilirsiniz. Ancak, çoğu zaman yalnızca teklif erişimi için yetkilendirilmiş sağlayıcıları istersiniz. Aşağıdaki adımlarda açıklandığı gibi özel bir teklif temsilci sonra sağlayıcı temsilcisi erişimi vardır.

2. Teklif temsilci. Git **DelegatedOffer**. Altında **ayarları**seçin **temsilci sağlayıcıları**, ardından **Ekle**.

3. Sağlayıcı temsilcisi için abonelik aşağı açılan listeden seçin ve ardından **temsilci**.

   ![Bir sağlayıcı temsilcisi Ekle](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Sağlayıcı temsilcisi teklif özelleştirir

Sağlayıcı temsilcisi olarak kullanıcı portalında oturum açın ve ardından teklif temsilcisini şablon olarak kullanarak yeni bir teklif oluşturun.

1. Seçin **+ kaynak Oluştur**, ardından **Kiracı sunar + planlar**, ardından **teklif**.

    ![Yeni bir teklif oluşturun](media/azure-stack-delegated-provider/image5.png)

2. Teklif için bir ad atayın. Bu örnekte **ResellerOffer**. Teklif temsilcisi, temel ve ardından bağlanacağı seçin **Oluştur**.

   ![Bir ad atayın](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Sağlayıcı temsilcisi onlara yetki verilen teklifler yalnızca seçebilirsiniz anlamak önemlidir. Bunlar, bu tekliflerin içerdiği değişiklik yapamazsınız. Yalnızca Azure Stack operatörü Bu teklif, örneğin, kendi planları ve kotaları değiştirerek değiştirebilirsiniz. Bir sağlayıcı temsilcisi, bir temel plan ve eklenti planları tekliften oluşturmak değil.

3. Sağlayıcı temsilcisi bu teklifleri kendi portal aracılığıyla genel erişime açmadan URL'si. Teklife genel hale getirmek üzere seçin **Gözat**, ardından **sunar**. Teklif seçin ve ardından **durumunu değiştir**.

4. Teklif temsilcisi genel artık yalnızca yetkilendirilmiş portal üzerinden görülebilir. Bulmak ve bu URL'yi değiştirmek için:

    a.  Seçin **Gözat**, ardından **tüm hizmetleri**ve ardından altındaki **genel** kategorisi seçin **abonelikleri**. Sağlayıcı temsilcisi aboneliğini seçin. Örneğin, **DPSubscription**, ardından **özellikleri**.

    b.  Portaldaki kopyalayın, Not Defteri gibi ayrı bir konuma URL.

    ![Sağlayıcı temsilcisi aboneliğini seçin](media/azure-stack-delegated-provider/dpportaluri.png)  

   Teklif temsilcisi olarak yetkilendirilmiş bir sağlayıcı oluşturma işlemi tamamlandı. Sağlayıcı temsilcisi oturumu kapatın ve tarayıcı penceresini kapatın.

### <a name="sign-up-for-the-offer"></a>Teklif için kaydolun

1. Yeni bir tarayıcı penceresinde temsilci portala gidin önceki adımda kaydettiğiniz URL'si. Portalda bir kullanıcı olarak oturum açın.

   >[!NOTE]
   >Teklif temsilcisi temsilci portalını sürece görünür değildir.

1. Panoda seçin **bir abonelik edinmeniz**. Sağlayıcı temsilcisi tarafından oluşturulan yalnızca teklif temsilcisi kullanıcıya sunulan görürsünüz.

   ![Görüntüleme ve teklifleri seçin](media/azure-stack-delegated-provider/image8.png)

Bir teklif için temsilci seçme işlemi tamamlandı. Artık bir kullanıcı bu teklif için bir abonelik alarak kaydolabilirsiniz.

## <a name="move-subscriptions-between-delegated-providers"></a>Sağlayıcı temsilcisi arasında taşıma abonelikler

Gerekirse, bir abonelik aynı dizine kiracısına ait sağlayıcı temsilcisi, yeni veya mevcut abonelikler arasında taşınabilir. Bu PowerShell cmdlet'i kullanılarak gerçekleştirilir [taşıma AzsSubscription](/powershell/module/azs.subscriptions.admin).

Bu durumlarda yararlı olur:

* Yerleşik sağlayıcı temsilcisi rolü ve, yeni bir takım üyesi için varsayılan sağlayıcı abonelikte önceden oluşturulmuş bu takım üyesi kullanıcı abonelikleri atamak istediğiniz.
* Aynı dizin kiracısında (Azure Active Directory) birden çok sağlayıcı temsilcisi aboneliğiniz varsa ve bunlar arasında kullanıcı aboneliklerini taşımanız gerekir. Bu senaryo, burada bir takım üyesi takımlar arasında taşır ve aboneliğini yeni takıma ayrılması gereken bir durumda olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir VM sağlama](azure-stack-provision-vm.md)
