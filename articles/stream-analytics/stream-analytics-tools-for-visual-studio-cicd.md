---
title: Sürekli tümleştirme ve akış analizi araçları ile geliştirme
description: Bu makalede, bir sürekli tümleştirme ve dağıtım işlemini ayarlamak için Azure akış analizi için Visual Studio Araçları kullanmayı açıklar.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 9/27/2017
ms.openlocfilehash: e4e831c602255df66f4c86ffa17336f51d2b52f7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30906282"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-tools"></a>Sürekli tümleştirme ve akış analizi araçları ile geliştirme
Bu makalede sürekli tümleştirme ve dağıtım işlemi kurmak için Visual Studio için Azure Stream Analytics araçları kullanmak için nasıl kullanılacağını açıklar.

Kullanım sürüm 2.3.0000.0 veya üstü, [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio) MSBuild için destek alma.

Bir NuGet paketi kullanılabilir: [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). MSBuild, yerel çalıştırma ve akış analizi Visual Studio projeleri sürekli tümleştirme ve dağıtım işlemini desteklemek dağıtım araçları sağlar. 
> [!NOTE] 
NuGet paket yalnızca 2.3.0000.0 veya üzeri sürümü akış analiz araçları, Visual Studio için kullanılabilir. Visual Studio Araçları'nın önceki sürümlerinde oluşturulan projeleri varsa, bunları 2.3.0000.0 veya üstü sürüme açmanız yeterlidir ve kaydedin. Ardından yeni özellikleri etkinleştirilir. 

Daha fazla bilgi için bkz: [Visual Studio için Stream Analytics Araçları](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Standart Visual Studio MSBuild deneyimi gibi bir projeyi derlemek için iki seçeneğiniz vardır. Projeye sağ tıklayın ve ardından **yapı**. Aynı zamanda **MSBuild** komut satırından NuGet paketi.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Stream Analytics Visual Studio projesi başarıyla oluşturduğunda, altında aşağıdaki iki Azure Resource Manager şablon dosyalarını oluşturur **bin / Debug/perakende / Deploy** klasörü: 

*  Resource Manager şablon dosyası

       [ProjectName].JobTemplate.json 

*  Resource Manager parametre dosyası

       [ProjectName].JobTemplate.parameters.json   

Parameters.json dosyasındaki varsayılan parametreler, Visual Studio projesi ayarlarında arasındadır. Başka bir ortama dağıtmak istiyorsanız, parametrelerin uygun şekilde değiştirin.

> [!NOTE] 
Tüm kimlik bilgileri için varsayılan değerleri ayarlamak için null. Olduğunuz *gerekli* buluta dağıtmadan önce değerleri ayarlamak için.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Nasıl yapılır hakkında daha fazla bilgi [bir Resource Manager şablon dosyası ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Nasıl yapılır hakkında daha fazla bilgi [Resource Manager şablonu içindeki bir parametre olarak bir nesne kullanmak](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).


## <a name="command-line-tool"></a>Komut satırı aracı

### <a name="build-the-project"></a>Projeyi derleme
NuGet paketi olarak adlandırılan bir komut satırı aracı olan **SA.exe**. Proje derleme ve sürekli tümleştirme ve sürekli teslim işlemini kullanabilirsiniz rasgele bir makinede yerel sınama destekler. 

Dağıtım dosyaları varsayılan olarak geçerli dizinin altında yer alır. Çıkış yolu aşağıdaki - OutputPath parametresini kullanarak belirtebilirsiniz:

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Betik üzerinde yerel olarak test etme

Projeniz Visual Studio'da yerel giriş dosyaları belirtilen sahip, kullanarak otomatik komut dosyası test çalıştırabilirsiniz *localrun* komutu. Elde edilen sonucu geçerli dizinin altında yer alır.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-a-job-definition-file-to-use-with-the-stream-analytics-powershell-api"></a>Stream Analytics PowerShell API'si ile kullanmak için bir iş tanımı dosyası oluştur

*Arm* komutu proje şablonu ve yapı giriş olarak aracılığıyla oluşturulan iş şablon parametresi dosyaları alır. Ardından, bunları Stream Analytics PowerShell API'si ile kullanılan bir iş tanımı JSON dosyası birleştirir.

```
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```
Örnek:
```
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


