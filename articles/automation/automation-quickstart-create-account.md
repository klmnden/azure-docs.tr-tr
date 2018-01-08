---
title: "Azure Hızlı Başlangıcı - Azure Otomasyonu hesabı oluşturma | Microsoft Docs"
description: "Azure Otomasyonu hesabı oluşturmayı ve runbook çalıştırmayı öğrenin"
services: automation
author: csand-msft
ms.author: csand
ms.date: 12/13/2017
ms.topic: quickstart
ms.service: automation
ms.openlocfilehash: 17f711a0678c977fd2331f7596e482a0f0db6df2
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Azure Otomasyonu hesaplarını Azure üzerinden oluşturabilirsiniz. Bu yöntem, Otomasyon hesaplarını ve ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Otomasyon hesabı oluşturma ve hesapta runbook çalıştırma adımları ele alınmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure oturumu açın

## <a name="create-automation-account"></a>Otomasyon hesabı oluşturma

1. Azure'un sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

1. **İzleme + Yönetim**'i ve ardından **Otomasyon**'u seçin.

1. Hesap bilgilerini girin. **Azure Farklı Çalıştır hesabı oluştur** alanında **Evet**'i seçerek Azure kimlik doğrulamasını kolaylaştıran yapıtların otomatik olarak etkinleştirilmesini sağlayın. İşlemi tamamladığınızda Otomasyon hesabı dağıtımını başlatmak için **Oluştur**'a tıklayın.

    ![Otomasyon hesabınız hakkındaki bilgileri sayfaya girin](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

1. Otomasyon hesabı Azure panosuna sabitlenir. Dağıtım tamamlandığında Otomasyon hesabına genel bakış sayfası otomatik olarak açılır.

    ![Otomasyon hesabına genel bakış](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Bir runbook'u çalıştırma

Öğretici runbook'larından birini çalıştırın.

1. **SÜREÇ OTOMASYONU**'nın altındaki **Runbook'lar** öğesine tıklayın. Runbook'ların listesi görüntülenir. Varsayılan olarak hesapta etkin halde birden fazla öğretici amaçlı runbook bulunur.

    ![Otomasyon hesabı runbook'larının listesi](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. **AzureAutomationTutorialScript** runbook'unu seçin. Runbook'a genel bakış sayfası açılır.

    ![Runbook'a genel bakış](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. **Başlat**'a tıklayın ve **Runbook'u Başlat** sayfasında **Tamam**'ı seçerek runbook'u başlatın.

    ![Runbook işi sayfası](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. **İş durumu**, **Çalışıyor** olarak değiştikten sonra runbook işi çıktısını görüntülemek için **Çıktı** veya **Tüm Günlükler**'e tıklayın. Bu öğretici runbook'u için çıktıda Azure kaynaklarınızın listesi yer alır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Otomasyon hesabını ve tüm ilişkili kaynakları silin. Bunu yapmak için Otomasyon hesabı için kaynak grubunu seçip **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Otomasyon hesabı dağıttınız, runbook işi başlattınız ve iş sonuçlarını görüntülediniz. Azure Otomasyonu hakkında daha fazla bilgi edinmek için ilk runbook'unuzu oluşturmanızı sağlayacak hızlı başlangıç sayfasına gidin.

> [!div class="nextstepaction"]
> [Otomasyon Hızlı Başlangıcı - Runbook Oluşturma](./automation-quickstart-create-runbook.md)
