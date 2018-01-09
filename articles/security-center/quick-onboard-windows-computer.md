---
title: "Azure Güvenlik Merkezi Hızlı Başlangıç - yerleşik Güvenlik Merkezi, Windows bilgisayarlara | Microsoft Docs"
description: "Bu Hızlı Başlangıç, bir Windows bilgisayarda Microsoft İzleme Aracısı sağlama gösterir."
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
ms.openlocfilehash: 50cbbca9181d67bc41632a4650c76b9636a72356
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Hızlı Başlangıç: Azure Güvenlik Merkezi yerleşik Windows bilgisayarlara
Sonra yerleşik, Azure aboneliklerinize Güvenlik Merkezi örnek şirket içi veya diğer bulut Azure dışında çalışan kaynakları için etkinleştirebileceğiniz Microsoft Monitoring Agent sağlama tarafından.

Bu hızlı başlangıç nasıl bir Windows bilgisayarda Microsoft Monitoring Agent yükleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bu hızlı başlangıç başlatmadan önce Güvenlik Merkezi'nin standart fiyatlandırma katmanı olmalıdır. Bkz: [Onboard Azure aboneliğinize Güvenlik Merkezi standart](security-center-get-started.md) yükseltme yönergeleri için. Güvenlik Merkezi'nin standart ödemeden ilk 60 gün süreyle deneyebilirsiniz.

## <a name="add-new-windows-computer"></a>Yeni Windows bilgisayar ekleme

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - genel bakış** açar.

 ![Güvenlik Merkezi'ne genel bakış][2]

3. Güvenlik Merkezi ana menüsündeki seçin **Onboarding Gelişmiş Güvenlik**.
4. Seçin **Azure olmayan bilgisayarlar eklemek istiyor musunuz**.

   ![Gelişmiş için yerleşik güvenlik][3]

5. Üzerinde **yeni Azure olmayan bilgisayarlar eklemek**, günlük analizi çalışma alanları listesi gösterilir. Liste içerir, varsa, otomatik sağlamayı etkin olduğunda, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanı. Bu çalışma alanı veya kullanmak istediğiniz başka bir çalışma alanını seçin.

    ![Azure olmayan bilgisayar ekleme][4]

  **Doğrudan Aracısı** dikey anahtarları aracı yapılandırmada kullanmak, çalışma alanı kimliği için ve bir Windows aracısını indirme bağlantı açar.

6.  Seçin **Windows Aracısı indirme** bağlantı kurulum dosyasını karşıdan yüklemek için bilgisayarınızın işlemci türü için uygulanabilir.

7.  Sağ tarafındaki **çalışma alanı kimliği**Kopyala simgesini seçin ve Kimliğini Not Defteri'ne yapıştırın.

8.  Sağ tarafındaki **birincil anahtar**Kopyala simgesini seçin ve anahtar Not Defteri'ne yapıştırın.

## <a name="install-the-agent"></a>Aracıyı yükleyin
İndirilen dosyanın hedef bilgisayarda artık yüklemeniz gerekir.

1. Dosyayı Kurulumu çalıştırın ve hedef bilgisayara kopyalayın.
2. Üzerinde **Hoş Geldiniz** sayfasında, **sonraki**.
3. Üzerinde **Lisans Koşulları'nı** sayfasında, lisans okuyun ve ardından **ediyorum**.
4. Üzerinde **hedef klasörü** sayfasında, değiştirmek veya varsayılan yükleme klasörünü ve ardından **sonraki**.
5. Üzerinde **aracı Kur Seçenekleri** sayfasında, aracıyı Azure günlük analizi (OMS) bağlayın ve ardından **sonraki**.
6. Üzerinde **Azure günlük analizi** sayfasında, yapıştırma **çalışma alanı kimliği** ve **çalışma alanı anahtarı (birincil anahtar)** , önceki yordamda Not Defteri'ne kopyalanır.
7. Günlük analizi çalışma alanı Azure kamu bulutta bilgisayarın raporlama, seçin **Azure ABD devlet kurumları** form **Azure bulut** açılır liste.  Bilgisayar günlük analizi hizmeti için bir proxy sunucu üzerinden iletişim kurması gerekirse seçin **Gelişmiş** URL'sini sağlayın ve bağlantı noktası proxy sunucusu sayısı.
8. Seçin **sonraki** gerekli yapılandırma ayarlarını sağlama tamamladıktan sonra.

  ![Aracıyı yükleyin][5]

9. Üzerinde **yüklemeye hazır** sayfasında, seçimlerinizi gözden geçirin ve ardından **yükleme**.
10. Üzerinde **yapılandırması başarıyla tamamlandı** sayfasında, **son**

Tamamlandığında, **Denetim Masası**'nda **Microsoft Monitoring Agent** gösterilir. Vardır, yapılandırmanızı gözden geçirin ve aracı bağlı olduğunu doğrulayın.

Aracısı yükleme ve yapılandırma hakkında daha fazla bilgi için bkz: [bağlanmak Windows bilgisayarları](../log-analytics/log-analytics-agent-windows.md#install-the-agent-using-setup).

Şimdi, Azure Vm'leri ve Azure olmayan bilgisayarlar, tek bir yerde izleyebilirsiniz. Altında **işlem**, tüm VM'ler ve öneriler ile birlikte genel bakış vardır. Her sütunun bir dizi öneriyi temsil eder. Renk Bu öneri için VM'nin ya da bilgisayarın geçerli güvenlik durumunu temsil eder. Güvenlik Merkezi, ayrıca güvenlik uyarıları bu bilgisayarlar için tüm algılamaların ortaya çıkarır.

  ![Dikey işlem][6]

Simgeler üzerinde gösterilen iki tür vardır **işlem** dikey penceresinde:

![simge1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Azure olmayan bilgisayar

![simge2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, bir Windows bilgisayardan aracısını kaldırabilirsiniz.

Aracıyı kaldırmak için:

1. **Denetim Masası**'nı açın.
2. **Programlar ve Özellikler**'i açın.
3. **Programlar ve Özellikler**'de **Microsoft Monitoring Agent**'ı seçin ve **Kaldır**'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç bir Windows bilgisayarda Microsoft İzleme Aracısı sağlandı. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirmek için öğretici devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Tanımlayın ve güvenlik ilkeleri değerlendirin](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
