---
title: "Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için Azure CLI kullanma | Microsoft Docs"
description: "Uyumlu olmayan kaynakları belirlemek üzere bir Azure İlkesi ataması oluşturmak için PowerShell kullanın."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 88ceb47d46b66e716c6c263098d5b9458e4aff22
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Azure CLI ile Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturun

Azure’da uyumluluğu anlamanın ilk adımı kendi mevcut kaynaklarınızın durumunu bilmektir. Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, hangi sanal makinelerin yönetilen disk kullanmadığını ve bu nedenle *uyumsuz* olduğunu başarılı bir şekilde belirlemiş olacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup Yönetilen Diskleri Olmayan Sanal Makineleri Denetle tanımını atayacağız. Bu ilke tanımı, ilke tanımında ayarlanan koşullar ile uyumlu olmayan kaynakları belirler.

Yeni ilke ataması oluşturmak için bu adımları izleyin.

Tüm ilke tanımlarını görüntüleyin ve “Yönetilen Diskleri Olmayan Sanal Makineleri Denetle” ilke tanımını bulun:

```azurecli
az policy definition list
```

Azure İlkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Şunlara benzer yerleşik ilke tanımları görürsünüz:

- Etiketi ve değerini zorla
- Etiketi ve değerini uygula
- SQL Server Sürüm 12.0 gerektir

Daha sonra, ilke tanımını atamak için aşağıdaki bilgileri sağlayıp aşağıdaki komutu çalıştırın:

- İlke ataması için **Ad**’ı görüntüler. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* seçeneğini kullanalım.
- **İlke** - Bu, atamayı oluşturmak için kullandığınız ilke tanımıdır. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımıdır
- Bir **kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir.

  Önceden kaydolduğunuz aboneliği (veya kaynak grubunu) kullanın. Bu örnekte sırasıyla şu abonelik kimliğini ve kaynak grubu adını kullanıyoruz: **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** - **FabrikamOMS**. Bunları çalıştığınız aboneliğin kimliği ve kaynak grubunun adıyla değiştirdiğinizden emin olun.

Komut şöyle görünmelidir:

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

   Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa, yukarıda gösterildiği gibi **Uyumlu olmayan kaynaklar** sekmesinde görünür.

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
