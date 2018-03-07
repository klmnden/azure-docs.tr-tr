---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç - Linux bilgisayarlarınızı Güvenlik Merkezi’ne ekleme | Microsoft Docs"
description: "Bu hızlı başlangıç, Linux bilgisayarlarınızı Güvenlik Merkezi’ne nasıl ekleyebileceğinizi gösterir."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: terrylan
ms.openlocfilehash: 05e4bed0f9b4dfb6d1879408085447ef53db8655
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Hızlı başlangıç: Linux bilgisayarlarını Azure Güvenlik Merkezi’ne ekleme
Azure aboneliklerinizi ekledikten sonra Linux Aracısı’nı sağlayarak Azure dışında (örneğin, şirket içinde veya diğer bulutlarda) çalışan Linux kaynakları için Güvenlik Merkezi’ni etkinleştirebilirsiniz.

Bu hızlı başlangıçta bir Linux bilgisayara Linux Aracısı'nın nasıl yükleneceği gösterir.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bu hızlı başlangıçtaki adımlara geçmeden önce Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Yükseltme yönergeleri için bkz. [Azure Aboneliğinizi Güvenlik Merkezi Standart’a ekleme](security-center-get-started.md). Güvenlik Merkezi’nin Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz.

## <a name="add-new-linux-computer"></a>Yeni Linux bilgisayarı ekleme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

 ![Güvenlik Merkezi’ne genel bakış][2]

3. Güvenlik Merkezi ana menüsünden **Onboarding to advanced security** (Gelişmiş güvenliğe ekleme) seçeneğini belirleyin.
4. **Do you want to add non-Azure computers** (Azure dışı bilgisayarlar eklemek istiyor musunuz?) seçeneğini belirleyin.
   ![Gelişmiş güvenliğe ekleme][3]

5. **Add new non-Azure computers** (Azure dışı yeni bilgisayarlar ekle) sayfasında Log Analytics çalışma alanlarınızın listesi gösterilir. Bu liste, uygun durumlarda, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulmuş varsayılan çalışma alanını içerir. Bu çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.

    ![Azure dışı bilgisayar ekleme][4]

6.  **Direct Agent** (Doğrudan Aracı) sayfasının **DOWNLOAD AND ONBOARD AGENT FOR LINUX** (Linux için aracıyı indir ve ekle) bölümünden **copy** (Kopyala) düğmesini seçerek *wget* komutunu kopyalayın.

7.  Not Defteri'ni açın ve bu komutu yapıştırın. Bu dosyayı Linux bilgisayarınızdan erişilebilen bir konuma kaydedin.

## <a name="install-the-agent"></a>Aracıyı yükleme

1.  Linux bilgisayarınızda daha önce kaydedilen dosyayı açın. İçeriğin tamamını seçin, kopyalayın, bir terminal konsolu açın ve komutu yapıştırın.
2.  Yükleme tamamlandığında *pgrep* komutunu çalıştırarak *omsagent*’ın yüklü olduğunu doğrulayın. Komut tarafından aşağıda gösterildiği gibi *omsagent* PID’si (İşlem Kimliği) döndürülür:

  ![Aracıyı yükleme][5]

Linux için Güvenlik Merkezi Aracısı günlükleri şu yolda bulunabilir: */var/opt/microsoft/omsagent/<workspace id>/log/*

  ![Aracı günlükleri][6]

30 dakikayı bulabilecek bir süre geçtikten sonra Linux bilgisayarı Güvenlik Merkezi’nde görünür.

Artık Azure VM’lerinizi ve Azure dışı bilgisayarlarınızı tek bir yerde izleyebilirsiniz. **Compute** (İşlem) bölümünde tüm VM’lere ve bilgisayarlara genel bakışın yanı sıra öneriler sunulur. Her sütun, bir dizi öneriyi temsil eder. Renk, ilgili öneri için VM'nin ya da bilgisayarın geçerli güvenlik durumunu temsil eder. Güvenlik Merkezi ayrıca bu bilgisayarlara yönelik tüm algılamaları Güvenlik uyarıları bölümünde gösterir.

  ![Compute (İşlem) dikey penceresi][7] **Compute** (İşlem) dikey penceresinde temsil edilen iki tür simge vardır:

  ![simge1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Azure dışı bilgisayar

  ![simge2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında, aracıyı Linux bilgisayardan kaldırabilirsiniz.

Aracıyı kaldırmak için:

1. Linux aracısı [evrensel betiğini](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) bilgisayara indirin.
2. Bilgisayarda paket .sh dosyasını aracıyı ve yapılandırmasını tamamen kaldıran *--purge* bağımsız değişkeni ile çalıştırın.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir Linux bilgisayarında aracıyı sağladınız. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirme ile ilgili öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png
