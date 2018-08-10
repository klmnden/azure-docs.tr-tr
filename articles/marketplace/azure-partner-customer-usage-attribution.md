---
title: Azure iş ortağı ve müşteri kullanım attribution
description: Azure Marketi çözümleri için müşteri kullanımını izlemenizi nasıl genel bakış
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/26/2018
ms.author: ellacroi
ms.openlocfilehash: 95ad327380707dcfe14aa5aa3d91b8da2309eb05
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630900"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure iş ortağı müşteri kullanım attribution

Azure için bir yazılım ortağı olarak, çözümlerinizi ya da Azure bileşenlerini gerektirir veya doğrudan Azure altyapınıza dağıtılacak.  Bugün, bir müşteri, bir iş ortağı çözümü dağıtır ve Azure kaynaklarını sağlar, Azure büyümeye etkisi, optik öğrenmek zor ve bu dağıtımların durumunu görünürlük elde etmek iş ortağı zor. Daha yüksek bir görünürlük düzeyi ekleyerek Microsoft satış takımlarıyla hizalama ve Microsoft iş ortağı programları için kredi elde iş ortaklarına yardımcı olur.   

Microsoft iş ortaklarının daha iyi bir sonucu bir müşterinin Azure üzerinde yazılım dağıtımı olan Azure kullanımını izlemenize yardımcı olmak için yeni bir yöntem oluşturur. Bu yeni yöntemi, Azure hizmetlerinin dağıtımı düzenlemek için Azure Resource Manager kullanarak temel alır.

Microsoft iş ortağı olarak, bir müşterinin adıma sağlama tüm Azure kaynakları ile Azure kullanım ilişkilendirebilirsiniz.  Bu ilişkilendirme, Azure marketi, hızlı başlangıç depo, özel github depoları ve hatta 1'de 1 müşteri katılımı aracılığıyla gerçekleştirilebilir.  İzlemeyi etkinleştirmek için iki yaklaşım vardır kullanılabilir:

- Azure Resource Manager şablonları: Azure Resource Manager şablonları veya iş ortağının yazılımlarını çalıştırmak üzere Azure hizmetlerini dağıtmak için çözüm şablonları. İş ortakları altyapı ve Azure çözümünüzü yapılandırmasını tanımlayan Azure Resource Manager şablonu oluşturabilirsiniz. Bir Azure Resource Manager şablonu oluşturma, siz ve müşterilerinizin yaşam döngüsü boyunca çözümünüzü dağıtmak ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olmanızı sağlar. 

- Azure Resource Manager API'leri: iş ortakları, doğrudan ya da bir Azure Resource Manager şablonu dağıtmak için Azure Resource Manager API'leri çağırabilirsiniz veya API oluşturmak için Azure Hizmetleri doğrudan sağlamak çağırır. 

## <a name="method-1-azure-resource-manager-templates"></a>1. yöntem: Azure Resource Manager şablonları 

Bugün birçok iş ortağı çözümü, Azure Resource Manager şablonlarını kullanarak bir müşterinin aboneliğini üzerinde dağıtılır.  Azure Marketi'nde, github'da veya bir hızlı başlangıç olarak kullanılabilir bir Azure Resource Manager şablonu zaten varsa bu yeni izleme yöntemi etkinleştirmek için şablonunuzu değiştirme işleminin oldukça olmalıdır.  Bir Azure Resource Manager şablonu bugün kullanmıyorsanız, Azure Resource Manager şablonları ve oluşturmak nasıl daha iyi anlamanıza yardımcı olacak bazı bağlantılar aşağıda verilmiştir: 

*   [Oluşturma ve İlk Azure Resource Manager şablonunuzu dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Azure Marketi için bir çözüm şablonu oluşturmak için kılavuz](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="instructions-add-a-guid-to-your-existing-azure-resource-manager-template"></a>Yönergeler: bir GUID mevcut Azure Resource Manager şablonunuzu ekleyin.

GUID ekleme, tek bir ana şablon dosyasının değiştirilmesini şöyledir:
 1. Oluşturulan değeri eb7927c8-dd66-43e1-b0cf-c346a422063 olduğunu düşünelim, bir GUID oluştur
 2. Azure Resource Manager şablonunu açın
 3. Ana Şablon dosyasında yeni bir kaynak ekleyin. Kaynak yalnızca mainTemplate.json içinde olması gerekir veya tüm içinde değil, azuredeploy.json iç içe veya bağlı şablonları.
 4. GUID "PID-sonra" yukarıda da gösterildiği gibi girin.

   Bu örnek gibi görünmelidir: `pid-eb7927c8-dd66-43e1-b0cf-c346a422063`

 5. Şablonu hatalar için denetleyin
 6. Şablon uygun depoları yeniden yayımlayın

## <a name="sample-template-code"></a>Örnek şablon kodu

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/tracking-sample-code-for-lu-1.PNG?token=Ak8ZDB0JzsBdUGlKEIeHNJRS7b0BWn4Gks5bbMwwwA%3D%3D)


## <a name="method-2-azure-resource-manager-apis"></a>Yöntem 2: Azure Resource Manager API'leri

Bazı durumlarda, Azure hizmetlerini dağıtmak için Azure Resource Manager REST API'leri karşı doğrudan çağrı yapmak tercih edebilirsiniz. [Azure, birden çok SDK'ları destekler](https://docs.microsoft.com/azure/#pivot=sdkstools) bunu etkinleştirmek için.  Sdk'lardan birini kullanın veya doğrudan kaynakları dağıtmak için REST API'lerini çağırma.

Bir Azure Resource Manager şablonu kullanıyorsanız, yukarıdaki yönergeleri kullanarak çözümünüzü etiketlemelisiniz.  Siz değil bir Azure Resource Manager şablonu kullanarak ve doğrudan API çağrıları yapma Azure kaynak kullanımı ilişkilendirmek için dağıtım hala etiketleyebilirsiniz. 

**Azure Resource Manager API'leri kullanarak bir dağıtım etiketleme:** isteğindeki kullanıcı aracısını üst bilgisindeki bir GUID içerecektir, API çağrıları tasarlarken, bu yaklaşım için. SKU veya her teklif için GUID'i eklenmesi gerekir.  Dize öneki ile biçimlendirilmiş olması gerekir PID - ve üretilen iş ortağı GUID.   

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG?token=Ak8ZDDiokRcj4PJj0aMkZmfF8BdOuOTzks5bbM35wA%3D%3D)

>[!Note] 
>Kullanıcı Aracısı içine eklenmek için GUID biçimi: pid-eb7927c8-dd66-43e1-b0cf-c346a422063 / / "PID-sonra", GUID girin

Dize biçimi büyük/küçük harf önemlidir. "PID-" önekini dahil edilmezse, verileri sorgulamak mümkün değildir. Farklı SDK'ları farklı şekilde bunu yapabilirsiniz.  Bu yöntem uygulamak için destek ve yaklaşım için tercih edilen Azure SDK'sını gözden geçirmek gerekir. 

**Python SDK'sını kullanarak bir örnek:** Python için gereken "yapılandırma" özniteliğini kullanın. Yalnızca bir UserAgent için ekleyebilirsiniz. Örnek aşağıda verilmiştir:

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/python-for-lu.PNG?token=Ak8ZDK5Um4J6oY-7x25tuBpa168BEiYMks5bbMuUwA%3D%3D)

>Bu her istemci için yapılması gerekir, genel statik yapılandırma (her istemci çalıştığına emin olmak için bir istemci fabrikası yapmayı tercih. 
>[Ek başvuru bilgileri](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)

Azure PowerShell veya Azure CLI kullanarak bir dağıtım etiketleme: kaynakları AzurePowerShell aracılığıyla dağıtırsanız, aşağıdaki yöntemi kullanarak, GUID ekleyebilirsiniz:

```

[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

Azure CLI'yı kullanırken, GUID ekleme AZURE_HTTP_USER_AGENT ortam değişkenini ayarlayın.  Kapsamdaki bir betiğin veya kabuk kapsam kullanılmak üzere küresel olarak ayarlamak için bu değişkeni ayarlayabilirsiniz:

```

export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'


```

## <a name="registering-guidsoffers"></a>GUID'ler/teklifler kaydediliyor

Bizim izlemede dahil edilecek GUID için sırada bu kayıtlı olması gerekir.  

Tüm kayıtlar için şablon GUID'leri Azure Market bulut iş ortağı portalı (CPP) aracılığıyla gerçekleştirilir. 

Geçerli [Azure Marketi](http://aka.ms/listonazuremarketplace) Bugün ve get bulut iş ortağı portalına erişim.

*   İş ortakları için gerekli olacak [CPP içinde bir profille](https://docs.microsoft.com/azure/marketplace/become-publisher) ve teklife Azure Market veya Appsource'ta listelemek için teşvik 
*   İş ortakları birden çok GUID'yi kaydetmek mümkün olacaktır. 
*   İş ortakları da Market dışı çözüm şablonları/teklifleri için bir GUID kayda mümkün olacaktır

Sonra GUID şablonunuzu veya kullanıcı aracısı eklendi ve tüm dağıtımların izlenen CPP içinde kaydedilen GUID. 

## <a name="verification-of-guid-deployment"></a>GUID dağıtım doğrulama 

Şablonunuzu değiştirilmiş ve test dağıtımı gerçekleştirilen sonra dağıtılan ve etiketli kaynakları almak için aşağıdaki PowerShell betiğini kullanabilirsiniz. 

GUID Azure Resource Manager şablonunuzu başarıyla eklenip eklenmediğini doğrulamak için kullanabilirsiniz. Azure Resource Manager API'si dağıtım için geçerli değildir.

Azure'da oturum açın ve betiği çalıştırmadan önce doğrulamak istediğiniz dağıtım içeren aboneliği seçin. Dağıtım abonelik bağlamında çalıştırılması gerekir.

Dağıtım GUID ve kaynak grubu adı olan gerekli parametreleri.

Özgün komut dosyasının bulabilirsiniz [burada](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1).

```

Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName'
)

#get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

#find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

#find all deploymentOperations in a deployment by name (since PowerShell does not surface outputResources on the deployment or correlationId on the deploymentOperation)

foreach ($deployment in $deployments){

#get deploymentOperations by deploymentName and then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}


```

## <a name="guidance-on-creating-guids"></a>GUID'ler Oluşturma Kılavuzu

Bir GUID (genel benzersiz tanımlayıcı) benzersiz başvuru 32 onaltılık bir sayıdır. Bir GUID oluşturmak için izleme GUID'ler oluşturmak için bir GUID Oluşturucu kullanmanız gerekir.  Vardır birden çok [çevrimiçi GUID oluşturucuları](https://www.bing.com/search?q=guid%20generator&qs=n&form=QBRE&sp=-1&ghc=2&pq=guid%20g&sc=8-6&sk=&cvid=0BAFAFCD70B34E4296BB97FBFA3E1B4E) kullanabilirsiniz.

Her bir teklif ve dağıtım kanalı için benzersiz bir GUID Oluştur önerilir.  Örneğin, varsa iki çözümleri ve her ikisi de bir şablon dağıtılır ve Azure Marketi'nde ve GitHub üzerinde kullanılabilir.  Dört GUID'leri oluşturun:

*   Bir Azure Marketi'nde teklif 
*   Github'da bir teklif
*   Azure Marketi'nde teklif B 
*   Github'da teklif B

Raporlama iş ortağı (Microsoft iş ortağı kimliği) ve GUID tarafından gerçekleştirilir. 

Daha ayrıntılı bir düzeyde başka bir deyişle, (burada bir teklifini türevlerini SKU'lar) SKU'su GUID'leri izlemek seçebilirsiniz.

## <a name="guidance-on-privacy-and-data-collection"></a>Gizlilik ve veri toplama hakkında rehberlik

İş ortakları, Azure Resource Manager GUID izleme içeren dağıtımlar iş ortağına bu dağıtımların ile ilişkili Azure kullanım bildirmek için Microsoft'a izin veren müşterileri bilgilendirmek için Mesajlaşma sağlamanız gerekir.  Aşağıda bazı örnek dil verilmiştir. "İş ortağı" gösterir burada kendi şirket adınızı doldurmanız gerekir. Ayrıca, iş ortakları, dil ile kendi verilerini kanaldan hariç tutulacak müşteriler için seçenekleri dahil olmak üzere, gizlilik ve koleksiyon ilkeleri hizalar emin olun: 

**Azure Resource Manager şablon dağıtımları için**

Bu şablonu dağıtırken, Microsoft mümkün olacaktır dağıtılan Azure kaynaklarıyla ortak yazılım yüklemesi tanımlayın.  Microsoft yazılımı desteklemek için kullanılan Azure kaynakları ilişkilendirebilmesi olacaktır.  Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Bu veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter. 

**SDK'sı veya API'si dağıtımları için**

İş ortağı yazılım dağıtma, Microsoft ne zaman iş ortağı yazılım yüklemesi dağıtılan Azure kaynaklarıyla belirleyin.  Microsoft yazılımı desteklemek için kullanılan Azure kaynakları ilişkilendirebilmesi olacaktır.  Microsoft, ürünlerinde en iyi deneyimleri sağlamak ve işlerini çalıştırmak için bu bilgileri toplar. Bu veriler toplanır ve adresinde bulunabilir Microsoft'un gizlilik ilkeleri tarafından yönetilen https://www.microsoft.com/trustcenter.

## <a name="support"></a>Destek

İzleyin Yardım almak için aşağıdaki adımları:
 1. Bulunan destek sayfasını ziyaret edin [go.microsoft.com/fwlink/?linkid=844975](https://go.microsoft.com/fwlink/?linkid=844975)
 2. Kullanım ilişkilendirmesi ile-ilgili sorunları için sorun türü seçin: **Market ekleme** ve Kategori: **diğer** ve ardından **başlatma isteği.** 
>[!Note]
>Azure Market bulut iş ortağı portalı - select sorun türü erişim sorunları için: **Market ekleme** ve Kategori: **erişim sorunu** ve ardından **başlatma isteği.**
 3. Sonraki sayfada gerekli alanları doldurun ve tıklayın **devam.**
 4. Sonraki sayfada serbest metin alanları doldurun.  
 

 
>[!Important] 
>Olay başlığı ile doldurun **"ISV kullanımını izleme"** ve sonra büyük serbest metin alanına ayrıntılı sorununuzu açıklayın.  Formu tamamlayın ve tıklayın **Gönder**.

## <a name="faqs"></a>SSS

**Şablona GUID ekleme avantajı nedir?**

Microsoft iş ortakları kendi şablonlarını ve ınsights müşteri dağıtımları görünümünü influenced kullanımlarına sağlar.  Ayrıca hem Microsoft hem de iş ortağı satış ekipleri arasında daha yakından engagement sürücüsü için bu bilgileri kullanabilirsiniz. Ayrıca hem Microsoft hem de iş ortağı Azure büyüme üzerinde tek bir ortağın etkisi daha tutarlı bir görünümünü almak için kullanabilirsiniz. 

**Kimin bir GUID bir şablona ekleyebilir miyim?**

İzleme kaynak müşterileri için Azure iş ortağı çözümünün bağlanmak için hedeflenen kullanım.  Kullanım verileri için bir iş ortağının Microsoft iş ortağı ağı kimliği (MPN kimliği) bağlıdır ve raporlama iş ortaklarına bulut iş ortağı portalı (CPP) kullanıma sunulacaktır.  

**Bir GUID eklendikten sonra değiştirilebilir mi?**
 
Evet, müşteri veya uygulama iş ortağı şablonu özelleştirmek ve değiştirebilir veya GUID kaldırın. İş ortaklarının müşterileri ve iş ortakları kaldırma veya düzenleme izleme GUID önlemek için kaynak GUID ve role proaktif olarak açıklamak öneririz.  GUID değiştirmek, yalnızca yeni, var olmayan dağıtımlar ve kaynakları etkiler.

**Ne zaman raporlama kullanıma sunulacak?**

Raporlama bir beta sürümünü hemen kullanılabilir olması gerekir.  Raporlama, bulut iş ortağı portalı (CPP) ile tümleştirilecektir.

**Ben bir GitHub gibi Microsoft dışı deposundan dağıtılan şablonları izleyebilir miyim?**

Evet, GUID şablon dağıtıldığında mevcut olduğu sürece, kullanım izlenir.  
İş ortakları, Azure Marketi dışında yayımlanan ilgili şablonlarını kaydetmek için bulut iş ortağı Portalı'nda bir profile sahip için gereklidir. 

**Şablonu Azure Market'ten GitHub gibi başka depolar ve dağıtılması durumunda bir fark var mı?**

Evet, teklifleri Azure Market'te yayımlama iş ortakları, Azure Market'ten dağıtımları hakkında daha ayrıntılı veri alabilirsiniz.  İş ortakları, müşteriler Azure Marketi portalı ve Azure Yönetim Portalı'nda, teklif açıklamanızı yararlı olacaktır. Azure marketi'ndeki teklif ayrıca iş ortağı için adayları.

**Peki bireysel müşteri katılımı için özel bir şablon oluşturabilirim?**

GUID şablona eklemek hala kabul edilir.  Kaydedilen varolan bir GUID'si kullanırsanız, raporlama dahil edilir.  Yeni bir GUID oluşturursanız, yeni bir GUID izlemede dahil almak için kaydolun gerekecektir.

**Müşteri de raporlama alıyor?**

Müşteriler şu anda kaynakların müşteri tanımlı kaynak grupları Azure Yönetim Portalı ve bunların kullanımını izleme olanağına sahip olursunuz.   

**Bu izleme Metodoloji dijital iş ortağı, kaydı (DPOR için) benzer?**

Kullanım ve dağıtım için bir iş ortağı çözümünün bağlanma yeni bu yöntem, Azure kullanımı için bir iş ortağı çözümü bağlamak için bir mekanizma sağlamanız yöneliktir.  Aboneliğe DPOR danışmanlık (sistem tümleştiricisi) ya da Yönetim (yönetilen hizmet sağlayıcısı) ortak bir müşterinin Azure aboneliğiyle ilişkilendirmek için tasarlanmıştır.   
