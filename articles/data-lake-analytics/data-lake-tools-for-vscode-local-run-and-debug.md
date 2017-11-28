---
title: "Azure Data Lake araçları: U-SQL yerel çalıştırma ve Visual Studio Code ile yerel hata ayıklama | Microsoft Docs"
description: "Azure Data Lake araçları için Visual Studio Code yerel çalıştırma ve yerel hata ayıklama için nasıl kullanılacağını öğrenin."
Keywords: VScode,Azure Data Lake Tools,Local run,Local debug,Local Debug,preview storage file,upload to storage path
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: d109e4d57f4ad5ab2be73805ba41bf9ed362cccb
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="u-sql-local-run-and-local-debug-for-windows-with-visual-studio-code"></a>U-SQL yerel çalıştırma ve Visual Studio Code ile Windows için yerel hata ayıklama
Bu belgede, erken kodlama aşama hızlandırmak için ya da Visual Studio Code yerel kodda hata ayıklamak için bir yerel geliştirme makinede U-SQL işleri çalıştırma öğrenin. Visual Studio Code ile Azure Data Lake aracı hakkında yönergeler için bkz [kullanım Azure Data Lake araçları Visual Studio Code için](data-lake-analytics-data-lake-tools-for-vscode.md). 


## <a name="set-up-the-u-sql-local-run-environment"></a>U-SQL yerel çalıştırma ortamını ayarlama

1. Komut palet açın ve ardından girin için Ctrl + Shift + P seçin **ADL: yerel çalıştırma bağımlılık indirme** paketlerini indirmek için.  

   ![ADL LocalRun bağımlılık paketlerini yükleyin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Gösterilen yolundan bağımlılık paketlerini bulun **çıkış** bölmesinde ve sonra BuildTools ve Win10SDK 10240 yükleyin. Bir örnek yolu şöyledir:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Bağımlılık paketlerini bulun](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 yüklemeye **BuildTools**, visualcppbuildtools_full.exe LocalRunDependency klasöründe ve ardından sihirbazdaki yönergeleri izleyin.   

    ![BuildTools yükleyin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 yüklemeye **Win10SDK 10240**, sdksetup.exe LocalRunDependency/Win10SDK_10.0.10240_2 klasöründe ve ardından sihirbazdaki yönergeleri izleyin.  

    ![Win10SDK yükleme 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Ortam değişkeni ayarlayın. Ayarlama **SCOPE_CPP_SDK** ortam değişkenine:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Yerel çalışma hizmetini başlatın ve yerel bir hesap için U-SQL işi gönderin 
İlk kez kullanıcı için kullanması **ADL: yerel çalıştırma bağımlılık indirme** yüklemediyseniz yerel çalıştırma paketlerini indirmek için [U-SQL yerel çalıştırma ortamını ayarlama](#set-up-the-u-sql-local-run-environment).

1. Komut palet açın ve ardından girin için Ctrl + Shift + P seçin **ADL: yerel çalıştırma Hizmeti'ni Başlat**.   
2. Seçin **kabul** Microsoft Yazılım Lisans Koşulları'nı ilk kez kabul etmek için. 

   ![Microsoft Yazılımı Lisans Koşulları'nı kabul edin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Cmd konsolu açılır. İlk kez kullanıcılar için girmenize gerek **3**ve ardından veri giriş ve çıkış yerel klasör yolunu bulun. Diğer seçenekler için varsayılan değerleri kullanabilirsiniz. 

   ![Visual Studio Code yerel çalıştırma cmd için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Komut paletini açmak, girmek için Ctrl + Shift + P seçin **ADL: işi Gönder**ve ardından **yerel** yerel hesabınıza işi göndermek için.

   ![Visual Studio Code için Data Lake araçları yerel seçin](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. İşi gönderdikten sonra gönderim ayrıntıları görüntüleyebilirsiniz. Gönderme ayrıntılarını görüntülemek için seçin **jobUrl** içinde **çıkış** penceresi. Cmd konsolundan iş gönderme durumunu da görüntüleyebilirsiniz. Girin **7** daha fazla iş ayrıntılarını bilmek istiyorsanız cmd konsolunda.

   ![Data Lake araçları için Visual Studio Code yerel çıkış çalıştırma](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake araçları Visual Studio Code yerel cmd durumunu çalıştırmak için](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>U-SQL işi için yerel bir hata ayıklama Başlat  
İlk kez kullanıcı için:

1. Kullanım **ADL: yerel çalıştırma bağımlılık indirme** yüklemediyseniz yerel çalıştırma paketlerini indirmek için [U-SQL yerel çalıştırma ortamını ayarlama](#set-up-the-u-sql-local-run-environment).
2. .NET Core SDK 2.0 önerildiği gibi ileti kutusunda yüklü değilse yükleyin.
 
  ![Dotnet anımsatıcı yükler](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Yükleme C# Visual Studio kodu için ileti kutusunda önerilen değilse yüklü. Tıklatın **yükleme** devam ve VSCode yeniden başlatın.

    ![C# yükleme anımsatıcısı](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Yerel hata ayıklama gerçekleştirmek için aşağıdaki adımları izleyin:
  
1. Komut palet açın ve ardından girin için Ctrl + Shift + P seçin **ADL: yerel çalıştırma Hizmeti'ni Başlat**. Cmd konsolu açılır. Olduğundan emin olun **DataRoot** ayarlanır.
2. Bir kesme noktası, C# kod arkasında ayarlayın.
3. Komut Dosyası Düzenleyicisi, sağ tıklatın ve seçin dön **ADL: yerel hata ayıklama**.
    
   ![Visual Studio Code yerel hata ayıklama sonuç için Data Lake araçları](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio Code için Azure Data Lake Araçları’nı kullanma](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL Python, R ve Azure Data Lake Analytics VSCode içinde CSharp ile geliştirme](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [Azure Data Lake Analytics işleri için U-SQL derlemeleri geliştirin](data-lake-analytics-u-sql-develop-assemblies.md)
* [PowerShell kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Portalı'nı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanın](data-lake-analytics-data-lake-tools-get-started.md)
* [Kullanım Data Lake Analytics(U-SQL) Kataloğu](data-lake-analytics-use-u-sql-catalog.md)
