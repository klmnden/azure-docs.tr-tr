---
title: Azure iş ortağı ve müşteri kullanım attribution | Azure Market
description: Azure Marketi çözümleri için müşteri kullanımını izlemenizi nasıl genel bakış
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: yijenj
ms.service: marketplace
ms.topic: article
ms.date: 11/17/2018
ms.author: yijenj
ms.openlocfilehash: 09ce4cdc6ab4556f0ba68507bb23d09e02ae0357
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66296821"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure iş ortağı müşteri kullanımı ilişkilendirmesi

Bir yazılım iş ortağı Azure çözümlerinizi Azure bileşenlerini gerektirir veya doğrudan Azure altyapısının üzerinde dağıtılması gerekir. Bir iş ortağı çözümü dağıtmak ve kendi Azure kaynaklarını hazırlayacak müşteriler dağıtım durumunu görmenize zor ve optik Azure büyümesi üzerindeki etkisi içine alın. Daha yüksek bir görünürlük düzeyi eklediğinizde, Microsoft satış takımlarıyla Hizala ve Microsoft iş ortağı programları için kredi elde edin.

Microsoft artık ortakları Azure kullanımınızın müşteri dağıtımları yazılımlarını Azure üzerinde daha iyi izlemek için bir yöntem sunar. Azure Hizmetleri dağıtımını düzenlemek için Azure Resource Manager yeni yöntemi kullanır.

Microsoft iş ortağı olarak, Azure kullanımı, bir müşterinin adıma sağlamanız tüm Azure kaynakları ile ilişkilendirebilirsiniz. Azure marketi, hızlı başlangıç depo, özel GitHub depoları ve bire bir müşteri katılımı ile ilişki kurabilir. Müşteri kullanım attribution üç dağıtım seçeneklerini destekler:

- Azure Resource Manager şablonları: İş ortakları, iş ortağının yazılımlarını çalıştırmak üzere Azure hizmetlerini dağıtmak için Resource Manager şablonlarını kullanabilirsiniz. İş ortakları, altyapı ve bunların Azure çözüm yapılandırmasını tanımlamak için bir Resource Manager şablonu oluşturabilirsiniz. Siz ve müşterilerinizin yaşam döngüsü boyunca çözümünüzü dağıtmak Resource Manager şablonu sağlar. Kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz.
- Azure Resource Manager API'leri: İş ortakları, doğrudan bir Resource Manager şablonu dağıtma veya doğrudan Azure hizmetleri sağlamak için API çağrıları oluşturmak için Resource Manager API'leri çağırabilirsiniz.
- Terraform: İş ortakları, Terraform gibi bulut orchestrator, Resource Manager şablonu dağıtma veya doğrudan Azure hizmetlerini dağıtmak için kullanabilirsiniz.

Müşteri kullanım attribution için yeni dağıtım ve zaten dağıtılmış var olan kaynakları etiketleme desteklemez.

Müşteri kullanım attribution gerekli [Azure uygulama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer): Çözüm şablonu teklif, Azure Market'te yayımlanan.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma
İş ortağı çözümlerinin çoğu, Resource Manager şablonlarını kullanarak bir müşterinin aboneliğini üzerinde dağıtılır. Azure marketi, GitHub üzerinde veya bir hızlı başlangıç olarak kullanılabilir bir Resource Manager şablonu varsa müşteri kullanım attribution etkinleştirmek için şablonunuzu değiştirme işlemi oldukça olmalıdır.

Oluşturma ve çözüm şablonları yayımlama hakkında daha fazla bilgi için bkz.

* [Oluşturma ve ilk Resource Manager şablonunuzu dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).
* [Azure uygulama teklifi](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer).
* Video: [Çözüm şablonları ve yönetilen uygulamaları Azure Market'e](https://channel9.msdn.com/Events/Build/2018/BRK3603).


## <a name="add-a-guid-to-your-template"></a>Bir GUID şablonunuza ekleyin

Bir genel benzersiz tanıtıcısı (GUID) eklemek için ana şablon dosyası için tek bir değişiklik yapın:

1. [Bir GUID Oluştur](#create-guids) önerilen yöntemi kullanarak ve [, GUID kayda](#register-guids-and-offers).

1. Resource Manager şablonunu açın.

1. Ana Şablon dosyasında yeni bir kaynak ekleyin. Kaynak yer alması gerekir **mainTemplate.json** veya **azuredeploy.json** dosya yalnızca ve, tüm iç içe veya bağlı şablonları.

1. Sonra GUID değeri girin **PID -** önek (örneğin, pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Hatalar için şablonu denetleyin.

1. Şablon uygun depoları yeniden yayımlayın.

1. [Şablon dağıtımı'nda GUID başarısını doğrulama](#verify-the-guid-deployment).

### <a name="sample-resource-manager-template-code"></a>Örnek Resource Manager şablonu kodu

Şablonunuz için izleme kaynakları etkinleştirmek için Kaynaklar bölümü altında aşağıdaki ek kaynak eklemeniz gerekir. Lütfen değiştirdiğinizden emin olun. ana şablon dosyasına eklerken kendi giriş içeren örnek kodlar aşağıda.
Kaynak olarak eklenmesi gerekir **mainTemplate.json** veya **azuredeploy.json** dosya yalnızca ve, tüm iç içe veya bağlı şablonları.

```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>Resource Manager API'leri kullanın

Bazı durumlarda, Azure hizmetlerini dağıtmak için Resource Manager REST API'leri karşı doğrudan çağrı yapmak tercih edebilirsiniz. [Azure, birden çok SDK'ları destekler](https://docs.microsoft.com/azure/#pivot=sdkstools) bu çağrılarını etkinleştirir. Sdk'lardan birini kullanın veya doğrudan kaynakları dağıtmak için REST API'lerini çağırma.

Resource Manager şablonu kullanıyorsanız, daha önce açıklanan yönergeleri izleyerek çözümünüzün etiketlemelisiniz. Değildir ve bir Resource Manager şablonu kullanarak doğrudan API çağrıları yapma, Azure kaynaklarının kullanımını ilişkilendirmek için dağıtım hala etiketleyebilirsiniz.

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Resource Manager API'leri ile bir dağıtım etiketi

Atıf, API çağrıları tasarlarken, müşteri kullanımını etkinleştirmek için istekte kullanıcı aracısının üst bilgisindeki bir GUID içerir. Her teklif için GUID'i veya SKU ekleyin. Biçim dizesi ile **PID -** önek ve iş ortağı tarafından oluşturulan GUID içerir. Kullanıcı Aracısı içine eklenmek için GUID biçimi örneği aşağıda verilmiştir:

![Örnek GUID biçimi](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> Dize biçimi büyük/küçük harf önemlidir. Varsa **PID -** önekini dahil değilse, verileri sorgulamak mümkün değildir. Farklı SDK'ları farklı bir şekilde izleyin. Bu yöntem uygulamak için destek ve yaklaşım izleme için tercih edilen Azure SDK'sını gözden geçirin.

#### <a name="example-the-python-sdk"></a>Örnek: Python SDK'sı

Python için kullanma **config** özniteliği. Öznitelik, yalnızca bir UserAgent için ekleyebilirsiniz. Bir örneği aşağıda verilmiştir:

![Bir kullanıcı aracısı için öznitelik Ekle](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Her istemci için öznitelik ekleyin. Genel statik yapılandırma yoktur. Her istemci izleme emin olmak için bir istemci fabrikası etiketi. Daha fazla bilgi için bkz. Bu [istemci fabrikası örneği github'daki](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79).

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Azure PowerShell kullanarak bir dağıtım etiketi

Kaynakları Azure PowerShell aracılığıyla dağıtırsanız, aşağıdaki yöntemi kullanarak, GUID ekleyin:

```powershell
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Azure CLI kullanarak bir dağıtım etiketi

Azure CLI, GUID eklenecek kullandığınızda, **AZURE_HTTP_USER_AGENT** ortam değişkeni. Bir betik kapsamında bu değişkeni ayarlayabilirsiniz. Ayrıca, değişken Kabuk kapsam için genel olarak ayarlayabilirsiniz:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```
Daha fazla bilgi için [Go için Azure SDK](https://docs.microsoft.com/go/azure/).

## <a name="use-terraform"></a>Terraform kullanarak

Terraform destek Azure sağlayıcısının 1.21.0 kullanılabilir sürüm: [ https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019 ](https://github.com/terraform-providers/terraform-provider-azurerm/blob/master/CHANGELOG.md#1210-january-11-2019).  Bu destek Terraform ile çözüm dağıtan tüm iş ortakları için geçerlidir ve tüm kaynaklar dağıtılır ve Azure sağlayıcısı tarafından ölçülen (1.21.0 sürümü veya üzeri).

Azure Terraform sağlayıcısı adlı yeni bir isteğe bağlı alan eklendiğinde [ *partner_id* ](https://www.terraform.io/docs/providers/azurerm/#partner_id) olduğu izleme çözümünüz için kullandığınız GUID belirttiğiniz yerdir. Bu alanın değeri de kaynağı *ARM_PARTNER_ID* ortam değişkeni.

```
provider "azurerm" {
          subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          client_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          ……
          # new stuff for ISV attribution
          partner_id = “xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"}
```
Müşteri kullanım attribution tarafından izlenen Terraform üzerinden kendi dağıtımını almak isteyen iş ortakları, aşağıdakileri yapmanız gerekir:

* (GUID, her teklif veya SKU için eklenmelidir) bir GUID oluştur
* Değerini ayarlamak için Azure sağlayıcısı güncelleştirme *partner_id* GUID'e (GUID ile "PID-", yalnızca bunu gerçek GUID yok ön düzeltme)

## <a name="create-guids"></a>GUID oluştur

Bir GUID 32 onaltılık basamak olan bir başvuru benzersiz sayıdır. İzleme için GUID'leri oluşturmak için bir GUID Oluşturucu kullanmanız gerekir. Azure depolama ekibi oluşturan bir [GUID generator form](https://aka.ms/StoragePartners) bir GUID biçimi, e-posta gönderilir ve farklı izleme sistemleri arasında yeniden kullanılabilir.

> [!Note]
> Kullanmanızı önemle tavsiye edilir [Azure Depolama'nın GUID generator form](https://aka.ms/StoragePartners) , GUID oluşturmak için. Daha fazla bilgi için müşterilerimize [SSS](#faq).

Her bir teklif ve dağıtım kanalı her ürün için benzersiz bir GUID oluşturun öneririz. Bölünecek raporlama istemiyorsanız ürünün birden çok dağıtım kanalları için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir şablonu kullanarak bir ürünün dağıtılmasını ve Azure Marketi'nde ve GitHub üzerinde kullanılabilir, oluşturabilir ve 2 ayrı GUID'ler kaydedin:

*   Azure Marketi'nde bir ürün
*   Github'da bir ürün

Raporlama GUID'leri ve iş ortağı değeri (Microsoft iş ortağı kimliği) ile gerçekleştirilir.

Ayrıca, GUID'leri SKU'ları bir teklifini türevlerini nerede SKU gibi daha ayrıntılı bir düzeyde izleyebilirsiniz.

## <a name="register-guids-and-offers"></a>YAZMAÇ GUID'leri ve teklifler

Müşteri kullanım attribution GUID'leri kaydedilmesi gerekir.

Tüm kayıtlar için şablon GUID'leri Azure Market bulut iş ortağı portalı (CPP) aracılığıyla gerçekleştirilir.

Şablonunuz için veya kullanıcı aracısı GUID ekleme ve CPP GUID kayda sonra tüm dağıtımlar izlenir.

1. Geçerli [Azure Marketi](https://aka.ms/listonazuremarketplace) ve CPP erişin.

   * İş ortakları için gerekli [CPP içinde bir profille](https://docs.microsoft.com/azure/marketplace/become-publisher). Teklife Azure Market veya Appsource'ta listelemek için önerilir.
   * İş ortakları, birden çok GUID'yi kaydedebilirsiniz.
   * İş ortakları, Market dışı çözüm şablonları ve teklifler için bir GUID kaydedebilirsiniz.

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).

1. Sağ üst köşede, hesap simgesini ve ardından **yayımcı profilinizle**.

   ![Yayımcı profilini seçin](media/marketplace-publishers-guide/guid-image-for-lu.png)

1. Üzerinde **profil sayfası**seçin **izleme GUID ekleme.**

   ![GUID izleme Ekle'yi seçin](media/marketplace-publishers-guide/guid-how-to-add-tracking.png)

1. İçinde **izleme GUID** kutusunda, uygulamanızın izleme girin GUID. Yalnızca olmadan GUID girin **PID -** önek. İçinde **özel açıklama** kutusunda, teklif adı ve açıklama girin.

   ![Profil sayfası](media/marketplace-publishers-guide/guid-dev-center-login.png)

   ![GUID girin ve teklif açıklaması](media/marketplace-publishers-guide/guid-dev-center-example.png)

1. Birden fazla GUID kaydetmek için işaretleyin **izleme GUID ekleme** yeniden. Ek kutuları sayfada görüntülenir.

   ![İzleme GUID ekleme yeniden seçin](media/marketplace-publishers-guide/guid-dev-center-example-add.png)

   ![Başka bir GUID girin ve teklif açıklaması](media/marketplace-publishers-guide/guid-dev-center-example-description.png)

1. **Kaydet**’i seçin.

   ![Kaydet'i seçin](media/marketplace-publishers-guide/guid-dev-center-save.png)

Şablonunuz için veya kullanıcı aracısı GUID ekleme ve CPP GUID kayda sonra tüm dağıtımlar izlenir.

## <a name="verify-the-guid-deployment"></a>GUID dağıtımı doğrulama

Şablonunuzu değiştirmeniz ve bir test dağıtım'ı çalıştırdıktan sonra dağıtılan ve etiketli kaynakları almak için aşağıdaki PowerShell betiğini kullanın.

GUID, Resource Manager şablonunuzu başarıyla eklendiğini doğrulamak için komut dosyasını kullanabilirsiniz. Betik, Resource Manager API'si veya Terraform dağıtımlar için geçerli değildir.

Azure'da oturum açın. Komut dosyası çalıştırılmadan önce doğrulamak istediğiniz dağıtım ile bir abonelik seçin. Abonelik bağlamında, dağıtım betiği çalıştırın.

**GUID** ve **resourceGroup** dağıtım adı olan gerekli parametreleri.

Alabileceğiniz [özgün komut dosyasının](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) GitHub üzerinde.

```powershell
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzResourceGroupDeployment -ResourceGroupName
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="report"></a>Rapor

İş ortağı merkezi analiz Panonuzda müşteri kullanım attribution için raporu bulabilirsiniz. ([https://partner.microsoft.com/en-us/dashboard/mpn/analytics/CPP/MicrosoftAzure](https://partner.microsoft.com/dashboard/mpn/analytics/CPP/MicrosoftAzure)). Raporu görmek için oturum açmak için iş ortağı merkezi kimlik bilgilerinizi kullanmanız gerekir. Tüm sorunlarla raporla ya da oturum açın, Get destek bölümde izleyerek bir destek isteği oluşturun.

İzlenen şablonu raporu görmek için iş ortağı ilişkilendirme türü açılan listesinde seçin.

![Müşteri kullanım attribution raporu](media/marketplace-publishers-guide/customer-usage-attribution-report.png)

## <a name="notify-your-customers"></a>Müşterilerinizin bildir

İş ortakları, müşterilerine müşteri kullanım attribution kullanan dağıtımlar hakkında bilgilendirmek. Microsoft iş ortağı için bu dağıtımları ile ilişkili Azure kullanım bildiriyor. Aşağıdaki örnekler, müşterileriniz bu dağıtımlar hakkında bilgilendirmek üzere kullanabileceğiniz içerik ekleyin. Örneklerde değiştirin \<iş ortağı > şirket adınızı ile. İş ortakları, bildirim izlemede hariç tutulacak müşteriler için seçenekleri dahil olmak üzere, gizlilik ve koleksiyon ilkeleri, tüm verilerini sahip hizalar emin olmanız gerekir.

### <a name="notification-for-resource-manager-template-deployments"></a>Resource Manager şablon dağıtımları için bildirim

Bu şablonu dağıtırken Microsoft yüklenmesini tanımlayamayabilir \<iş ortağı > yazılım Azure kaynaklarıyla dağıtılır. Microsoft yazılım desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilmesi. Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter.

### <a name="notification-for-sdk-or-api-deployments"></a>SDK'sı veya API'si dağıtımlar için bildirim

Dağıtırken \<iş ortağı > yazılım, Microsoft yüklenmesini tanımlayamayabilir \<iş ortağı > dağıtılan Azure kaynaklarıyla yazılım. Microsoft yazılım desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilmesi. Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter.

## <a name="get-support"></a>Destek alın

Rapor veya iş ortağı merkezi için oturum açma ile herhangi bir sorunla karşılaşırsanız, iş ortağı merkezi destek ekibi ile burada bir destek isteği oluşturun: [https://partner.microsoft.com/en-US/support](https://partner.microsoft.com/support)

![](./media/marketplace-publishers-guide/partner-center-log-in-support.png)

Market ekleme ve/veya müşteri kullanım attribution yardıma ihtiyacınız varsa, bu adımları izleyin.

1. Git [destek sayfasını](https://go.microsoft.com/fwlink/?linkid=844975).

1. Altında **sorun türü**seçin **Market ekleme**.

1. Seçin **kategori** sorununuz için:

   - Kullanım ilişkilendirme sorunlarda seçin **diğer**.
   - Azure Market CPP ile erişim sorunları için seçin **erişim sorunu**.

     ![Sorun kategorisini seçin](media/marketplace-publishers-guide/lu-article-incident.png)

1. Seçin **isteği Başlat**.

1. Sonraki sayfada, gerekli değerleri girin. Seçin **devam**.

1. Sonraki sayfada, gerekli değerleri girin.

   > [!Important]
   > İçinde **olay başlığı** kutusuna **ISV kullanımını izleme**. Ayrıntılı sorununuzu açıklayın.

   ![ISV kullanımını izleme olay başlığını girin](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Formu doldurun ve ardından **Gönder**.

Bir Microsoft iş ortağı teknik anlamak ve müşteri kullanım attribution birleştirmek için Danışmanı teknik satış öncesi, dağıtım ve uygulama geliştirme senaryoları için teknik Rehber de alabilir.

### <a name="how-to-submit-a-technical-consultation-request"></a>Teknik danışmanlığı talebinizi nasıl

1. Ziyaret [ https://aka.ms/TechnicalJourney ](https://aka.ms/TechnicalJourney).
1. Teknik yolculuğunuza görüntülemek için bulut altyapısı seçin ve yönetim ve yeni bir sayfa açılır.
1. Dağıtım hizmetler gönderme isteği düğmesine tıklayın.
1. MSA (MPN hesabı) veya AAD'nizi (iş ortağı Pano hesap) kullanarak oturum açın oturum açma kimlik bilgileriniz bağlı olarak, bir çevrimiçi istek formunu açar:
    * Tamamlamak/kişi bilgilerini gözden geçirin.
    * Danışmanlığı ayrıntılarını önceden doldurulmuş ya da açılan menüden seçin.
    * Bir başlık ve sorunun açıklamasını girin (sağlamak mümkün olduğunca çok ayrıntı).
1. Gönder'e tıklayın

Görüntüleme ekran görüntüleri içeren yönergeler [ https://aka.ms/TechConsultInstructions ](https://aka.ms/TechConsultInstructions).

### <a name="whats-next"></a>Yenilikler

Bir Microsoft iş ortağı teknik gereksinimlerinizi kapsamını belirlemek için aramayı ayarlamak için Danışman tarafından kurulur.

## <a name="faq"></a>SSS

**Şablona GUID ekleme avantajı nedir?**

Microsoft iş ortaklarına kendi çözümlerini ve ınsights müşteri dağıtımları görünümünü influenced kullanımlarına sağlar. Hem Microsoft hem de iş ortağı satış ekipleri arasında daha yakından engagement sürücüsü için bu bilgileri kullanabilirsiniz. Hem Microsoft hem de iş ortağı verileri tek bir ortağın etkisi daha tutarlı bir görünüm Azure büyüme üzerinde almak için kullanabilirsiniz.

**Bir GUID eklendikten sonra değiştirilebilir mi?**

Evet, müşteri veya uygulama iş ortağı şablonu özelleştirmek ve değiştirebilir veya GUID kaldırabilirsiniz. İş ortaklarının müşterileri ve iş ortakları kaldırma veya düzenleme GUID önlemek için kaynak GUID ve role proaktif olarak açıklamak öneririz. GUID değiştirilmesi, yalnızca yeni, var olmayan dağıtımlar ve kaynakları etkiler.

**Ben bir GitHub gibi Microsoft dışı deposundan dağıtılan şablonları izleyebilir miyim?**

Evet, GUID şablon dağıtıldığında mevcut olduğu sürece, kullanım izlenir. İş ortakları, Azure Marketi dışında dağıtım için kullanılan GUID'leri kaydetmek için CPP profil sağlamak için gereklidir.

**Müşteri de raporlama alıyor?**

Müşteriler kullanımlarını kaynakların veya müşteri tarafından tanımlanan kaynak grupları Azure portalında izleyebilirsiniz.

**Bu Metodoloji dijital iş ortağı, kaydı (DPOR için) benzer?**

Bu yeni kullanım ve dağıtım için bir iş ortağı çözümünün bağlanma yöntemini Azure kullanımı için bir iş ortağı çözümü bağlamak için bir mekanizma sağlar. Aboneliğe DPOR danışmanlık (sistem tümleştiricisi) ya da Yönetim (yönetilen hizmet sağlayıcısı) ortak bir müşterinin Azure aboneliğiyle ilişkilendirmek için tasarlanmıştır.

**Azure Depolama'nın GUID Oluşturucu form kullanmanın avantajı nedir?**

Azure Depolama'nın GUID Oluşturucu formunu gerekli biçiminde bir GUID oluşturun garanti edilir. Ayrıca, herhangi bir Azure Depolama'nın veri düzlemi izleme yöntemlerini kullanıyorsanız, Market denetim düzlemi izleme için aynı GUID yararlanabilirsiniz. Bu, iş ortağı attribution için tek bir birleştirilmiş GUID ayrı GUID'ler korumak zorunda kalmadan yararlanmanıza olanak sağlar.

**Azure Marketi'nde bir çözüm şablonu teklifi için özel, özel bir VHD kullanabilir miyim?**

Hayır. Sanal makine görüntüsünü Azure Marketi'nde gelir, bkz: [ https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines ](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines).

Markette özel VHD'nizi kullanarak bir VM teklifi oluşturma ve böylece kimse görebilirsiniz özel olarak işaretleyin. Ardından bu VM için çözüm şablonunda başvuru.

**Güncelleştirilemedi *contentVersion* ana şablonunun?**

Olası bir hatayı herhangi bir nedenden dolayı eski contentVersion bekleyen başka bir şablondan bir TemplateLink kullanarak şablon dağıtıldığında bazı durumlarda. Geçici çözüm, meta veri özelliğini kullanmaktır:

```
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "metadata": {
        "contentVersion": "1.0.1.0"
    },
    "parameters": {
```
