---
title: "Başlatılamaz rolleri sorunlarını giderme | Microsoft Docs"
description: "Neden bir bulut hizmeti rolü başlatılamayabilir bazı yaygın nedenler şunlardır. Bu sorunların çözümlerini de sağlanır."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: ec33ba08c6284e90edc1870eef4bf3059b917efb
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Başlatılamaz bulut hizmeti rolleriyle ilgili sorunları giderme
Çözümleri Azure bulut hizmetlerine başlatılamaz rolleri ilgili ve bazı yaygın sorunlar şunlardır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Eksik DLL'leri veya bağımlılıkları
Yanıt vermeyen rolleri ve arasında geçiş yapma rolleri **başlatılıyor**, **meşgul**, ve **durdurma** durumları DLL'leri veya derlemeleri eksik kaynaklanabilir.

DLL'leri veya derlemeleri eksik Belirtiler şunlar olabilir:

* Rol örneği dolaşma **başlatılıyor**, **meşgul**, ve **durdurma** durumları.
* Rol örneği için geçmiştir **hazır** ancak web uygulamanıza giderseniz, sayfa görünmez.

Bu sorunları incelemeye için önerilen çeşitli yöntemler vardır.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Web rolü eksik DLL sorunları tanılama
Dağıtılan bir Web sitesine gidin, bir web rolü ve tarayıcı aşağıdakine benzer bir sunucu hatası görüntüler, DLL eksik olduğunu gösteriyor olabilir.

!['/' Uygulamasında sunucu hatası.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Özel hatalar devre dışı bırakarak sorunlarını tanılamak
Daha ayrıntılı hata bilgileri özel hata modu kapalı olarak ayarlamak web rolü için web.config yapılandırma ve hizmet dağıtarak görüntülenebilir.

Uzak Masaüstü'nü kullanmadan daha ayrıntılı hataları görüntülemek için:

1. Microsoft Visual Studio çözümü açın.
2. İçinde **Çözüm Gezgini**, web.config dosyasını bulun ve açın.
3. Web.config dosyasında system.web bölümünü bulun ve aşağıdaki satırı ekleyin:

    ```xml
    <customErrors mode="Off" />
    ```
4. Dosyayı kaydedin.
5. Yeniden paketleyin ve hizmeti yeniden dağıtın.

Hizmet imzalanmasını sonra eksik derleme veya DLL adı ile bir hata iletisi görürsünüz.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Uzaktan hata görüntüleyerek sorunlarını tanılamak
Rol erişmek ve daha ayrıntılı hata bilgileri uzaktan görüntülemek için Uzak Masaüstü'nü kullanabilirsiniz. Uzak Masaüstü'nü kullanarak hataları görüntülemek için aşağıdaki adımları kullanın:

1. Azure SDK 1.3 veya üstü yüklü olduğundan emin olun.
2. Visual Studio kullanarak çözüm dağıtımı sırasında "Yapılandırma Uzak Masaüstü bağlantıları için..." seçin. Uzak Masaüstü bağlantısı yapılandırma hakkında daha fazla bilgi için bkz: [Azure rolleri ile Uzak Masaüstü kullanarak](../vs-azure-tools-remote-desktop-roles.md).
3. Örnek bir durumu gösterir sonra Microsoft Azure Klasik portalında, **hazır**, rol örnekleri birine tıklayın.
4. Tıklatın **Bağlan** simgesine **uzaktan erişim** Şerit alanı.
5. Sanal makineye uzak masaüstü yapılandırması sırasında belirtilen kimlik bilgilerini kullanarak oturum açın.
6. Bir komut penceresi açın.
7. `IPconfig` yazın.
8. IPv4 adresi değerini not edin.
9. Internet Explorer'ı açın.
10. Adres ve web uygulamasının adını yazın. Örneğin, `http://<IPV4 Address>/default.aspx`.

Web sitesine giderek daha açık hata iletileri artık döndürür:

* '/' Uygulamasında sunucu hatası.
* Açıklama: Geçerli web isteğinin yürütülmesi sırasında işlenmemiş bir özel durum oluştu. Lütfen hata hakkındaki ve kodda geldiği daha fazla bilgi için yığın izlemesi gözden geçirin.
* Özel durum ayrıntıları: System.IO.FIleNotFoundException: dosya veya derleme yüklenemedi ' Microsoft.WindowsAzure.StorageClient, sürüm 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 =' ya da bağımlılıklarından biri. Sistem belirtilen dosyayı bulamıyor.

Örneğin:

!['/' Uygulamasında açık sunucu hatası](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>İşlem öykünücüsü kullanarak sorunları tanılamak
Microsoft Azure işlem öykünücüsü tanılamak ve bağımlılıklar ve web.config hataları eksik sorunlarını gidermek için kullanabilirsiniz.

Tanılama bu yöntemi kullanmanın en iyi sonuçlar için bir bilgisayarı veya Windows temiz bir yüklemesini sanal makine kullanmanız gerekir. En iyi Azure ortamının benzetimini yapmak için Windows Server 2008 R2 x64 kullanın.

1. Tek başına bir sürümünü yüklemek [Azure SDK'sı](https://azure.microsoft.com/downloads/).
2. Geliştirme makinenizde bulut hizmeti projesi oluşturun.
3. Windows Gezgini'nde klasörüne bulut hizmeti projesine gidin.
4. .Csx klasörü ve .cscfg dosyasını sorunları hata ayıklamak için kullanmakta olduğunuz bilgisayara kopyalayın.
5. Temiz makinede bir Azure SDK komut istemi penceresi açıp `csrun.exe /devstore:start`.
6. Komut isteminde `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. Rol başladığında, Internet Explorer'da ayrıntılı hata bilgileri görürsünüz. Daha fazla sorunu tanılamak için standart Windows sorun giderme araçları da kullanabilirsiniz.

## <a name="diagnose-issues-by-using-intellitrace"></a>IntelliTrace kullanarak sorunları tanılamak
Çalışan ve .NET Framework 4 kullanan web rolleri için kullanabileceğiniz [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), Microsoft Visual Studio Enterprise'da kullanılabilir olduğu.

Hizmet etkin IntelliTrace ile dağıtmak için aşağıdaki adımları izleyin:

1. Azure SDK 1.3 veya üstü yüklü olduğunu onaylayın.
2. Çözüm, Visual Studio kullanarak dağıtın. Dağıtım sırasında kontrol **.NET 4 rolleri için IntelliTrace'i etkinleştirin** onay kutusu.
3. Bir örneğini başlatır, açın **Sunucu Gezgini**.
4. Genişletme **Azure\\bulut Hizmetleri** düğümü ve dağıtımını bulun.
5. Dağıtım rol örnekleri görene kadar genişletin. Örneklerden birini sağ tıklayın.
6. Seçin **görünüm IntelliTrace günlüklerini**. **IntelliTrace Özet** açılır.
7. Özet özel durumlara bölümünü bulun. Özel durumlar varsa, bölüm etiketli **özel durum verileri**.
8. Genişletme **özel durum verileri** ve Ara **System.IO.FileNotFoundException** hatalarla aşağıdakine benzer:

![Özel durum verileri, eksik dosya veya derleme](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adres eksik DLL'ler ve derlemeler
Eksik DLL ve derleme hataları gidermek için şu adımları izleyin:

1. Çözümü Visual Studio'da açın.
2. İçinde **Çözüm Gezgini**, açık **başvuruları** klasör.
3. Hata tanımlanan derleme'yi tıklayın.
4. İçinde **özellikleri** bölmesinde bulun **kopya yerel özellik** ve değerine **doğru**.
5. Bulut hizmeti yeniden dağıtın.

Tüm hataları düzelttikten olduğunu doğruladıktan sonra hizmet denetlemeden dağıtabileceğiniz **.NET 4 rolleri için IntelliTrace'i etkinleştirin** onay kutusu.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi görüntülemek [sorun giderme makalelerini](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için bkz: [Kevin Williamson'ın blog dizisini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
