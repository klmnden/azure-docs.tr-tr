---
title: Kaydet ve Azure DevTest labs'deki görüntülerini dağıtma | Microsoft Docs
description: Azure DevTest Labs'de bir özel görüntü fabrikası oluşturma hakkında bilgi edinin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: feabd055833e5f0d850138af528cce1da82cae49
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60622679"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Özel görüntüleri birden çok laboratuvara kaydetme ve dağıtma
Bu makalede arka planda önceden oluşturulmuş sanal makineler (VM) özel görüntüleri kaydetmek için adımları sağlar. Ayrıca, bu özel görüntüler kuruluştaki diğer DevTest Labs'de nasıl dağıtılacağını da kapsar.

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki öğeler zaten koşulların karşılanması:

- Azure DevTest Labs görüntü Fabrika için bir laboratuvar.
- Görüntü Fabrika otomatik hale getirmek için kullanılan bir Azure DevOps projesi.
- Betikler ve yapılandırmada (Bizim örneğimizde, önceki adımda bahsedilen aynı DevOps projesi) içeren kaynak kodu konumu.
- Azure Powershell görevleri otomatik düzende gerçekleştirmek için tanımı oluşturun.

Gerekirse, izleyeceğiniz adımlar [Azure DevOps bir görüntü fabrikası çalıştırma](image-factory-set-up-devops-lab.md) oluşturmak veya bu öğeleri ayarlamak için. 

## <a name="save-vms-as-generalized-vhds"></a>VM genelleştirilmiş VHD olarak Kaydet
Var olan VM genelleştirilmiş VHD olarak kaydedin.  Var olan VM genelleştirilmiş VHD olarak kaydetmek için bir PowerShell komut dosyası yoktur. Bunu kullanmak için ilk olarak, başka bir ekleyin **Azure Powershell** aşağıdaki görüntüde gösterildiği gibi görev için derleme tanımı:

![Azure PowerShell adım ekleme](./media/save-distribute-custom-images/powershell-step.png)

Listede yeni bir görev oluşturduktan sonra biz aşağıdaki görüntüde gösterildiği gibi tüm ayrıntıları doldurabilir için öğeyi seçin: 

![PowerShell ayarları](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Genelleştirilmiş özelleştirilmiş özel görüntüler
İçinde [Azure portalında](https://portal.azure.com), özel görüntü sanal makineyi oluştururken, genelleştirilmiş bir ya da özelleştirilmiş bir özel görüntü sahip olmayı seçebilirsiniz.

- **Özelleştirilmiş özel görüntü:** Makinede Sysprep/sağlamayı kaldırma çalıştırılmadı. Bu görüntü (anlık) var olan sanal makinede işletim sistemi diski tam bir kopyası olduğunu anlamına gelir.  Aynı dosyaları, uygulamaları, kullanıcı hesapları, bilgisayar adı ve benzeri, tüm bu özel bir görüntüden yeni bir makine oluşturduğumuzda yok.
- **Genelleştirilmiş özel görüntü:** Makinede Sysprep/sağlamayı kaldırma çalıştırıldı.  Bu işlem çalıştığında, bu kullanıcı hesaplarını kaldırır, bilgisayar adını kaldırır, kullanıcı kayıt defteri kovanları, vs. için başka bir sanal makine oluştururken özelleştirilebilir görüntüyü Genelleştirme amacıyla kullanıma kaldırır.  (Sysprep çalıştırılarak) bir sanal makineyi Genelleştir, geçerli sanal makine işlem yok eder: yeniden artık işlevsel olmayacaktır.

Özel görüntüleri görüntü fabrikada yaslama için betik VHD'ler önceki adımda oluşturduğunuz tüm sanal makineler için kaydedin (tanımlanan bir Azure kaynak etikete göre).

## <a name="update-configuration-for-distributing-images"></a>Görüntüleri dağıtmak için güncelleştirme yapılandırması
İşlemin sonraki adımda ihtiyaç diğer labs kullanarak görüntü Fabrika Laboratuvar alanından özel görüntüleri gönderme sağlamaktır. Bu işlem çekirdek parçasıdır **labs.json** yapılandırma dosyası. Bu dosyada bulabilirsiniz **yapılandırma** görüntü fabrikada bulunan klasör.

Labs.json yapılandırma dosyasında listelenen iki önemli nokta vardır:

- Abonelik kimliği ve Laboratuvar adını kullanarak belirli hedef Laboratuvar tanıtan.
- Yapılandırma köküne göreli yollar olarak laboratuvara gönderilen görüntüleri belirli kümesi. (Bu klasördeki tüm görüntüleri almak için) tüm klasörü belirtebilirsiniz çok.

Örnek labs.json dosyasında listelenen iki labs aşağıda verilmiştir. Bu durumda, iki farklı labs görüntülere dağıtıyoruz.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Derleme görevi oluşturma
Bu makalede daha önce gördüğünüz aynı adımları kullanarak ek bir ekleme **Azure Powershell** derleme görevi, derleme tanımı. Aşağıdaki görüntüde gösterildiği gibi ayrıntıları girin: 

![Görüntüleri dağıtmak için görev oluşturun](./media/save-distribute-custom-images/second-build-task-powershell.png)

Parametreler şunlardır: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Bu görevi, herhangi bir özel görüntü görüntü fabrikada mevcut alır ve Labs.json dosyasında tanımlanan tüm laboratuvarlara gönderir.

## <a name="queue-the-build"></a>Derlemeyi kuyruğa al
Dağıtım derleme görevi tamamlandıktan sonra her şeyin çalıştığından emin olmak için yeni bir yapıyı kuyruğa alın. Yapılandırma başarıyla tamamlandıktan sonra yeni özel görüntüleri Labs.json yapılandırma dosyasına girilen hedef Laboratuvardaki gösterilir.

## <a name="next-steps"></a>Sonraki adımlar
Serideki sonraki makalede görüntü factory bekletme ilkesi ve temizleme adımları ile güncelleştirin: [Bekletme ilkesi ayarlayabilir ve temizleme betikleri çalıştırma](image-factory-set-retention-policy-cleanup.md).
