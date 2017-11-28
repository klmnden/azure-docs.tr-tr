---
title: VMware vCenter etiketlemesi ile sanal makineleri Grup | Microsoft Docs
description: "Azure geçiş hizmeti ile bir değerlendirme çalıştırmadan önce bir grup oluşturmayı açıklar."
services: migration-planner
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: db33e31cdef143aa70809442457e447446a1681a
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
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
