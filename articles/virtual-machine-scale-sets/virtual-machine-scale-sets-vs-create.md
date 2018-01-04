---
title: "Visual Studio kullanarak sanal makine ölçek kümesi dağıtma | Microsoft Docs"
description: "Visual Studio ve Resource Manager şablonu kullanarak sanal makine ölçek kümeleri dağıtma"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73454abc11a832a1b7f4131bf13699bd0a94edea
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a>Visual Studio ile bir sanal makine ölçek kümesi oluşturma
Bu makalede, bir Azure sanal makine ölçek bir Visual Studio kaynak grubu dağıtımı kullanarak kümesi dağıtma gösterilmektedir.

[Azure sanal makine ölçek kümeleri](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) dağıtmak ve Otomatik ölçek benzer sanal makinelerle koleksiyonunu yönetmenize ve Yük Dengeleme için bir Azure işlem kaynağıdır. Sağlama ve sanal makine ölçekleme kümeleri kullanarak dağıtın [Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates). Azure CLI, PowerShell, REST kullanarak Azure Resource Manager şablonları dağıtılabilir ve ayrıca Visual Studio'dan doğrudan. Visual Studio bir Azure kaynak grubu dağıtım projesi bir parçası olarak dağıtılabilir örnek şablonları kümesi sağlar.

Azure kaynak grubu dağıtımları, Grup ve bir dizi ilgili Azure kaynaklarını tek dağıtım işleminde yayımlamak için bir yoldur. Bunları hakkında daha fazla burada öğrenebilirsiniz: [Visual Studio üzerinden Azure kaynak gruplarını oluşturma ve dağıtma](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Ön koşullar
Sanal makine ölçek kümeleri Visual Studio'da dağıtımına başlamak için aşağıdakiler gerekir:

* Visual Studio 2013 veya üzeri
* 2.7, 2.8 veya 2.9 Azure SDK

>[!NOTE]
>Bu yönergeler, Visual Studio ile kullandığınız varsayılmıştır [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Proje oluşturma
1. Seçerek Visual Studio'da yeni proje oluşturma **dosya | Yeni | Proje**.
   
    ![Yeni dosya][file_new]

2. Altında **Visual C# | Bulut**, seçin **Azure Resource Manager** bir Azure Resource Manager şablonunu dağıtmak için bir proje oluşturmak için.
   
    ![Proje oluşturma][create_project]

3. Şablonları listesinden Linux veya Windows sanal makine ölçek kümesi şablonu seçin.
   
   ![Şablonu seçin][select_Template]

4. Projeniz oluşturulduktan sonra PowerShell dağıtım betikleri, bir Azure Resource Manager şablonu ve parametre dosyası sanal makine ölçek kümesi için görürsünüz.
   
    ![Çözüm Gezgini][solution_explorer]

## <a name="customize-your-project"></a>Projenizi özelleştirme
Şimdi VM uzantısı özellikleri ekleme veya düzenleme Yük Dengeleme kuralları gibi uygulamanızın ihtiyaçları için özelleştirmek için şablonu düzenleyebilirsiniz. Varsayılan olarak, otomatik ölçeklendirme kurallarını eklemek kolaylaştırır AzureDiagnostics uzantısını dağıtmak için sanal makine ölçek kümesi şablonları yapılandırılır. Ayrıca, bir yük dengeleyici gelen NAT kuralları ile yapılandırılmış bir ortak IP adresi ile dağıtır. 

Yük Dengeleyici SSH (Linux) veya RDP (Windows) ile VM örnekleri bağlanmanıza olanak sağlar. Ön uç bağlantı noktası aralığı 50000'den başlar. Linux için bu anlamına gelir, varsa, bağlantı noktası 50000, SSH, yönlendirilir ilk VM ölçek kümesindeki bağlantı noktası 22. 50001 bağlantı noktasına bağlanan ikinci VM 22 noktasına vb. yönlendirilir.

 Visual Studio ile şablonlarınızı düzenlemek için en iyi yolu, parametreleri, değişkenler ve kaynakları düzenlemek için JSON ana hattı kullanmaktır. Dağıtmadan önce şemanın bir anlayış şablonunuzda hataları ortaya Visual Studio işaret edebilir.

![JSON Gezgini][json_explorer]

## <a name="deploy-the-project"></a>Projeyi dağıtın
1. Sanal makine ölçek kümesi kaynağı oluşturmak için Azure Resource Manager şablonu dağıtma. Proje düğümüne sağ tıklayın ve seçin **dağıtma | Yeni dağıtım**.
   
    ![Şablon dağıtma][5deploy_Template]
    
2. Aboneliğinizi "Dağıtmak için kaynak grubu" iletişim kutusunda seçin.
   
    ![Şablon dağıtma][6deploy_Template]

3. Buradan, şablonu dağıtmak için Azure kaynak grubu oluşturabilirsiniz.
   
    ![Yeni Kaynak Grubu][new_resource]

4. Bundan sonra öğesini **parametreleri Düzenle** şablonunuza geçirilen parametreler girmek üzere. Dağıtımı oluşturmak için gerekli olan işletim sistemi için kullanıcı adı ve parola sağlayın. Visual Studio yüklüyse PowerShell araçları yoksa denetlemek için önerilir **parolaları kaydetme** gizli bir PowerShell komut satırı istemi önlemenize veya kullanmak için [keyvault Destek](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Parametreleri Düzenle][edit_parameters]

5. Şimdi **dağıtma**. **Çıkış** penceresi dağıtımının ilerleme durumunu gösterir. Eylemi yürütmenin Not **dağıtma AzureResourceGroup.ps1** komut dosyası.
   
   ![Çıktı penceresi][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi keşfetme
Dağıtım tamamlandıktan sonra yeni sanal makine ölçek kümesi Visual Studio'da görüntüleyebileceğiniz **Cloud Explorer** (listesini Yenile). Cloud Explorer uygulamaları geliştirirken Visual Studio'da Azure kaynaklarını yönetmenizi sağlar. Ayrıca, sanal makine ölçek kümesindeki görüntüleyebileceğiniz [Azure portal](https://portal.azure.com) ve [Azure kaynak Gezgini](https://resources.azure.com/).

![Bulut Gezgini][cloud_explorer]

 Portal, Azure kaynak Gezgini, bir pencere "örnek görünüme" vermek ve aynı zamanda Baktığınız kaynaklar için PowerShell komutlarını gösteren keşfedin ve Azure kaynaklarını hata ayıklamak için kolay bir yol sağlarken web tarayıcısı olan Azure altyapınıza görsel olarak yönetmek için en iyi yolu sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio aracılığıyla sanal makine ölçek kümeleri başarıyla dağıttıktan sonra bir kez daha fazla projenizi uygulama gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Örneğin, Otomatik ölçek ekleyerek yapılandırın bir **Insights** altyapı şablonunuzu (örneğin, tek başına VM'ler) ekleme ya da özel betik uzantısı kullanarak uygulamaları dağıtma kaynak. İyi örnek şablonları bulunabilir [Azure hızlı başlangıç şablonlarını](https://github.com/Azure/azure-quickstart-templates) GitHub deposunu ("vmss" arayın).

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
