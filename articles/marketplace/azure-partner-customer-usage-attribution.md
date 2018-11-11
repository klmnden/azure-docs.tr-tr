---
title: Azure iş ortağı ve müşteri kullanım attribution
description: Azure Marketi çözümleri için müşteri kullanımını izlemenizi nasıl genel bakış
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: yijenj
ms.openlocfilehash: 7937f3d0db414d7a9cc2adaefd4324d49d734fcb
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51280682"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure iş ortağı müşteri kullanım attribution

Bir yazılım iş ortağı Azure çözümlerinizi Azure bileşenlerini gerektirir veya doğrudan Azure altyapısının üzerinde dağıtılması gerekir. Bir iş ortağı çözümü dağıtmak ve kendi Azure kaynaklarını hazırlayacak müşteriler dağıtım durumunu görmenize zor ve optik Azure büyümesi üzerindeki etkisi içine alın. Daha yüksek bir görünürlük düzeyi eklediğinizde, Microsoft satış takımlarıyla Hizala ve Microsoft iş ortağı programları için kredi elde edin.   

Microsoft artık ortakları Azure kullanımınızın müşteri dağıtımları yazılımlarını Azure üzerinde daha iyi izlemek için bir yöntem sunar. Azure Hizmetleri dağıtımını düzenlemek için Azure Resource Manager yeni yöntemi kullanır.

Microsoft iş ortağı olarak, Azure kullanımı, bir müşterinin adıma sağlamanız tüm Azure kaynakları ile ilişkilendirebilirsiniz. Azure marketi, hızlı başlangıç depo, özel GitHub depoları ve bire bir müşteri katılımı ile ilişki kurabilir. İzlemeyi etkinleştirmek için iki yaklaşım kullanılabilir:

- Azure Resource Manager şablonları: Resource Manager şablonları ya da iş ortağının yazılımlarını çalıştırmak üzere Azure hizmetlerini dağıtmak için çözüm şablonları. İş ortakları, altyapı ve bunların Azure çözüm yapılandırmasını tanımlamak için bir Resource Manager şablonu oluşturabilirsiniz. Siz ve müşterilerinizin yaşam döngüsü boyunca çözümünüzü dağıtmak Resource Manager şablonu sağlar. Kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz. 

- Azure Resource Manager API'leri: İş ortakları doğrudan bir Resource Manager şablonu dağıtma veya doğrudan Azure hizmetleri sağlamak için API çağrıları oluşturmak için Resource Manager API'leri çağırabilirsiniz. 

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma

İş ortağı çözümlerinin çoğu, Resource Manager şablonlarını kullanarak bir müşterinin aboneliğini üzerinde dağıtılır. Azure marketi, GitHub üzerinde veya bir hızlı başlangıç olarak kullanılabilir bir Resource Manager şablonu varsa yeni izleme yöntemini etkinleştirmek için şablonunuzu değiştirme işlemi oldukça olmalıdır. Bir Azure Resource Manager şablonu kullanmıyorsanız, Resource Manager şablonları ve oluşturmak nasıl daha iyi anlamanıza yardımcı olacak bazı bağlantılar aşağıda verilmiştir: 

*   [Oluşturma ve ilk Resource Manager şablonunuzu dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Bir çözüm şablonu oluşturmak için Azure Marketi](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

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
Lütfen değiştirdiğinizden emin olun. ana şablon dosyasına eklerken kendi giriş içeren örnek kodlar aşağıda.
Kaynak olarak eklenmesi gerekir **mainTemplate.json** veya **azuredeploy.json** dosya yalnızca ve, tüm iç içe veya bağlı şablonları.
```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the mainTemplate.json (do not add the entire file)
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

Bu izleme yaklaşımı için bir GUID, API çağrıları tasarlarken isteğindeki kullanıcı aracısını üst bilgisindeki içerir. Her teklif için GUID'i veya SKU ekleyin. Biçim dizesi ile **PID -** önek ve iş ortağı tarafından oluşturulan GUID içerir. Kullanıcı Aracısı içine eklenmek için GUID biçimi örneği aşağıda verilmiştir: 

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

```
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Azure CLI kullanarak bir dağıtım etiketi

Azure CLI, GUID eklenecek kullandığınızda, **AZURE_HTTP_USER_AGENT** ortam değişkeni. Bir betik kapsamında bu değişkeni ayarlayabilirsiniz. Ayrıca, değişken Kabuk kapsam için genel olarak ayarlayabilirsiniz:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```

## <a name="create-guids"></a>GUID oluştur

Bir GUID 32 onaltılık basamak olan bir başvuru benzersiz sayıdır. İzleme için GUID'leri oluşturmak için bir GUID Oluşturucu kullanmanız gerekir. Azure depolama ekibi oluşturan bir [GUID generator form](https://aka.ms/StoragePartners) bir GUID biçimi, e-posta gönderilir ve farklı izleme sistemleri arasında yeniden kullanılabilir. 

> [!Note]
> Yüksek oranda kullanmanızı öneririz. olduğu [Azure Depolama'nın GUID generator form](https://aka.ms/StoragePartners) , GUID oluşturmak için. Daha fazla bilgi için müşterilerimize [SSS](#faq).

Her bir teklif ve dağıtım kanalı için benzersiz bir GUID oluşturun. Bir şablon kullanarak iki çözümleri dağıtmak ve Azure Marketi'nde ve GitHub üzerinde her biri kullanılabilir ise, dört GUID'leri oluşturmanız gerekir:

*   Bir Azure Marketi'nde teklif 
*   Github'da bir teklif
*   Azure Marketi'nde teklif B 
*   Github'da teklif B

Raporlama, iş ortağı değeri (Microsoft iş ortağı kimliği) ve GUID tarafından gerçekleştirilir. 

Ayrıca, GUID'leri SKU'ları bir teklifini türevlerini nerede SKU gibi daha ayrıntılı bir düzeyde izleyebilirsiniz.

## <a name="register-guids-and-offers"></a>YAZMAÇ GUID'leri ve teklifler

Bir GUID de bizim izleme dahil etmek için GUID kayıtlı olması gerekir.  

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

GUID, Resource Manager şablonunuzu başarıyla eklendiğini doğrulamak için komut dosyasını kullanabilirsiniz. Betik, Resource Manager API'si dağıtım için geçerli değildir.

Azure'da oturum açın. Komut dosyası çalıştırılmadan önce doğrulamak istediğiniz dağıtım ile bir abonelik seçin. Abonelik bağlamında, dağıtım betiği çalıştırın.

**GUID** ve **resourceGroup** dağıtım adı olan gerekli parametreleri.

Alabileceğiniz [özgün komut dosyasının](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) GitHub üzerinde.

```
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="notify-your-customers"></a>Müşterilerinizin bildir

İş ortakları, müşterilerine Resource Manager izleme GUID kullanan dağıtımlar hakkında bilgilendirmek. Microsoft iş ortağı için bu dağıtımları ile ilişkili Azure kullanım bildiriyor. Aşağıdaki örnekler, müşterileriniz bu dağıtımlar hakkında bilgilendirmek üzere kullanabileceğiniz içerik ekleyin. Örneklerde değiştirin \<iş ortağı > şirket adınızı ile. İş ortakları, bildirim izlemede hariç tutulacak müşteriler için seçenekleri dahil olmak üzere, gizlilik ve koleksiyon ilkeleri, tüm verilerini sahip hizalar emin olmanız gerekir. 

### <a name="notification-for-resource-manager-template-deployments"></a>Resource Manager şablon dağıtımları için bildirim

Bu şablonu dağıtırken Microsoft yüklenmesini tanımlayamayabilir \<iş ortağı > yazılım Azure kaynaklarıyla dağıtılır. Microsoft yazılım desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilmesi. Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter. 

### <a name="notification-for-sdk-or-api-deployments"></a>SDK'sı veya API'si dağıtımlar için bildirim

Dağıtırken \<iş ortağı > yazılım, Microsoft yüklenmesini tanımlayamayabilir \<iş ortağı > dağıtılan Azure kaynaklarıyla yazılım. Microsoft yazılım desteklemek için kullanılan Azure kaynaklarını ilişkilendirebilmesi. Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter.

## <a name="get-support"></a>Destek alın

Yardıma ihtiyacınız varsa, bu adımları izleyin.

1. Git [destek sayfasını](https://go.microsoft.com/fwlink/?linkid=844975). 

1. Altında **sorun türü**seçin **Market ekleme**.

1. Seçin **kategori** sorununuz için:

   - Kullanım ilişkilendirme sorunlarda seçin **diğer**.
   - Azure Market CPP ile erişim sorunları için seçin **erişim sorunu**.
   
    ![Sorun kategorisini seçin](media/marketplace-publishers-guide/lu-article-incident.png)

1. Seçin **isteği Başlat**.

1. Sonraki sayfada, gerekli değerleri girin. **Devam**'ı seçin.

1. Sonraki sayfada, gerekli değerleri girin.

   > [!Important] 
   > İçinde **olay başlığı** kutusuna **ISV kullanımını izleme**. Ayrıntılı sorununuzu açıklayın.
   
   ![ISV kullanımını izleme olay başlığını girin](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Formu doldurun ve ardından **Gönder**.

## <a name="faq"></a>SSS

**Şablona GUID ekleme avantajı nedir?**

Microsoft iş ortakları kendi şablonlarını ve ınsights müşteri dağıtımları görünümünü influenced kullanımlarına sağlar. Hem Microsoft hem de iş ortağı satış ekipleri arasında daha yakından engagement sürücüsü için bu bilgileri kullanabilirsiniz. Hem Microsoft hem de iş ortağı verileri tek bir ortağın etkisi daha tutarlı bir görünüm Azure büyüme üzerinde almak için kullanabilirsiniz. 

**Kimin bir GUID bir şablona ekleyebilir miyim?**

İzleme kaynak iş ortağı çözümünün müşterinin Azure kullanımına bağlanmak için tasarlanmıştır. Kullanım verileri, bir iş ortağının Microsoft iş ortağı ağı kimliğini (MPN kimliği) bağlıdır. Raporlama, CPP iş ortakları için kullanılabilir.

**Bir GUID eklendikten sonra değiştirilebilir mi?**
 
Evet, müşteri veya uygulama iş ortağı şablonu özelleştirmek ve değiştirebilir veya GUID kaldırabilirsiniz. İş ortaklarının müşterileri ve iş ortakları kaldırma veya düzenleme izleme GUID önlemek için kaynak GUID ve role proaktif olarak açıklamak öneririz. GUID değiştirilmesi, yalnızca yeni, var olmayan dağıtımlar ve kaynakları etkiler.

**Ne zaman raporlama kullanıma sunulacak?**

Raporlama bir beta sürümünü hemen kullanılabilir olması gerekir. Raporlama CPP tümleştirilecektir.

**Ben bir GitHub gibi Microsoft dışı deposundan dağıtılan şablonları izleyebilir miyim?**

Evet, GUID şablon dağıtıldığında mevcut olduğu sürece, kullanım izlenir. İş ortakları, Azure Marketi dışında yayımlanan ilgili şablonları kaydetmek için CPP profil sağlamak için gereklidir. 

**Şablonu Azure Market'ten GitHub gibi başka depolar ve dağıtılması durumunda bir fark var mı?**

Evet, teklifleri Azure Market'te yayımlama iş ortakları, Azure Market'ten dağıtımları hakkında daha ayrıntılı veri alabilirsiniz. İş ortakları, müşteriler Azure Marketi portalı hem de Azure portalında, teklif açıklamanızı yararlanır. Azure Marketi'ndeki teklif ayrıca iş ortağı için adayları.

**Peki bireysel müşteri katılımı için özel bir şablon oluşturabilirim?**

GUID şablona eklemek hala Hoş Geldiniz. Mevcut bir kayıtlı GUID kullanırsanız, raporlama dahil edilir. Yeni bir GUID oluşturursanız, izleme dahil etmek için yeni bir GUID'ı kaydetmeniz gerekir.

**Müşteri de raporlama alıyor?**

Müşteriler kullanımlarını kaynakların veya müşteri tarafından tanımlanan kaynak grupları Azure portalında izleyebilirsiniz.   

**Bu izleme Metodoloji dijital iş ortağı, kaydı (DPOR için) benzer?**

Bu yeni kullanım ve dağıtım için bir iş ortağı çözümünün bağlanma yöntemini Azure kullanımı için bir iş ortağı çözümü bağlamak için bir mekanizma sağlar. Aboneliğe DPOR danışmanlık (sistem tümleştiricisi) ya da Yönetim (yönetilen hizmet sağlayıcısı) ortak bir müşterinin Azure aboneliğiyle ilişkilendirmek için tasarlanmıştır.   

**Azure Depolama'nın GUID Oluşturucu form kullanmanın avantajı nedir?**

Azure Depolama'nın GUID Oluşturucu formunu gerekli biçiminde bir GUID oluşturun garanti edilir. Ayrıca, herhangi bir Azure Depolama'nın veri düzlemi izleme yöntemlerini kullanıyorsanız, Market denetim düzlemi izleme için aynı GUID yararlanabilirsiniz. Bu, iş ortağı attribution için tek bir birleştirilmiş GUID ayrı GUID'ler korumak zorunda kalmadan yararlanmanıza olanak sağlar.
