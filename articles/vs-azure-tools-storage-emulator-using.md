---
title: Yapılandırma ve Visual Studio ile depolama öykünücüsü kullanma | Microsoft Docs
description: Yapılandırma ve Visual Studio ile depolama öykünücüsü kullanma
services: visual-studio-online
author: ghogen
manager: douge
assetId: c8e7996f-6027-4762-806e-614b93131867
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/17/2017
ms.author: ghogen
ms.openlocfilehash: 39e2071a62d6a1f6ee050f862856815048e50430
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128305"
---
# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Yapılandırma ve Visual Studio ile depolama öykünücüsü kullanma
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Genel Bakış
Depolama öykünücüsü, yerel geliştirme makinenizde Azure'da kullanıma sunulan Blob, kuyruk ve tablo depolama hizmetleri taklit eden bir yardımcı Azure SDK'sı geliştirme ortamını içerir. Azure depolama hizmetleri kullanan bir bulut hizmeti oluşturma ve depolama hizmetleri çağıran herhangi bir dış uygulama yazma, kodunuzu yerel olarak depolama öykünücüsü test edebilirsiniz. Microsoft Visual Studio için Azure Araçları, Visual Studio'ya depolama öykünücüsünün yönetimini tümleştirin. Azure Araçları ilk kez kullanıldığında depolama öykünücüsü veritabanı başlatın, çalıştırdığınızda veya kodunuzu Visual Studio'dan hata ayıklama depolama öykünücüsü hizmeti başlatır ve aracılığıyla Azure Depolama Gezgini'ni depolama öykünücüsü veri salt okunur erişim sağlar.

Depolama öykünücüsü sistem gereksinimleri ve özel yapılandırma yönergeleri de dahil olmak üzere hakkında ayrıntılı bilgi için bkz. [geliştirme ve sınama için Azure depolama öykünücüsü kullanma](storage/common/storage-use-emulator.md).

> [!NOTE]
> Depolama öykünücüsü benzetim ve Azure depolama hizmetleri arasında işlev bazı farklar vardır. Bkz: [farklar arasında depolama öykünücüsü ve Azure depolama hizmetleri](storage/common/storage-use-emulator.md) belirli farklar hakkında bilgi için Azure SDK belgelerinde.
> 
> 

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Depolama öykünücüsü için bağlantı dizesini yapılandırma
Depolama öykünücüsü rol içinde kod erişmek için bir Azure depolama hesabına işaret edecek şekilde daha sonra değiştirilebilir ve depolama öykünücüsü için işaret eden bir bağlantı dizesini yapılandırmak isteyeceksiniz. Bir depolama hesabına bağlanmak için çalışma zamanında rolünüz okuyabilen bir yapılandırma ayarı bir bağlantı dizesidir. Bağlantı dizeleri oluşturma hakkında daha fazla bilgi için bkz. [yapılandırma Azure Storage bağlantı dizelerini](/azure/storage/common/storage-configure-connection-string).

> [!NOTE]
> Depolama öykünücüsü hesaba bir başvuru kodunuzdan kullanarak döndürebilir **DevelopmentStorageAccount** özelliği. Depolama öykünücüsü kodunuzdan erişmek istediğiniz, ancak uygulamanızı azure'a yayımlamayı düşünüyorsanız, Azure depolama hesabınıza erişmek ve bu bağlantıyı kullanmak için kodunuzu değiştirmek için bir bağlantı dizesi oluşturmanız gerekir, bu yaklaşım düzgün şekilde çalışır yayımlamadan önce dize. Bir bağlantı dizesi, depolama öykünücüsü hesabı ve Azure depolama hesabı arasında sıklıkla geçiş yapıyorsanız, bu işlemi basitleştirir.
> 
> 

## <a name="initializing-and-running-the-storage-emulator"></a>Depolama öykünücüsü'nü çalıştıran ve başlatma
Çalıştırın veya hizmetinizi Visual Studio'da hata ayıklama, Visual Studio otomatik olarak depolama öykünücüsü başlatır belirtebilirsiniz. Çözüm Gezgini içinde kısayol menüsünü açın, **Azure** projesini ve ardından **özellikleri**. Üzerinde **geliştirme** sekmesinde **Azure depolama öykünücüsü'nü başlatmak** listesinde **True** (Bunu zaten bu değeri ayarlanmadıysa).

İlk kez çalıştırdığınızda veya hizmetinizi Visual Studio'dan hata ayıklama depolama öykünücüsü başlatma işlemini başlatır. Bu işlem ve depolama öykünücüsü için yerel bağlantı noktaları ayırır ve depolama öykünücüsü veritabanı oluşturur. Bu işlem tamamlandıktan sonra depolama öykünücüsü veritabanı silinmedikçe yeniden çalıştırmanız gerekmez.

> [!NOTE]
> Azure Araçları Haziran 2012 sürümünden itibaren depolama öykünücüsü, varsayılan olarak, SQL Express LocalDB içinde çalışır. Azure Araçları daha önceki sürümlerde, SQL Express 2005 veya 2008 ' in varsayılan örneğine karşı depolama öykünücüsü çalıştıran önce yüklemeniz gereken Azure SDK'sını yükleyebilirsiniz. Storage öykünücüsüne karşı bir SQL Express veya adlandırılmış bir'ın adlandırılmış örneği veya Microsoft SQL Server varsayılan örneğini de çalıştırabilirsiniz. Depolama öykünücüsü ve varsayılan örnekten başka bir örneğine karşı çalıştırmak için bkz'nı yapılandırmanız gerekiyorsa [geliştirme ve sınama için Azure depolama öykünücüsü kullanma](storage/common/storage-use-emulator.md).
> 
> 

Depolama öykünücüsü yerel depolama hizmetlerinin durumunu görüntülemek ve başlatmak için durdurun ve bunları sıfırlamak için bir kullanıcı arabirimi sağlar. Depolama öykünücüsü hizmeti başlatıldıktan sonra kullanıcı arabirimi görüntüler veya başlatabilir veya Microsoft Azure öykünücüsü Windows görev çubuğunda bildirim alanı simgesini sağ tıklayarak hizmetini durdurun.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Sunucu Gezgini'nde depolama öykünücüsü verileri görüntüleme
Sunucu Gezgininde Azure depolama, verileri görüntüleyebilir ve ayarlarını depolama öykünücüsü dahil olmak üzere, depolama hesapları, blob ve tablo verileri değiştirmek sağlar. Bkz: [yönetme Azure Blob depolama kaynaklarını depolama Gezgini'yle](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) daha fazla bilgi için.

