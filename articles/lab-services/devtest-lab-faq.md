---
title: Azure DevTest Labs SSS | Microsoft Docs
description: Azure DevTest Labs hakkında sık sorulan sorulara yanıtlar bulun.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: d8fc929b21bedcb3e7e2bd3f5ed1d6c867bca3c8
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803383"
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs SSS
Azure DevTest Labs hakkında en yaygın soruların yanıtlarını alın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Genel**

## <a name="blog-post"></a>Blog gönderisi
DevTest Labs takım blogumuzda 20 Mart 2019'ten itibaren kullanımdan kaldırılmıştır. 

### <a name="where-can-i-track-feature-updates-going-forward"></a>Bundan sonra özellik güncelleştirmeleri nerede izleyebilir miyim?
Bundan sonra biz özellik güncelleştirmeleri ve/veya bilgilendirici blog gönderilerini Azure blogunda gönderme ve Azure güncelleştirir. Bu blog gönderileri, ayrıca gerekli yerlerde belgelerimize bağlayacaksınız.

Abone [DevTest Labs Azure blogunu](https://azure.microsoft.com/blog/tag/azure-devtest-labs/) ve [DevTest Labs Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=devtest-lab) DevTest Labs'de yeni özellikler hakkında bilgi sahibi olmak için.

### <a name="what-happens-to-the-existing-blog-posts"></a>Mevcut blog gönderilerine yapılan ne olacak?
Şu anda (kesinti güncelleştirmeler hariç) geçirme mevcut blog gönderilerine yönelik çalışmalarımız bizim [DevTest Labs belgeleri](devtest-lab-overview.md). MSDN blog'u kullanım dışı olduğunda için DevTest Labs belgeleri genel bakış yönlendirilirsiniz. Yeniden yönlendirilen sonra 'Filtresi tarafından' başlığında aradığınız makalesi için arama yapabilirsiniz. Biz tüm gönderileri, henüz geçirilmeyen henüz ancak bu ay sonuna kadar yapılması gerektiğini unutmayın. 


### <a name="where-do-i-see-outage-updates"></a>Kesinti güncelleştirmeleri nerede görebilirim?
Biz ileriye dönük bizim Twitter tanıtıcısı kullanarak kesinti güncelleştirmeler yayınlayarak. Bizi kesintiler ve bilinen hatalar en son güncelleştirmeleri almak için Twitter'da takip edin.

### <a name="twitter"></a>Twitter 
Bizim Twitter tanıtıcısı: [@azlabservices](https://twitter.com/azlabservices)

## <a name="what-if-my-question-isnt-answered-here"></a>Peki sorumun cevabı burada bulamadığınız?
Sorunuzu burada listelenmiyorsa, bize bildirin ve yanıt bulmanıza yardımcı olabiliriz.

* Bu SSS, sonunda bir soru gönderin. Azure Cache takım ve diğer topluluk üyelerinin bu makaleyle ilgili etkileşim kurun.
* Geniş bir kitleye ulaşmak için bir soru gönderin [Azure DevTest Labs MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Azure DevTest Labs takım ve diğer topluluk üyelerinin ile etkileşim kurun.
* Özellik istekleri için istek ve fikirleri için gönderme [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Azure DevTest Labs neden kullanmalıyım?
Azure DevTest Labs, takımınızın zamandan ve paradan tasarruf edebilirsiniz. Geliştiriciler, çeşitli farklı tabanlarını kullanarak kendi ortamlarını oluşturabilir. Bunlar ayrıca, yapıtları hızla dağıtın ve uygulamaları yapılandırmak için kullanabilirsiniz. Özel görüntüleri ve formülleri kullanarak sanal makineleri (VM'ler) şablon olarak kaydedin ve kolayca takım arasında yeniden oluşturun. DevTest Labs, yöneticiler israfı azaltın ve bir takımın ortamlarını yönetmek için kullanabilir, Laboratuvar ayrıca çeşitli yapılandırılabilir ilkeler sunar. Bu ilkeler, otomatik kapatma, maliyet eşik, kullanıcı ve en fazla VM boyutu başına en fazla VM içerir. DevTest Labs hakkında daha ayrıntılı açıklama için bkz: [genel bakış](devtest-lab-overview.md) veya [tanıtım videosunu](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>"Sorunsuz Self-Servis" ne demektir?
Sorunsuz Self Servis, geliştiricilere ve test edicilere kendi ortamlarını gerektiğinde oluşturmak anlamına gelir. Yöneticiler, DevTest Labs harcamaların azaltılmasına ve maliyetlerin en aza yardımcı olduğunu bilmesinin güvenliğe sahip. Yöneticiler, en fazla VM sayısı, hangi VM boyutlarının izin verilir ve VM'ler çalışmaya ve kapatma zaman belirtebilirsiniz. DevTest Labs ayrıca nasıl Laboratuvar kaynaklarını kullanıldığını haberdar olmanıza yardımcı olmak için uyarılar ayarlayın ve maliyetleri izleme kolaylaştırır.

## <a name="how-can-i-use-devtest-labs"></a>DevTest Labs nasıl kullanabilirim?
DevTest Labs, geliştirme gerektirir veya test ortamları ve hızla yeniden oluşturmak veya maliyet tasarrufu ilkeleri kullanarak yönetmek istediğiniz zaman yararlıdır.

Müşterilerimiz için DevTest Labs kullanan bazı senaryolar aşağıda verilmiştir:

* Geliştirme yönetme ve test ortamları tek bir yerde. Maliyetleri azaltmak ve paylaşmak için özel görüntü oluşturma için ilkeleri kullanma takım arasında oluşturur.
* Geliştirme aşamaları boyunca disk durumunu kaydetmek için özel görüntüleri kullanarak bir uygulama geliştirin.
* Maliyet ilerleme durumu ile ilgili olarak izleyin.
* Kalite güvencesi test etmek için yığın test ortamları oluşturun.
* Yapıtlar ve formülleri kolayca yapılandırmak ve çeşitli ortamlarda bir uygulama oluşturmak için kullanın.
* Sonları gerçekleştirilen HACK (işbirliğine dayalı geliştirme veya test çalışma) için sanal makineleri dağıtmak ve olay sona erdiğinde daha sonra kolayca bunları sağlamasını kaldırma.

## <a name="how-am-i-billed-for-devtest-labs"></a>DevTest Labs için nasıl faturalandırılırım?
DevTest Labs ücretsiz bir hizmettir. Laboratuvarları oluşturma ve ilkeleri, şablonlar ve yapıtlar DevTest Labs'de yapılandırma ücretsizdir. Yalnızca VM'ler, depolama hesapları ve sanal ağlar gibi laboratuvarlarınızı kullanılan Azure kaynakları için ödeme yaparsınız. Laboratuvar kaynaklarını maliyeti hakkında daha fazla bilgi için bkz. [Azure DevTest Labs fiyatlandırma](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Güvenlik**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Farklı güvenlik düzeylerine DevTest Labs nedir?
Güvenlik erişimi göre belirlenir [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/built-in-roles.md). Erişimi nasıl çalıştığını öğrenmek için bu izin, bir rolü olan ve bir kapsam arasındaki farkları öğrenmek için RBAC tarafından tanımlandığı şekilde yardımcı olur.

* **İzni**: Bir izin, belirli bir eylem için tanımlanmış bir erişimdir. Örneğin, bir izin, tüm sanal makineler için okuma erişimi olabilir.
* **Rol**: Bir rol, gruplandırılmış ve bir kullanıcıya atanmış izinler kümesidir. Örneğin, bir abonelik sahibi rolüne sahip bir kullanıcı bir Abonelikteki tüm kaynaklara erişebilir.
* **Kapsam**: Bir kapsam, bir Azure kaynak hiyerarşi içinde düzeyidir. Örneğin, bir kapsam bir kaynak grubu, tek bir laboratuvar veya tüm abonelik olabilir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak rolleri iki tür vardır:

* **Laboratuvar sahibi**: Laboratuvar sahibi Laboratuvardaki tüm kaynaklara erişebilir. Laboratuvar sahibi ilkeleri değiştirebilir, okuma ve yazma için herhangi bir VM, sanal ağı değiştirin ve benzeri.
* **Laboratuvar kullanıcı**: Bir laboratuvar kullanıcı Vm'leri, ilkeleri ve sanal ağlar gibi tüm Laboratuvar kaynaklarını görüntüleyebilir. Ancak, bir laboratuvar kullanıcı ilkeleri veya diğer kullanıcılar tarafından oluşturulmuş tüm Vm'leri değiştiremez. 

Ayrıca, DevTest Labs'de özel roller oluşturabilirsiniz. DevTest Labs'de özel roller oluşturma konusunda bilgi almak için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kapsamları hiyerarşik olduğundan, bir kullanıcı belirli bir kapsamda izinlere sahip olduğunda kullanıcı kapsamı içindeki her alt düzey kapsamda Bu izinler otomatik olarak verilir. Örneği için abonelik sahibi rolüne atanmış bir kullanıcı, kullanıcı bir Abonelikteki tüm kaynaklara erişim izni vardır. Bu kaynaklar, tüm sanal makineler, tüm sanal ağları ve tüm labs içerir. Abonelik sahibi, Laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, bunun tersi doğru değildir. Laboratuvar sahibi abonelik düzeyinden daha düşük bir kapsam bir laboratuvar erişebilir. Bu nedenle, Laboratuvar sahibi Vm'leri, sanal ağlar veya Laboratuvar dışında olan diğer kaynakları göremez.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Kullanıcıların belirli bir görevi gerçekleştirmek bir rolü nasıl oluşturulur?
Özel roller oluşturma ve izinleri için rol atama hakkında kapsamlı bir makale için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Bir rol oluşturur, bir komut dosyası örneği aşağıdadır *DevTest Labs kullanıcısı Gelişmiş*, tüm Vm'leri Laboratuvardaki durdurmak ve başlatmak iznine sahip:

    $policyRoleDef = Get-AzRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzRoleDefinition -Role $policyRoleDef  


**CI/CD tümleştirmesi ve Otomasyon**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>DevTest Labs, my CI/CD araç zinciri ile tümleştiriliyor mu?
Azure DevOps kullanıyorsanız, kullanabileceğiniz bir [DevTest Labs görevlerini uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) DevTest labs'deki yayın işlem hattınızı otomatik hale getirmek için. Bu uzantı ile gerçekleştirebileceğiniz görevlerden bazıları şunlardır:

* Oluşturun ve otomatik olarak bir VM dağıtın. VM en son sürümle Azure dosya kopyalama veya PowerShell Azure DevOps Hizmetleri görevleri kullanarak da yapılandırabilirsiniz.
* Daha fazla bilgi için aynı VM'de bir hatayı yeniden oluşturmak için test edildikten sonra otomatik olarak bir VM'nin durumunu yakalayın.
* Sürüm ardışık düzeninin sonunda sanal makine artık gerekli değilse silin.

Aşağıdaki blog teklif rehberlik ve Azure DevOps Hizmetleri Uzantısı kullanma hakkında bilgi gönderir:

* [DevTest Labs ve Azure DevOps uzantısı](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Azure DevOps Services'dan bir DevTest Labs Laboratuvardaki yeni VM dağıtma](https://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [DevTest Labs'de sürekli dağıtımlar için Azure DevOps Services sürüm Yönetimi'ni kullanma](https://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Diğer sürekli tümleştirme (CI) için / sürekli teslim (CD) araç zincirlerinden, aynı senaryoları elde edebileceğiniz dağıtma [Azure Resource Manager şablonları](https://aka.ms/dtlquickstarttemplate) kullanarak [Azure PowerShell cmdlet'lerini](../azure-resource-manager/resource-group-template-deploy.md) ve [.NET SDK'ları](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Ayrıca [için DevTest Labs REST API'leri](https://aka.ms/dtlrestapis) araç zincirinizi ile tümleştirmek için.  


**Sanal makineler**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-page-that-i-see-in-devtest-labs"></a>DevTest Labs'de bkz. sanal makineler sayfasındaki sanal makineleri neden göremiyorum?
DevTest Labs'de bir VM oluşturduğunuzda, bu VM'ye erişmesine izin verilir. Labs sayfasında hem üzerinde VM görüntüleyebileceğiniz **sanal makineler** sayfası. DevTest Labs Laboratuvar kullanıcı rolüne atanan kullanıcılar, Laboratuvar Laboratuvar içinde oluşturulmuş tüm Vm'leri görebilirsiniz **tüm sanal makineler** sayfası. Ancak, DevTest Labs Laboratuvar kullanıcı rolüne sahip kullanıcılar diğer kullanıcıların oluşturduğu VM kaynaklarına okuma erişimi otomatik olarak verilmez. Bu nedenle, bu sanal makineler üzerinde görüntülenmez **sanal makineler** sayfası.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Özel bir görüntü ile bir formül arasındaki fark nedir?
Bir özel görüntü sanal sabit disk (VHD) hizmetidir. Formül, ek ayarlarla yapılandırın ve ardından kaydedebilir ve yeniden bir görüntüsüdür. Özel bir görüntü, aynı temel, sabit görüntüsünü kullanarak çeşitli ortamları hızlıca oluşturmak istiyorsanız tercih edilebilir. Formül, bir sanal ağın veya alt ağın bir parçası olarak veya belirli bir boyutta bir VM ile en son parçaları, sanal Makinenizin yapılandırmasını yeniden oluşturmak istiyorsanız daha iyi olabilir. Daha ayrıntılı bir açıklama için bkz. [özel görüntüleri ve formülleri DevTest labs'deki karşılaştırma](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Nasıl birden çok VM aynı şablondan tek seferde oluşturabilirim?
Aynı anda birden çok VM aynı şablonu oluşturmak için iki seçeneğiniz vardır:
* Kullanabileceğiniz [Azure DevOps görev uzantımızı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks). 
* Yapabilecekleriniz [Resource Manager şablonu oluşturma](devtest-lab-add-vm.md#save-azure-resource-manager-template) bir VM oluştururken ve [Windows PowerShell Resource Manager şablonu dağıtmayı](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Mevcut Azure Vm'lerimi my DevTest Labs Laboratuvar nasıl taşırım?
DevTest Labs mevcut Vm'lerinizi kopyalamak için:

1. Bir PowerShell Betiği kullanarak mevcut sanal makinenizin VHD dosyasını kopyalayın:
   * Kaynak Yöneticisi: [CopyRmVHDFromVMToLab.ps1](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyRmVHDFromVMToLab.ps1)
   * Klasik: [CopyClassicVHDFromVMToLab.ps1](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyClassicVHDFromVMToLab.ps1)
2. [Özel görüntü oluşturma](devtest-lab-create-template.md) DevTest Labs Laboratuvarınızı içinde.
3. Laboratuvarda özel görüntü bir VM oluşturun.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Birden çok disk planlamadığım Vm'lerime ne ekleyebilir miyim?
Evet, sanal makineleriniz için birden fazla disk ekleyebilirsiniz.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Test için bir Windows işletim sistemi görüntüsü kullanmak isterseniz bir MSDN aboneliği satın almanız gerekir mi?
Windows istemci işletim sistemi görüntüleri (Windows 7 veya sonraki bir sürümü), geliştirme veya Azure'da test kullanmak için aşağıdakilerden birini yapmalısınız:

- [Bir MSDN aboneliği satın alma](https://www.visualstudio.com/products/how-to-buy-vs).
- Bir Kurumsal Sözleşme yaptıysanız, bir Azure aboneliği ile oluşturma [Kurumsal geliştirme ve Test teklifi](https://azure.microsoft.com/offers/ms-azr-0148p).

Her MSDN teklifi için Azure KREDİLERİ hakkında daha fazla bilgi için bkz. [Visual Studio aboneleri için aylık Azure kredisi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Özel görüntüleri oluşturmak için VHD dosyalarını karşıya yükleme işleminin nasıl otomatikleştirebilirim?
Özel görüntüleri oluşturmak için VHD dosyalarını karşıya yükleme otomatikleştirmek için iki seçeneğiniz vardır:

* Kullanım [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) kopyalayın veya lab ile ilişkili depolama hesabına VHD dosyalarını yükleyin.
* Kullanım [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md). Depolama Gezgini Windows, OS X ve Linux'ta çalışan tek başına bir uygulamadır.   

Laboratuvarınızı ile ilişkili hedef depolama hesabını bulmak için:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Sol menüden **kaynak grupları**.
3. Bulun ve Laboratuvarınızı ile ilişkili kaynak grubunu seçin.
4. Altında **genel bakış**, depolama hesaplarından birini seçin.
5. **Bloblar**'ı seçin.
6. Karşıya yükleme listesinde arayın. Yoksa, 4. adıma geri dönün ve başka bir depolama hesabı deneyin.
7. Kullanım **URL** , AzCopy komutunda hedef olarak.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>My Laboratuvardaki tüm Vm'leri silme işleminin nasıl otomatikleştirebilirim?
Vm'leri Azure portalında Laboratuvarınızı silebilirsiniz. Ayrıca, tüm VM'lerin bir PowerShell Betiği kullanarak laboratuvarınızda silebilirsiniz. Aşağıdaki örnekte, altında **değiştirmek için değerleri** açıklama, parametre değerlerini değiştirin. Alabileceğiniz `subscriptionId`, `labResourceGroup`, ve `labName` Azure portalında Laboratuvar bölmesinden değerleri.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Connect-AzAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.Name -like "$($lab.Name)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzResource -ResourceId $labVM.ResourceId -Force
    }

**Yapıtlar**
## <a name="what-are-artifacts"></a>Yapıtları nelerdir?
Yapıtlar, en yeni güncelleştirmeleri dağıtmak veya bir VM'ye geliştirme araçlarınızı dağıtmak için kullanabileceğiniz özelleştirilebilir öğeleridir. Sanal Makineyi oluştururken sanal makinenizde yapıtları ekleyin. VM sağlandıktan sonra yapıtları dağıtın ve sanal makinenize yapılandırın. Önceden mevcut olan çeşitli yapıtları kullanılabilir bizim [ortak GitHub deposu](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). Ayrıca [kendi yapıtları Yazar](devtest-lab-artifact-author.md).


**Laboratuvar yapılandırma**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Bir Resource Manager şablonundan bir laboratuvarı nasıl oluşturabilirim?
Sunuyoruz bir [Laboratuvar Azure Resource Manager şablonları GitHub deposunda](https://aka.ms/dtlquickstarttemplate) olarak dağıtabileceğiniz- ya da laboratuvarlarınızı için özel şablonlar oluşturmak için değiştirin. Her şablonda laboratuvarını olarak dağıtmak için bir bağlantı-kendi Azure aboneliğinizdir. Veya şablonu özelleştirebilirsiniz ve [PowerShell veya Azure CLI kullanarak dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Neden Vm'lerimin rastgele adlara sahip farklı kaynak gruplarında oluşturulur? Yeniden adlandırdığımda veya bu kaynak gruplarını değiştirme?
DevTest Labs sanal makineleri için kullanıcı izni veya erişimi yönetebilmeniz için kaynak gruplarını, bu şekilde oluşturulur. Bir VM için başka bir kaynak grubuna taşımak ve istediğiniz adını kullanmak olsa da, kaynak gruplarına değişiklikler yapmayın öneririz. Bu deneyimi daha fazla esneklik sağlayan geliştirme konusunda durmaksızın çalışıyoruz.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Aynı abonelik altında kaç labs oluşturabilirim?
Abonelik başına oluşturulabilecek labs sayısı belirli bir sınırı yoktur. Ancak, abonelik başına kullanılan kaynakları miktarı sınırlıdır. Okuyabilirsiniz [limitler ve kotalar Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Laboratuvar sanal makine sayısı oluşturabilirim?
Laboratuvar oluşturulan VM'ler sayısı belirli bir sınır yoktur. Ancak, abonelik başına kullanılan kaynakların (sanal makine çekirdeklerine, genel IP adresleri ve benzeri) sınırlıdır. Okuyabilirsiniz [limitler ve kotalar Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Doğrudan bağlantısını benim laboratuvara nasıl paylaşırım?

1. Azure portalında laboratuvara gidin.
2. Tarayıcınızdan Laboratuvar URL'yi kopyalayın ve ardından Laboratuvar Kullanıcılarınızla paylaşabilirsiniz.

> [!NOTE]
> Bir laboratuvar kullanıcının olan bir dış kullanıcı olup olmadığını bir [Microsoft hesabı](#what-is-a-microsoft-account), ancak paylaşılan bağlantı erişmeye çalıştıklarında kimin kuruluşunuzun Active Directory örneğine üye değil, kullanıcı bir hata iletisini görebilirsiniz. Bir dış kullanıcı bir hata iletisi görürse, adlarının ilk Azure portalının sağ üst köşedeki seçmesini isteyin. Ardından **dizin** menüsü, kullanıcının bölümünü Laboratuvar bulunduğu dizin seçebilirsiniz.
>
>

## <a name="what-is-a-microsoft-account"></a>Microsoft hesabı nedir?
Bir Microsoft hesabı olan neredeyse her şey Microsoft cihazlar ve hizmetler ile yapmak için kullandığınız hesaptır. Bir e-posta adresi ve Skype, Outlook.com, OneDrive, oturum açmak için kullandığınız parola olan Windows phone ve Xbox Live. Tek bir hesap, dosyaları, fotoğraflar, kişiler ve ayarları, tüm cihazlardan izleyebilirsiniz anlamına gelir.

> [!NOTE]
> Bir Microsoft hesabı kullanılan çağrılabilir bir *Windows Live ID*.
>
>


**Sorun giderme**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>My yapıt VM oluşturma sırasında başarısız oldu. Bunu nasıl giderebilirim?
Başarısız bir yapıt için günlükleri alma hakkında bilgi için bkz: [DevTest labs'deki yapıt hatalarını tanılama nasıl](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Neden olmayan mevcut sanal ağ düzgün bir şekilde kaydediliyor?
Sanal ağ adı nokta içeren bir olasılıktır. Bu durumda, dönemleri kaldırarak veya bunları çizgilerle değiştirerek deneyin. Ardından, sanal ağ kaydetmeyi yeniden deneyin.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Ben bir VM'den PowerShell sağladığınızda neden "üst kaynağı bulunamadı" hatası alıyorum?
Bir kaynak başka bir kaynak için bir üst, alt kaynak oluşturmadan önce üst kaynak mevcut olması gerekir. Üst kaynak mevcut değilse, gördüğünüz bir **ParentResourceNotFound** ileti. Üst kaynak üzerinde bir bağımlılık belirtmezseniz, alt kaynak önce üst dağıtılmış olabilir.

Sanal makineleri bir laboratuar ortamında bir kaynak grubu altında alt kaynaklardır. PowerShell kullanarak Vm'leri dağıtmak için Resource Manager şablonları kullandığınızda, PowerShell Betiği belirtilen kaynak grubu adı Laboratuvar tamamlandığında, kaynak grubu adı olmalıdır. Daha fazla bilgi için [yaygın Azure dağıtım hatalarını giderme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Bir VM dağıtımı başarısız olursa daha fazla hata bilgilerini nerede bulabilirim?
Sanal makine dağıtım hataları, etkinlik günlüklerinde yakalanır. Laboratuvar VM etkinlik günlükleri altında bulabilirsiniz **denetim günlükleri** veya **sanal makine tanılama** Laboratuvar VM sayfasında kaynak menüsündeki (VM'den seçtikten sonra sayfanın görünür **My sanal makineleri** listesi).

Bazı durumlarda, VM dağıtımı başlamadan önce dağıtım hatası oluşur. Sanal makine ile oluşturulan bir kaynak için abonelik sınırı aşıldığında bir örnektir. Bu durumda, hata ayrıntılarını Laboratuvar düzeyinde etkinlik günlüğünde yakalanır. Etkinlik günlükleri alt kısmında bulunan **yapılandırması ve ilkelerini** ayarları. Azure'da günlüklerini etkinlik kullanma hakkında daha fazla bilgi için bkz: [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
