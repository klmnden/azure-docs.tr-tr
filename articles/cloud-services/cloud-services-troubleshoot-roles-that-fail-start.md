---
title: Başlamak için rolleri sorunlarını giderme | Microsoft Docs
description: Neden bir bulut hizmeti rolü başlatılamayabilir bazı yaygın nedenler aşağıda verilmiştir. Bu sorunların çözümlerini de sağlanır.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: d2daae2a3317d3b48748262d87ab8d7f7e13f2b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60653390"
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Başlatmak için Cloud Service rolleri için sorun giderme
İşte bazı yaygın sorunlar ve çözümleri başlatmak için rolleri Azure Cloud Services'a ilgili.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Eksik DLL'leri veya bağımlılıkları
Yanıt vermeyen rolleri ve arasında geçiş yapma rolleri **başlatılıyor**, **meşgul**, ve **durdurma** DLL'lere veya derlemeleri eksik durumları oluşabilir.

Belirtiler DLL'lere veya derlemeleri eksik olabilir:

* Rol örneğiniz dolaşma **başlatılıyor**, **meşgul**, ve **durdurma** durumları.
* Rol Örneğiniz için geçmiştir **hazır** ancak web uygulamanıza gidin, sayfa görünmez.

Bu sorunları araştırma için önerilen çeşitli yöntemler vardır.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Bir web rolü eksik DLL sorunları tanılayın
Dağıtılan bir Web sitesine gidin, bir web rolü ve tarayıcı aşağıdakine benzer bir sunucu hatası görüntüler, bir DLL eksik olduğunu gösteriyor olabilir.

!['/' Uygulamasında sunucu hatası.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Özel hatalar devre dışı bırakarak sorunlarını tanılayın
Daha ayrıntılı hata bilgileri özel hata modu kapalı olarak ayarlamak web rolü için web.config yapılandırma ve hizmet yükleyecek görüntüleyebilir.

Uzak Masaüstü'nü kullanmadan daha ayrıntılı hataları görüntülemek için:

1. Microsoft Visual Studio içinde çözümü açın.
2. İçinde **Çözüm Gezgini**, web.config dosyasını bulun ve açın.
3. Web.config dosyasında system.web bölümünü bulun ve aşağıdaki satırı ekleyin:

    ```xml
    <customErrors mode="Off" />
    ```
4. Dosyayı kaydedin.
5. Yeniden paketleyin ve hizmeti yeniden dağıtın.

Hizmet yeniden sonra eksik derleme veya DLL adı ile bir hata iletisi görürsünüz.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Uzaktan hata görüntüleyerek sorunlarını tanılayın
Rol erişim ve daha ayrıntılı hata bilgileri uzaktan görüntülemek için Uzak Masaüstü'nü kullanabilirsiniz. Uzak Masaüstü'nü kullanarak hataları görüntülemek için aşağıdaki adımları kullanın:

1. 1.3 veya sonraki bir sürümü Azure SDK'ın yüklü olduğundan emin olun.
2. Visual Studio kullanarak çözüm dağıtımı sırasında Uzak Masaüstü'nü etkinleştirin. Daha fazla bilgi için [Visual Studio kullanarak Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme](cloud-services-role-enable-remote-desktop-visual-studio.md).
3. Microsoft Azure Portal'da örneğinin durumunu gösterir. bir kez **hazır**, uzaktan örneğe bağlanın. Cloud Services ile Uzak Masaüstü kullanma hakkında daha fazla bilgi için bkz. [rol örnekleri uzaktan](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances).
5. Sanal makineye uzak masaüstü yapılandırması sırasında belirtilen kimlik bilgilerini kullanarak oturum açın.
6. Komut penceresi açın.
7. `IPconfig` yazın.
8. IPv4 adresi değerini not edin.
9. Internet Explorer'ı açın.
10. Adres ve web uygulaması adını yazın. Örneğin, `http://<IPV4 Address>/default.aspx`.

Bir Web sitesine yönlendirir, artık daha açık hata iletileri döndürülür:

* '/' Uygulamasında sunucu hatası.
* Açıklama: Geçerli web isteği yürütülmesi sırasında işlenmeyen bir özel durum oluştu. Yığın izleme hatası ve kodda kaynaklandığı daha fazla bilgi için lütfen inceleyin.
* Özel durum ayrıntıları: System.IO.FIleNotFoundException: Dosya veya derleme yüklenemedi ' Microsoft.WindowsAzure.StorageClient, sürümü 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 =' veya bağımlılıklarından biri. Sistem belirtilen dosyayı bulamıyor.

Örneğin:

!['/' Uygulamasında açık sunucu hatası](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>İşlem öykünücüsü kullanarak sorunları tanılayın
Tanılama ve eksik bağımlılıklar ve web.config hataları, sorunları gidermek için Microsoft Azure işlem öykünücüsü'nü kullanabilirsiniz.

Tanılama bu yöntemi kullanarak en iyi sonuçlar için bir bilgisayarı veya Windows temiz bir yüklemesi olan sanal makine kullanmanız gerekir. En iyi Azure ortamının benzetimini yapmak için Windows Server 2008 R2 x64 kullanın.

1. Tek başına sürümünü [Azure SDK'sı](https://azure.microsoft.com/downloads/).
2. Geliştirme makinesinde bulut hizmeti projesi oluşturun.
3. Windows Gezgini'nde, bulut hizmeti projesi bin\debug klasörüne gidin.
4. .csx klasörü ve .cscfg dosyasında hata ayıklamak için kullanmakta olduğunuz bilgisayara kopyalayın.
5. Temiz bir makinede bir Azure SDK komut istemi penceresi açıp `csrun.exe /devstore:start`.
6. Komut isteminde `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. Rol başladığında, Internet Explorer'da ayrıntılı hata bilgileri görürsünüz. Daha fazla sorunu tanılamak için standart Windows sorun giderme araçları da kullanabilirsiniz.

## <a name="diagnose-issues-by-using-intellitrace"></a>IntelliTrace kullanarak sorunları tanılayın
Çalışan ve .NET Framework 4 kullanan web rolleri için kullanabileceğiniz [IntelliTrace](/visualstudio/debugger/intellitrace), Microsoft Visual Studio Enterprise'da kullanılabilir olduğu.

Etkin IntelliTrace ile hizmeti dağıtmak için aşağıdaki adımları izleyin:

1. Azure SDK 1.3 veya üzeri yüklü olduğunu onaylayın.
2. Visual Studio kullanarak çözümü dağıtın. Dağıtım sırasında kontrol **.NET 4 rolleri için IntelliTrace'i etkinleştirin** onay kutusu.
3. Örnek başladıktan sonra açın **Sunucu Gezgini**.
4. Genişletin **Azure\\Cloud Services** düğüm ve dağıtımını bulun.
5. Dağıtım rol örneklerini görene kadar genişletin. Örneklerinden birine sağ tıklayın.
6. Seçin **görünümü protokoly IntelliTrace**. **IntelliTrace özeti** açılır.
7. Özet özel durumlar bölümünü bulun. Özel durumlar varsa, bölüm etiketli **özel durum verileri**.
8. Genişletin **özel durum verileri** ve Ara **System.IO.FileNotFoundException** aşağıdakine benzer hatalar:

![Özel durum verileri, eksik dosya veya derleme](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Eksik DLL'ler ve derlemeleri adresi
Eksik DLL ve derleme hatalarını gidermek için şu adımları izleyin:

1. Visual Studio içinde çözümü açın.
2. İçinde **Çözüm Gezgini**açın **başvuruları** klasör.
3. Hata tanımlanan derleme tıklayın.
4. İçinde **özellikleri** bölmesinde bulun **Yereli Kopyala özelliğini** ve değerine **True**.
5. Bulut hizmetine yeniden dağıtın.

Tüm hatalar düzeltildikten doğruladıktan sonra hizmeti denetlemeden dağıtabilirsiniz **.NET 4 rolleri için IntelliTrace'i etkinleştirin** onay kutusu.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazlasını görüntüle [sorun giderme makaleleri](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için bkz: [Kevin Williamson'ın blog dizisini](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
