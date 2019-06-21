---
title: Visual Studio kullanarak sanal makine ölçek kümesi dağıtma | Microsoft Docs
description: Visual Studio ve Resource Manager şablonu kullanarak sanal makine ölçek kümeleri dağıtma
services: virtual-machine-scale-sets
ms.custom: H1Hack27Feb2017
ms.workload: na
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: manayar
ms.openlocfilehash: 3d472aeaae7e7f02eba58aadea1df042d6c0f27b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204712"
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a>Visual Studio ile bir sanal makine ölçek kümesi oluşturma
Bu makalede bir Azure sanal makine bir Visual Studio kaynak grubu dağıtımı kullanarak ölçek kümesi dağıtmayı gösterir.

[Azure sanal makine ölçek kümeleri](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) dağıtma ve otomatik ölçeklendirme ile benzer sanal makine koleksiyonunu yönetin ve Yük Dengeleme için bir Azure işlem kaynağıdır. Sağlama ve sanal makine ölçek kümeleri kullanarak dağıtın [Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates). Azure Resource Manager şablonları kullanarak Azure CLI, PowerShell, REST dağıtılabilir ve ayrıca doğrudan Visual Studio'dan. Visual Studio, bir Azure kaynak grubu dağıtım projesi bir parçası olarak dağıtılabilir örnek şablonları, bir dizi sağlar.

Azure kaynak grubu dağıtımlarında, Grup ve bir dizi ilgili Azure kaynaklarını tek bir dağıtım işleminde yayımlamak için bir yoludur. Bunlar hakkında daha fazla buradan bilgi edinebilirsiniz: [Oluşturma ve Visual Studio aracılığıyla Azure kaynak grupları dağıtma](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Ön koşullar
Visual Studio'da sanal makine ölçek kümeleri dağıtımına başlamak için aşağıdakiler gerekir:

* Visual Studio 2013 veya üzeri
* Azure SDK 2.7, 2.8 veya 2.9

>[!NOTE]
>Bu yönergeler, kullanmakta olduğunuz Visual Studio ile varsayar [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Proje oluşturma
1. Seçerek Visual Studio'da yeni proje oluşturma **dosya | Yeni | Proje**.
   
    ![Yeni dosya][file_new]

2. Altında **Visual C# | Bulut**, seçin **Azure Resource Manager** bir Azure Resource Manager şablonu dağıtmak için bir proje oluşturmaktır.
   
    ![Proje oluşturma][create_project]

3. Şablonlar listesinden Linux veya Windows sanal makine ölçek kümesi şablonunu seçin.
   
   ![Şablonu seçin][select_Template]

4. Projeniz oluşturulduktan sonra PowerShell dağıtım betiklerini, bir Azure Resource Manager şablonu ve parametre dosyası için sanal makine ölçek kümesi görürsünüz.
   
    ![Çözüm Gezgini][solution_explorer]

## <a name="customize-your-project"></a>Projenizi özelleştirme
Artık VM uzantısı özellikleri ekleme veya Yük Dengeleme kuralları düzenleme gibi uygulamanızın ihtiyaçları için özelleştirmek için bir şablonu düzenleyebilirsiniz. Varsayılan olarak, otomatik ölçeklendirme kuralları eklemenizi kolaylaştırır AzureDiagnostics uzantısını dağıtmak için sanal makine ölçek kümesi şablonlarının yapılandırılır. Ayrıca, bir yük dengeleyici gelen NAT kuralları ile yapılandırılmış bir genel IP adresi ile dağıtır. 

Yük Dengeleyici (Linux) SSH veya RDP (Windows) ile sanal makine örneklerine bağlanmanıza olanak sağlar. Ön uç bağlantı noktası aralığı 50000'den başlar. Linux için başka bir deyişle, varsa, SSH bağlantı noktası 50000, size yönlendirilir ilk VM ölçek kümesindeki bağlantı noktası 22. Bağlantı noktası 50001 bağlanan ikinci sanal makinenin 22 numaralı bağlantı noktasına ve benzeri yönlendirilir.

 Visual Studio ile şablonlarınızı düzenlemek için en iyi yolu, parametreleri, değişkenler ve kaynakları düzenlemek için JSON ana hattını kullanmaktır. Dağıtmadan önce şemayı bir anlayış şablonunuzdaki hataları ortaya Visual Studio işaret edebilir.

![JSON Gezgini][json_explorer]

## <a name="deploy-the-project"></a>Projeyi dağıtın
1. Sanal makine ölçek kümesi kaynak oluşturmak için Azure Resource Manager şablonu dağıtın. Proje düğümünü sağ tıklatın ve seçin **dağıtma | Yeni dağıtım**.
   
    ![Şablon dağıtma][5deploy_Template]
    
2. "Dağıtmak için kaynak grubu" iletişim kutusunda aboneliğinizi seçin.
   
    ![Şablon dağıtma][6deploy_Template]

3. Buradan, şablonunuzu dağıtmak için bir Azure kaynak grubu oluşturabilirsiniz.
   
    ![Yeni kaynak grubu][new_resource]

4. Ardından, **parametreleri Düzenle** , şablona geçirilen parametrelerini girin. Dağıtımı oluşturmak için gerekli olan işletim sistemi için kullanıcı adı ve parola sağlayın. Yüklü Visual Studio için PowerShell araçları sahip değilseniz, kontrol etmek için önerilir **parolaları Kaydet** gizli bir PowerShell komut satırı istemi önlemek veya bunları kullanmanızı [keyvault Destek](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Parametreleri Düzenle][edit_parameters]

5. Şimdi tıklayarak **Dağıt**. **Çıkış** penceresinde dağıtımın ilerleme durumunu gösterir. Eylemi yürütmenin Not **Dağıt AzureResourceGroup.ps1** betiği.
   
   ![Çıktı Penceresi][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>Sanal makine ölçek kümenizi keşfetme
Dağıtım tamamlandığında, yeni sanal makine ölçek kümesi Visual Studio'da görüntüleyebilirsiniz **Cloud Explorer** (yenileme). Cloud Explorer uygulamalar geliştirmeye devam ederken Visual Studio'daki Azure kaynaklarını yönetmenizi sağlar. Ayrıca, sanal makine ölçek kümesi'nde görüntüleyebilirsiniz [Azure portalında](https://portal.azure.com) ve [Azure kaynak Gezgini](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 Portal, Azure kaynak Gezgini, bir pencere "örneği görünümüne" ayırabilir ve aynı zamanda PowerShell komutlarını gösteren keşfedin ve Azure kaynakları, hata ayıklama için kolay bir yol sağlarken Azure altyapınıza bir web tarayıcısı ile görsel olarak yönetmek için en iyi yolu sağlar. kaynaklar için aradığınız.

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio aracılığıyla sanal makine ölçek kümeleri, başarıyla dağıtıldıktan sonra daha fazla projeniz uygulama gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Örneğin, ekleyerek otomatik ölçeklendirmeyi yapılandırma bir **Insights** şablonunuzu (tek başına VM gibi) altyapı ekleme ya da özel betik uzantısı kullanarak dağıtmaya kaynak. Örnek şablonları bulunabilir [Azure hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates) GitHub deposu ("vmss" aratın).

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
