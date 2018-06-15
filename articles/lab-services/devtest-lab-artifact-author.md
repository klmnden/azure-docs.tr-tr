---
title: DevTest Labs sanal makineniz için özel yapılar oluşturma | Microsoft Docs
description: Azure DevTest Labs ile kullanmak üzere kendi yapıları Yazar öğrenin.
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
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 268b9af7835c51d78812b35aff5aaac585961b01
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787629"
---
# <a name="create-custom-artifacts-for-your-devtest-labs-virtual-machine"></a>DevTest Labs sanal makineniz için özel yapılar oluşturma

Bu makalede açıklanan adımları genel bir bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Genel Bakış
Kullanabileceğiniz *yapıları* dağıtıp VM sağlamak sonra uygulamanızı ayarlama. Yapı tanımı dosyası ve bir Git deposu klasöründe depolanan diğer komut dosyalarını bir yapı oluşur. Yapı tanımı dosyaları JSON ve bir VM yüklemek istediğiniz belirtmek için kullanabileceğiniz ifadeler oluşur. Örneğin, bir yapı, çalıştırılacak komutu ve komutu çalıştırdığınızda kullanılabilen parametreleri adını tanımlayabilirsiniz. Yapı tanımı dosyası içinde başka komut dosyaları ada göre başvurabilirsiniz.

## <a name="artifact-definition-file-format"></a>Yapı tanımı dosyası biçimi
Aşağıdaki örnek, bir tanım dosyasını temel yapınızı oluşturan bölümleri gösterir:

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
| $schema |Hayır |JSON şema dosyası konumu. JSON şema dosyası tanım dosyasının geçerliliğini sınamak yardımcı olabilir. |
| başlık |Evet |Laboratuar ortamında görüntülenen yapı adı. |
| açıklama |Evet |Laboratuar ortamında görüntülenen yapı açıklaması. |
| iconUri |Hayır |Laboratuar ortamında görüntülenen simge URI'si. |
| targetOsType |Evet |İşletim sistemi yapı yüklendiği VM. Windows ve Linux bunun desteklenen seçeneklerdir. |
| parametreler |Hayır |Bir makinede yapı yükleme komutunu çalıştırdığınızda, sağlanan değerler. Bu, yapı özelleştirmenize yardımcı olur. |
| KomutÇalıştır |Evet |Yapı bir VM üzerinde yürütülen komutu yükleyin. |

### <a name="artifact-parameters"></a>Yapı parametreleri
Tanım dosyası Parametreler bölümünde, bir yapı yükledikleri zaman, hangi değerlerin bir kullanıcı girişi belirtin. Yapı yükleme komut bu değerlere başvurabilirsiniz.

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
| type |Evet |Parametre değeri türü. İzin verilen türleri için aşağıdaki listeye bakın. |
| Görünen adı |Evet |Laboratuvara kullanıcıya görüntülenen parametresinin adı. | |
| açıklama |Evet |Laboratuar ortamında görüntülenen parametre açıklaması. |

İzin verilen türleri şunlardır:

* dizesi (tüm geçerli JSON)
* int (geçerli bir JSON tamsayı)
* bool (tüm geçerli JSON Boolean)
* dizi (geçerli bir JSON dizisi)

## <a name="artifact-expressions-and-functions"></a>Yapı ifadeleri ve İşlevler
İfade kullanabilir ve yapı oluşturmak için işlevleri yükleme komutu.
İfadeler ile parantez içine ([ve]) ve yapı yüklendiğinde değerlendirilir. İfadeler bir JSON dizesi değerindeki herhangi bir yerde görünebilir. İfadeler her zaman başka bir JSON değerini döndürür. Köşeli ayraç ([]) ile başlayan bir sabit dize kullanmanız gerekiyorsa, iki ayraç ([[) kullanmanız gerekir.
Genellikle, ifadeler işlevleriyle bir değer oluşturmak için kullanılır. Yalnızca JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi **functionName (arg1, arg2, arg3)**.

Aşağıdaki liste, genel işlevler gösterir:

* **parameters(parameterName)**: Yapı komutu çalıştırdığınızda, sağlanan bir parametre değeri döndürür.
* **concat (arg1, arg2, arg3,...)** : Birden çok dize değerlerini birleştirir. Bu işlev bağımsız değişkenleri, çeşitli alabilir.

Aşağıdaki örnek, ifadeler ve işlevleri bir değer oluşturmak için nasıl kullanılacağı gösterilmektedir:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Özel yapı oluşturma

1. Bir JSON Düzenleyicisi'ni yükleyin. Yapı tanımı dosyalar üzerinde çalışmak için bir JSON Düzenleyicisi gerekir. Kullanmanızı öneririz [Visual Studio Code](https://code.visualstudio.com/), Windows, Linux ve OS X için kullanılabilir olduğu.
2. Bir örnek artifactfile.json tanım dosyasını alın. DevTest Labs ekibi tarafından oluşturulan yapıları kullanıma bizim [GitHub deposunu](https://github.com/Azure/azure-devtestlab). Kendi yapıları oluşturmanıza yardımcı olabilir yapılarının zengin bir kitaplık oluşturduk. Yapı tanımı dosyasını indirin ve kendi yapıları oluşturmak için değişiklik.
3. IntelliSense ' ndan yararlanır. IntelliSense, yapı tanım dosyası oluşturmak için kullanabileceğiniz geçerli öğeleri görmek için kullanın. Bir öğenin değerleri için farklı seçenekler de görebilirsiniz. Örneğin, ne zaman düzenleme **targetOsType** öğesi, IntelliSense gösterir, iki seçenek, Windows veya Linux için.
4. Yapı depolama bir [Git deposu](devtest-lab-add-artifact-repo.md).
   
   1. Her yapı için ayrı bir dizin oluşturun. Dizin adı yapı adıyla aynı olmalıdır.
   2. Yapı tanımı dosyası (artifactfile.json), oluşturduğunuz dizinde depolayın.
   3. Yapı Yükleme komutundan başvurulan komut dosyalarını depolar.
      
      Yapı klasörü nasıl görünebileceği örnek aşağıda verilmiştir:
      
      ![Yapı klasörü örneği](./media/devtest-lab-artifact-author/git-repo.png)
5. Yapıları depo laboratuvara ekleme. Bkz: [yapıları ve şablonlar için Git deposu ekleme](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>İlgili makaleler
* [DevTest Labs de yapı hataları tanılamak nasıl](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'de bir Resource Manager şablonu kullanarak bir VM mevcut bir Active Directory etki alanına katılma](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Git yapıt deposu laboratuvara ekleme](devtest-lab-add-artifact-repo.md).

