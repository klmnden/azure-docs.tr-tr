---
title: Azure Stack için hizmet sorumlusu oluşturma | Microsoft Docs
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir bir hizmet sorumlusu oluşturmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: sethmanheim
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: sethm
ms.reviewer: thoroet
ms.openlocfilehash: 77940a52c0817b9eaf49cdf7d1a2d284c5e662e3
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42366109"
---
# <a name="give-applications-access-to-azure-stack-resources-by-creating-service-principals"></a>Hizmet sorumluları oluşturma tarafından Azure Stack kaynaklara uygulamaları erişimi verin

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack kaynaklara bir hizmet sorumlusu Azure Resource Manager kullanan oluşturarak, bir uygulama erişimi verebilirsiniz. Bir hizmet sorumlusu kullanarak temsilci belirli izinleri sağlayan [rol tabanlı erişim denetimi](azure-stack-manage-permissions.md).

En iyi uygulama, uygulamalarınız için hizmet sorumluları kullanmanız gerekir. Hizmet sorumluları, aşağıdaki nedenlerden dolayı kendi kimlik bilgilerini kullanarak bir uygulama çalıştırmaya tercih edilir:

* Hizmet sorumlusu kendi hesap izinleri farklı olan izinler atayabilirsiniz. Genellikle, tam olarak hangi uygulamanın yapması için bir hizmet sorumlusunun izinlerini kısıtlanır.
* Rol veya sorumlulukları değiştirirseniz uygulamanın kimlik bilgilerini değiştirmek izniniz yok.
* Kimlik doğrulaması katılımsız bir komut dosyası çalıştırılırken otomatik hale getirmek için bir sertifika kullanabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo

Azure Resource Manager kullanarak Azure kaynaklarını envantere almak üzere gereken bir yapılandırma yönetim uygulamanız var. Bir hizmet sorumlusu oluşturma ve okuyucu rolüne atayın. Bu rol, Azure kaynaklarına uygulama salt okunur erişim sağlar.

## <a name="getting-started"></a>Başlarken

Bir kılavuz olarak, bu makaledeki adımları kullanın:

* Uygulamanız için hizmet sorumlusu oluşturun.
* Uygulamanızı kaydetmenizi ve kimlik doğrulama anahtarı oluşturun.
* Uygulamanızı bir role atayın.

Azure Stack için Active Directory yapılandırdığınız şekilde, bir hizmet sorumlusu oluşturma belirler. Aşağıdaki seçeneklerden birini seçin:

* Bir hizmet sorumlusu için oluşturma [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad).
* Bir hizmet sorumlusu için oluşturma [Active Directory Federasyon Hizmetleri (AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

Hizmet sorumlusuna bir rol aynı Azure için atama adımlarını AD ve AD FS. Hizmet sorumlusu oluşturduktan sonra [temsilci izinleri](azure-stack-create-service-principals.md#assign-role-to-service-principal) role atayarak.

## <a name="create-a-service-principal-for-azure-ad"></a>Azure AD hizmet sorumlusu oluşturma

Azure Stack, Azure AD kimlik deposu olarak kullanıyorsa, bir hizmet sorumlusu Azure, Azure portalını kullanarak olduğu gibi aynı adımları kullanarak oluşturabilirsiniz.

>[!NOTE]
Sahip olduğunuzu denetleyin [Azure AD izinleri gerekli](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) bir hizmet sorumlusu oluşturma işlemi başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Uygulamanız için hizmet sorumlusu oluşturmak için:

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtları** > **Ekle**.
3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü. Değerleri ayarladıktan sonra seçin **Oluştur**.

### <a name="get-credentials"></a>Kimlik bilgilerini al

Programlamayla oturum açılırken, uygulamanızın ve bir kimlik doğrulama anahtarı kimliği kullanın. Bu değerleri almak için:

1. Gelen **uygulama kayıtları** Active Directory'de, uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Uygulamalarda [örnek uygulamalar](#sample-applications) kullanın **istemci kimliği** söz konusu olduğunda **uygulama kimliği**.

     ![Uygulama için uygulama kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

4. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

>[!IMPORTANT]
Anahtar, anahtar kaydettikten sonra **değer** görüntülenir. Daha sonra anahtarı alınamıyor çünkü bu değeri yazın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

![Anahtar değeri uyarısı için kaydedilen anahtarı.](./media/azure-stack-create-service-principal/image15.png)

Son adım [uygulamanızı rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma

AD FS kimlik deposu olarak kullanarak Azure Stack dağıttıysanız, aşağıdaki görevler için PowerShell kullanabilirsiniz:

* Bir hizmet sorumlusu oluşturun.
* Hizmet sorumlusu, rol atayın.
* Hizmet sorumlusunun kimliğini kullanarak oturum açın.

Hizmet sorumlusu oluşturma hakkında daha fazla ayrıntı için bkz. [AD FS için hizmet sorumlusu oluşturma](../azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).

## <a name="assign-the-service-principal-to-a-role"></a>Hizmet sorumlusu, rol atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: yerleşik roller](../../role-based-access-control/built-in-roles.md).

>[!NOTE]
Bir abonelik, kaynak grubu veya bir kaynak düzeyinde bir rolün kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne sahip bir uygulama, uygulama kaynak grubundaki kaynakların okuyabilirsiniz anlamına gelir.

Aşağıdaki adımlar, bir hizmet sorumlusuna bir rol atamak için bir kılavuz olarak kullanın.

1. Azure Stack Portalı'nda, uygulamayı atamak istediğiniz kapsam düzeyini gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**.

2. Uygulamaya atamak için bir abonelik seçin. Bu örnekte, Visual Studio Enterprise bir aboneliktir.

     ![Atama için Visual Studio Enterprise aboneliği seçin](./media/azure-stack-create-service-principal/image16.png)

3. Seçin **erişim denetimi (IAM)** abonelik için.

     ![Erişim denetimi seçin](./media/azure-stack-create-service-principal/image17.png)

4. **Add (Ekle)** seçeneğini belirleyin.

5. Uygulamayı atamak istediğiniz rolü seçin.

6. Uygulamanız için arama yapın ve seçin.

7. Seçin **Tamam** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görebilirsiniz.

Bir hizmet sorumlusu oluşturuldu ve rol atanmış göre uygulamanızı Azure Stack kaynaklara erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)
