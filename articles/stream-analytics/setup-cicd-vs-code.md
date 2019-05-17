---
title: Sürekli tümleştirme ve Azure Stream Analytics CI/CD npm paketi ile geliştirin
description: Bu makalede, bir sürekli tümleştirme ve dağıtım işlemi ayarlamak için Azure Stream Analytics CI/CD npm paketini kullanmayı açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 7fe46712d610d881c21653461d12e4f8efecb468
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827883"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Sürekli tümleştirme ve Stream Analytics CI/CD npm paketi ile geliştirin
Bu makalede, bir sürekli tümleştirme ve dağıtım işlemi ayarlamak için Azure Stream Analytics CI/CD npm paketini kullanmayı açıklar.

## <a name="build-the-vs-code-project"></a>VS Code projeyi oluşturun

Sürekli tümleştirme ve dağıtım kullanarak Azure Stream Analytics işleri için etkinleştirebilirsiniz **asa streamanalytics cıcd** npm paketi. Npm paketi, Azure Resource Manager şablonları oluşturmak için araçlar sağlar [Stream Analytics Visual Studio Code projeleri](quick-create-vs-code.md). Bunu Windows, macOS ve Linux üzerinde Visual Studio Code yükleme olmadan kullanılabilir.

Yapılandırmasını tamamladıktan [paketini karşıdan](https://www.npmjs.com/package/azure-streamanalytics-cicd), Azure Resource Manager şablonları çıktısını almak için aşağıdaki komutu kullanın. Varsa **outputPath** belirtilmezse, şablonları yerleştirilecek **Dağıt** projenin altında klasör **bin** klasör.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

Stream Analytics Visual Studio Code Proje başarıyla oluşturulursa, altında aşağıdaki iki Azure Resource Manager şablonu dosyaları oluşturur **bin / Debug/perakende / Deploy** klasörü: 

*  Resource Manager şablon dosyası

       [ProjectName].JobTemplate.json 

*  Resource Manager parametre dosyası

       [ProjectName].JobTemplate.parameters.json   

Visual Studio projenize ayarlarında parameters.json dosyasındaki varsayılan parametreleri arasındadır. Parametreleri, başka bir ortama dağıtmak istiyorsanız, uygun şekilde değiştirin.

> [!NOTE]
> Tüm kimlik bilgilerini varsayılan değerlerin ayarlandığından null. İşiniz **gerekli** buluta dağıtmadan önce değerleri ayarlamak için.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Kullanma hakkında daha fazla bilgi edinin [bir Resource Manager şablon dosyası ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Kullanma hakkında daha fazla bilgi edinin [Resource Manager şablonunda bir parametre olarak bir nesne kullanmasını](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Yönetilen kimliği çıkış havuzu olarak Azure Data Lake Store Gen1 kullanmak için hizmet sorumlusu Azure'a dağıtmadan önce PowerShell kullanarak erişebilmesi gerekir. Kullanma hakkında daha fazla bilgi edinin [ADLS Gen1 yönetilen kimliği ile Resource Manager şablonu ile dağıtma](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma](quick-create-vs-code.md)
* [Stream Analytics sorguları Visual Studio Code (Önizleme) ile yerel olarak test etme](vscode-local-run.md)
* [Visual Studio Code (Önizleme) ile Azure Stream Analytics'i keşfedin](vscode-explore-jobs.md)
