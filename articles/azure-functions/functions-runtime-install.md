---
title: Azure işlevleri çalışma zamanı yükleme | Microsoft Docs
description: Azure işlevleri çalışma zamanı Önizleme 2 yükleme
services: functions
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: f8ce27bf28f73818932f2ac9056d4fdd573679e8
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
ms.locfileid: "26290666"
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Azure işlevleri çalışma zamanı preview 2 yükleyin

Azure işlevleri çalışma zamanı Önizleme 2 yüklemek istiyorsanız, aşağıdaki adımları izleyin:

1. En düşük gereksinimleri makinenizi geçirir emin olun.
1. Karşıdan [Azure işlevleri çalışma zamanı Önizleme yükleyici](https://aka.ms/azafrv2).
1. Azure işlevleri çalışma zamanı preview 1 kaldırın.
1. Azure işlevleri çalışma zamanı Önizleme 2 yükleyin.
1. Azure işlevleri çalışma zamanı Önizleme 2 yapılandırmasını tamamlayın.
1. Azure işlevleri çalışma zamanı önizlemede ilk işlevinizi oluşturma

## <a name="prerequisites"></a>Ön koşullar

Azure işlevleri çalışma zamanı Önizleme yüklemeden önce aşağıdaki kaynaklardan olması gerekir:

1. Microsoft Windows Server 2016 veya Microsoft Windows 10 oluşturucuları Update (Professional veya Enterprise Edition) çalıştıran bir makineye.
1. Ağınızın içinde çalışan bir SQL Server örneği.  Gereken en düşük sürümü, SQL Server Express ' dir.

## <a name="uninstall-previous-version"></a>Önceki sürümü kaldırın

Azure işlevleri çalışma zamanı Önizleme daha önce yüklediyseniz, en son sürümü yüklemeden önce kaldırmalısınız.  Azure işlevleri çalışma zamanı Önizleme Windows'daki Program Ekle/Kaldır program kaldırarak kaldırın.

## <a name="install-the-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı Preview yükleyin

Azure işlevleri çalışma zamanı Önizleme yükleyici Azure işlevleri çalışma zamanı Önizleme yönetimi ve çalışan rolleri yüklenmesinde size kılavuzluk eder.  Yönetim ve çalışan rolü aynı makinede yüklemek mümkündür.  Ancak, daha fazla işlevi uygulamalar ekleme gibi daha fazla çalışan rolleri birden çok Worker üzerine işlevlerinizi ölçeklendirme yapabilmek için ek makinelerde dağıtmanız gerekir.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Yönetim ve çalışan rolü aynı makineye yükleme

1. Azure işlevleri çalışma zamanı Önizleme yükleyiciyi çalıştırın.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici][1]

1. **İleri**’ye tıklayın.
1. Koşullarını okuduğunuzu sonra **EULA**, **onay kutusunu** koşullarını kabul tıklatıp **sonraki** ilerlemek için.
1. Bu makinede yüklemek istediğiniz rol seçin **işlevleri yönetim rolü** ve/veya **işlevleri çalışan rolü** tıklatıp **sonraki**.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici - rolü seçimi][3]

    > [!NOTE]
    > Yükleyebileceğiniz **işlevleri çalışan rolü** birçok diğer makinelere. Bunu yapmak için aşağıdaki yönergeleri izleyin ve yalnızca seçin **işlevleri çalışan rolü** yükleyicisinde.

1. Tıklatın **sonraki** sağlamak için **Azure işlevleri çalışma zamanı Kurulum Sihirbazı'nı** makinenizde yükleme işlemini başlatın.
1. Tamamlandıktan sonra Kurulum Sihirbazı'nı başlatır **Azure işlevleri çalışma zamanı** yapılandırma aracı.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici tamamlandı][6]

    > [!NOTE]
    > Üzerinde yüklüyorsanız **Windows 10** ve **kapsayıcı** özelliği daha önce etkinleştirilmemiş, **Azure işlevleri çalışma zamanı Kurulumu** makinenizi isteyip istemediğinizi sorar yüklemeyi tamamlamak için.

## <a name="configure-the-azure-functions-runtime"></a>Azure işlevleri çalışma zamanı yapılandırma

Azure işlevleri çalışma zamanı yüklemenin tamamlanması için yapılandırmasını tamamlamanız gerekir.

1. **Azure işlevleri çalışma zamanı** yapılandırma aracı gösterir hangi rollerin makinenizde yüklenir.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırma aracı][7]

1. Tıklatın **veritabanı** sekmesinde, bağlantı ayrıntılarını belirtme dahil olmak üzere, SQL Server örneği için girin bir [veritabanı ana anahtarı](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine), tıklatıp **Uygula**.  Bir SQL Server örneği bağlantısı çalışma zamanı desteklemek için sırayla bir veritabanı oluşturmak Azure işlevleri Çalışma Zamanı Modülü için gereklidir.

    ![Azure işlevleri çalışma zamanı Önizleme veritabanı yapılandırması][8]

1. Tıklatın **kimlik bilgileri** sekmesi.  Burada, tüm işlevi uygulamalarınızı barındırmak için bir dosya paylaşımı ile kullanmak için iki yeni kimlik bilgileri oluşturmanız gerekir.  Belirtin **kullanıcı adı** ve **parola** için KOMBİNASYON **dosya paylaşımı sahibi** ve **dosya paylaşımı kullanıcısı**, ardından**Uygulamak**.

    ![Azure işlevleri çalışma zamanı Önizleme kimlik][9]

1. Tıklatın **dosya paylaşımı** sekmesi.  Burada, dosya paylaşım konumu ayrıntılarını belirtmeniz gerekir.  Dosya Paylaşımı sizin için oluşturulabilir veya'ı tıklatın ve var olan bir dosya paylaşımı için kullanabileceğiniz **Uygula**.  Yeni bir dosya paylaşımı konumu seçerseniz Azure işlevleri çalışma zamanı tarafından kullanılmak üzere bir dizin belirtmeniz gerekir.

    ![Azure işlevleri çalışma zamanı Önizleme dosya paylaşımı][10]

1. Tıklatın **IIS** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı yapılandırma aracı oluşturur IIS'de Web siteleri ayrıntılarını gösterir.  Özel bir DNS adı burada Azure işlevleri çalışma zamanı Önizleme portalı için belirtebilir.  Tıklatın **Uygula** tamamlamak için.

    ![Azure işlevleri çalışma zamanı Önizleme IIS][11]

1. Tıklatın **Hizmetleri** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı Yapılandırma aracında hizmetlerinin durumunu gösterir.  Varsa **Azure işlevleri konak Etkinleştirme hizmeti** olan ilk yapılandırmadan sonra çalışmıyor, tıklatın **Hizmeti'ni Başlat**.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırması tamamlandı][12]

1. Gözat **Azure işlevleri çalışma zamanı Portal** olarak `https://<machinename>.<domain>/`.

    ![Azure işlevleri çalışma zamanı Önizleme portalı][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı önizlemede ilk işlevinizi oluşturma

Azure işlevleri çalışma zamanı önizlemede ilk işlevinizi oluşturmak için

1. Gözat **Azure işlevleri çalışma zamanı Portal** https:// olarak<machinename>.<domain> Örneğin https://mycomputer.mydomain.com
1. İsteyip istemediğiniz sorulur **oturum**, etki alanı hesabı kullanıcı adı ve parola, aksi takdirde yerel hesap kullanıcı adı ve parola Portalı'nda oturum için bir etki alanı kullanması dağıttıysanız.

![Azure işlevleri çalışma zamanı Önizleme portalı oturum açma][14]

1. İşlevi uygulamaları oluşturmak için bir abonelik oluşturmanız gerekir.  Portalın sol üst köşesinde tıklatın  **+**  abonelikleri yanındaki seçeneği

![Azure işlevleri çalışma zamanı Önizleme portalı abonelikleri][15]

1. Seçin **DefaultPlan**, aboneliğiniz için bir ad girin ve tıklatın **oluşturma**.

![Azure işlevleri çalışma zamanı Önizleme portalı abonelik planı ve adı][16]

1. Tüm işlevi uygulamalarınızın portalın sol bölmesinde listelenir.  Yeni bir işlev uygulaması oluşturmak için başlığı seçin **işlev uygulamalarının** tıklatıp  **+**  seçeneği.

1. İşlev uygulamanız için bir ad girin, doğru abonelik seçin, hangi, önce karşı program istediğiniz Azure işlevleri çalışma zamanı sürümü **oluştur**

![Azure işlevleri çalışma zamanı Önizleme portalı yeni işlev uygulaması][17]

1. Yeni işlev uygulamanız portalın sol bölmesinde listelenir.  İşlevler seçin ve ardından **yeni işlev** portalın Orta bölmede üstünde.

![Azure işlevleri çalışma zamanı Önizleme şablonları][18]

1. Zamanlayıcı tetikleyicisi işlevi seçin, sağ taraftaki çıkma içinde işlevinizi adlandırın ve zamanlamasını değiştirmek `*/5 * * * * *` (Bu cron ifade beş saniyede yürütmek Zamanlayıcı işlevinizin neden) tıklatıp **oluştur**

![Azure işlevleri çalışma zamanı önizleme yeni Zamanlayıcı işlevi yapılandırma][19]

1. İşlevinizi şimdi oluşturuldu.  Genişleterek işlevi uygulamanızın yürütme günlüğü görüntüleyebilirsiniz **günlük** portalın alt bölmesinde.

![Azure işlevleri çalışma zamanı Önizleme işlevi yürütme][20]

<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-Progress.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer6-InstallComplete.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[13]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
[14]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Login.png
[15]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions.png
[16]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions1.png
[17]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewFunctionApp.png
[18]: ./media/functions-runtime-install/AzureFunctionsRuntime_v1FunctionsTemplates.png
[19]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewTimerFunction.png
[20]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_RunningV2Function.png
