---
title: DevTest Labs sanal makineniz için özel yapıtlar oluşturma | Microsoft Docs
description: Azure DevTest Labs ile kullanmak için kendi yapıtları Yazar öğrenin.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: 0d1e269a1818f013bc14842bc541216d7f31bc84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60311134"
---
# <a name="create-custom-artifacts-for-your-devtest-labs-virtual-machine"></a>DevTest Labs sanal makineniz için özel yapıtlar oluşturma

Bu makalede açıklanan adımlara genel bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
>
>

## <a name="overview"></a>Genel Bakış
Kullanabileceğiniz *yapıtları* dağıtıp, bir VM sağlama sonra uygulamanızı ayarlama. Bir yapı tanımı dosyası ve bir klasördeki bir Git deposunda depolanan diğer komut dosyalarını bir yapıt oluşur. Yapıt tanımı dosyaları JSON ve bir VM'ye yüklemek istediğinizi belirtmek için kullanabileceğiniz ifadeler oluşur. Örneğin, çalıştırmak için bir komutu ve komutu çalıştırdığınızda kullanılabilen parametreleri bir yapıt adı tanımlayabilirsiniz. Yapıt tanım dosyası içinde diğer komut dosyaları ada göre bakabilirsiniz.

## <a name="artifact-definition-file-format"></a>Yapıt tanımlama dosyası biçimi
Aşağıdaki örnek, bir tanım dosyasının temel yapıyı oluşturan bölümlerin gösterir:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Öğe adı | Gerekli mi? | Açıklama |
| --- | --- | --- |
| $schema |Hayır |JSON şema dosyasının konumu. JSON şema dosyası tanım dosyasının geçerliliğini sınamak için yardımcı olabilir. |
| başlık |Evet |Laboratuar ortamında görüntülenen yapıtın adı. |
| açıklama |Evet |Laboratuar ortamında görüntülenen yapıt açıklaması. |
| iconUri |Hayır |Laboratuar ortamında gösterilen simge URI'si. |
| targetOsType |Evet |Yapıt yüklendiği VM'nin işletim sistemi. Desteklenen Seçenekler şunlardır: Windows ve Linux. |
| parametreler |Hayır |Yapıt yükleme komutunu bir makinede çalıştırıldığında, sağlanan değerler. Bu, yapıt özelleştirmenize yardımcı olur. |
| runCommand |Evet |Yapıt bir VM'de çalıştırılan komut yükleyin. |

### <a name="artifact-parameters"></a>Yapıt parametreleri
Tanım dosyasının parametreleri bölümünde, hangi değerlerin bir kullanıcının bir yapıtı yüklediğinizde girebilirsiniz belirtin. Yapıt yükleme komutu bu değerlere başvurabilirsiniz.

Parametreler tanımlamak için aşağıdaki yapısını kullanın:

    "parameters": {
      "<parameterName>": {
        "type": "<type-of-parameter-value>",
        "displayName": "<display-name-of-parameter>",
        "description": "<description-of-parameter>"
      }
    }

| Öğe adı | Gerekli mi? | Açıklama |
| --- | --- | --- |
| type |Evet |Parametre değerinin türü. İzin verilen türler için aşağıdaki listeye bakın. |
| displayName |Evet |Laboratuvardaki kullanıcıya görüntülenen parametrenin adı. |
| açıklama |Evet |Laboratuar ortamında görüntülenen parametre açıklaması. |

İzin verilen türleri şunlardır:

* dize (geçerli bir JSON dizesi)
* int (geçerli bir JSON tamsayı)
* bool (tüm geçerli JSON Boolean)
* dizi (geçerli bir JSON dizisi)

## <a name="artifact-expressions-and-functions"></a>Yapıt ifadeler ve İşlevler
İfadeleri kullanabilirsiniz ve yapı oluşturmak için işlevleri yükleme komutu.
İfadeleri köşeli parantez ile içine ([ve]) ve yapıt yüklendiğinde değerlendirilir. İfadeleri herhangi bir JSON dizesi değerinin görünebilir. İfadeler her zaman başka bir JSON değeri döndürür. Bir köşeli ayraç ([]) ile başlayan bir sabit dizesi kullanmanız gerekiyorsa, iki köşeli ayraçlar ([[) kullanmanız gerekir.
Genellikle, ifadeleri işlevleri ile bir değer oluşturmak için kullanırsınız. Yalnızca JavaScript'te işlev çağrıları olarak biçimlendirilmiş gibi **functionName (arg1, arg2, arg3)**.

Aşağıdaki liste, genel işlevleri gösterir:

* **parameters(parameterName)**: Yapıt komutu çalıştırdığınızda, sağlanan bir parametre değeri döndürür.
* **concat (arg1, arg2, arg3,...)** : Birden çok dize değerleri birleştirir. Bu işlev bağımsız değişken alabilir.

Aşağıdaki örnek, ifadeler ve İşlevler bir değer oluşturmak için nasıl kullanılacağını gösterir:

    runCommand": {
        "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Özel yapıt oluşturma

1. Bir JSON Düzenleyicisi'ni yükleyin. Yapıt tanımı dosyaları ile çalışmak için bir JSON Düzenleyicisi gerekir. Kullanmanızı öneririz [Visual Studio Code](https://code.visualstudio.com/), Windows, Linux ve OS X için kullanılabilen.
2. Bir örnek artifactfile.json tanım dosyasını alın. DevTest Labs ekibi tarafından oluşturulan yapılar kullanıma sunduğumuz [GitHub deposu](https://github.com/Azure/azure-devtestlab). Kendi yapıtları oluşturmanıza yardımcı olabilecek yapıtlar içeren zengin bir kitaplık oluşturduk. Bir yapı tanımı dosyasını indirin ve kendi yapıtlar oluşturmak için ona değişiklikler.
3. IntelliSense yararlanır. Bir yapı tanımı dosyası oluşturmak için kullanabileceğiniz geçerli öğelerini görmek için IntelliSense'i kullanın. Ayrıca, bir öğenin değerler için farklı seçenekleri görebilirsiniz. Örneğin, ne zaman düzenleme **targetOsType** öğe, IntelliSense gösterir, iki seçenek, Windows veya Linux için.
4. Yapıt içinde Store [genel bir Git deposu için DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) veya [kendi Git deponuzda](devtest-lab-add-artifact-repo.md). Genel depoda, diğer kullanıcılar tarafından doğrudan kullanın veya bunları ihtiyaçlarınıza uyacak şekilde özelleştirmeniz paylaşılan yapıtları görüntüleyebilirsiniz.
   
   1. Her bir yapıt için ayrı bir dizin oluşturun. Dizin adı yapıt adıyla aynı olmalıdır.
   2. Yapıt tanımı dosyası (artifactfile.json), oluşturduğunuz dizine Store.
   3. Yapıt yükleme komutu başvurulan komut dosyaları Store.
      
      Bir yapı klasörüne nasıl görünebileceği örnek aşağıda verilmiştir:
      
      ![Yapıt klasör örneği](./media/devtest-lab-artifact-author/git-repo.png)
5. Depo için laboratuvar yapıtlarını depolamak için kendi deposu kullanıyorsanız, makaledeki yönergeleri izleyerek ekleyin: [Yapıtlar ve şablonlar için Git deposu ekleme](devtest-lab-add-artifact-repo.md).

## <a name="related-articles"></a>İlgili makaleler
* [DevTest Labs yapıt hatalarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir sanal makine mevcut bir Active Directory etki alanına katılın](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [laboratuvara Git yapıt deposu ekleme](devtest-lab-add-artifact-repo.md).
