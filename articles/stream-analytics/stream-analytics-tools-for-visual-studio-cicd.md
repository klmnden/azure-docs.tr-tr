---
title: Sürekli tümleştirme ve Stream Analytics araçları ile geliştirme
description: Bu makalede, bir sürekli tümleştirme ve dağıtım işlemi ayarlamak için Azure Stream Analytics için Visual Studio Araçları kullanmayı açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2017
ms.openlocfilehash: 641254be37ac0019ee6a256fc99f96fc3bfb75a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761514"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-tools"></a>Sürekli tümleştirme ve Stream Analytics araçları ile geliştirme
Bu makalede, Visual Studio için Azure Stream Analytics araçları bir sürekli tümleştirme ve dağıtım sürecini kullanmayı kullanmayı açıklar.

2.3.0000.0 sürümünü kullanın veya üstü, [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) MSBuild için destek alma.

Bir NuGet paketi kullanılabilir: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Bu, Stream Analytics Visual Studio projelerinin sürekli tümleştirme ve dağıtım işlemini destekleyen MSBuild, yerel çalıştırma ve dağıtım araçlarını sağlar. 
> [!NOTE]
> Visual Studio için NuGet paketini yalnızca 2.3.0000.0 veya Stream Analytics Araçları'nın sürümünden sonraki bir sürümü kullanılabilir. Visual Studio Araçları'nın önceki sürümlerinde oluşturulmuş projeleri varsa 2.3.0000.0 veya sürümünden sonraki bir sürümü yalnızca açın ve kaydedin. Ardından, yeni özellikler etkinleştirilir. 

Daha fazla bilgi için [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Standart Visual Studio MSBuild deneyimi gibi bir proje oluşturmak için iki seçeneğiniz vardır. Projeye sağ tıklayın ve ardından **yapı**. Ayrıca **MSBuild** komut satırından NuGet paketi olarak.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Stream Analytics Visual Studio Proje başarıyla oluşturulursa, altında aşağıdaki iki Azure Resource Manager şablonu dosyaları oluşturur **bin / Debug/perakende / Deploy** klasörü: 

*  Resource Manager şablon dosyası

       [ProjectName].JobTemplate.json 

*  Resource Manager parametre dosyası

       [ProjectName].JobTemplate.parameters.json   

Visual Studio projenize ayarlarında parameters.json dosyasındaki varsayılan parametreleri arasındadır. Parametreleri, başka bir ortama dağıtmak istiyorsanız, uygun şekilde değiştirin.

> [!NOTE]
> Tüm kimlik bilgilerini varsayılan değerlerin ayarlandığından null. İşiniz *gerekli* buluta dağıtmadan önce değerleri ayarlamak için.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Kullanma hakkında daha fazla bilgi edinin [bir Resource Manager şablon dosyası ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Kullanma hakkında daha fazla bilgi edinin [Resource Manager şablonunda bir parametre olarak bir nesne kullanmasını](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Yönetilen kimliği çıkış havuzu olarak Azure Data Lake Store Gen1 kullanmak için hizmet sorumlusu Azure'a dağıtmadan önce PowerShell kullanarak erişebilmesi gerekir. Kullanma hakkında daha fazla bilgi edinin [ADLS Gen1 yönetilen kimliği ile Resource Manager şablonu ile dağıtma](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).


## <a name="command-line-tool"></a>Komut satırı aracı

### <a name="build-the-project"></a>Projeyi derleme
NuGet paketi olarak adlandırılan bir komut satırı aracı olan **SA.exe**. Bu, Proje yapı ve sürekli tümleştirme ve sürekli teslim işlem içinde kullanabileceğiniz rastgele bir makinede yerel test destekler. 

Dağıtım dosyaları varsayılan olarak geçerli dizininin altına yerleştirilir. Çıkış yolu aşağıdaki - OutputPath parametresini kullanarak belirtebilirsiniz:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Betik üzerinde yerel olarak test etme

Projenizi Visual Studio'da yerel giriş dosyası belirtildi, kullanarak bir otomatik komut test çalıştırabilirsiniz *localrun* komutu. Çıkış sonucu geçerli dizininin altına yerleştirilir.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Stream Analytics PowerShell API'si ile kullanmak için bir iş tanımı dosyası oluştur

*Arm* komutu, proje şablonu ve giriş olarak derleme üzerinden oluşturulmuş iş şablon parametre dosyaları alır. Ardından, bunları Stream Analytics PowerShell API'si ile kullanılabilecek bir iş tanımı JSON dosyasını birleştirir.

```powershell
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Örnek:
```powershell
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


