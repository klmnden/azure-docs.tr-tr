---
title: Azure Marketi - Azure'ı kullanarak bir Windows sanal masaüstü Önizleme konak havuzu oluşturma
description: Azure Marketi aracılığıyla bir Windows sanal masaüstü Önizleme konak havuzu oluşturmak nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: f692303140db1441aa34aacef62523d7f596dba1
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204726"
---
# <a name="tutorial-create-a-host-pool-by-using-the-azure-marketplace"></a>Öğretici: Azure Marketi kullanarak bir konak havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü Önizleme Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Her konak havuzu, fiziksel masaüstünde yaptıkları gibi kullanıcı etkileşim kurabilir bir uygulama grubu içerebilir.

Bu öğreticide, bir Microsoft Azure Marketi teklifi kullanarak bir Windows sanal masaüstü Kiracı içinde bir ana bilgisayar havuz oluşturma açıklanır. Görevler aşağıdakileri içerir:

> [!div class="checklist"]
> * Windows sanal masaüstü konak havuzu oluşturun.
> * Bir kaynak grubu ile Vm'leri bir Azure aboneliği oluşturun.
> * VM'ler, Active Directory etki alanına katılın.
> * Vm'leri ile Windows sanal masaüstüne kaydedin.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>Yeni bir konak havuz sağlama teklifi Azure Marketi çalıştırın

Yeni bir konak havuz sağlama teklifi Azure Marketi çalıştırmak için:

1. Seçin **+** veya **+ kaynak Oluştur**.
2. Girin **Windows Sanal Masaüstü** Market arama penceresinde.
3. Seçin **Windows sanal masaüstü - bir ana makine havuzu sağlama**ve ardından **Oluştur**.

Uygun dikey pencereleri bilgilerini girmek için yönergeleri izleyin.

### <a name="basics"></a>Temel Bilgiler

İşte neler **Temelleri** dikey penceresinde:

1. Windows sanal masaüstü Kiracı içinde benzersiz olan konak havuzu için bir ad girin.
2. Kişisel Masaüstü için uygun seçeneği belirleyin. Seçerseniz **Evet**, bu ana havuzuna bağlanan her kullanıcı kalıcı olarak bir sanal makineye atanacak.
3. Windows sanal masaüstü istemcileri için oturum açın ve bir masaüstü tamamlanmadan teklifi Azure Marketi sonra erişebilen kullanıcıların virgülle ayrılmış listesini girin. Örneğin, atamak istediğiniz user1@contoso.com ve user2@contoso.com erişmek, girin "user1@contoso.com,user2@contoso.com."
4. Seçin **Yeni Oluştur** ve yeni kaynak grubu için bir ad sağlayın.
5. İçin **konumu**, Active Directory sunucusu bağlantısı olan bir sanal ağ olarak aynı konumu seçin.
6. **Tamam**’ı seçin.

### <a name="configure-virtual-machines"></a>Sanal makineleri yapılandırma

İçin **sanal makineleri yapılandırma** dikey penceresinde:

1. Varsayılanları kabul edin ya da sayısını ve boyutunu sanal makinelerin özelleştirin.
2. Bir ön eki sanal makinelerin adlarını girin. Örneğin, "ön eki" adını girin, sanal makineler "ön eki-0," "ön eki-1" vb. çağrılır.
3. **Tamam**’ı seçin.

### <a name="virtual-machine-settings"></a>Sanal makine ayarları

İçin **sanal makine ayarlarını** dikey penceresinde:

>[!NOTE]
> Azure Active Directory etki alanı Hizmetleri (Azure AD DS) bir ortama Vm'lerinize birleştirdiğimiz, etki alanı katılma kullanıcınızın üyesi olduğundan emin olun [AAD DC Administrators grubu](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-admingroup#task-3-configure-administrative-group).

1. İçin **görüntü kaynağı**kaynağını seçin ve nasıl bulacağınızı ve depolamak için uygun bilgileri girin. Yönetilen diskleri kullanmayı tercih ederseniz, .vhd dosyasını içeren depolama hesabını seçin.
2. Vm'leri Active Directory etki alanına etki alanı hesabının kullanıcı asıl adını ve parolasını girin. Bu aynı kullanıcı adı ve parola sanal makinelerde yerel bir hesap oluşturulur. Daha sonra bu yerel hesaplar sıfırlayabilirsiniz.
3. Active Directory sunucusu bağlantısı olan bir sanal ağ seçin ve ardından sanal makineleri barındırmak için bir alt ağ seçin.
4. **Tamam**’ı seçin.

### <a name="windows-virtual-desktop-preview-tenant-information"></a>Windows sanal masaüstü Önizleme Kiracı bilgileri

İçin **Windows sanal masaüstü Kiracı bilgileri** dikey penceresinde:

1. İçin **Windows sanal masaüstü Kiracı grubu adı**, kiracınızın içeren Kiracı grubu adını girin. Belirli bir sağlanan sürece varsayılan olarak bırakın. Kiracı grubu adı.
2. İçin **Windows sanal masaüstü Kiracı adı**, burada bu konak havuzu oluşturmadan Kiracı adını girin.
3. Windows sanal masaüstü Kiracı RDS sahibi olarak kimlik doğrulaması için kullanmak istediğiniz kimlik bilgileri türünü belirtin. Tamamlanmışsa [PowerShell öğretici ile hizmet sorumluları ve rol atamalarını oluşturma](./create-service-principal-role-powershell.md)seçin **hizmet sorumlusu**. Zaman **Azure AD Kiracı Kimliğinizi** görünen kimliği için hizmet sorumlusu içeren Azure Active Directory örneğini girin.
4. Kiracı yönetici hesabı için kimlik bilgilerini girin. Yalnızca bir parola kimlik bilgisi ile hizmet sorumluları desteklenir.
5. **Tamam**’ı seçin.

## <a name="complete-setup-and-create-the-virtual-machine"></a>Kurulumu tamamlayın ve sanal makine oluşturun

Son iki dikey pencere için:

1. Üzerinde **özeti** dikey penceresinde, kurulum bilgilerini gözden geçirin. Bir şeyle değiştirmek istiyorsanız uygun dikey penceresine geri dönün ve devam etmeden önce değişikliği yapın. Bilgileri doğru görünüyorsa, seçin **Tamam**.
2. Üzerinde **satın** dikey penceresinde, Azure Market'ten satın alma hakkında ek bilgileri gözden geçirin.
3. Seçin **Oluştur** konak havuzunuz dağıtılacak.

Oluşturmakta olduğunuz kaç VM sayısına bağlı olarak bu işlem 30 dakika veya daha sürebilir.

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>(İsteğe bağlı) Masaüstü uygulama grubuna ek kullanıcılar atayın

Tamamlandığında teklifi Azure Marketi sonra sanal makinelerinizde tam oturum Masaüstü test başlamadan önce daha fazla kullanıcı Masaüstü uygulama grubuna atayabilirsiniz. Azure Marketi'nde teklifin varsayılan kullanıcılar zaten ekledikten ve daha fazlasını eklemek istemiyorsanız, bu bölümü atlayabilirsiniz.

Masaüstü uygulama grubuna kullanıcıları atamak için bir PowerShell penceresi açmanız gerekir. Bundan sonra aşağıdaki iki cmdlet girmeniz gerekir.

Windows sanal masaüstü ortama oturum açmak için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Kullanıcılar, bu cmdlet'i kullanarak masaüstü uygulama grubuna ekleyin:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Kullanıcının UPN kullanıcının kimliğini Azure Active Directory'de eşleşmesi gerekir (örneğin, user1@contoso.com). Birden çok kullanıcı eklemek istiyorsanız, her kullanıcı için bu cmdlet'i çalıştırmanız gerekir.

Bu adımları tamamladıktan sonra Masaüstü uygulama grubuna eklenen kullanıcılar Windows sanal masaüstüne desteklenen Uzak Masaüstü istemcileri ile oturum açın ve bir kaynağı için bir oturum Masaüstü bakın.

Geçerli desteklenen istemciler şunlardır:

- [Windows 7 ve Windows 10 için Uzak Masaüstü İstemcisi](connect-windows-7-and-10.md)
- [Windows sanal masaüstü web istemcisi](connect-web.md)

>[!IMPORTANT]
>Güvenliğini sağlamaya yardımcı olmak için azure'da Windows sanal masaüstü ortamınızı Vm'lerinizde gelen bağlantı noktası 3389 açmayın öneririz. Windows sanal masaüstü açık bir konak havuzun Vm'leri erişmek kullanıcılar için 3389 numaralı gelen bağlantı noktası gerektirmez. Sorun giderme amacıyla 3389 numaralı bağlantı noktası açmanız gerekiyorsa, kullanmanızı öneririz [tam zamanında VM erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Sonraki adımlar

Havuz ve atanan kullanıcılar, Masaüstü erişmek için bir konak yaptığınız, konak havuzunuz RemoteApp programları ile doldurabilirsiniz. Windows sanal masaüstü uygulamalarını yönetme hakkında daha fazla bilgi için bu öğreticiye bakın:

> [!div class="nextstepaction"]
> [Uygulama grupları öğretici yönetme](./manage-app-groups.md)
