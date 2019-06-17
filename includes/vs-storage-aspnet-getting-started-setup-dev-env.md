---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a7c696870e22e1692ca5ed778e47f8e4cc00615a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160731"
---
## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölüm geliştirme ortamını ayarlama aracılığıyla size yol gösterir. Bu, bir ASP.NET MVC uygulaması oluşturma, bağlı hizmetler bağlantı ekleme, denetleyici ekleme ve gereken ad alanının yönergelerini belirtme içerir.

### <a name="create-an-aspnet-mvc-app-project"></a>Bir ASP.NET MVC uygulaması projesi oluşturma

1. Visual Studio'yu açın.

1. Ana menüden **dosya** > **yeni** > **proje**.

1. İçinde **yeni proje** iletişim kutusunda **Web** > **ASP.NET Web uygulaması (.NET Framework)** . İçinde **adı** alanında, belirtin **StorageAspNet**. **Tamam**’ı seçin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. İçinde **yeni ASP.NET Web uygulaması** iletişim kutusunda **MVC**ve ardından **Tamam**.

    ![Ekran görüntüsü, yeni ASP.NET Web uygulaması iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlanmak için bağlı hizmetler kullanın

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın.

1. Bağlam menüsünden seçin **Ekle** > **bağlı hizmet**.

1. İçinde **bağlı hizmetler** iletişim kutusunda **Azure depolama ile bulut depolama**.

    ![Bağlı hizmetler ekran iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. İçinde **Azure depolama** Azure depolama hesabı Bu öğretici için kullanılacak iletişim kutusunda seçin. Yeni bir Azure depolama hesabı oluşturmak için Seç **yeni depolama hesabı oluşturma**ve formu doldurun. Ya da mevcut bir depolama hesabını seçmek veya yeni bir oluşturduktan sonra seçin **Ekle**. Visual Studio, Azure depolama ve bir depolama bağlantı dizesi için NuGet paketi yükler **Web.config**.

1. İçinde **Çözüm Gezgini**, sağ **bağımlılıkları**, seçin **NuGet paketlerini Yönet**ve en son sürümü için bir NuGet paket başvurusu ekleme Microsoft.Azure.ConfigurationManager.

> [!TIP]
> Bir depolama hesabı oluşturma hakkında bilgi edinmek için [Azure portalında](https://portal.azure.com), bakın [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
>
> Kullanarak bir depolama hesabı oluşturabilirsiniz [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md), veya [Azure Cloud Shell](../articles/cloud-shell/overview.md).
