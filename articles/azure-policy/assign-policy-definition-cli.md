---
title: "Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için Azure CLI kullanma | Microsoft Docs"
description: "Uyumlu olmayan kaynakları belirlemek üzere bir Azure İlkesi ataması oluşturmak için PowerShell kullanın."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 76725f3ebeaf5af4f2ab8aadb303d862fa111ecb
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Azure CLI ile Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturun

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir. Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile *uyumsuzdur*.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmak için, bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup Yönetilen Diskleri Olmayan Sanal Makineleri Denetle tanımını atayacağız. Bu ilke tanımı, ilke tanımında ayarlanan koşullar ile uyumlu olmayan kaynakları belirler.

Yeni ilke ataması oluşturmak için şu adımları izleyin:

1. Aboneliğinizin kaynak sağlayıcısı ile çalıştığından emin olmak için, Policy Insights kaynak sağlayıcısını kaydedin. Bir kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini gerçekleştirme iznine sahip olmanız gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur.

    Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

    ```azurecli
    az provider register --namespace Microsoft.PolicyInsights
    ```

    Komut, kayıt işleminin devam ettiğini belirten bir ileti döndürür.

    Aboneliğinizde kaynak sağlayıcısından edindiğiniz kaynak türleri varken, bir kaynak sağlayıcısının kaydını silemezsiniz. Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [Kaynak Sağlayıcıları ve Türleri](../azure-resource-manager/resource-manager-supported-services.md).

2. Tüm ilke tanımlarını görüntüleyin ve *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımını bulun:

    ```azurecli
az policy definition list
```

    Azure İlkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Şunlara benzer yerleşik ilke tanımları görürsünüz:

    - Etiketi ve değerini zorla
    - Etiketi ve değerini uygula
    - SQL Server Sürüm 12.0 gerektir

3. Daha sonra, ilke tanımını atamak için aşağıdaki bilgileri sağlayıp aşağıdaki komutu çalıştırın:

    - İlke ataması için **Ad**’ı görüntüler. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* seçeneğini kullanalım.
    - **İlke** - Bu, atamayı oluşturmak için kullandığınız ilke tanımıdır. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımıdır
    - Bir **kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir.

    Önceden kaydolduğunuz aboneliği (veya kaynak grubunu) kullanın. Bu örnekte **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** abonelik kimliğini ve **FabrikamOMS** kaynak grubu adını kullanıyoruz. Bunları çalıştığınız aboneliğin kimliği ve kaynak grubunun adıyla değiştirdiğinizden emin olun.

    Komut şuna benzemelidir:

    ```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilkedir. Bu kapsamın dahilinde de yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Bu yeni atama altında uyumlu olmayan kaynakları görüntülemek için:

1. Azure İlkesi sayfasına geri dönün.
2. Sol bölmede **Uyumluluk**’u seçin ve oluşturduğunuz **İlke Ataması**’nı arayın.

   ![İlke uyumluluğu](media/assign-policy-definition/policy-compliance.png)

   Bu yeni atamayla uyumlu olmayan mevcut kaynaklar, **Uyumlu olmayan kaynaklar** sekmesi altında görünür. Yukarıdaki resimde, uyumlu olmayan kaynaklar gösterilmektedir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlar, bu hızlı başlangıcı temel alır. Sonraki kılavuzlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu komutu çalıştırarak oluşturduğunuz atamayı silin:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı atadınız.

İlkeleri atama hakkında daha fazla bilgi edinmek için, **gelecekte** oluşturduğunuz kaynakların uyumlu olduğundan emin olmak üzere, şu öğretici ile devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./create-manage-policy.md)
