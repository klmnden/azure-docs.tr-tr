---
title: "Yönetim Portalı'ndan Azure yığını için yedeklemeyi etkinleştirme | Microsoft Docs"
description: "Böylece bir hata olduğunda Azure yığın geri altyapı geri Hizmet Yönetim Portalı aracılığıyla etkinleştirin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Yedekleme Azure yığını için Yönetim Portalı'ndan etkinleştir

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Altyapı geri Hizmet Yönetim Portalı aracılığıyla Azure yığın yedeklemeleri oluşturabilmesi etkinleştirin. Bunlar kullanabileceğiniz bir hata olduğunda ortamınızı geri yüklemek için ups yedekler.

## <a name="enable-backup"></a>Yedeklemeyi etkinleştirme

1. Azure yığın Yönetim Portalı'ndaki açmak [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Seçin **daha fazla hizmet** > **altyapı yedekleme**. Seçin **yapılandırma** içinde **altyapı yedekleme** dikey.

    ![Azure yığın - yedekleme denetleyicisi ayarları](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Yolunu yazın **yedekleme depolama konumu**. Ayrı bir aygıta bir dosya paylaşımında barındırılan yolu için bir Evrensel Adlandırma Kuralı (UNC) dize kullanmanız gerekir. Bir UNC dize paylaşılan dosyaları veya aygıt konumunu belirtir. Hizmet için bir IP adresi kullanabilirsiniz. Bir olağanüstü durum durumunda yedek verilerin kullanılabilirliğini sağlamak için cihaz ayrı bir konumda olmalıdır.
    > [!Note]  
    > Kurumsal ortamınıza ad çözümlemesi Azure yığın altyapı ağdan ortamınızı destekleyip desteklemediğini IP yerine bir FQDN kullanabilirsiniz.
4. Tür **kullanıcıadı** etki alanı ve kullanıcı adı'nı kullanarak. Örneğin, `Contoso\administrator`.
5. Tür **parola** kullanıcı için.
5. Parolayı yeniden yazın **parolayı onayla**.
6. Önceden paylaşılan bir anahtar sağlamak **şifreleme anahtarı** kutusu. Yedekleme dosyaları bu anahtar kullanılarak şifrelenmiş. Bu anahtarı güvenli bir yerde sakladığınızdan emin olun. Bu anahtar ilk kez ayarlama veya anahtarı gelecekte döndürme sonra bu anahtar bu arabirimden görüntüleyemezsiniz. Önceden paylaşılan anahtar oluşturmak daha fazla yönerge için komut dosyaları izleyin [PowerShell ile Azure yığınının yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Seçin **Tamam** yedek denetleyicisi ayarlarınızı kaydetmek için.

Bir yedekleme yürütmek için Azure yığın araçları yükleyin ve ardından PowerShell cmdlet'ini çalıştırın gereksinim **başlangıç AzSBackup** Azure yığın yönetim düğümü üzerinde. Daha fazla bilgi için bkz: [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Sonraki adımlar

 - Yedekleme, bkz: çalıştırmayı öğrenin [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md ).
- Yedekleme çalıştığını doğrulamak bilgi [Onayla yedekleme Yönetim Portalı'nda tamamlandı](azure-stack-backup-back-up-azure-stack.md ).
