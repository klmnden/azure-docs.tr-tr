---
title: Yapılandırma ve Visual Studio ile depolama öykünücüsünü kullanma | Microsoft Docs
description: Yapılandırma ve Visual Studio ile depolama öykünücüsünü kullanma
services: visual-studio-online
author: ghogen
manager: douge
assetId: c8e7996f-6027-4762-806e-614b93131867
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 8/17/2017
ms.author: ghogen
ms.openlocfilehash: c502d5e0869d35ded5c3ba7e790da0558d219e0e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792251"
---
# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Yapılandırma ve Visual Studio ile depolama öykünücüsünü kullanma
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Genel Bakış
Azure SDK geliştirme ortamı depolama öykünücüsü, Blob, kuyruk ve tablo depolama hizmetleri, yerel geliştirme makinenizde Azure'da kullanılabilir benzetim yapan bir yardımcı program içerir. Kullanıyorsanız Azure storage hizmetleri kullanan bir bulut hizmeti oluşturma ve depolama hizmetleri çağıran herhangi bir dış uygulama yazma, kodunuzu yerel depolama öykünücüsünü karşı test edebilirsiniz. Microsoft Visual Studio için Azure Araçları, Visual Studio'ya depolama öykünücüsünü yönetimini tümleştirin. Azure Araçları ilk kullanımda depolama öykünücüsü veritabanını başlatılamadı, çalıştırmak veya Visual Studio kodunuzdan hata ayıklama depolama öykünücüsü hizmeti başlatılır ve Azure Storage Gezgini üzerinden depolama öykünücüsü veri salt okunur erişim sağlar.

Sistem gereksinimleri ve özel yapılandırma yönergeleri de dahil olmak üzere depolama öykünücüsünü hakkında ayrıntılı bilgi için bkz: [geliştirme ve sınama için Azure Storage öykünücüsünü kullanma](storage/common/storage-use-emulator.md).

> [!NOTE]
> Bazı depolama öykünücüsü benzetimi ve Azure storage Hizmetleri arasındaki işlevsel farklılıklar vardır. Bkz: [farklar arasında depolama öykünücüsü ve Azure Storage Hizmetleri](storage/common/storage-use-emulator.md) belirli farklılıklar hakkında bilgi için Azure SDK belgelerinde.
> 
> 

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Depolama öykünücüsü için bağlantı dizesini yapılandırma
Bir rolü içindeki kodundan depolama öykünücüsünü erişmek için bir Azure depolama hesabı işaret edecek şekilde daha sonra değiştirilebilir ve depolama öykünücüsünü işaret eden bir bağlantı dizesi yapılandırmak istediğiniz. Bir bağlantı dizesi rolünüze bir depolama hesabına bağlanmak için çalışma zamanında okuyabilen bir yapılandırma ayardır. Bağlantı dizeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure uygulamayı yapılandırma](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

> [!NOTE]
> Kullanarak bir depolama öykünücüsü hesabı başvuru kodunuzdan döndürebilir **DevelopmentStorageAccount** özelliği. Bu yaklaşım depolama öykünücüsünü kodunuzdan erişmek istiyor ancak Azure uygulamanızı yayımlamayı düşünüyorsanız, Azure depolama hesabınıza erişmek ve bu bağlantıyı kullanmak için kodunuzu değiştirmek için bir bağlantı dizesi oluşturmanız gerekecektir düzgün çalışır yayımlamadan önce dizesi. Bir bağlantı dizesi, bir Azure depolama hesabı ve depolama öykünücüsü hesabı arasında sıklıkla geçiş yapıyorsanız, bu işlemi basitleştirir.
> 
> 

## <a name="initializing-and-running-the-storage-emulator"></a>Başlatma ve depolama öykünücüsü çalıştırma
Çalıştırın ya da hizmetiniz Visual Studio'da hata ayıklama Visual Studio otomatik olarak depolama öykünücüsünü başlatır belirtebilirsiniz. Çözüm Gezgini'nde için kısayol menüsünü açın, **Azure** proje ve seçin **özellikleri**. Üzerinde **geliştirme** sekmesinde **Başlat Azure Storage öykünücüsü** listesinde, seçin **True** (Bu zaten bu değer ayarlanmamışsa).

İlk kez çalıştırma ya da hizmetiniz Visual Studio'da hata ayıklama depolama öykünücüsünü başlatma işlemini başlatır. Bu işlem yerel bağlantı noktaları için depolama öykünücüsü ayırır ve depolama öykünücüsü veritabanı oluşturur. Tamamlandıktan sonra bu işlem depolama öykünücüsü veritabanı silinmedikçe çalıştırmanız gerekmez.

> [!NOTE]
> Azure Araçları Haziran 2012 sürüm ile başlayarak, depolama öykünücüsü, varsayılan olarak, SQL Express LocalDB çalışır. SQL Express 2005 veya 2008, varsayılan örneği karşı depolama öykünücüsü Azure Araçları önceki sürümlerde çalışır önce yüklemeniz gereken Azure SDK'sı yükleyebilirsiniz. Storage öykünücüsüne karşı SQL Express veya bir adlandırılmış'ın adlandırılmış örneği veya Microsoft SQL Server varsayılan örneğini de çalıştırabilirsiniz. Örneği varsayılan örnek dışında karşı çalıştırmak için bkz: depolama öykünücüsünü yapılandırmanız gerekiyorsa [geliştirme ve sınama için Azure Storage öykünücüsünü kullanma](storage/common/storage-use-emulator.md).
> 
> 

Depolama öykünücüsü yerel depolama hizmetlerinin durumunu görüntüleyin ve başlatmak için durdurmak ve bunları sıfırlamak için bir kullanıcı arabirimi sağlar. Depolama öykünücüsü Hizmet başladıktan sonra kullanıcı arabirimi görüntüler veya başlatabilir veya Microsoft Azure öykünücüsü Windows görev çubuğundaki bildirim alanı simgesine sağ tıklayarak hizmetini durdurun.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Server Explorer'da depolama öykünücüsü verileri görüntüleme
Sunucu Gezgininde Azure depolama, veri görüntülemek ve depolama öykünücüsü de dahil olmak üzere, depolama hesapları blob ve tablo verilerinizi ayarlarını değiştirmek sağlar. Bkz: [Depolama Gezgini ile Azure Blob Storage'ı yönetme kaynaklarını](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) daha fazla bilgi için.

