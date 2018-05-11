---
title: Azure Otomasyonu hesabını yönetme
description: Bu makalede sertifika yenileme, silme ve yanlış yapılandırılma gibi Otomasyon hesabınızın yapılandırmasını yönetme işlemleri açıklanmaktadır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: aa16fa3a4c7c6e954179e8100d30cce45e450ab9
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="manage-azure-automation-account"></a>Azure Otomasyonu hesabını yönetme
Otomasyon hesabınızın süresi dolmadan önce sertifikayı yenilemeniz gerekir. Farklı Çalıştır hesabının tehlikede olduğunu düşünüyorsanız, hesabı silip yeniden oluşturabilirsiniz. Bu bölümde bu işlemlerin nasıl gerçekleştirileceği ele alınmaktadır.

## <a name="self-signed-certificate-renewal"></a>Otomatik olarak imzalanan sertifika yenileme
Farklı Çalıştır hesabı için oluşturduğunuz otomatik olarak imzalanan sertifikanın süresi, oluşturulduktan bir yıl sonra dolar. Sertifikayı süresi dolmadan önce herhangi bir zamanda yenileyebilirsiniz. Sertifikayı yenilediğinizde, sıraya alınmış ya da o anda çalışan ve Farklı Çalıştır hesabı ile kimliği doğrulanmış runbook’ların olumsuz yönde etkilenmemesi için geçerli sertifika saklanır. Sertifika, sona erme tarihine kadar geçerliliğini sürdürür.

> [!NOTE]
> Otomasyon Farklı Çalıştır hesabınızı, kuruluş sertifika yetkiliniz tarafından yayınlanan bir sertifika kullanmak üzere yapılandırdıysanız ve bu seçeneği kullanırsanız, kurumsal sertifika otomatik olarak imzalanan bir sertifikayla değiştirilir.

Sertifikayı yenilemek için aşağıdakileri yapın:

1. Azure portalında Otomasyon hesabınızı açın.

2. **Otomasyon Hesabı** bölümünde 
3. , **Hesap Ayarları** altındaki **Hesap özellikleri** bölmesinde **Farklı Çalıştır Hesapları**'nı seçin.

    ![Otomasyon hesabı özellikleri bölmesi](media/automation-manage-account/automation-account-properties-pane.png)
3. **Farklı Çalıştır Hesapları** özellikleri sayfasında sertifikasını yenilemek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin.

4. Seçili hesabın **Özellikler** bölmesinde **Sertifikayı yenile**'ye tıklayın.

    ![Farklı Çalıştır hesabının sertifikasını yenileme](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını silme
Bu bölümde bir Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını silip yeniden oluşturma işlemi açıklamaktadır. Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur. Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını sildikten sonra Azure portalında yeniden oluşturabilirsiniz.

1. Azure portalında Otomasyon hesabınızı açın.

2. **Otomasyon hesabı** sayfasında **Farklı Çalıştır Hesapları**'nı seçin.

3. **Farklı Çalıştır Hesapları** özellikleri sayfasında silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin. Ardından, seçili hesabın **Özellikler** bölmesinde **Sil**'e tıklayın.

 ![Farklı Çalıştır hesabını silme](media/automation-manage-account/automation-account-delete-runas.png)

4. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

5. Hesap silindikten sonra **Farklı Çalıştır Hesapları** özellikler sayfasında **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek hesabı yeniden oluşturabilirsiniz.

 ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Yanlış yapılandırma
İlk kurulum sırasında, Farklı Çalıştır veya Klasik Farklı Çalıştır hesabının düzgün çalışması için gerekli olan bazıları silinmiş veya düzgün oluşturulmamış olabilir. Bu öğeler şunlardır:

* Sertifika varlığı
* Bağlantı varlığı
* Farklı Çalıştır hesabının katkıda bulunan rolünden kaldırılması
* Azure AD'de hizmet sorumlusu veya uygulama

Yanlış yapılandırmanın önceki ve diğer örneklerinde, Otomasyon hesabı değişiklikleri algılar ve hesabın **Farklı Çalıştır Hesapları** özellikleri sayfasında *Tamamlanmadı* durumunu gösterir.

![Tamamlanmamış Farklı Çalıştır hesabı yapılandırma durumu](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Farklı Çalıştır hesabını seçtiğinizde hesabın **Özellikler** bölmesinde aşağıdaki hata iletisi görüntülenir:

![Tamamlanmamış Farklı Çalıştır yapılandırma uyarısı iletisi](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

Hesabı silip yeniden oluşturarak Farklı Çalıştır hesabıyla ilgili bu sorunları hızlı bir şekilde çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet Sorumluları hakkında daha fazla bilgi için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](../active-directory/active-directory-application-objects.md).
* Azure Automation’da Rol Tabanlı Erişim Denetimi hakkında daha fazla bilgi için bkz. [Azure Automation’da rol tabanlı erişim denetimi](automation-role-based-access-control.md).
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services sertifikalarına genel bakış](../cloud-services/cloud-services-certs-create.md).