---
title: "Kurtarma Hizmetleri kasası hakkında SSS | Microsoft Belgeleri"
description: "SSS&quot;nin bu sürümü, Azure Backup hizmetinin Genel Önizleme sürümünü destekler. Backup aracısı, yedekleme ve bekletme, kurtarma, güvenlik ve Azure Backup çözümü ile ilgili diğer sık sorulan soruların yanıtları."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "yedekleme çözümü; backup hizmeti"
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: markgal;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: e12d533ac5befe020c0aad4aa64ca9ed50349c3d
ms.openlocfilehash: 271f447a36f42330ed7b8dea78b32c06eebdeda6


---
# <a name="recovery-services-vault---faq"></a>Kurtarma Hizmetleri kasası - SSS
Bu makalede, Kurtarma Hizmetleri kasasına özgü bilgiler sağlanmaktadır ve makale, [Azure Backup hakkında SSS](backup-azure-backup-faq.md) makalesi için tamamlayıcı niteliktedir. Azure Backup ile ilgili SSS, Azure Backup hizmeti hakkında tam kapsamlı sorular ve yanıtlar sağlamaktadır.  

Azure Backup ile ilgili sorularınızı, bu makalenin veya ilgili bir makalenin Disqus bölümünde sorabilirsiniz. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Kurtarma Hizmetleri kasaları Resource Manager tabanlıdır. Backup kasaları (Klasik mod) hala destekleniyor mu? <br/>
Evet, Yedekleme kasaları hâlâ destekleniyor. [Klasik portalda](https://manage.windowsazure.com) Backup kasaları oluşturun. [Azure portalında](https://portal.azure.com) Kurtarma Hizmetleri kasaları oluşturun. Ancak gelecekteki tüm geliştirmeler yalnızca Kurtarma Hizmetleri kasasında kullanılabileceği için, kurtarma hizmetleri kasası oluşturmanızı öneririz.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Bir Backup kasasının Kurtarma Hizmetleri kasasına geçişini sağlayabilir miyim? <br/>
Ne yazık ki hayır, şu an için bir Backup kasasının içeriğinin Kurtarma Hizmetleri kasasına geçişini sağlayamazsınız. Bu işlevi eklemeye yönelik çalışmalarımız devam ediyor ancak işlev Genel Önizleme kapsamında sunulmuyor.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik portalda oluşturulan bir VM’yi (klasik mod VM’leri) veya Azure portalında oluşturulan bir VM’yi (Resource Manager tabanlı), Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Klasik VM’lerimi yedekleme kasasında yedekledim. Şimdi, VM’lerimi klasik moddan Resource Manager moduna geçirmek istiyorum.  Bunları nasıl kurtarma hizmetleri kasasında yedekleyebilirim?
Yedekleme kasasındaki klasik VM yedekleri, VM’leri klasik moddan Resource Manager moduna geçirdiğinizde kurtarma hizmetleri kasasında otomatik olarak yedeklenmez. VM yedeklerinin geçirilmesi için lütfen bu adımları izleyin:

1. Yedekleme kasasıda, **Korunan Öğeler** sekmesine gidin ve VM’yi seçin. [Korumayı Durdur](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines)’a tıklayın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın.
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makine için karşılık gelen depolama ve ağın da Resource Manager moduna geçirildiğinden emin olun.
3. Bir kurtarma hizmetleri kasası oluşturun ve kasa panosunun üstündeki **Backup** işlemini kullanarak, geçirilen sanal makinede yedeklemeyi yapılandırın. [Kurtarma hizmetleri kasasında yedeklemeyi etkinleştirme](backup-azure-vms-first-look-arm.md) hakkında daha fazla bilgi edinin



<!--HONumber=Feb17_HO3-->


