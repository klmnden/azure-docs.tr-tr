---
title: "Yönetim Portalı'ndan Azure yığını için yedeklemeyi etkinleştirme | Microsoft Docs"
description: "Böylece bir hata olduğunda Azure yığın geri altyapı Yedekleme Hizmet Yönetim Portalı aracılığıyla etkinleştirin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 456a0db9771f5963c8d4375d54a22257f6ca1c56
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yedekleme Azure yığını için Yönetim Portalı'ndan etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın yedeklemeleri oluşturabilmesi altyapı Yedekleme Hizmet Yönetim Portalı aracılığıyla etkinleştirin. Bu yedeklemeler, ortamınızın bir arıza olması durumunda geri yüklemek için kullanabilirsiniz.

> [!Note]  
> Konsolu aracılığıyla yedekleme etkinleştirmeden önce yedekleme hizmeti yapılandırmanız gerekir. Yedekleme hizmeti PowerShell kullanarak yapılandırabilirsiniz. Daha fazla bilgi için bkz: [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Yedeklemeyi etkinleştirme

1. Azure yığın Yönetim Portalı'ndaki açmak [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Seçin **daha fazla hizmet** > **altyapı yedekleme**. Seçin **yapılandırma** içinde **altyapı yedekleme** dikey.

    ![Azure yığın - yedekleme denetleyicisi ayarları](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Yolunu yazın **yedekleme depolama konumu**. Ayrı bir cihaz üzerinde barındırılan bir dosya paylaşımına yol için bir Evrensel Adlandırma Kuralı (UNC) dizesi kullanın. Bir UNC dize paylaşılan dosyaları veya aygıt konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Olağanüstü durum sonra yedek verilerin kullanılabilirliğini sağlamak için aygıt ayrı bir konumda olmalıdır.
    > [!Note]  
    > Ortamınızı Kurumsal ortamınıza ad çözümlemesi Azure yığın altyapı ağdan destekliyorsa, IP yerine bir FQDN kullanabilirsiniz.
4. Tür **kullanıcıadı** etki alanı ve kullanıcı adı'nı kullanarak. Örneğin, `Contoso\administrator`.
5. Tür **parola** kullanıcı için.
5. Parolayı yeniden yazın **parolayı onayla**.
6. Önceden paylaşılan bir anahtar sağlamak **şifreleme anahtarı** kutusu. Yedekleme dosyaları bu anahtar kullanılarak şifrelenmiş. Bu anahtarı güvenli bir yerde sakladığınızdan emin olun. Bu anahtar ilk kez ayarlama veya anahtarı gelecekte döndürme sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla yönerge için komut dosyaları izleyin [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Seçin **Tamam** yedek denetleyicisi ayarlarınızı kaydetmek için.

Bir yedekleme yürütmek için Azure yığın araçları yükleyin ve ardından PowerShell cmdlet'ini çalıştırın gereksinim **başlangıç AzSBackup** Azure yığın yönetim düğümü üzerinde. Daha fazla bilgi için bkz: [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Sonraki adımlar

 - Bir yedekleme işinin öğrenin. Bkz: [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).
- Yedekleme çalıştığını doğrulamak öğrenin. Bkz: [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md ).
