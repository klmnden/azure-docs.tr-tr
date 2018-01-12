---
title: "Kaynak gruplarını otomatik kaldırma | Microsoft Belgeleri"
description: "Bir Azure Otomasyonu senaryosunun, aboneliğinizdeki tüm kaynak gruplarını kaldırmaya yönelik runbook’lar içeren PowerShell İş Akışı sürümü."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: e1734bdd22ecfc4e54074f02582f5a8eca7d4f59
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Otomasyonu senaryosu - kaynak gruplarının kaldırılmasını otomatik hale getirme
Birçok müşteri birden fazla kaynak grubu oluşturur. Bazıları üretim uygulamalarını yönetmek için, bazıları ise geliştirme, test ve hazırlık ortamları olarak kullanılabilir. Bu kaynakların dağıtımının otomatik hale getirilmesi bir özelliktir, ancak bir kaynak grubunun tek bir düğme tıklanarak kullanımdan kaldırılması da başka bir özelliktir. Bu ortak yönetim görevini Azure Otomasyonu'nu kullanarak basit hale getirebilirsiniz. Bu özellik, örneğin MSDN veya Microsoft İş Ortağı Ağı Bulut Temel Bileşenleri programı gibi bir üye teklifi üzerinden harcama limitine sahip bir Azure aboneliği ile çalışıyorsanız yararlı olur.

Bu senaryo bir PowerShell runbook'u temel alır ve aboneliğinizden belirttiğiniz bir veya daha fazla kaynak grubunu kaldırmak için tasarlanmıştır. Runbook’un varsayılan ayarı, devam etmeden önce test etmektir. Bu ayar, kaynak grubunu bu yordamı tamamlamak için hazır olmadan yanlışlıkla silmenizi önler.   

## <a name="getting-the-scenario"></a>Senaryoyu alma
Bu senaryo, [PowerShell Galerisi](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript)’nden indirebileceğiniz PowerShell runbook’u içerir. Doğrudan Azure portal’daki [Runbook Galerisi](automation-runbook-gallery.md)’nden de indirebilirsiniz.<br><br>

| Runbook | Açıklama |
| --- | --- |
| Remove-ResourceGroup |Bir veya daha fazla Azure kaynak grubunu ve ilişkili kaynakları abonelikten kaldırır. |

<br>
Bu runbook için aşağıdaki giriş parametreleri tanımlanmıştır:

| Parametre | Açıklama |
| --- | --- |
| NameFilter (Gerekli) |Silmeyi amaçladığınız kaynak gruplarını sınırlandırmak için bir ad filtresi belirtir. Virgülle ayrılmış bir liste kullanarak birden çok değer geçirebilirsiniz.<br>Filtre büyük küçük harfe duyarlı değildir ve dizeyi içeren herhangi bir kaynak grubunu eşleştirir. |
| PreviewMode (İsteğe bağlı) |Hangi kaynak gruplarının silineceğini görmek için runbook’u yürütür, ancak herhangi bir işlem yapmaz.<br>Varsayılan değer, runbook’a geçirilen bir veya daha fazla kaynak grubunun yanlışlıkla silinmesini önlemeye yardımcı olmak üzere **true** olarak ayarlanmıştır. |

## <a name="install-and-configure-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma
### <a name="prerequisites"></a>Önkoşullar
Bu runbook [Azure Farklı Çalıştır hesabı](automation-sec-configure-azure-runas-account.md) kullanarak kimlik doğrulaması yapar.    

### <a name="install-and-publish-the-runbooks"></a>Runbook yükleme ve yayımlama
Runbook’u indirdikten sonra [Runbook’ları içeri aktarma yordamları](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation) içindeki yordamı kullanarak içeri aktarabilirsiniz. Runbook’u Otomasyon hesabınıza başarıyla içeri aktarıldıktan sonra yayımlayın.

## <a name="using-the-runbook"></a>Runbook’u kullanma
Aşağıdaki adımlar bu runbook’un yürütülmesinde size yol gösterir ve nasıl çalıştığını anlamanıza yardımcı olur. Bu örnekte yalnızca runbook’u test edeceksiniz, kaynak grubunu gerçekten silmeyeceksiniz.  

1. Azure portal’da Otomasyon hesabınızı açın ve **Runbook'lar** seçeneğine tıklayın.
2. **Remove-ResourceGroup** runbook’unu seçin ve **Başlat**’a tıklayın.
3. Runbook’u başlattığınızda **Runbook’u Başlat** dikey penceresi açılır ve parametreleri yapılandırabilirsiniz. Aboneliğinizde test için kullanabileceğiniz ve yanlışlıkla silinirse zararı olmayacak kaynak gruplarının adlarını girin.<br> ![Remove-ResouceGroup parametreleri](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Seçili kaynak gruplarının yanlışlıkla silinmesini önlemek için **Previewmode**’un **true** olarak ayarlandığından emin olun.  Bu runbook’un, kendisini çalıştıran Otomasyon hesabını içeren kaynak grubunu kaldırmayacağını **unutmayın**.  
   >
   >
4. Tüm parametre değerlerini yapılandırdıktan sonra **Tamam**’a tıklarsanız runbook yürütme kuyruğuna alınır.  

**Remove-ResourceGroup** runbook işinin ayrıntılarını Azure portal’da görüntülemek için runbook’ta **İşler**’i seçin. İş özetinde, giriş parametreleri ve çıkış akışına ek olarak işe ilişkin genel bilgiler ve gerçekleşen özel durumlar gösterilir.<br> ![Remove-ResourceGroup runbook iş durumu](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

**İş Özeti** bölümünde çıkış, uyarı ve hata akışlarından iletiler bulunur. Runbook yürütmeyle ilgili ayrıntılı sonuçları görüntülemek için **Çıkış**’ı seçin.<br> ![Remove-ResourceGroup runbook çıkış sonuçları](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kendi runbook’unuzu oluşturmaya başlamak için bkz. [Azure Otomasyonu’nda runbook oluşturma veya içeri aktarma](automation-creating-importing-runbook.md).
* PowerShell İş Akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell İş Akışı runbook uygulamam](automation-first-runbook-textual.md).
