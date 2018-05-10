---
title: Azure DevTest Labs SSS | Microsoft Docs
description: Azure DevTest Labs ilgili sık sorulan soruların yanıtlarını bulun.
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
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: a295cad2bf1cafce4dc64909174e9417daa7918e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs SSS
Bazı Azure DevTest Labs ilgili en sık sorulan soruların yanıtlarını alın.

**Genel**
## <a name="what-if-my-question-isnt-answered-here"></a>Ne sorumun cevabı burada cevaplanıp değil mi?
Sorunuzun yanıtını burada listelenmiyorsa, biz yanıt bulmanıza yardımcı olmak için bize bildirin.

* Bu SSS, sonunda bir soru gönderin. Azure önbelleği ekip ve diğer topluluk üyeleri bu makale hakkında göster.
* Daha geniş bir kitleye ulaşmak için üzerinde bir soru gönderin [Azure DevTest Labs MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Azure DevTest Labs ekip ve diğer topluluk üyeleri göster.
* Özellik istekleri için istekleri ve için fikirleri göndermek [Azure DevTest Labs kullanıcı sesi](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Azure DevTest Labs neden kullanmalıyım?
Azure DevTest Labs takım zaman ve para kaydedebilirsiniz. Geliştiriciler kendi ortamlarında birkaç farklı tabanları kullanarak oluşturabilirsiniz. Bunlar ayrıca, yapıları hızlı bir şekilde dağıtmak ve uygulamaları yapılandırmak için kullanabilirsiniz. Özel resimler ve formülleri kullanarak sanal makineleri (VM'ler) şablon olarak Kaydet ve kolayca takımda yeniden oluşturun. DevTest Labs Ayrıca Yöneticiler atık küçültmek ve bir ekibin ortamları yönetmek için kullanabilirsiniz, Laboratuvar birkaç yapılandırılabilir ilkeleri de sunar. Bu ilkeler Otomatik kapatma, maliyet eşik, kullanıcı ve en fazla VM boyutu başına en fazla VMs içerir. DevTest Labs daha ayrıntılı bir açıklaması için bkz: [genel bakış](devtest-lab-overview.md) veya [tanıtım videosunu](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>"Sorunsuz Self-Servis" ne anlama geliyor?
Self Servis sorunsuz geliştiriciler ve sınayıcılar kendi ortamları gerektiği şekilde oluşturun anlamına gelir. Yöneticiler DevTest Labs atık ve denetim maliyetleri en aza indirmenize yardımcı bilmesinin güvenliğe sahip değildir. Yöneticiler hangi VM boyutları, VM'ler, üst sınırını izin verilir ve ne zaman VM'ler başlatıldı ve bilgisayarı kapat belirtebilirsiniz. DevTest Labs ayrıca maliyetleri izlemek ve nasıl Laboratuvar kaynaklarını kullanıldığını farkında olmanıza yardımcı olmak için uyarıları ayarlamak kolaylaştırır.

## <a name="how-can-i-use-devtest-labs"></a>DevTest Labs nasıl kullanabilir miyim?
DevTest Labs dilediğiniz zaman, geliştirme gerektirir veya sınama ortamlarında ve hızlı bir şekilde yeniden oluşturun ya da maliyet tasarrufu ilkeleri kullanarak bunları yönetmek istediğiniz yararlıdır.

Burada, müşterilerimizin DevTest Labs için kullandığı bazı senaryolar vardır:

* Tek bir yerde ortamlarında test ve geliştirme yönetin. Maliyetleri azaltmak ve paylaşmak için özel görüntülerinizi oluşturmak için kullanım ilkeleri takımda oluşturur.
* Uygulama geliştirme aşamaları boyunca disk durumunu kaydetmek için özel resimler kullanarak geliştirin.
* Maliyet ilerleme durumu ile ilgili olarak izler.
* Kalite Güvence test etmek için yığın test ortamları oluşturma.
* Yapılar ve formülleri kolayca yapılandırmak ve çeşitli ortamlar uygulamada yeniden oluşturmak için kullanın.
* VM'ler hackathons (işbirlikçi geliştirme veya test çalışma) dağıtın ve olay sona erdiğinde sonra kolayca bunları sağlamayı sonlandırın.

## <a name="how-am-i-billed-for-devtest-labs"></a>DevTest Labs için nasıl faturalandırılır?
DevTest Labs ücretsiz bir hizmettir. Labs oluşturma ve ilkeleri, şablonları ve yapıları DevTest Labs'de yapılandırma ücretsizdir. Yalnızca sanal makineleri, depolama hesapları ve sanal ağlar gibi labs kullanılan Azure kaynakları için ücret ödersiniz. Laboratuvar kaynaklarını maliyeti hakkında daha fazla bilgi için bkz [Azure DevTest Labs fiyatlandırma](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Güvenlik**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>DevTest Labs içinde farklı güvenlik düzeylerine nelerdir?
Güvenlik erişimi belirlenir [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/built-in-roles.md). Erişim nasıl çalıştığını öğrenin izin, bir rolü ve bir kapsam arasındaki farklar öğrenmek için RBAC tarafından tanımlanan yardımcı olabilir.

* **İzni**: belirli bir eylemi için tanımlanmış bir erişim bir izni yok. Örneğin, bir izin tüm VM'ler için okuma erişimi olabilir.
* **Rol**: gruplandırılır ve bir kullanıcıya atanmış izinleri rolüdür. Örneğin, bir abonelik sahibi rolüne sahip bir kullanıcı bir abonelik içindeki tüm kaynaklara erişebilir.
* **Kapsam**: bir Azure kaynak hiyerarşi içinde bir düzey kapsamıdır. Örneğin, bir kapsam bir kaynak grubu, tek bir laboratuvar veya tüm abonelik olabilir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlamak rolleri iki tür vardır:

* **Laboratuvar sahibi**: bir laboratuvar sahibi laboratuar ortamında tüm kaynaklara erişim izni vardır. Bir laboratuvar sahibi ilkeleri değiştirmek, okuma ve herhangi bir VM yazma, sanal ağı değiştirin ve benzeri.
* **Laboratuvar kullanıcı**: Laboratuvar kullanıcı VM'ler, ilkeleri ve sanal ağlar gibi tüm Laboratuvar kaynaklarını görüntüleyebilir. Ancak, Laboratuvar kullanıcı ilkeleri veya diğer kullanıcılar tarafından oluşturulan herhangi bir VM değiştirilemiyor. 

DevTest Labs'de özel roller de oluşturabilirsiniz. DevTest Labs'de özel roller oluşturma konusunda bilgi almak için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Kullanıcının belirli bir kapsamda izinleri olduğunda kapsamları hiyerarşik olduğundan kullanıcı otomatik olarak kapsamdaki her alt düzey kapsamındaki bu izinler verilir. Örneği için abonelik sahibi rolü atanmış bir kullanıcı, kullanıcı bir abonelikte tüm kaynaklara erişim izni vardır. Bu kaynaklar, tüm sanal makineleri, tüm sanal ağları ve tüm labs içerir. Abonelik sahibi Laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, bunun tersi geçerli değildir. Bir laboratuvar sahibi abonelik düzeyinden daha düşük bir kapsam bir laboratuvar erişebilir. Bu nedenle, bir laboratuvar sahip sanal makineleri, sanal ağları veya Laboratuvar dışında olan diğer kaynakları göremiyorum.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Belirli bir görevi gerçekleştirmek kullanıcıların izin vermek için bir rolü nasıl oluşturulur?
Özel roller oluşturmanızı ve izinleri rol atama hakkında kapsamlı bir makale için bkz: [belirli Laboratuvar ilkeleri için kullanıcı izinleri verin](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Bir rol oluşturur bir komut dosyası örneği burada verilmiştir *DevTest Labs gelişmiş kullanıcı*, laboratuarda tüm sanal makineleri durdurmak ve başlatmak için izni vardır:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**CI/CD tümleştirme ve Otomasyon**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>DevTest Labs my CI/CD araç zinciri ile tümleşik çalışıyor mu?
Visual Studio Team Services kullanıyorsanız, kullanabileceğiniz bir [DevTest Labs görevler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) yayın hattınızı DevTest Labs otomatik hale getirmek için. Bu uzantı ile gerçekleştirebileceğiniz görevlerden bazıları şunlardır:

* Oluşturabilir ve otomatik olarak bir VM dağıtabilirsiniz. VM ile en son sürüme Azure dosya kopyalama veya PowerShell Team Services görevleri kullanarak da yapılandırabilirsiniz.
* Daha fazla araştırma için aynı VM'de bir hatayı yeniden test sonra otomatik olarak bir VM durumunu yakalayın.
* Artık gerekli olmadığında yayın ardışık düzen sonunda VM silin.

Teklif yönerge ve Team Services uzantısı kullanma hakkında bilgi için aşağıdaki blog yazılarını:

* [DevTest Labs ve Visual Studio Team Services uzantısı](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Yeni bir VM Team Services bir DevTest Labs laboratuarda dağıtma](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [DevTest Labs sürekli dağıtımları için Team Services sürüm Yönetimi'ni kullanma](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Diğer sürekli tümleştirme (CI) / kesintisiz teslim (CD) toolchains aynı senaryolarla elde edebileceğiniz dağıtma [Azure Resource Manager şablonları](https://aka.ms/dtlquickstarttemplate) kullanarak [Azure PowerShell cmdlet'lerini](../azure-resource-manager/resource-group-template-deploy.md) ve [.NET SDK'ları](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Aynı zamanda [DevTest Labs için REST API'leri](http://aka.ms/dtlrestapis) , araç zinciri ile tümleştirmek için.  


**Sanal makineler**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-blade-that-i-see-in-devtest-labs"></a>DevTest Labs'de bakın sanal makineleri dikey penceresinde VM'ler neden göremiyorum?
DevTest Labs'de VM oluşturduğunuzda, bu VM'ye erişmesine izin verilir. VM labs dikey penceresinde hem de görüntüleyebilirsiniz **sanal makineleri** dikey. DevTest Labs Laboratuvar kullanıcı rolüne atanan kullanıcılar Laboratuvar Laboratuvar 's üzerinde oluşturulan tüm sanal makineleri görebilir **tüm sanal makineleri** dikey. Ancak, DevTest Labs Laboratuvar kullanıcı rolüne sahip kullanıcılar diğer kullanıcıların oluşturduğu VM kaynakları okuma erişimi otomatik olarak verilmez. Bu nedenle, bu sanal makineleri üzerinde görüntülenmez **sanal makineleri** dikey.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Özel görüntü formül arasındaki fark nedir?
Bir sanal sabit disk (VHD) bir özel görüntüdür. Ek ayarlarla yapılandırın ve ardından kaydedebilir ve yeniden bir görüntü formülüdür. Özel görüntü aynı temel, değişmez görüntüsünü kullanarak hızlı bir şekilde çeşitli ortamlar oluşturmak istiyorsanız tercih edilebilir. Bir formülü son bitle VM yapılandırmasına bir sanal ağ veya alt ağ bir parçası olarak ya da belirli bir boyutu VM'i olarak yeniden istiyorsanız daha iyi olabilir. Daha ayrıntılı bir açıklama için bkz: [özel resimler ve DevTest Labs formüller karşılaştırma](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Nasıl birden çok VM aynı şablonu aynı anda oluşturulur?
Aynı anda birden çok VM aynı şablonu oluşturmak için iki seçeneğiniz vardır:
* Kullanabileceğiniz [Visual Studio Team Services görevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks). 
* Yapabilecekleriniz [Resource Manager şablonu oluştur](devtest-lab-add-vm.md#save-azure-resource-manager-template) bir VM oluştururken ve [Windows PowerShell'i Resource Manager şablonu dağıtmayı](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>My DevTest Labs laboratuara nasıl mevcut Azure Vm'lerimin hareket?
Varolan Vm'leriniz için DevTest Labs kopyalamak için:

1. Var olan VM VHD dosyasını kullanarak kopyalayın bir [Windows PowerShell komut dosyası](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1).
2. [Özel görüntü oluşturma](devtest-lab-create-template.md) DevTest Labs Laboratuvarınızı içinde.
3. Özel görüntüsünden laboratuarda bir VM oluşturun.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Birden çok disk Vm'lerimin için ekleyebilir miyim?
Evet, Vm'leriniz için birden çok disk ekleyebilirsiniz.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>My test etmek için bir Windows işletim sistemi görüntüsünü kullanmak istiyorsanız bir MSDN aboneliği satın var mı?
Windows istemci işletim sistemi görüntülerini (Windows 7 veya sonraki bir sürümünü), geliştirme veya Azure'da sınama kullanmak için aşağıdakilerden birini yapmalısınız:

- [Bir MSDN aboneliği satın](https://www.visualstudio.com/products/how-to-buy-vs).
- bir Kurumsal Anlaşma varsa, bir Azure aboneliği ile oluşturma [Enterprise geliştirme ve Test teklif](https://azure.microsoft.com/offers/ms-azr-0148p).

Her bir MSDN teklif için Azure kredisi hakkında daha fazla bilgi için bkz: [Visual Studio aboneleri için aylık Azure kredi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Özel görüntülerinizi oluşturmak için VHD dosyaları karşıya yükleme işleminin nasıl otomatikleştirmek?
Özel görüntülerinizi oluşturmak için karşıya yükleme VHD dosyalarını otomatikleştirmek için iki seçeneğiniz vardır:

* Kullanım [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) kopyalamanız veya laboratuvarı ile ilişkili depolama hesabı VHD dosyaları karşıya yükleme.
* Kullanım [Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md). Depolama Gezgini, Windows, OS X ve Linux çalıştıran tek başına bir uygulamadır.   

Laboratuvarınızı ile ilişkili hedef depolama hesabını bulmak için:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Soldaki menüden seçin **kaynak grupları**.
3. Bul ve Laboratuvarınızı ile ilişkili kaynak grubunu seçin.
4. Altında **genel bakış**, depolama hesaplarından birini seçin.
5. Seçin **BLOB'lar**.
6. Karşıya listesinde arayın. Hiçbiri yoksa, 4. adıma dönün ve başka bir depolama hesabı deneyin.
7. Kullanım **URL** AzCopy komut hedef olarak.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>My Laboratuvar tüm sanal makineleri silme işlemini nasıl otomatikleştirmek?
Laboratuvarınızı Azure portalında VM'ler silebilirsiniz. Ayrıca, bir PowerShell komut dosyası kullanarak tüm sanal makineleri laboratuvarınızda silebilirsiniz. Aşağıdaki örnekte, altında **değiştirmek için değerleri** açıklama, parametre değerlerini değiştirin. Alabilirsiniz `subscriptionId`, `labResourceGroup`, ve `labName` Azure portalında Laboratuvar bölmesinden değerleri.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Connect-AzureRmAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Yapıları**
## <a name="what-are-artifacts"></a>Yapıları nelerdir?
En son BITS dağıtmak veya bir VM'ye geliştirme araçları dağıtmak için kullanabileceğiniz özelleştirilebilir öğeler ürünleridir. VM oluşturduğunuzda, yapılar, VM'e ekleyin. VM sağlandıktan sonra yapıları dağıtabilir ve VM yapılandırabilirsiniz. Çeşitli önceden var olan yapıları kullanılabilir bizim [ortak GitHub deposunu](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). Ayrıca [kendi yapıları Yazar](devtest-lab-artifact-author.md).


**Laboratuvarı yapılandırması**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Resource Manager şablonu nasıl bir laboratuvar oluşturulsun mu?
Sunduğumuz bir [GitHub deposunu Laboratuvar Azure Resource Manager şablonları](https://aka.ms/dtlquickstarttemplate) olarak dağıtabileceğiniz- ya da sizin labs için özel şablonlar oluşturmak için değiştirin. Her bir şablon olarak Laboratuvar dağıtmak için bir bağlantı vardır-kendi Azure aboneliği. Veya, şablon özelleştirebilirsiniz ve [PowerShell veya Azure CLI kullanarak dağıtmak](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Neden Vm'lerimin farklı kaynak gruplarında, rasgele adları oluşturulur? Yeniden adlandırma veya miyim bu kaynak gruplarını değiştirme?
DevTest Labs kullanıcı izinleri ve VM'ler için erişimi yönetebilmeniz için kaynak gruplarını bu şekilde oluşturulur. Bir VM başka bir kaynak grubuna taşımak ve istediğiniz adı kullanın, ancak kaynak gruplarına değişiklik yapma öneririz. Daha fazla esneklik izin vermek için bu deneyimini geliştirir üzerinde çalışıyoruz.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Kaç tane labs aynı abonelik altında oluşturabilirim?
Abonelik başına oluşturulan labs sayısı belirli bir sınırı yoktur. Ancak, abonelik başına kullanılan kaynak miktarını sınırlıdır. Hakkında bilgi edinebilirsiniz [sınırlarını ve kotaları Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Kaç VM Laboratuvar oluşturabilirim?
Laboratuvar oluşturulan VM'ler üzerinde belirli bir sınır yoktur. Ancak, abonelik başına kullanılan kaynaklar (VM çekirdek, genel IP adresleri ve benzeri) sınırlıdır. Hakkında bilgi edinebilirsiniz [sınırlarını ve kotaları Azure aboneliklerinin](../azure-subscription-service-limits.md) ve [bu sınırları artırmak nasıl](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>My Laboratuvar nasıl doğrudan bir bağlantı paylaşmak?

1. Azure portalında Laboratuvar gidin.
2. Tarayıcınızdan Laboratuvar URL'yi kopyalayın ve Laboratuvar Kullanıcılarınızla paylaşabilirsiniz.

> [!NOTE]
> Laboratuvarı kullanıcısı olan bir dış kullanıcının olup olmadığını bir [Microsoft hesabı](#what-is-a-microsoft-account), ancak paylaşılan bağlantıyı erişmeye çalıştığınızda kimin kuruluşunuzun Active Directory örneğine üyesi değil, kullanıcı bir hata iletisi görebilirsiniz. Bir dış kullanıcı bir hata iletisi görür adlarının ilk Azure portalının sağ üst köşesinde seçmesini isteyin. Ardından **Directory** menüsünde, kullanıcı bölümünü Laboratuvar bulunduğu dizini seçin.
>
>

## <a name="what-is-a-microsoft-account"></a>Microsoft hesabı nedir?
Bir Microsoft hesabı neredeyse Microsoft cihazları ve Hizmetleri ile yaptığınız her şey için kullandığınız hesaptır. Bir e-posta adresi ve Skype, Outlook.com, OneDrive, oturum açmak için kullandığınız parola olan Windows phone ve Xbox Live. Tek bir hesap, dosyaları, fotoğraflarınızı, kişiler ve ayarları, herhangi bir cihazda izleyebilirsiniz anlamına gelir.

> [!NOTE]
> Bir Microsoft hesabı kullanılan çağrılacak bir *Windows Live ID*.
>
>


**Sorun giderme**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>My yapı VM oluşturma sırasında başarısız oldu. Bunu nasıl giderebilirim?
İçin başarısız, yapı günlükleri alma hakkında bilgi için bkz: [DevTest Labs de yapı hataları tanılamak nasıl](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Neden değil my sanal varolan ağ düzgün bir şekilde kaydediliyor?
Bir olasılık, sanal ağ adı nokta içeriyor olabilir. Bu durumda, dönemleri kaldırma veya kısa çizgi ile değiştirmeyi deneyin. Ardından, sanal ağ kaydetmeyi yeniden deneyin.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>PowerShell VM'den sağladığınızda neden "üst kaynağı bulunamadı" hatası alıyorum?
Bir kaynağı başka bir kaynak için bir üst olduğunda, üst kaynak alt kaynağı oluşturmadan önce mevcut olması gerekir. Üst kaynak mevcut değilse, gördüğünüz bir **ParentResourceNotFound** ileti. Üst kaynakta bir bağımlılık belirtmezseniz, alt kaynak önce üst dağıtılmış olabilir.

Sanal makineleri bir laboratuar ortamında bir kaynak grubu altında alt kaynaklardır. PowerShell kullanarak sanal makineleri dağıtmak için Resource Manager şablonları kullandığınızda, PowerShell Betiği sağlanan kaynak grubu adı laboratuvarı kaynak grubu adı olmalıdır. Daha fazla bilgi için bkz: [ortak Azure dağıtım hatalarını giderme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>VM dağıtımı başarısız olursa daha fazla hata bilgileri nereden bulabilirim?
VM Dağıtım hataları, etkinlik günlükleri yakalanır. Laboratuvar VM etkinlik günlükleri altında bulabilirsiniz **denetim günlüklerini** veya **sanal makine tanılamaları** Laboratuvar 's VM dikey penceresinde kaynak menüsünde (sanal makineden seçtikten sonra dikey penceresi görünür **My sanal makineler** listesi).

Bazı durumlarda, VM dağıtımı başlamadan önce dağıtım hatası oluşur. VM ile oluşturulmuş bir kaynak için abonelik sınırı aşıldığında bir örnektir. Bu durumda, hata ayrıntılarını Laboratuvar düzeyi etkinlik günlüklerde yakalanır. Etkinlik günlükleri alt kısmında bulunan **yapılandırma ve ilkeleri** ayarlar. Etkinlik kullanma hakkında daha fazla bilgi Azure'da oturum için bkz: [görüntülemek kaynakları eylemlerini denetlemek için etkinlik günlükleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
