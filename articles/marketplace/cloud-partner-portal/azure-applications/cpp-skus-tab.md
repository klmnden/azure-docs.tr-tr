---
title: Bir Azure uygulaması teklif SKU'ları yapılandırma | Azure Market
description: Azure SKU'ları yapılandırma yönetilen uygulama ve bir Azure çözümü şablonu.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: ef4ea2419c64d0376023ea5d291460df48a51c63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943436"
---
# <a name="azure-application-skus-tab"></a>Azure uygulama SKU'ları sekmesi

Bu makalede, Azure uygulamanız için SKU'ları oluşturmak için SKU'ları sekmesini kullanmayı açıklar. 

> [!IMPORTANT]
> Bir SKU yapılandırma adımlarını, yönetilen uygulama teklifi ve bir çözüm şablonu teklif için farklıdır. Bu makalede bu farklılıklar belirtilmiştir. 

## <a name="configure-azure-application-skus"></a>Azure uygulama SKU'ları yapılandırma

### <a name="create-a-new-sku"></a>Yeni bir SKU oluşturma

Yeni bir SKU'ya oluşturmak için aşağıdaki adımları kullanın:

1. Seçin **SKU'ları** sekmesi.
2. SKU'ları seçin **+ yeni SKU**.

    ![Yeni SKU istemi](./media/azureapp-plus-sku.png)

3. Yeni SKU açılan pencerede, türü bir **SKU kimliği**. Bu kimlik, 50 karakterle sınırlıdır ve yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi oluşmalıdır. SKU kimliği tire bitemez.
4. SKU kimliği ürün URL'ler, Resource Manager şablonları (varsa) bulunan müşteriler tarafından görülebilir ve faturalandırma bildirir. Bu kimliği, teklif yayımlanma sonra değiştiremezsiniz.

### <a name="sku-details-for-a-solution-template"></a>Bir çözüm şablonu için SKU ayrıntıları

Sonraki ekran yakalama için çözüm şablonu SKU Ayrıntıları formunu gösterir.

![Çözüm şablonu için SKU ayrıntı formu](./media/azureapp-sku-details-solutiontemplate.png)

Aşağıdaki SKU değerleri sağlayın.  Bir yıldız işareti ile eklenmiş alanlar gereklidir.

|    Alan         |       Açıklama                                                            |
|  ---------       |     ---------------                                                          |
|  **Başlık\***     | SKU için bir başlık. Bu konu başlığı, bu öğe için galerisinde görüntülenir.   |
| **Özeti\***    | SKU'ların kısa bir Özet açıklaması. (En fazla 100 karakterdir.)  |
| **Açıklaması\*** | SKU, ayrıntılı bir açıklaması. Temel HTML desteklenir.                 | 
| **SKU türü\***   | Azure uygulama çözümü, select türü ***çözüm şablonu** bu senaryo için. |
| **Bulut kullanılabilirlik\*** | SKU konumu. Varsayılan değer **genel Azure**.  <b/>   **Genel Azure** -uygulama Market tümleştirmesine sahip tüm genel Azure bölgelerinde müşterilere dağıtılabilir olacaktır.  <b/>   **Azure kamu Bulutu** -uygulama Azure kamu Bulutundaki dağıtılacak. Yayımlama için önce [Azure kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), Microsoft, yayımcılar test etmek ve bu ortamda beklendiği gibi çalışır, doğrulama önerir. Hazırlama ve test etmek için istek bir [deneme hesabı](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).  |
| **Bu, özel bir SKU mi?\*** | Seçin **Evet** varsa bu SKU yalnızca müşterilerin grubunu seçmek için kullanılabilir. |
|   |   |

  > [!NOTE] 
  > Microsoft Azure kamu, ABD Federal, eyalet, yerel veya Kabile ve iş ortakları bu kuruluşlara hizmet vermek uygun müşteriler için denetimli erişimi olan bir devlet kurumu-topluluk bulutudur.


### <a name="sku-details-for-managed-application"></a>Yönetilen uygulama için SKU ayrıntıları

Sonraki ekran görüntüsü yakalamayı yönetilen bir uygulama için SKU Ayrıntıları formunu gösterir.

   ![Yönetilen uygulama için SKU ayrıntı formu](./media/azureapp-sku-details-managedapplication.png)

Aşağıdaki SKU ayarlarını yapılandırın. Bir yıldız işareti ile eklenmiş alanlar gereklidir.

|    Alan         |       Açıklama                                                            |
|  ---------       |     ---------------                                                          |
|  **Başlık\***     | SKU için bir başlık. Bu konu başlığı, bu öğe için galerisinde görüntülenir.   |
| **Özeti\***    | SKU'ların kısa bir Özet açıklaması. (En fazla 100 karakterdir.)  |
| **Açıklaması\*** | SKU, ayrıntılı bir açıklaması. Temel HTML desteklenir.                 | 
| **SKU türü\***   | Azure uygulama çözümü, select türü ***yönetilen uygulamayı** bu senaryo için. 
| **Bulut kullanılabilirlik\*** | SKU konumu. Varsayılan değer **genel Azure**.  <b/>   **Genel Azure** -uygulama Market tümleştirmesine sahip tüm genel Azure bölgelerinde müşterilere dağıtılabilir olacaktır.  <b/>   **Azure kamu Bulutu** -uygulama Azure kamu Bulutundaki dağıtılacak. Yayımlama için önce [Azure kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), Microsoft, yayımcılar test etmek ve bu ortamda beklendiği gibi çalışır, doğrulama önerir. Hazırlama ve test etmek için istek bir [deneme hesabı](https://azure.microsoft.com/offers/ms-azr-usgov-0044p/).   Microsoft Azure kamu, ABD Federal, eyalet, yerel veya Kabile ve iş ortakları bu kuruluşlara hizmet vermek uygun müşteriler için denetimli erişimi olan bir devlet kurumu-topluluk bulutudur. |
| **Bu, özel bir SKU mi?\*** | Seçin **Evet** varsa bu SKU yalnızca müşterilerin grubunu seçmek için kullanılabilir. |
| **Ülke/bölge kullanılabilirliği\*** | Kullanım **bölgeleri seçin** kullanılabilir ülkeler/bölgeler listesini görüntülemek için. Her bir ülke/bölge denetleyin ve ardından **Tamam** , çekme kaydetmek için.  <b/>   ![Ülke ve bölge kullanılabilirliği listesi](./media/azure-app-select-country-region.png)  |
| **Eski fiyatlandırma\*** | SKU'da ABD Doları / ay fiyatı. Fiyatlar, yerel para birimi kullanılarak yapılandırma sonrasında geçerli döviz kurları ayarlanır. Sonuç olarak bu ayarlar sahip olduğundan bu doğrulayın. Ayarlayın ya da ülke/bölge fiyat tek tek görüntülemek için lütfen fiyatlandırma elektronik tabloya dışarı aktarma ve özel fiyatlandırma ile içeri aktarma.  İçeri/dışarı aktarma fiyatlandırma veri etkinleştirmek için fiyatlandırma değişiklikleri kaydetmeniz gerekir.  |
| **Basitleştirilmiş bir para birimi fiyatlandırması\*** | SKU'da ABD Doları / ay fiyatı. Bu, eski fiyatlandırma ile aynı olmalıdır. Daha fazla bilgi için [Basitleştirilmiş para birimi fiyatlandırma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-update-existing-offer). |
|  |  |


### <a name="package-details-for-solution-template"></a>Paket ayrıntılarını, çözüm şablonu

   ![Paket ayrıntılarını, çözüm şablonu](./media/azureapp-sku-pkgdetails-solutiontemplate.png)

Şu bilgileri sağlayın **Paket ayrıntılarını** değerleri.  Bir yıldız işareti ile eklenmiş alanlar gereklidir.

- **Sürüm\***  -karşıya yükleyeceğiniz paketin sürümü. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır.
- **Paket dosyası (.zip)\***  -bu paketi bir .zip dosyası kaydedilen aşağıdaki dosyaları içerir.
  - MainTemplate.json - çözüm/uygulamayı dağıtmak ve çözüm için tanımlanan kaynakları oluşturmak için kullanılan dağıtım şablonu dosyası. Daha fazla bilgi için [dağıtım şablonu dosyalarını yazmak nasıl](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).
  - createUIDefinition.json - bu dosya, bu çözüm/uygulama sağlamak için kullanıcı arabirimi oluşturmak için Azure portal tarafından kullanılır. Daha fazla bilgi için [yönetilen uygulamanız için oluşturma Azure portal kullanıcı arabirimi](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

  >[!IMPORTANT] 
  >Bu paket, herhangi bir iç içe geçmiş şablonlar veya bu uygulama sağlamak için gereken komut dosyaları içermelidir. MainTemplate.json dosyası ve createUIDefinition.json dosyası kök klasöründe olması gerekir.


### <a name="package-details-for-managed-application"></a>Yönetilen uygulama için Paket Ayrıntıları

   ![Yönetilen uygulama için Paket Ayrıntıları](./media/azureapp-sku-pkgdetails-managedapplication.png)

Aşağıdaki Paket ayrıntılarını sağlayın.  Bir yıldız işareti ile eklenmiş alanlar gereklidir.

- **Sürüm\***  -karşıya yükleyeceğiniz paketin sürümü. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır.
- **Paket dosyası (.zip)\***  -bu paketi bir .zip dosyası kaydedilen aşağıdaki dosyaları içerir.
  - applianceMainTemplate.json - çözüm/uygulamayı dağıtmak ve bu kaynakları oluşturmak için kullanılan dağıtım şablonu dosyası tanımlanır. Daha fazla bilgi için [hızlı başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal). 
  - applianceCreateUIDefinition.json - bu dosya, bu çözüm/uygulama sağlamak için kullanıcı arabirimi oluşturmak için Azure portal tarafından kullanılır. Daha fazla bilgi için [yönetilen uygulamanız için oluşturma Azure portal kullanıcı arabirimi](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).
  - mainTemplate.json - yalnızca Microsoft.Solution/appliances kaynağı içeren şablon dosyası. Daha fazla bilgi için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). <br>
Bu kaynağın aşağıdaki anahtar özelliklerini not edin:
    - "tür" - değeri, Market tarafından yönetilen uygulama söz konusu olduğunda "Market" olmalıdır.
    - "ManagedResourceGroupId" - Müşteri aboneliğinde applianceMainTemplate.json içinde tanımlanan tüm kaynakların dağıtılacağı kaynak grubunda.
    - Paketi benzersiz olarak tanımlayan "Publisherpackageıd" dizesi. Bu değer şu şekilde oluşturulması gerekiyor: [Publisherıd] bir birleşimi olan. [OfferId]-[SKUID] Önizleme. [PackageVersion].

  >[!IMPORTANT] 
  >Bu paket, herhangi bir iç içe geçmiş şablonlar veya bu uygulama sağlamak için gereken komut dosyaları içermelidir. Bu dosyaları kök klasöründe olması gerekir:  MainTemplate.json applianceMainTemplate.json ve applianceCreateUIDefinition.json.

- **Kiracı kimliği\***  -kuruluşunuzun Azure Active Directory Kiracı kimliği.
- **JIT erişim etkinleştirilsin mi? \***  – Select **Evet** Just-ın-Time etkinleştirmek için bu teklifi kullanarak müşteri dağıtımları için yönetim erişim.

  >[!NOTE] 
  >JIT etkinleştirirseniz, JIT erişimini desteklemek üzere CreateUiDefinition.json dosyasını güncelleştirmeniz gerekir.

Yönetilen bir uygulama için yetkilendirme ve ilke ayarları yapılandırmanız gerekir.


#### <a name="authorization"></a>Yetkilendirme

Azure Active Directory tanıtıcısı kullanıcı, Grup veya yönetilen kaynak grubuna izin vermek istediğiniz uygulamaya ekleyin. İzin verilen rol tanımı kimliği tarafından belirtilir Sahibi, katkıda bulunan veya herhangi bir özel rol olabilir.


#### <a name="policy-settings"></a>İlke ayarları

Yönetilen uygulama ile uyumlu ilkelerini ekleyin. Azure kaynak ilkeleri hakkında daha fazla bilgi için bkz: [Azure İlkesi nedir?](../../../governance/policy/overview.md)

   ![Yönetilen bir uygulama için yetkilendirme ve ilke ayarları](./media/azureapp-sku-details-managedapp-auth-policy.png)

**Yeni bir yetkilendirme oluşturmak için:**

1. Altında **yetkilendirme**seçin **+ yeni yetkilendirme**.
2. İçin **sorumlusu kimliği**, Azure Active Directory tanıtıcısı kullanıcı, Grup veya yönetilen kaynak grubuna izin vermek istediğiniz uygulama yazın. İzin verilen rol tanımı tarafından belirtilir.
3. İçin **rol tanımı**, açılan listeden aşağıdaki seçeneklerden birini seçin:  Sahibi veya katkıda bulunan. Daha fazla bilgi için [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

>[!NOTE] 
>Birden çok yetkilendirmeleri eklenebilir. Ancak, bir Active Directory kullanıcı grubu oluşturun ve kendi kimliği "Principalıd." belirtmek için önerilir Bu SKU güncelleştirme gerek kalmadan daha fazla kullanıcı kullanıcı grubuna eklenmesine olanak sağlar.

**Yeni bir ilke oluşturmak için:**

1. Altında **ilke ayarları**seçin **+ yeni ilke**.
2. İçin **ilke adı**, ilke için bir ad girin. Ad uzunluğu en fazla 50 karakterdir.
3. İçin **ilkeleri**, açılır listeden seçeneklerden birini belirleyin. Veri sağlayıcısı uygulamasını veri kullandığında etkinleştirilmesini ister ilkeyi seçin. Daha fazla bilgi için [Azure ilkesi örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/index).

    ![Yönetilen bir uygulama için ilke ayarları](./media/azureapp-sku-policy-settings.png)

4. İçin **ilke SKU**, ücretsiz veya standart SKU türü ilke olarak seçin. Standart SKU denetim ilkeleri için gereklidir.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla teklifinizi tanımlamak ve Pazarlama Varlıkları tedarik [Marketi sekmesinden](./cpp-marketplace-tab.md). 
