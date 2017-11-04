---
title: "Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturmak için Azure CLI kullanın | Microsoft Docs"
description: "Uyumlu olmayan kaynakları tanımlamak için bir Azure ilke ataması oluşturmak için PowerShell kullanın."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 764554a6afcc7912c53fc5000a6af44abb2adc99
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Azure CLI ile Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke atamasını oluşturma

Azure'da anlama uyumluluk ilk adımı, kendi geçerli kaynaklarla göze burada bilmektir. Bu hızlı başlangıç yönetilen diskleri kullanmıyorsanız sanal makineleri tanımak amacıyla bir ilke atamasını oluşturma sürecinde adımları.

Bu işlemin sonunda hangi sanal makineleri yönetilen diskleri kullanmıyorsanız başarıyla tanımladınız ve bu nedenle *uyumlu olmayan*.
.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="opt-in-to-azure-policy"></a>Azure ilke kabul

Azure ilke genel Önizleme'de kullanıma sunulmuştur ve erişim isteyen kaydetmeniz gerekir.

1. Git Azure ilke https://aka.ms/getpolicy ve select **kaydolun** sol bölmede.

   ![İlke Ara](media/assign-policy-definition/sign-up.png)

2. Azure ilke Aboneliklerde seçerek kabul **abonelik** çalışmak istediğiniz listesi. Ardından **kaydetmek**.

   ![Azure ilke kullanmayı kabulü](media/assign-policy-definition/preview-opt-in.png)

   İsteğiniz Önizleme için otomatik olarak onaylanır. Lütfen sisteme kaydınızı işlemek 30 dakika bekleyin.

## <a name="create-a-policy-assignment"></a>Bir ilke atamasını oluşturma

Bu hızlı başlangıç içinde bir ilke ataması oluşturun ve yönetilen diskleri tanımı olmadan denetim sanal makineleri atayın. Bu ilke tanımı ilke tanımı'nda ayarlanan koşulları ile uyumlu olmayan kaynakları tanımlar.

Yeni bir ilke ataması oluşturmak için aşağıdaki adımları izleyin.

Tüm ilke tanımları görüntüleyebilir ve "Yönetilen diskleri olmadan denetim sanal makineler" ilke tanımı bulunamadı:

```azurecli
az policy definition list
```

Azure ilke ile birlikte gelen zaten yerleşik ilke tanımlarında kullanabilirsiniz. Yerleşik ilke tanımları gibi görürsünüz:

- Etiket ve değerini zorla
- Etiket ve değerini Uygula
- SQL Server sürümü 12.0 gerektirir

Ardından, aşağıdaki bilgileri sağlayın ve ilke tanımı atamak için aşağıdaki komutu çalıştırın:

- Görüntü **adı** ilke ataması için. Bu durumda, kullanalım *yönetilen diski olmayan sanal makineler denetim*.
- **İlke** – devre dışı, kullanmakta olduğunuz atamayı oluşturmak için temel ilke tanımı, budur. Bu durumda, ilke tanımı – olduğu *denetim yönetilen diski olmayan sanal makineler*
- A **kapsam** - hangi kaynakların bir kapsamı belirler veya kaynakları gruplandırma ilke ataması üzerinde zorlanan. Bir abonelik için kaynak gruplarını aralığında.

  Bu abonelik kimliği - kullanarak Biz bu örnekte, daha önce kaydettiğiniz Azure ilkesine seçti zaman abonelik (veya kaynak grubu) kullanmak **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** ve kaynak grubu adı - **FabrikamOMS**. Bu abonelik Kimliğini ve çalıştığınız kaynak grubunun adı ile değiştirdiğinizden emin olun.

Bu komut aşağıdaki gibi görünmelidir.

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Bir ilke atamasını belirli bir kapsamda gerçekleşmesi için atanan bir ilkedir. Bu kapsam için bir kaynak grubu yönetim grubundan değişiklik gösterebilir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleyin

Bu yeni atama altında uyumlu olmayan kaynakları görüntülemek için:

1. Azure ilke sayfasına gidin.
2. Seçin **Uyumluluk** sol bölmesinde ve arama **ilke ataması** oluşturduğunuz.

   ![İlke uyumluluğu](media/assign-policy-definition/policy-compliance.png)

   Bu yeni atama ile uyumlu olmayan tüm mevcut kaynaklar varsa, bunlar altında görünmesini **uyumsuz kaynakları** sekmesinde, yukarıda gösterildiği gibi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlarını Bu hızlı başlangıç oluşturun. Sonraki öğreticilerde ile çalışmaya devam etmeyi planlıyorsanız, temiz bu quickstart oluşturulan kaynakları yukarı değil. Devam etmek düşünmüyorsanız, şu komutu çalıştırarak oluşturduğunuz atamasını silin:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke tanımı atanır.

Kaynakları oluşturduğunuz emin olmak için ilke atama hakkında daha fazla bilgi edinmek için **gelecekteki** uyumlu, Öğretici için devam edin:

> [!div class="nextstepaction"]
> [Oluşturma ve ilkelerini yönetme](./create-manage-policy.md)
