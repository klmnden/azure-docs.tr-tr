---
title: "Bir teklifi Azure yığınında abone | Microsoft Docs"
description: "Azure yığınında teklifleri için abonelikleri oluşturma"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Azure yığınında teklifleri için abonelikleri oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Çalıştırdıktan sonra [teklifi oluşturmak](azure-stack-create-offer.md), kullanıcıların, kullanmadan önce bu teklif için bir abonelik gerekiyor.   
- Bulut operatörü olarak, Yönetici portalı'nı bir kullanıcı için bir abonelik oluşturabilirsiniz.  Oluşturduğunuz abonelikler için ortak ve özel teklifler olabilir.
- Kiracı kullanıcı olarak, Kullanıcı Portalı'nı kullandığınızda, ortak bir teklif için abone olabilirsiniz.  

Aşağıdaki bölümlerde, kullanıcılar ve kullanıcı olarak bir teklif abone olmak nasıl için abonelik oluştururken, bulut operatörlerinin için yönergeler sağlar.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Bir bulut operatörü olarak Abonelik Oluştur
Bulut operatörleri, bir kullanıcı için bir teklif aboneliği oluşturmak için Yönetim Portalı'nı kullanabilirsiniz.  Dizin Kiracı üyeleri için abonelikleri oluşturabilirsiniz.  Zaman [çoklu kiracı](azure-stack-enable-multitenancy.md) olduğu Etkin Abonelikler kullanıcılar için ek dizin kiracılar oluşturabilirsiniz.

Ortak ve özel teklifler için abonelikleri oluşturabilirsiniz.  Varsa, kiracıların kendi abonelikleri oluşturma, tüm teklifleri özel hale getirmek ve kiracılar adına abonelikleri oluşturmak istiyor musunuz. Bu yaklaşım, Azure yığın dış faturalama veya hizmet Kataloğu sistemler ile tümleştirdiğinizde yaygındır.

Bir kullanıcı için bir abonelik oluşturduktan sonra bu kullanıcının kullanıcı portalında oturum açabilir ve teklif abone olduğunuz bulur.  

### <a name="to-create-the-subscription-for-a-user"></a>Bir kullanıcı için Abonelik oluşturmak için
1.  Yönetim Portalı'nda Git **kullanıcı aboneliklerini.**
2.  Seçin **Ekle** açmak için **yeni kullanıcı abonelik** bölmesi. Burada aşağıdaki ayrıntıları belirtin:  

  a. **Görünen ad** – olarak görünür aboneliği tanımlamak için bir kolay ad *kullanıcı abonelik adı*.

  b. **Kullanıcı** – Bu abonelik için kullanılabilir directory kiracısındaki bir kullanıcı belirtin. Kullanıcı adı olarak görünür *sahibi*.  Kullanıcı adının biçimi kimlik çözümünüzü bağlıdır. Örneğin:   

     -  **Azure AD:***&lt;Kullanıcı1 > @&lt;contoso.onmicrosoft.com >* 

     -   **AD FS:***&lt;Kullanıcı1 > @&lt;azurestack.local >*      

  c.    **Dizin Kiracı** – kullanıcı hesabına ait olduğu dizin Kiracı seçin. Çoklu kiracı etkin değil, yalnızca yerel dizin Kiracı kullanılabilir olur.

3.  Seçin **teklif** açmak için **sunar** bölmesinde ve ardından bir **teklif** Bu abonelik için. Bir kullanıcı için bir abonelik oluşturuyorsanız olduğundan, özel veya genel bir teklif seçebilirsiniz.

4.  Seçin **oluşturma** abonelik oluşturmak için. **Kullanıcı aboneliklerini** bölmesi artık yeni abonelik görüntüler.  Daha sonra kullanıcı Kullanıcı Portalı'na kapattığında, bu abonelik ayrıntılarını görüntüleyebilirsiniz.

### <a name="to-make-an-add-on-plan-available"></a>Bir eklenti planı kullanılabilir hale getirmek için  
Bulut operatörü, herhangi bir zamanda önceden oluşturulmuş bir abonelik için bir eklenti plana ekleyebilirsiniz:   
1.  Yönetim Portalı'nda Git **daha Hizmetleri** > **kullanıcı aboneliklerini**ve ardından değiştirmek istediğiniz aboneliği seçin.   

2.  Seçin **eklentileri** > **Ekle** açmak için **planı Ekle** bölmesi.  

3.  Eklenti olarak eklemek istediğiniz planı seçin.  Konsolu sonra abonelikle ilişkilendirilmiş planı görüntüler.




## <a name="create-a-subscription-as-a-user"></a>Bir kullanıcı olarak Abonelik Oluştur
Bir kullanıcı olarak, kullanıcı portalı bulun ve ortak sunar ve eklenti planları için dizin Kiracı (kuruluş) abone olmak için oturum açabilirsiniz. Ne zaman Azure yığın ortamını destekler [çoklu kiracı](azure-stack-enable-multitenancy.md) Uzak dizin Kiracı tekliflerini abone olabilirsiniz.

### <a name="to-subscribe-to-an-offer"></a>Teklife abone olmak için
1. [Oturum](azure-stack-connect-azure-stack.md) tıklatın ve Azure yığın Kullanıcı Portalı'nı (https://portal.local.azurestack.external) için **bir abonelik edinmeniz**.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. İçinde **görünen adı** alan, aboneliğiniz için bir ad yazın, tıklatın **teklif**, teklifleri birini tıklatın **bir teklif seçin** bölmesi ve ardından  **Oluşturma**.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Oluşturduğunuz abonelik görüntülemek için **daha fazla hizmet**, tıklatın **abonelikleri**, yeni aboneliğinizi'ye tıklayın.  

Teklife abone olduktan sonra hangi hizmetlerin yeni abonelik parçası olan görmek için portal yenileyin.

### <a name="to-subscribe-to-an-add-on-plan"></a>Bir eklenti plana abone olmak için
Bir teklif bir eklenti planı varsa, aboneliğinizi herhangi bir zamanda bu planı ekleyebilirsiniz.  

1. Kullanıcı Portalı'nda seçin **daha fazla hizmet** > **abonelikleri**ve ardından değişiklik istediğiniz aboneliği seçin.

2. Seçin **ekleme planı** düğmesine tıklayın ve ardından istediğiniz eklentiyi planı seçin. Varsa **planı Ekle** kullanılamıyor, ardından eklenti plan teklifin yok Bu abonelikle ilişkili değil.



## <a name="next-steps"></a>Sonraki adımlar
[Sanal makine sağlama](azure-stack-provision-vm.md)
