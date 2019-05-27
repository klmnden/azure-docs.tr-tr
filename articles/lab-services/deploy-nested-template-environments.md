---
title: Azure DevTest Labs içinde iç içe geçmiş Resource Manager şablonu ortamlarını dağıtma | Microsoft Docs
description: Azure DevTest Labs ile ortamları sağlamak için Azure Resource Manager şablonları iç içe geçmiş dağıtmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: spelluru
ms.openlocfilehash: eec0cde4a36449f85998bfb04d16f1d52c68bb19
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835293"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Test ortamları için iç içe geçmiş Azure Resource Manager şablonlarını dağıtma
İç içe geçmiş dağıtım ana Resource Manager şablonu içindeki diğer Azure Resource Manager şablonları yürütmek sağlar. Hedeflenen ve amaca özel şablonları kümesine dağıtımınızı ayırmak sağlar. Bu test, yeniden ve Okunabilirlik açısından avantajları sağlar. Makaleyi [Azure kaynakları dağıtılırken bağlı şablonları kullanma](../azure-resource-manager/resource-group-linked-templates.md) bu çözümün iyi bir genel bakış ile birkaç kod örneği sağlanmıştır. Bu makalede, Azure DevTest Labs kullanarak belirli bir örnek sağlar. 

## <a name="key-parameters"></a>Anahtar parametreleri
Sıfırdan kendi Resource Manager şablonu oluşturmanız mümkün olsa da kullanmanızı öneririz [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) Visual Studio'da, geliştirme ve hata ayıklama şablonları yapmayı kolaylaştırır. İç içe geçmiş dağıtım kaynağı için azuredeploy.json eklediğinizde, Visual Studio şablon daha esnek hale getirmek için birden fazla öğe ekler. Bu öğeler, ikincil bir şablon ve parametreleri dosyası, ana şablon dosyası içinde değişken adlarını ve yeni dosyalar için depolama konumu için iki parametre ile bir alt klasör içerir. **_ArtifactsLocation** ve **_artifactsLocationSasToken** DevTest Labs kullanan anahtar parametreleri. 

DevTest Labs'ın ortamlar ile nasıl çalıştığı hakkında bilgi sahibi değilseniz, bkz. [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md). Şablonlarınızı DevTest labs'deki bir laboratuvara bağlı deposunda depolanır. Bu şablonları ile yeni bir ortam oluşturduğunuzda, dosyaların Laboratuvardaki bir Azure depolama kapsayıcısına taşınır. Tanımlamak ve iç içe geçmiş dosyaları kopyalamak için DevTest Labs _artifactsLocation ve _artifactsLocationSasToken parametreleri tanımlar ve alt klasörleri depolama kapsayıcısını kadar kopyalar. Ardından, bu otomatik olarak konumunu ve paylaşılan erişim imzası (SaS) belirteci parametreleri ekler. 

## <a name="nested-deployment-example"></a>İç içe geçmiş dağıtım örneği
Bir iç içe dağıtımın basit bir örnek aşağıda verilmiştir:

```json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "_artifactsLocation": {
        "type": "string"
    },
    "_artifactsLocationSasToken": {
        "type": "securestring"
    }},
"variables": {
    "NestOneTemplateFolder": "nestedtemplates",
    "NestOneTemplateFileName": "NestOne.json",
    "NestOneTemplateParametersFileName": "NestOne.parameters.json"},
    "resources": [
    {
        "name": "NestOne",
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2016-09-01",
        "dependsOn": [ ],
        "properties": {
            "mode": "Incremental",
            "templateLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            },
            "parametersLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateParametersFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            }
        }    
    }],
"outputs": {}
```

Bu şablon içeren depoyu klasöründe bir alt sahip `nestedtemplates` dosyalarla **NestOne.json** ve **NestOne.parameters.json**. İçinde **azuredeploy.json**, URI şablonu yapıtları konumu, iç içe geçmiş şablon klasörü kullanılarak derlendi için şablon dosyası adı iç içe geçmiş. Benzer şekilde, parametrelerini URI'sini yapıtları konumu, iç içe geçmiş şablon klasörü ve parametre dosyası için iç içe geçmiş şablon kullanarak oluşturulmuştur. 

Visual Studio'da aynı proje yapısını görüntüsü aşağıda verilmiştir: 

![Visual Studio'da proje yapısı](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

Birincil klasöründe ancak herhangi bir tek düzey daha derin ek klasörler ekleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın: 

- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Yapılandırma ve Azure DevTest Labs'de ortak ortamlardaki kullanma](devtest-lab-configure-use-public-environments.md)
- [Azure DevTest Labs'de sanal ağ, Laboratuvar için bir ortama bağlanın](connect-environment-lab-virtual-network.md)