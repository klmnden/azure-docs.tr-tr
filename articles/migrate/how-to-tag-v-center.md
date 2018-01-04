---
title: VMware vCenter etiketlemesi ile sanal makineleri Grup | Microsoft Docs
description: "Azure geçiş hizmeti ile bir değerlendirme çalıştırmadan önce bir grup oluşturmayı açıklar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 967c2a1fc0e5144c6c7d5c8c4a81cec3d77ffdb5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>VCenter etiketleme ile Grup VM'ler

Bu makalede makineler için bir grup oluşturmak nasıl [Azure geçirmek](migrate-overview.md) VMware Vcenter'da etiketleme kullanarak değerlendirmesi. Bir grup oluşturduğunuzda, kullanmak istediğiniz etiketi kategori belirtin. 

## <a name="set-up-tagging"></a>Etiketleme ayarlama

Azure geçirmek dağıtım sırasında bir şirket içi Azure VM geçişi bir vCenter sunucusu tarafından yönetilen ESXi konakları üzerinde çalışan makineler bulur. Bulma işlemi önce etiketleme vCenter ayarlamanız gerekir.

1. VMware vSphere Web istemcisi, vCenter sunucusuna gidin.
2. Tıklatın **etiketleri**, tüm geçerli etiketler gözden geçirmek için.
3. Bir VM etiketlemek için seçin **ilişkili nesneleri** > **sanal makineleri**ve ardından kullanmak istediğiniz etikete VM seçin.
4. İçinde **Özet** > **etiketleri**, tıklatın **atamak**. 
5. Tıklatın **yeni etiket**ve bir etiket adı ve açıklama belirtin.
6. Etiket için bir kategori crate için seçin **yeni kategori** aşağı açılan listesinde.
7. Önem düzeyi bir kategori adı ve açıklamasını belirtin. Daha sonra, **Tamam**'a tıklayın.

    ![VM etiketleri](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Etiketleme grupları oluşturmak için kullanın

1. Şirket içi makineler bulma açıklandığı gibi kümesi [VMware değerlendirme Öğreticisi](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms).
2. İçinde **gruplandırma için etiket kategori**, üzerinde değerlendirme grup temel alabilir vCenter etiketi kategorisini seçin. Azure geçirme otomatik olarak seçilen kategori için bir grup oluşturur.

    

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM değerlendirmesi ayarlamak](tutorial-assessment-vmware.md).
