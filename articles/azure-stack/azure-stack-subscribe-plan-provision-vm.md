---
title: Bir teklifi Azure yığınında abone | Microsoft Docs
description: Azure yığınında teklifleri için abonelikleri oluşturma
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/11/2018
ms.author: brenduns
ms.openlocfilehash: 9153649774a67533649fb62da83a3f50abd592da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295219"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Azure yığınında teklifleri için abonelikleri oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Çalıştırdıktan sonra [teklifi oluşturmak](azure-stack-create-offer.md), kullanıcıların, kullanmadan önce bu teklif için bir abonelik gerekiyor. İki yolla kullanıcıların abone bir teklif için:

- Bulut operatörü olarak, Yönetici portalı'nı bir kullanıcı için bir abonelik oluşturabilirsiniz. Oluşturduğunuz abonelikler için ortak ve özel teklifler olabilir.
- Kiracı kullanıcı olarak, Kullanıcı Portalı'nı kullandığınızda, ortak bir teklif için abone olabilirsiniz.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Bir bulut operatörü olarak Abonelik Oluştur

Bulut operatörleri, bir kullanıcı için bir teklif aboneliği oluşturmak için Yönetim Portalı'nı kullanabilirsiniz.  Dizin Kiracı üyeleri için abonelikleri oluşturabilirsiniz.  Zaman [çoklu kiracı](azure-stack-enable-multitenancy.md) olduğu Etkin Abonelikler kullanıcılar için ek dizin kiracılar oluşturabilirsiniz.

Kiracılarınızın kendi abonelikleri oluşturma, Teklifleriniz özel yapmak ve kiracılar için abonelikler oluşturmak için istemezsiniz. Bu yaklaşım, Azure yığın dış faturalama veya hizmet Kataloğu sistemler ile tümleştirdiğinizde yaygındır.

Bir kullanıcı için bir abonelik oluşturduktan sonra kullanıcı portalında oturum açın ve teklif abone bakın.  

### <a name="to-create-a-subscription-for-a-user"></a>Bir kullanıcı için bir abonelik oluşturmak için

1. Yönetim Portalı'nda Git **kullanıcı aboneliklerini.**
2. **Add (Ekle)** seçeneğini belirleyin. Altında **yeni kullanıcı abonelik**, aşağıdaki bilgileri girin:  

   - **Görünen ad** – olarak görünür aboneliği tanımlamak için bir kolay ad *kullanıcı abonelik adı*.
   - **Kullanıcı** – Bu abonelik için kullanılabilir directory kiracısındaki bir kullanıcı belirtin. Kullanıcı adı olarak görünür *sahibi*.  Kullanıcı adının biçimi kimlik çözümünüzü bağlıdır. Örneğin:

     - **Azure AD:***&lt;Kullanıcı1 > @&lt;contoso.onmicrosoft.com >* 

     - **AD FS:***&lt;Kullanıcı1 > @&lt;azurestack.local >* 

   - **Dizin Kiracı** – kullanıcı hesabına ait olduğu dizin Kiracı seçin. Çoklu kiracı etkinleştirmediyseniz, yalnızca yerel dizin kiracınız kullanılabilir.

3. Seçin **teklif**. Altında **sunar**, seçin bir **teklif** Bu abonelik için. Bir kullanıcı için bir abonelik oluşturuyorsanız çünkü seçin **özel** erişilebilirlik durumu.

4. Seçin **oluşturma** abonelik oluşturmak için. Yeni Abonelik altında görürsünüz **kullanıcı aboneliği**. Kullanıcının kullanıcı portalında oturum açtığında bunların abonelik ayrıntılarını görebilirsiniz.

### <a name="to-make-an-add-on-plan-available"></a>Bir eklenti planı kullanılabilir hale getirmek için

Bulut operatörü, herhangi bir zamanda önceden oluşturulmuş bir abonelik için bir eklenti plana ekleyebilirsiniz:

1. Yönetim Portalı'nda seçin **daha Hizmetleri** > **kullanıcı aboneliklerini**. Değiştirmek istediğiniz aboneliği seçin.

2. Seçin **eklentileri** ve ardından **+ Ekle**.  

3. Altında **planı Ekle**, eklenti olarak istediğiniz planı seçin.

## <a name="create-a-subscription-as-a-user"></a>Bir kullanıcı olarak Abonelik Oluştur

Bir kullanıcı olarak, kullanıcı portalı bulun ve ortak sunar ve eklenti planları için dizin kiracınız için (kuruluş) abone olmak için oturum açabilirsiniz.

>[!NOTE]
>Azure yığın ortamınızı destekleyip desteklemediğini [çoklu kiracı](azure-stack-enable-multitenancy.md) teklifleri için bir Uzak dizin kiracısı de abone olabilirsiniz.

### <a name="to-subscribe-to-an-offer"></a>Teklife abone olmak için

1. [Oturum](azure-stack-connect-azure-stack.md) Azure yığın kullanıcı portalı (https://portal.local.azurestack.external) seçip **bir abonelik edinmeniz**.

   ![Abonelik edinin](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. Altında **bir abonelik edinmeniz**, abonelikte kolay adını girin **görünen adı**. Seçin **teklif** ve altında **bir teklif seçin**, bir teklif seçin. Seçin **oluşturma** abonelik oluşturmak için.

   ![Teklif oluşturma](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Teklife abone olduktan sonra hangi hizmetlerin yeni abonelik parçası olan görmek için portal yenileyin.
4. Oluşturduğunuz abonelik görmek için seçin **daha fazla hizmet** ve ardından **abonelikleri**. Abonelik Ayrıntıları görmek için aboneliği seçin.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Bir eklenti plana abone olmak için

Bir teklif bir eklenti planı varsa, aboneliğinizi herhangi bir zamanda bu planı ekleyebilirsiniz.  

1. Kullanıcı Portalı'nda seçin **daha fazla hizmet** > **abonelikleri**ve ardından değişiklik istediğiniz aboneliği seçin. Kullanılabilir, eklenti planların varsa **+ Ekle planı** etkindir ve bölme için **eklenti planları**.

   >[!NOTE]
   >Varsa **+ Ekle planı** etkin değil yok sonra eklenti planların bu abonelikle ilişkili teklifin.

1. Seçin **+ Ekle planı** veya **eklenti planları** döşeme. Altında **eklenti planları**, eklemek istediğiniz planı seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](azure-stack-provision-vm.md)
