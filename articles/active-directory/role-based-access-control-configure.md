<properties
    pageTitle="Azure portalında Rol Tabanlı Erişim denetimini kullanma | Microsoft Azure"
    description="Azure Portal'da Rol Tabanlı Erişim Denetimi ile erişim yönetimine başlayın. Kaynaklarınıza izinler atamak için rol atamalarını kullanın."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="kgremban"/>


# Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın

> [AZURE.SELECTOR]
- [Azure portalı](role-based-access-control-azure-portal.md)
- [Klasik Azure portalı](role-based-access-control-configure.md)

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz. Bu makale, Azure portalında RBAC ile çalışmaya başlamanıza yardımcı olur. RBAC'nin erişimi yönetmenize nasıl yardımcı olduğu konusunda daha fazla bilgi isterseniz bkz. [Rol Tabanlı Erişim Denetimi Nedir?](role-based-access-control-what-is.md).

## Erişimi görüntüleme
Kimin bir kaynağa, kaynak grubuna veya aboneliğe erişimi olduğunu [Azure portal](https://portal.azure.com)'da ana dikey penceresinde görebilirsiniz. Örneğin, kimin kaynak gruplarımızdan birine erişimi olduğunu görmek istiyoruz:

1. Soldaki gezinti çubuğunda **Kaynak grupları** seçeneğini belirleyin.  
    ![Kaynak grupları - simge](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. **Kaynak grupları** dikey penceresinde kaynak grubunun adını seçin.
3. Kaynak grubu dikey penceresinin sağ üst kısmında **Kullanıcılar**'ı seçin.  
    ![Kullanıcılar - simge](./media/role-based-access-control-configure/users_icon.png)
4. **Kullanıcılar** dikey penceresi, kaynak grubuna erişim verilen tüm kullanıcıları, grupları ve uygulamaları listeler.  

    ![Kullanıcılar dikey penceresi - devralınmış veya atanmış erişim ekran görüntüsü](./media/role-based-access-control-configure/view-access.png)

Bazı kullanıcıların **Atanmış**, diğerlerinin **Devralınmış** erişime sahip olduğuna dikkat edin. Erişim, özellikle kaynak grubuna atanmış veya üst aboneliğe yapılan atamadan devralınmış olabilir.

> [AZURE.NOTE] Yeni RBAC modelinde, klasik abonelik yöneticileri ve ortak yöneticileri aboneliğin sahipleri olarak kabul edilir.


## Erişim Ekleme
Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz.

1. **Kullanıcılar** dikey penceresinde **Ekle**'yi seçin.  
    ![Ekle - simge](./media/role-based-access-control-configure/add_icon.png)  
2. **Rol seçin** dikey penceresinde atamak istediğiniz rolü seçin.
3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.  

    ![Kullanıcı ekle dikey penceresi - arama ekran görüntüsü](./media/role-based-access-control-configure/grant-access2.png)

4. Atamayı oluşturmak için **Tamam**'ı seçin. **Kullanıcı ekleme** açılır penceresi ilerleme durumunu izler.  
    ![Kullanıcı ekleniyor ilerleme çubuğu - ekran görüntüsü](./media/role-based-access-control-configure/addinguser_popup.png)

Bir rol ataması başarıyla eklendikten sonra **Kullanıcılar** dikey penceresinde görüntülenir.

## Erişimi Kaldırma

1. **Kullanıcılar** dikey penceresinde rol atamasını seçin.
2. Atama ayrıntıları dikey penceresinde **Kaldır**'ı seçin.  
    ![Kaldır - simge](./media/role-based-access-control-configure/remove_icon.png)
3. Kaldırmayı onaylamak için **Evet**'i seçin.  
    ![Kullanıcılar dikey penceresi - rolden kaldır ekran görüntüsü](./media/role-based-access-control-configure/remove-access1.png)

Devralınmış atamalar kaldırılamaz. Aşağıdaki görüntüde kaldır düğmesinin gri olduğuna dikkat edin. Bunun yerine, **Atama Konumu** ayrıntısına bakın. Rol atamasını kaldırmak için orada listelenen kaynağa gidin.

![Kullanıcılar dikey penceresi - devralınmış erişim kaldır düğmesini devre dışı bırakır ekran görüntüsü](./media/role-based-access-control-configure/remove-access2.png)

## Erişimi yönetmeye yönelik diğer araçlar
Azure portal dışındaki araçlarda Azure RBAC komutları ile roller atayabilir ve erişimi yönetebilirsiniz.  Önkoşullar hakkında daha fazla bilgi edinmek ve Azure RBAC komutlarını kullanmaya başlamak için bağlantıları izleyin.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure Komut Satırı Arabirimi](role-based-access-control-manage-access-azure-cli.md)
- [REST API](role-based-access-control-manage-access-rest.md)

## Sonraki Adımlar
- [Erişim değişiklik geçmişi raporu oluşturma](role-based-access-control-access-change-history-report.md)
- Bkz. [RBAC yerleşik rolleri](role-based-access-built-in-roles.md)
- Kendiniz için [Azure RBAC'de özel roller](role-based-access-control-custom-roles.md) tanımlama



<!--HONumber=Oct16_HO1-->


