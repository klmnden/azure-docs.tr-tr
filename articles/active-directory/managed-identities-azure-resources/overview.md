---
title: Azure kaynakları için yönetilen kimlikler
description: Azure kaynakları için yönetilen kimliklere genel bakış.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 10/23/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: d70dfceb0101c4f6dbd76f3c6b34d85e5255aa72
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59261471"
---
# <a name="what-is-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler nedir?

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bulut uygulamaları oluştururken yaygın olarak karşılaşılan bir zorluk, bulut hizmetlerinde kimlik doğrulaması yapmak için kodunuzda bulunan kimlik bilgilerinin yönetimidir. Kimlik bilgilerinin güvenlik altında tutulması önemli bir görevdir. İdeal olan kimlik bilgilerinin geliştirici iş istasyonlarında asla gösterilmemesi ve kaynak denetimine kaydedilmemesidir. Azure Key Vault kimlik bilgilerini, gizli dizileri ve diğer anahtarları güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. 

Azure Active Directory (Azure AD) hizmetindeki Azure kaynakları yönetilen hizmetleri bu sorunu çözer. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri olmadan kimlik doğrulaması yapabilirsiniz.

Azure kaynakları için yönetilen kimlikler özelliği, Azure abonelikleri için Azure AD ile ücretsiz olarak sunulmaktadır. Ek ücret alınmaz.

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

## <a name="terminology"></a>Terminoloji

Aşağıdaki terimler yönetilen kimlikleri, Azure kaynaklarını belgeleri kümesi için kullanılır:

- **İstemci kimliği** -ilk kendi sağlama sırasında bir uygulama ve hizmet sorumlusu bağlı Azure AD tarafından oluşturulan benzersiz bir tanımlayıcı.
- **Sorumlu Kimliği** -nesne Kimliğini bir Azure kaynağı için rol tabanlı erişim vermek için kullanılan, yönetilen kimliği için hizmet sorumlusu nesnesi.
- **Azure örnek meta veri hizmeti (IMDS)** -REST uç noktasını tüm Iaas Vm'leri için erişilebilir Azure Resource Manager aracılığıyla oluşturulan. Uç noktası, yalnızca VM içinden erişilebilir bir bilinen yönlendirilemeyen IP adresinde (169.254.169.254 numaralı) kullanılabilir.

## Azure kaynakları için yönetilen kimlikleri nasıl çalışır?<a name="how-does-it-work"></a>

İki tür yönetilen kimlik vardır:

- **Sistem tarafından atanan yönetilen kimlik** doğrudan bir Azure hizmet örneğinde etkinleştirilir. Kimlik etkinleştirildiğinde Azure, örneğin aboneliğinin güvendiği Azure AD kiracısında örnek için bir kimlik oluşturur. Kimlik oluşturulduktan sonra, kimlik bilgileri örneğe sağlanır. Sistem tarafından atanan kimliğin yaşam döngüsü, içinde etkinleştirildiği Azure hizmet örneğine doğrudan bağlıdır. Örnek silinirse, Azure AD'deki kimlik bilgileri ve kimlik Azure tarafından otomatik olarak temizlenir.
- **Kullanıcı tarafından atanan yönetilen kimlik**, tek başına bir Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir. Kullanıcı tarafından atanan kimliğin yaşam döngüsü, bu kimliğin atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir.

Dahili olarak, yönetilen özel bir tür yalnızca Azure kaynakları ile kullanılacak kilitlenen hizmet sorumluları Kimlikleridir. Yönetilen kimlik silindiğinde, karşılık gelen hizmet sorumlusu otomatik olarak kaldırılır. 

Kodunuzda Azure AD kimlik doğrulamasını destekleyen hizmetler için erişim belirteci istemek üzere yönetilen kimlik kullanılabilir. Hizmet örneği tarafından kullanılan kimlik bilgilerinin dağıtımıyla Azure ilgilenir. 

Aşağıdaki diyagramda yönetilen hizmet kimliklerinin Azure sanal makineleriyle (VM) nasıl çalıştığı gösterilmektedir:

![Yönetilen hizmet kimlikleri ve Azure VM’leri](media/overview/msi-vm-vmextension-imds-example.png)

|  Özellik    | Sistem tarafından atanan yönetilen kimlik | Kullanıcı tarafından atanan yönetilen kimlik |
|------|----------------------------------|--------------------------------|
| Oluşturma |  Bir Azure kaynağı (örneğin, bir Azure sanal makinesi veya Azure App Service) bir parçası olarak oluşturulan | Tek başına bir Azure kaynağı olarak oluşturuldu |
| Yaşam döngüsü | Yönetilen kimlik ile oluşturulmuş bir Azure kaynak yaşam döngüsü paylaşıldığı. <br/> Üst kaynak silindiğinde, yönetilen kimlik olarak da silinir. | Bağımsız yaşam döngüsü. <br/> Açıkça silinmelidir. |
| Azure kaynakları arasında paylaşma | Paylaşılamaz. <br/> Yalnızca tek bir Azure kaynağı ile ilişkili olabilir. | Paylaşılabilir <br/> Aynı kullanıcı tarafından atanan yönetilen kimlik birden fazla Azure kaynak ile ilişkilendirilebilir. |
| Genel kullanım örnekleri | İçinde tek bir Azure kaynak içeren iş yükleri <br/> İş yükleri bağımsız kimlikleri gerekir. <br/> Örneğin, tek bir sanal makinede çalışan bir uygulama | Birden çok kaynak üzerinde çalışan iş yükleri ve tek bir kimlik paylaşmasını sağlayabilirsiniz. <br/> Hazırlama akışında bir parçası olarak güvenli bir kaynağa öncesi yetkilendirme gerektiren iş yükleri. <br/> Burada kaynakları sık dönüştürülünceye iş yükleri, ancak izinleri tutarlı kalmalıdır. <br/> Örneğin, birden çok sanal makine aynı kaynağa erişmek için gereken yere iş yükü | 

### <a name="how-a-system-assigned-managed-identity-works-with-an-azure-vm"></a>Sistem tarafından atanmış yönetilen kimliğin Azure VM ile çalışma şekli

1. Azure Resource Manager sanal makinede sistem tarafından atanan yönetilen kimliği etkinleştirmek için bir istek alır.
2. Azure Resource Manager, Azure AD'de sanal makinenin kimliği için bir hizmet sorumlusu oluşturur. Hizmet sorumlusu, bu abonelik tarafından güvenilen Azure AD kiracısında oluşturulur.
3. Azure Resource Manager sanal makinede kimliği yapılandırır:
    1. Azure Instance Metadata Service kimliği uç noktasını hizmet sorumlusu istemci kimliği ve sertifikasıyla güncelleştirir.
    1. VM uzantısını (Ocak 2019'da kullanımdan kaldırılması planlanan) sağlar ve hizmet sorumlusu istemci kimliğini ve sertifikayı ekler. (Bu adımın kullanımdan kaldırılması planlanmaktadır.)
4. VM bir kimliğe sahip olduktan sonra hizmet sorumlusunu kullanarak Azure kaynaklarına VM erişimi sağlayabilirsiniz. Azure Resource Manager'ı çağırmak için Azure AD'de rol tabanlı erişim denetimini (RBAC) kullanarak VM hizmet sorumlusuna uygun rolü atayabilirsiniz. Key Vault'u çağırmak için kodunuza Key Vault'ta belirli bir gizli diziye veya anahtara erişim verebilirsiniz.
5. Sanal makinede çalışan kod, yalnızca VM içinden erişilebilir Azure örnek meta veri Hizmeti uç noktasından bir belirteç isteyebilirsiniz: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Resource parametresi belirtecin gönderildiği hizmeti belirtir. Azure Resource Manager ile kimlik doğrulaması için `resource=https://management.azure.com/` kullanın.
    - API version parametresi IMDS sürümünü belirtir; api-version=2018-02-01 veya üstünü kullanın.

> [!NOTE]
> Kodunuzu VM uzantısı uç noktasından bir belirteç de isteyebilir, ancak bu yakında kullanımdan planlanmaktadır. VM uzantısı hakkında daha fazla bilgi için bkz: [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

6. Azure AD'ye erişim belirteci isteyen bir çağrı yapılır (5. adımda belirtildiği gibi) ve bu çağrıda 3. adımda yapılandırılan istemci kimliği ve sertifikası kullanılır. Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuz erişim belirtecini bir çağrıda Azure AD kimlik doğrulamasını destekleyen hizmete gönderir.

### <a name="how-a-user-assigned-managed-identity-works-with-an-azure-vm"></a>Kullanıcı tarafından atanmış yönetilen kimliğin Azure VM ile çalışma şekli

1. Azure Resource Manager kullanıcı tarafından atanan bir yönetilen kimlik oluşturma isteği alır.
2. Azure Resource Manager, Azure AD'de kullanıcı tarafından atanan yönetilen kimlik için bir hizmet sorumlusu oluşturur. Hizmet sorumlusu, bu abonelik tarafından güvenilen Azure AD kiracısında oluşturulur.
3. Azure Resource Manager sanal makinede kullanıcı tarafından atanan yönetilen kimliği yapılandırmak için bir istek alır:
    1. Azure Instance Metadata Service kimliği uç noktasını kullanıcı tarafından atanan yönetilen kimlik hizmet sorumlusu istemci kimliği ve sertifikasıyla güncelleştirir.
    1. VM uzantısını sağlar ve kullanıcı tarafından atanan yönetilen kimlik hizmet sorumlusu istemci kimliğiyle sertifikasını ekler. (Bu adımın kullanımdan kaldırılması planlanmaktadır.)
4. Kullanıcı tarafından atanan yönetilen kimlik oluşturulduktan sonra hizmet sorumlusu bilgilerini kullanarak Azure kaynaklarına erişim verir. Azure Resource Manager'ı çağırmak için Azure AD'de RBAC özelliğini kullanarak kullanıcı tarafından atanan kimliğin hizmet sorumlusuna uygun rolü atayabilirsiniz. Key Vault'u çağırmak için kodunuza Key Vault'ta belirli bir gizli diziye veya anahtara erişim verebilirsiniz.

   > [!Note]
   > Bu adımı 3. adımdan önce de gerçekleştirebilirsiniz.

5. Sanal makinede çalışan kod, yalnızca VM içinden erişilebilir Azure örnek meta veri hizmeti kimlik uç noktasından bir belirteç isteyebilirsiniz: `http://169.254.169.254/metadata/identity/oauth2/token`
    - Resource parametresi belirtecin gönderildiği hizmeti belirtir. Azure Resource Manager ile kimlik doğrulaması için `resource=https://management.azure.com/` kullanın.
    - Client ID parametresi belirtecin hangi kimlik için istendiğini belirtir. Tek bir sanal makinede birden çok kullanıcı tarafından atanan kimlik olduğunda, belirsizliği ortadan kaldırmak için bu değer gereklidir.
    - API version parametresi, Azure Instance Metadata Service sürümünü belirtir. `api-version=2018-02-01` veya daha yüksek bir değer kullanın.

> [!NOTE]
> Kodunuzu VM uzantısı uç noktasından bir belirteç de isteyebilir, ancak bu yakında kullanımdan planlanmaktadır. VM uzantısı hakkında daha fazla bilgi için bkz: [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

6. Azure AD'ye erişim belirteci isteyen bir çağrı yapılır (5. adımda belirtildiği gibi) ve bu çağrıda 3. adımda yapılandırılan istemci kimliği ve sertifikası kullanılır. Azure AD bir JSON Web Token (JWT) erişim belirteci döndürür.
7. Kodunuz erişim belirtecini bir çağrıda Azure AD kimlik doğrulamasını destekleyen hizmete gönderir.

## <a name="how-can-i-use-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri nasıl kullanabilirim?

Yönetilen kimlikleri farklı Azure kaynaklarına erişme amacıyla kullanmayı öğrenmek için aşağıdaki öğreticileri deneyin.

> [!NOTE]
> Kullanıma [Microsoft Azure kaynakları için yönetilen kimlikleri uygulama](https://www.pluralsight.com/courses/microsoft-azure-resources-managed-identities-implementing) dahil olmak üzere yönetilen kimlikleri hakkında daha fazla bilgi ayrıntılı video izlenecek yollar desteklenen çeşitli senaryolar için kursu.

Yönetilen kimlikleri Windows VM ile kullanmayı öğrenin:

* [Azure Data Lake Store’a erişme](tutorial-windows-vm-access-datalake.md)
* [Azure Resource Manager’a erişme](tutorial-windows-vm-access-arm.md)
* [Azure SQL’e erişme](tutorial-windows-vm-access-sql.md)
* [Erişim anahtarı kullanarak Azure Depolama’ya erişme](tutorial-windows-vm-access-storage.md)
* [Paylaşılan erişim anahtarlarını kullanarak Azure Depolama’ya erişme](tutorial-windows-vm-access-storage-sas.md)
* [Azure Key Vault ile Azure AD dışı bir kaynağa erişme](tutorial-windows-vm-access-nonaad.md)

Yönetilen kimlikleri Linux VM ile kullanmayı öğrenin:

* [Azure Data Lake Store’a erişme](tutorial-linux-vm-access-datalake.md)
* [Azure Resource Manager’a erişme](tutorial-linux-vm-access-arm.md)
* [Erişim anahtarı kullanarak Azure Depolama’ya erişme](tutorial-linux-vm-access-storage.md)
* [Paylaşılan erişim anahtarlarını kullanarak Azure Depolama’ya erişme](tutorial-linux-vm-access-storage-sas.md)
* [Azure Key Vault ile Azure AD dışı bir kaynağa erişme](tutorial-linux-vm-access-nonaad.md)

Yönetilen kimlikleri diğer Azure hizmetleri ile kullanmayı öğrenin:

* [Azure App Service](/azure/app-service/overview-managed-identity)
* [Azure İşlevleri](/azure/app-service/overview-managed-identity)
* [Azure Logic Apps](/azure/logic-apps/create-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)
* [Azure Event Hubs](../../event-hubs/event-hubs-managed-service-identity.md)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)
* [Azure Container Instances](../../container-instances/container-instances-managed-identity.md)

## Bu özelliği hangi Azure hizmetleri destekliyor?<a name="which-azure-services-support-managed-identity"></a>

Azure kaynakları için yönetilen kimlikler, Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmak için kullanılabilir. Azure kaynakları için yönetilen kimlikler özelliğini destekleyen Azure hizmetlerinin listesi için bkz. [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](services-support-msi.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynakları için yönetilen kimlikler özelliğini kullanmaya başlamak için aşağıdaki hızlı başlangıçlardan yararlanın:

* [Resource Manager’a erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-windows-vm-access-arm.md)
* [Resource Manager’a erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma](tutorial-linux-vm-access-arm.md)
