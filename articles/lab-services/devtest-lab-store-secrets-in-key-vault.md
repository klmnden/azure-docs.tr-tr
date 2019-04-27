---
title: Azure DevTest labs'deki bir anahtar kasasındaki gizli dizileri Store | Microsoft Docs
description: Bir Azure anahtar Kasası'nda gizli dizileri depolamak ve formül, veya ortam bir VM oluşturulurken kullanma hakkında bilgi edinin.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: 17469d3602935715d570a496e12b6680269ff465
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622940"
---
# <a name="store-secrets-in-a-key-vault-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir anahtar kasasındaki gizli dizileri Store
Azure DevTest Labs'i kullanarak, karmaşık bir parola girmeniz gerekebilir: Windows vm'nizin Linux VM veya kişisel erişim belirteci bir yapıt Git deponuzu kopyalamak bir ortak SSH anahtarının parola. Gizli dizileri genellikle uzun ve rastgele karakterler sahiptir. Bu nedenle, başlarına zor ve kullanışsız bulabilir, özellikle birden çok kez aynı parolayı kullanmanız durumunda olabilir.

Bu sorunu çözmek ve ayrıca, gizli dizileri güvenli bir yerde tutmak için depolama gizli DevTest Labs destekleyen bir [Azure anahtar kasası](../key-vault/key-vault-overview.md). Bir kullanıcı ilk kez bir gizli dizi kaydettiğinde, DevTest Labs hizmeti Laboratuvar içeren ve gizli anahtar kasasındaki aynı kaynak grubunda bir anahtar kasasına otomatik olarak oluşturur. DevTest Labs, her kullanıcı için ayrı bir anahtar kasası oluşturulur. 

## <a name="save-a-secret-in-azure-key-vault"></a>Azure anahtar Kasası'nda bir gizli dizi Kaydet
Gizli anahtarı Azure anahtar Kasası'nda kaydetmek için aşağıdaki adımları uygulayın:

1. Seçin **My gizli dizileri** sol menüsünde.
2. Girin bir **adı** için gizli anahtarı. Bu formül, bir VM oluştururken açılan listenin ya da bir ortam adı görürsünüz. 
3. Gizli dizi olarak girin **değer**.

    ![Store gizli anahtarı](media/devtest-lab-store-secrets-in-key-vault/store-secret.png)

## <a name="use-a-secret-from-azure-key-vault"></a>Azure Key vault'tan bir gizli diziyi kullanın
Formül, bir VM oluşturmak için bir gizli dizi veya bir ortam girmek gerektiğinde el ile bir gizli dizi girin veya kaydedilmiş bir gizli dizi key vault'tan seçim yapın. Anahtar kasanızda depolanan gizli dizi kullanmak için aşağıdaki eylemleri gerçekleştirin:

1. Seçin **kaydedilmiş bir gizli diziyi kullanın**. 
2. Gizli anahtarı için aşağı açılan listeden seçin **gizli dizi seçme**. 

    ![VM gizli diziyi kullanın](media/devtest-lab-store-secrets-in-key-vault/secret-store-pick-a-secret.png)

## <a name="use-a-secret-in-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonunda bir gizli diziyi kullanın
Aşağıdaki örnekte gösterildiği gibi bir VM oluşturmak için kullanılan bir Azure Resource Manager şablonunda, gizli dizi adı belirtebilirsiniz:

![Gizli formülü ya da ortam kullanın](media/devtest-lab-store-secrets-in-key-vault/secret-store-arm-template.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Gizli dizi kullanarak VM oluşturma](devtest-lab-add-vm.md) 
- [Gizli dizi kullanarak formül oluşturma](devtest-lab-manage-formulas.md)
- [Gizli dizi kullanarak bir ortam oluşturun](devtest-lab-create-environment-from-arm.md)
