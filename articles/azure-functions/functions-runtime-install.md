---
title: Azure işlevleri çalışma zamanı yükleme | Microsoft Docs
description: Azure işlevleri çalışma zamanı Önizleme 2 yükleme
services: functions
author: apwestgarth
manager: stefsch
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: aae6bc41f3c2fc2c5f8cf63d07f6b4d79bb3564a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61023205"
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Azure işlevleri çalışma zamanı preview 2 sürümünü yükleyin

[!INCLUDE [intro](../../includes/functions-runtime-preview-note.md)]

Azure işlevleri çalışma zamanı Önizleme 2 yüklemek istiyorsanız, aşağıdaki adımları izleyin:

1. En düşük gereksinimleri makinenize geçirir emin olun.
1. İndirme [Azure işlevleri çalışma zamanı Önizleme yükleyici](https://aka.ms/azafrv2).
1. Azure işlevleri çalışma zamanı Önizleme 1 kaldırın.
1. Azure işlevleri çalışma zamanı preview 2 sürümünü yükleyin.
1. Azure işlevleri çalışma zamanı Önizleme 2 yapılandırmasını tamamlayın.
1. Azure işlevleri çalışma zamanı önizlemesinde ilk işlevinizi oluşturma

## <a name="prerequisites"></a>Önkoşullar

Azure işlevleri çalışma zamanı Önizleme yüklemeden önce aşağıdaki kaynaklara sahip olmalıdır:

1. Microsoft Windows Server 2016 veya Microsoft Windows 10 Creators Update (Professional veya Enterprise Edition) çalıştıran bir makine.
1. Ağınızda çalışan bir SQL Server örneği.  Gerekli en düşük sürüm, SQL Server Express ' dir.

## <a name="uninstall-previous-version"></a>Önceki sürümü kaldırmalısınız

Azure işlevleri çalışma zamanı Önizleme daha önce yüklediyseniz, en son sürümü yüklemeden önce kaldırmanız gerekir.  Azure işlevleri çalışma zamanı Önizleme Ekle/Kaldır'ı Windows içinde program kaldırarak kaldırın.

## <a name="install-the-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı Preview sürümünü yükleyin

Azure işlevleri çalışma zamanı Önizleme yükleyici, Azure işlevleri çalışma zamanı Önizleme yönetim ve çalışan rolleri yükleme size yol gösterir.  Yönetim ve çalışan rolü aynı makinede yüklemek mümkündür.  Ancak, daha fazla işlev uygulamaları ekledikçe daha fazla çalışan rolü üzerine birden çok çalışan işlevlerinizi ölçeklendirme için ek makineler dağıtmanız gerekir.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Yönetim ve çalışan rolü aynı makineye yükleyin.

1. Azure işlevleri çalışma zamanı Önizleme yükleyiciyi çalıştırın.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyicisi][1]

1. **İleri**’ye tıklayın.
1. Koşullarını okuduğunuzu sonra **EULA**, **kutuyu** koşullarını kabul edin ve **sonraki** ilerlemek için.
1. Bu makinede yüklemek istediğiniz rolleri seçin **işlevleri yönetim rolü** ve/veya **işlevleri çalışan rolü** tıklatıp **sonraki**.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici - rolü seçimi][3]

    > [!NOTE]
    > Yükleyebileceğiniz **işlevleri çalışan rolü** birçok diğer makinelere. Bunu yapmak için bu yönergeleri izleyin ve yalnızca belirli **işlevleri çalışan rolü** yükleyicisi.

1. Tıklayın **sonraki** olmasını **Azure işlevleri çalışma zamanı Kurulum Sihirbazı'nı** makinenizde yükleme işlemini başlatabilir.
1. İşlem tamamlandıktan sonra Kurulum Sihirbazı'nı başlatır **Azure işlevleri çalışma zamanı** yapılandırma aracı.

    ![Azure işlevleri çalışma zamanı Önizleme yükleyici tamamlandı][6]

    > [!NOTE]
    > Üzerinde yüklüyorsanız **Windows 10** ve **kapsayıcı** özelliği daha önce etkinleştirilmedi, **Azure işlevleri çalışma zamanı Kurulumu** makinenizi ister yüklemeyi tamamlamak için.

## <a name="configure-the-azure-functions-runtime"></a>Azure işlevleri çalışma zamanı yapılandırma

Azure işlevleri çalışma zamanı yüklemenin tamamlanması için yapılandırmasını tamamlamanız gerekir.

1. **Azure işlevleri çalışma zamanı** yapılandırma aracı makinenizde yüklü hangi rollerin gösterir.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırma aracı][7]

1. Tıklayın **veritabanı** sekmesinde, bağlantı ayrıntılarını girin belirterek de dahil olmak üzere SQL Server örneğiniz için bir [veritabanı ana anahtarı](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine), tıklatıp **Uygula**.  Çalışma zamanı desteği için bir veritabanı oluşturmak Azure işlevleri çalışma zamanı için sırada bir SQL Server örneğine bir bağlantı gereklidir.

    ![Azure işlevleri çalışma zamanı Önizleme veritabanı yapılandırması][8]

1. Tıklayın **kimlik bilgilerini** sekmesi.  Burada, tüm işlev uygulamaları barındırmak için bir dosya paylaşımı ile kullanmak için iki yeni kimlik bilgileri oluşturmanız gerekir.  Belirtin **kullanıcı adı** ve **parola** birleşimlerini **dosya paylaşımı sahibi** ve **dosya paylaşımı kullanıcısı**, ardından**Uygulamak**.

    ![Azure işlevleri çalışma zamanı Önizleme kimlik bilgileri][9]

1. Tıklayın **dosya paylaşımı** sekmesi.  Burada, dosya paylaşım konumunu ayrıntılarını belirtmeniz gerekir.  Dosya Paylaşımı, oluşturulabilir veya var olan bir dosya paylaşımı'nı kullanın ve tıklayın **Uygula**.  Yeni bir dosya paylaşımı konumu seçin, Azure işlevleri çalışma zamanı tarafından kullanım için bir dizin belirtmeniz gerekir.

    ![Azure işlevleri çalışma zamanı Önizleme dosya paylaşımı][10]

1. Tıklayın **IIS** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı yapılandırma aracı oluşturur ve IIS içinde Web siteleri ayrıntılarını gösterir.  Bir özel DNS adını buraya Azure işlevleri çalışma zamanı Önizleme portalı için belirtebilirsiniz.  Tıklayın **Uygula** tamamlanması.

    ![Azure işlevleri çalışma zamanı Önizleme IIS][11]

1. Tıklayın **Hizmetleri** sekmesi.  Bu sekme, Azure işlevleri çalışma zamanı Yapılandırma aracında hizmetlerin durumunu gösterir.  Varsa **Azure işlevleri konak Etkinleştirme hizmeti** olan ilk yapılandırmadan sonra çalışmıyor, tıklayın **Hizmeti'ni Başlat**.

    ![Azure işlevleri çalışma zamanı Önizleme yapılandırması tamamlandı][12]

1. Gözat **Azure işlevleri çalışma zamanı portalı** olarak `https://<machinename>.<domain>/`.

    ![Azure işlevleri çalışma zamanı Önizleme portalı][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Azure işlevleri çalışma zamanı önizlemesinde ilk işlevinizi oluşturma

Azure işlevleri çalışma zamanı önizlemesinde ilk işlevinizi oluşturma

1. Gözat **Azure işlevleri çalışma zamanı portalı** olarak `https://<machinename>.<domain>` örneğin `https://mycomputer.mydomain.com`.

1. İsteyip istemediğiniz sorulur **oturum**, etki alanı hesabı kullanıcı adı ve parola, aksi takdirde yerel hesap kullanıcı adı ve parola portalda oturum açmak için bir etki alanı kullanması dağıtılmış.

    ![Azure işlevleri çalışma zamanı Önizleme portalı oturum açma][14]

1. İşlev uygulamaları oluşturmak için abonelik oluşturmanız gerekir.  Portalın sol üst köşesinde tıklayın **+** abonelikleri yanındaki seçeneği.

    ![Azure işlevleri çalışma zamanı Önizleme portalı abonelikleri][15]

1. Seçin **DefaultPlan**, aboneliğiniz için bir ad girin ve tıklayın **Oluştur**.

    ![Azure işlevleri çalışma zamanı Önizleme portalı abonelik planı ve adı][16]

1. Tüm işlev uygulamalarınızın, portalın sol bölmesinde listelenir.  Yeni bir işlev uygulaması oluşturmak için başlığı seçin **işlev uygulamaları** tıklatıp **+** seçeneği.

1. İşlev uygulamanız için bir ad girin, doğru aboneliği seçin, istediğiniz karşı program ve Azure işlevleri çalışma zamanı sürümünü **oluştur**

    ![Azure işlevleri çalışma zamanı Önizleme portalında yeni işlev uygulaması][17]

1. Yeni işlev uygulamanızı portalın sol bölmesinde listelenir.  İşlevler'i seçin ve ardından **yeni işlev** portalın Orta bölmede üstünde.

    ![Azure işlevleri çalışma zamanı Önizleme şablonları][18]

1. Zamanlayıcı tetikleyicisi işlevi seçin, sağ açılır öğesinde işlevinizi adlandırın ve için zamanlamayı değiştirmek `*/5 * * * * *` (Bu cron ifadesi her beş saniyede yürütmek Zamanlayıcı işlevinizin neden) tıklayıp **oluştur**

    ![Azure işlevleri çalışma zamanı önizleme yeni Zamanlayıcı işlevi yapılandırması][19]

1. İşleviniz artık oluşturuldu.  İşlev uygulamanızın yürütme günlüğü genişleterek görüntüleyebileceğiniz **günlük** portalın alt bölmesinde.

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
