---
title: "Azure işlevleri çalışma zamanı yükleme | Microsoft Docs"
description: "Azure işlevleri çalışma zamanı yükleme"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: b6078ba7b553773294ebbf11949f7d3b53f46f0c
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="install-the-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı Preview yükleyin

Azure işlevleri çalışma zamanı Önizleme yüklemek istiyorsanız, şu adımları izlemelisiniz:

1. En düşük gereksinimleri makinenizi geçirir emin olun.
1. Karşıdan [Azure işlevleri çalışma zamanı Önizleme yükleyici](https://aka.ms/azafr).
1. Azure işlevleri çalışma zamanı preview yükleyin.
1. Azure işlevleri çalışma zamanı Önizleme yapılandırmasını tamamlayın.

## <a name="prerequisites"></a>Ön koşullar

Azure işlevleri çalışma zamanı Önizleme yüklemeden önce aşağıdakilere sahip olmanız gerekir:

1. Microsoft Windows Server 2016 veya Microsoft Windows 10 oluşturucuları Update (Professional veya Enterprise Edition) çalıştıran bir makineye.
1. Ağınızın içinde çalışan bir SQL Server örneği.  Gereken en düşük sürümü, SQL Server Express ' dir.

## <a name="install-the-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı Preview yükleyin

Azure işlevleri çalışma zamanı Önizleme yükleyici Azure işlevleri çalışma zamanı Önizleme yönetimi ve çalışan rolleri yüklenmesinde size kılavuzluk eder.  Yönetim ve çalışan rolü aynı makinede yüklemek mümkündür.  Ancak, daha fazla işlevler eklemek gibi birden çok Worker üzerine işlevlerinizi ölçeklendirme yapabilmek için ek makinelerde daha fazla çalışan rolleri dağıtmanız gerekir.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Yönetim ve çalışan rolü aynı makineye yükleme

1. Azure işlevleri çalışma zamanı Önizleme yükleyiciyi çalıştırın.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici][1]

1. **İleri**’ye tıklayın.
1. Koşullarını okuduğunuzu sonra **EULA**, **onay kutusunu** koşullarını kabul tıklatıp **sonraki** ilerlemek için.
1. Şimdi, bu makinede yüklemek istediğiniz rol seçin **işlevleri yönetim rolü** ve/veya **işlevleri çalışan rolü** tıklatıp **sonraki**.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici - rolü seçimi][3]

    > [!NOTE]
    > Yükleyebileceğiniz **işlevleri çalışan rolü** birçok diğer makinelere. Bunu yapmak için aşağıdaki yönergeleri izleyin ve yalnızca seçin **işlevleri çalışan rolü** yükleyicisinde.

1. Tıklatın **sonraki** sağlamak için **Azure işlevleri çalışma zamanı Kurulum Sihirbazı'nı** makinenizde yükleme işlemini başlatın.
1. Tamamlandıktan sonra Kurulum Sihirbazı'nı başlatır **Azure işlevleri çalışma zamanı** yapılandırma aracı.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici tamamlandı][5]

    > [!NOTE]
    > Üzerinde yüklüyorsanız **Windows 10** ve **kapsayıcı** özelliği daha önce etkinleştirilmemiş, **Azure işlevleri çalışma zamanı Kurulumu** makinenizi isteyip istemediğinizi sorar yüklemeyi tamamlamak için.

## <a name="configure-the-azure-functions-runtime"></a>Azure işlevleri çalışma zamanı yapılandırma

Azure işlevleri çalışma zamanı yüklemenin tamamlanması için yapılandırmasını tamamlamanız gerekir.

1. **Azure işlevleri çalışma zamanı** yapılandırma aracı gösterir hangi rollerin makinenizde yüklenir.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırma aracı][6]

1. Tıklatın **veritabanı** sekmesinde, SQL Server örneği için bağlantı ayrıntılarını girin ve tıklatın **Uygula**.  Bu çalışma zamanı desteklemek için sırayla bir veritabanı oluşturmak Azure işlevleri Çalışma Zamanı Modülü için gereklidir.
    
    ![Azure işlevleri çalışma zamanı Önizleme veritabanı yapılandırması][7]

1. Tıklatın **kimlik bilgileri** sekmesi.  Bu ekranda tüm Azure işlevleri barındırmak için iki yeni kimlik bilgileri kullanmak için bir dosya paylaşımı ile oluşturmanız gerekir.  Belirtin **kullanıcı adı** ve **parola** için KOMBİNASYON **dosya paylaşımı sahibi** ve **dosya paylaşımı kullanıcısı**, ardından**Uygulamak**.

    ![Azure işlevleri çalışma zamanı Önizleme kimlik bilgileri][8]

1. Tıklatın **dosya paylaşımı** sekmesi.  Bu ekranda ayrıntılarını belirtmelisiniz **dosya paylaşımı** konumu.  Bu sizin için oluşturulabilir veya var olan bir dosya paylaşımı kullanabilir ve tıklatın **Uygula**.  Yeni bir dosya paylaşımı konumu seçerseniz Azure işlevleri çalışma zamanı tarafından kullanılmak üzere bir dizin belirtmeniz gerekir.
    
    ![Azure işlevleri çalışma zamanı Önizleme dosya paylaşımı][9]

1. Tıklatın **IIS** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı yapılandırma aracı oluşturacak IIS'de Web siteleri ayrıntılarını gösterir.  Tıklatın **Uygula** tamamlamak için.

    ![Azure işlevleri çalışma zamanı Önizleme IIS][10]

1. Tıklatın **Hizmetleri** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı Yapılandırma aracında hizmetlerinin durumunu gösterir.  Eğer ilk yapılandırmadan sonra **Azure işlevleri konak Etkinleştirme hizmeti** tıklatın çalışmıyor **Hizmeti'ni Başlat**.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırma tamamlandı][11]

1. Son olarak göz atın **Azure işlevleri çalışma zamanı Portal** olarak `https://<machinename>/`.

    ![Azure işlevleri çalışma zamanı Önizleme portalı][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
