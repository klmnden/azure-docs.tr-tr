---
title: Azure yığını için bir hizmet sorumlusu oluşturma | Microsoft Docs
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir bir hizmet sorumlusu oluşturmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 3c9f114c2844021d515765888aa19f18a0adc10b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320761"
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Uygulamaları Azure yığın kaynaklar için hizmet asıl adı oluşturarak erişmenizi

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir hizmet asıl Azure Resource Manager kullanan oluşturarak Azure yığın kaynakları için bir uygulama erişim verebilirsiniz. Bir hizmet sorumlusu kullanarak temsilci belirli izinleri sağlar [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md).

En iyi uygulama, uygulamalarınız için hizmet asıl adı kullanmanız gerekir. Hizmet asıl adı aşağıdaki nedenlerle kendi kimlik bilgilerini kullanarak bir uygulamayı çalıştıran için tercih edilir:

* Hizmet sorumlusu kendi hesap izinlerini farklı olan izinleri atayabilirsiniz. Genellikle, bir hizmet sorumlusuna ilişkin izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Rol veya sorumlulukları uygulamanın kimlik bilgilerini değiştirirseniz gerekmez.
* Katılımsız betik çalıştırıldığında, kimlik doğrulama otomatikleştirmek için bir sertifika kullanabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo

Azure Resource Manager kullanarak Azure kaynaklarına stok için gereken bir yapılandırma yönetim uygulaması sahip. Bir hizmet sorumlusu oluşturma ve okuyucu rolüne atayın. Bu rol için Azure kaynaklarını uygulama salt okunur erişim sağlar.

## <a name="getting-started"></a>Başlarken

Bir kılavuz olarak bu makaledeki adımları kullanın:

* Uygulamanız için bir hizmet sorumlusu oluşturun.
* Uygulamanızı kaydetmenizi ve bir kimlik doğrulama anahtarı oluşturun.
* Uygulamanız için bir rol atayın.

Bir hizmet sorumlusu oluşturma Azure yığını için Active Directory yapılandırılmış şeklini belirler. Aşağıdaki seçeneklerden birini seçin:

* Bir hizmet için asıl oluşturma [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Bir hizmet için asıl oluşturma [Active Directory Federasyon Hizmetleri (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

Bir hizmet sorumlusu bir rol aynı Azure için atama adımlarını AD ve AD FS. Hizmet sorumlusu oluşturduktan sonra şunları yapabilirsiniz [temsilci izinleri](azure-stack-create-service-principals.md#assign-role-to-service-principal) rol atama tarafından.

## <a name="create-a-service-principal-for-azure-ad"></a>Azure AD için bir hizmet sorumlusu oluşturma

Azure yığın kimlik deposu olarak Azure AD kullanıyorsa, bir hizmet asıl Azure portalını kullanarak Azure, olduğu gibi aynı adımları kullanarak oluşturabilirsiniz.

>[!NOTE]
Sahip olduğunuz denetleyin [gereken Azure AD izinler](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) bir hizmet sorumlusu oluşturmaya başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Uygulamanız için bir hizmet sorumlusu oluşturmak için:

1. Oturum Azure hesabınıza [Azure portal](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtlar** > **Ekle**.
3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü için. Değerleri ayarladıktan sonra Seç **oluşturma**.

### <a name="get-credentials"></a>Kimlik bilgilerini al

Program aracılığıyla oturum açarken kimliği uygulamanız ve bir kimlik doğrulama anahtarı için kullanın. Bu değerleri almak için:

1. Gelen **uygulama kayıtlar** Active Directory'de uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Uygulamalarda [örnek uygulamaları](#sample-applications) kullanmak **istemci kimliği** için söz konusu olduğunda **uygulama kimliği**.

     ![Uygulama için uygulama kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

4. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

>[!IMPORTANT]
Anahtar, anahtar kaydettikten sonra **değeri** görüntülenir. Daha sonra anahtar alamadığından bu değeri yazın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

![Anahtar değeri uyarısı için kaydedilen anahtarı.](./media/azure-stack-create-service-principal/image15.png)

Son adım [uygulamanızı rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma

Azure AD FS kimlik deposu olarak kullanarak yığın dağıtılmışsa, aşağıdaki görevler için PowerShell kullanabilirsiniz:

* Bir hizmet sorumlusu oluşturun.
* Hizmet sorumlusu bir role atayın.
* Hizmet sorumlusunun kimliğini kullanarak oturum açın.

Hizmet sorumlusu oluşturma hakkında daha fazla bilgi için bkz [AD FS için hizmet sorumlusu oluşturma](../azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

## <a name="assign-the-service-principal-to-a-role"></a>Hizmet sorumlusu rol atama

Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
Abonelik, bir kaynak grubu veya bir kaynak düzeyinde bir rolün kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. Örneğin, bir kaynak grubu için okuyucu rolüne sahip bir uygulama, uygulama kaynakların kaynak grubunda okuyabilir anlamına gelir.

Aşağıdaki adımlar, bir rol için bir hizmet sorumlusu atamak için bir kılavuz olarak kullanın.

1. Azure yığın Portalı'nda uygulamaya atamak istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

2. Uygulamaya atamak istediğiniz aboneliği seçin. Bu örnekte, abonelik Visual Studio Enterprise ' dir.

     ![Atama için Visual Studio Enterprise aboneliği seçin](./media/azure-stack-create-service-principal/image16.png)

3. Seçin **erişim denetimi (IAM)** abonelik için.

     ![Erişim denetimi seçin](./media/azure-stack-create-service-principal/image17.png)

4. **Add (Ekle)** seçeneğini belirleyin.

5. Uygulamaya atamak istediğiniz rolü seçin.

6. Uygulamanız için arayın ve seçin.

7. Seçin **Tamam** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların görebilirsiniz.

Bir hizmet sorumlusu oluşturulur ve bir rolü atanmış göre uygulamanızı Azure yığın kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
