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
ms.openlocfilehash: fa5a57afa379c6bbe027be80f400fc176800d289
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158500"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Sürekli tümleştirme ve Stream Analytics CI/CD npm paketi ile geliştirin
Bu makalede, bir sürekli tümleştirme ve dağıtım işlemi ayarlamak için Azure Stream Analytics CI/CD npm paketini kullanmayı açıklar.

## <a name="build-the-vs-code-project"></a>VS Code projeyi oluşturun

Sürekli tümleştirme ve dağıtım kullanarak Azure Stream Analytics işleri için etkinleştirebilirsiniz **asa streamanalytics cıcd** npm paketi. Npm paketi, Azure Resource Manager şablonları oluşturmak için araçlar sağlar [Stream Analytics Visual Studio Code projeleri](quick-create-vs-code.md). Bunu Windows, macOS ve Linux üzerinde Visual Studio Code yükleme olmadan kullanılabilir.

Yapılandırmasını tamamladıktan [paketini karşıdan](https://www.npmjs.com/package/azure-streamanalytics-cicd), Azure Resource Manager şablonları çıktısını almak için aşağıdaki komutu kullanın. **ScriptPath** bağımsız değişkeni için mutlak yoludur **asaql** projenizdeki dosya. Asaproj.json ve JobConfig.json dosyaları, komut dosyası ile aynı klasörde olduğundan emin olun. Varsa **outputPath** belirtilmezse, şablonları yerleştirilecek **Dağıt** projenin altında klasör **bin** klasör.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Örnek (macos'ta ile)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Stream Analytics Visual Studio Code Proje başarıyla oluşturulursa, altında aşağıdaki iki Azure Resource Manager şablonu dosyaları oluşturur **bin / Debug/perakende / Deploy** klasörü: 

*  Resource Manager şablon dosyası

       [ProjectName].JobTemplate.json 

*  Resource Manager parametre dosyası

       [ProjectName].JobTemplate.parameters.json   

Visual Studio Code projenizdeki ayarlarından parameters.json dosyasındaki varsayılan parametreleri var. Parametreleri, başka bir ortama dağıtmak istiyorsanız, uygun şekilde değiştirin.

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
