---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç - yerleşik Güvenlik Merkezi, Linux bilgisayarlara | Microsoft Docs"
description: "Bu hızlı başlangıç şunların nasıl yapıldığını gösterir onboarding için Güvenlik Merkezi, Linux bilgisayarlara."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: 365afd2199b1b8b2e70d882f6a729384edbdffbc
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Hızlı Başlangıç: Azure Güvenlik Merkezi yerleşik Linux bilgisayarlara
Sonra yerleşik Azure aboneliklerinizi, Güvenlik Merkezi Linux Aracısı'nı sağlama tarafından örnek şirket içi ya da diğer bulut Azure dışında çalışan Linux kaynaklar için etkinleştirebilirsiniz.

Bu hızlı başlangıç Linux Aracısı'nı Linux bilgisayara yüklemek nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bu hızlı başlangıç başlatmadan önce Güvenlik Merkezi'nin standart fiyatlandırma katmanı olmalıdır. Bkz: [Onboard Azure aboneliğinize Güvenlik Merkezi standart](security-center-get-started.md) yükseltme yönergeleri için. Güvenlik Merkezi'nin standart ödemeden ilk 60 gün süreyle deneyebilirsiniz.

## <a name="add-new-linux-computer"></a>Yeni Linux bilgisayar ekleme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - genel bakış** açar.

 ![Güvenlik Merkezi'ne genel bakış][2]

3. Güvenlik Merkezi ana menüsündeki seçin **Onboarding Gelişmiş Güvenlik**.
4. Seçin **Azure olmayan bilgisayarlar eklemek istiyor musunuz**.
   ![Gelişmiş için yerleşik güvenlik][3]

5. Üzerinde **yeni Azure olmayan bilgisayarlar eklemek**, günlük analizi çalışma alanları listesi gösterilir. Liste içerir, varsa, otomatik sağlamayı etkin olduğunda, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanı. Bu çalışma alanı veya kullanmak istediğiniz başka bir çalışma alanını seçin.

    ![Azure olmayan bilgisayar ekleme][4]

6.  Üzerinde **doğrudan Aracısı** sayfasında **İNDİRİN ve YERLEŞİK ARACISI için LINUX**seçin **kopya** kopyalamak için düğmesi *wget* komutu.

7.  Not Defteri'ni açın ve bu komutu yapıştırın. Bu dosya, Linux bilgisayardan erişilebilir bir konuma kaydedin.

## <a name="install-the-agent"></a>Aracıyı yükleyin

1.  Linux bilgisayarınızda daha önce kaydettiğiniz dosyayı açın. Tüm içerik, kopya'yı seçin, bir terminal konsolunu açın ve Yapıştır komutu.
2.  Yükleme tamamlandıktan sonra doğrulayabilir *omsagent* çalıştırarak yüklü *pgrep* komutu. Komut döndürür *omsagent* PID aşağıda gösterildiği gibi (işlem kimliği):

  ![Aracıyı yükleyin][5]

Linux için Güvenlik Merkezi aracı günlüklerinde bulunabilir: */var/opt/microsoft/omsagent/<workspace id>/log/*

  ![Aracı için günlükler][6]

Bir süre sonra onu yeni Linux bilgisayar Güvenlik Merkezi'nde görünür 30 dakikaya kadar sürebilir.

Şimdi, Azure Vm'leri ve Azure olmayan bilgisayarlar, tek bir yerde izleyebilirsiniz. Altında **işlem**, tüm VM'ler ve öneriler ile birlikte genel bakış vardır. Her sütunun bir dizi öneriyi temsil eder. Renk Bu öneri için VM'nin ya da bilgisayarın geçerli güvenlik durumunu temsil eder. Güvenlik Merkezi, ayrıca güvenlik uyarıları bu bilgisayarlar için tüm algılamaların ortaya çıkarır.

  ![Dikey işlem][7] üzerinde gösterilen simgelerin iki tür vardır **işlem** dikey penceresinde:

  ![simge1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Azure olmayan bilgisayar

  ![simge2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, Linux bilgisayardan aracısını kaldırabilirsiniz.

Aracıyı kaldırmak için:

1. Linux Aracısı'nı indirme [Evrensel betik](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) bilgisayara.
2. Paket .sh dosyasını çalıştırmak *--Temizleme* bağımsız değişkeni bilgisayardaki aracı ve yapılandırmasını tamamen kaldırır.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç bölümünde Linux bilgisayardaki aracı sağlandı. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirmek için öğretici devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Tanımlayın ve güvenlik ilkeleri değerlendirin](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png
