---
title: U-SQL çalıştırın ve Azure Data Lake araçları Visual Studio Code için yerel olarak hata ayıklama
description: Çalıştırma ve U-SQL işleri yerel olarak hata ayıklamak için Visual Studio Code için Azure Data Lake Araçları'nı kullanmayı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 07/14/2017
ms.openlocfilehash: 765bcaab0f91e097be827bfa6e8f505ef5330d57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814199"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>U-SQL'i çalıştırma ve yerel olarak Visual Studio Code'da Hata Ayıkla
Bu makalede bir yerel geliştirme makinesindeki kodlama erken aşama hızlandırmak için veya Visual Studio Code yerel kodda hata ayıklamak için U-SQL işleri çalıştırmayı öğrenin. Visual Studio Code için Azure Data Lake aracı hakkında yönergeler için bkz: [kullanımı Azure Data Lake araçları Visual Studio Code için](data-lake-analytics-data-lake-tools-for-vscode.md).

Azure Data Lake araçları yalnızca Windows yüklemelerinde Visual Studio için U-SQL yerel çalıştırma ve U-SQL yerel olarak hata ayıklama eylemini desteklemektedir. MacOS ve Linux tabanlı işletim sistemleri yüklemelerinde, bu özelliği desteklemez.

## <a name="set-up-the-u-sql-local-run-environment"></a>U-SQL yerel çalıştırma ortamını ayarlama

1. Komut paletini açın ve ardından girmek için Ctrl + Shift + P seçin **ADL: Yerel çalıştırma paketini indirme** paketler indirilemedi.  

   ![ADL LocalRun bağımlılık paketlerini indirin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Gösterilen yolundan bağımlılık paketlerini bulun **çıkış** bölmesinde ve ardından BuildTools ve Win10SDK 10240'ı yükleyin. Bir örnek yol şu şekildedir:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Bağımlılık paketlerini bulun](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 için yükleme **BuildTools**, visualcppbuildtools_full.exe LocalRunDependency klasörüne tıklayın ve ardından sihirbazdaki yönergeleri izleyin.   

    ![BuildTools yükleyin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 yüklenecek **Win10SDK 10240**, sdksetup.exe LocalRunDependency/Win10SDK_10.0.10240_2 klasörüne tıklayın ve ardından sihirbazdaki yönergeleri izleyin.  

    ![Win10SDK yükleme 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Ortam değişkeni ayarlayın. Ayarlama **SCOPE_CPP_SDK** ortam değişkeni:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Yerel çalıştırma hizmeti başlatın ve yerel bir hesap için U-SQL işi gönderme 
İlk kez kullanıcı için **ADL: Yerel çalıştırma paketini indirme** tamamlamadıysanız, yerel çalışma paketleri indirmek için [U-SQL yerel çalıştırma ortamını ayarlama](#set-up-the-u-sql-local-run-environment).

1. Komut paletini açın ve ardından girmek için Ctrl + Shift + P seçin **ADL: Yerel çalıştırma hizmetini**.   
2. Seçin **kabul** ilk kez Microsoft Yazılımı Lisans koşulları kabul etmek için. 

   ![Microsoft Yazılımı Lisans Koşulları'nı kabul edin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Cmd konsolu açılır. İlk kullanıcılar için girmeniz gerekir. **3**ve veri giriş ve çıkış yerel klasör yolunu bulun. Diğer seçenekler için varsayılan değerleri kullanabilirsiniz. 

   ![Yerel çalıştırma cmd Visual Studio Code için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Komut paletini açın, girmek için Ctrl + Shift + P seçin **ADL: İşi Gönder**ve ardından **yerel** yerel hesabınıza işi göndermek için.

   ![Visual Studio Code için Data Lake araçları yerel seçin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. İşi gönderdikten sonra gönderme ayrıntılarını görüntüleyebilirsiniz. Gönderme ayrıntılarını görüntülemek için seçin **jobUrl** içinde **çıkış** penceresi. Cmd konsolundan iş gönderme durumunu da görüntüleyebilirsiniz. Girin **7** cmd konsolunda daha fazla iş ayrıntılarını bilmek istiyorsanız.

   ![Data Lake araçları Visual Studio Code yerel çıkış çalıştırmak için](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake araçları, Visual Studio Code yerel çalıştırma cmd durumu](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>U-SQL işi için yerel bir hata ayıklama Başlat  
İlk kez kullanıcı için:

1. Kullanım **ADL: Yerel çalıştırma paketini indirme** tamamlamadıysanız, yerel çalışma paketleri indirmek için [U-SQL yerel çalıştırma ortamını ayarlama](#set-up-the-u-sql-local-run-environment).
2. .NET Core SDK 2.0 önerildiği gibi bir ileti kutusu yüklü değilse yükleyin.
 
  ![Dotnet anımsatıcı yükler](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Yükleme C# yüklü değilse ileti kutusunda önerilen Visual Studio Code için. Tıklayın **yükleme** devam etmek ve ardından VSCode yeniden başlatın.

    ![C# ' ı yüklemek için anımsatıcı](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Yerel hata ayıklama gerçekleştirmek için aşağıdaki adımları izleyin:
  
1. Komut paletini açın ve ardından girmek için Ctrl + Shift + P seçin **ADL: Yerel çalıştırma hizmetini**. Cmd konsolu açılır. Emin olun **DataRoot** ayarlanır.
2. C# arka plan kod içinde bir kesme noktası ayarlayın.
3. Kod Düzenleyicisi, sütuna sağ tıklayıp dön **ADL: Yerel hata ayıklama**.
    
   ![Yerel hata ayıklama sonucu Visual Studio Code için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL Python, R ve CSharp için VSCode Azure Data Lake Analytics ile geliştirin](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [PowerShell kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure portalını kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanma](data-lake-analytics-data-lake-tools-get-started.md)
* [Data Lake kullanma Analytics(U-SQL) Kataloğu](data-lake-analytics-use-u-sql-catalog.md)
