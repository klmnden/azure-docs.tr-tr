---
title: Hızlı başlangıç - Azure portalda Windows sanal makinesi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure portalı kullanarak Windows sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/02/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b3f972fadfa972b581474c1a61769564f81260af
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560472"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Hızlı Başlangıç: Azure Portal’da Windows sanal makinesi oluşturma

Azure sanal makineleri (VM’ler), Azure portalı üzerinden oluşturulabilir. Bu yöntem, sanal makineleri ve tüm ilgili kaynaklarını oluşturmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Azure portalı kullanarak Azure’da Windows Server 2016 çalıştıran bir sanal makinenin (VM) nasıl dağıtılacağı gösterilir. VM’ye RDP oluşturup IIS web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.

1. İçinde **yeni** sayfasındaki **popüler**seçin **Windows Server 2016 Datacenter**.

1. **Temel Bilgiler** sekmesinde, **Proje ayrıntıları** altında, doğru aboneliğin seçildiğinden emin olun, ardından **Yeni oluştur** kaynak grubunu seçin. Tür *myResourceGroup* adı. 

    ![VM'niz için yeni bir kaynak grubu oluşturma](./media/quick-create-portal/project-details.png)

1. **Örnek ayrıntıları** altında, **Sanal makine adı** için *myVM* yazın ve **Konum** için *East US*'yi seçin. Diğer varsayılan değerleri bırakın.

    ![Örnek ayrıntıları bölümü](./media/quick-create-portal/instance-details.png)

1. **Yönetici hesabı** altında, *azureuser* gibi bir kullanıcı adı ve bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.

    ![Kullanıcı adınızı ve parolanızı girin](./media/quick-create-portal/administrator-account.png)

1. **Gelen bağlantı noktası kuralları** altında **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **RDP (3389)** ve **HTTP** değerlerini seçin.

    ![RDP ve HTTP için bağlantı noktaları açma](./media/quick-create-portal/inbound-port-rules.png)

1. Kalan varsayılan ayarları bırakın, ardından sayfanın alt kısmındaki **Gözden geçir + oluştur** düğmesini seçin.

    ![Gözden geçirme ve oluşturma](./media/quick-create-portal/review-create.png)


## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun. Bu yönergeler VM’nize bir Windows bilgisayarından nasıl bağlanacağınızı gösterir. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

1. Sanal makine özellikleri sayfasında **Bağlan** düğmesine tıklayın. 

    ![Portaldan bir Azure sanal makinesine bağlanma](./media/quick-create-portal/portal-quick-start-9.png)
    
2. **Sanal makineye bağlan** sayfasında, 3389 numaralı bağlantı noktası üzerinden DNS adına göre bağlanmak için varsayılan seçenekleri olduğu gibi bırakın ve **RDP dosyasını indir**’e tıklayın.

2. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan**’a tıklayın. 

3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adını **localhost**\\*username* olarak yazın, sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıyı oluşturmak için **Evet** veya **Devam**’a tıklayın.

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için IIS web sunucusunu yükleyin. VM’de bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

İşiniz bittiğinde, RDP’nin sanal makine bağlantısını kapatın.


## <a name="view-the-iis-welcome-page"></a>IIS karşılama sayfasını görüntüleme

Portalda VM'yi seçin ve VM özetinde IP adresinin sağ tarafındaki **Kopyalamak için tıklayın** düğmesini kullanarak adresi kopyalayın ve bir tarayıcı sekmesine yapıştırın. Varsayılan IIS karşılama sayfası açılır ve şu şekilde görünmelidir:

![Varsayılan IIS sitesi](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Sanal makinenin kaynak grubunu seçin ve ardından **Sil**. Kaynakları silme işlemini bitirmek için kaynak grubu adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, basit bir sanal makine dağıttınız, web trafiği için bir ağ bağlantı noktası açtınız ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
