# [Azure Active Directory Belgeleri](index.md)

# Genel Bakış
## [Azure Active Directory nedir?](fundamentals/active-directory-whatis.md)
## [Azure kimlik yönetimi hakkında](fundamentals/identity-fundamentals.md)
## [Azure kimlik çözümlerini anlama](fundamentals/understand-azure-identity-solutions.md)
## [Karma kimlik çözümü seçin](choose-hybrid-identity-solution.md)
## [Azure aboneliklerini ilişkilendirme](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Yerleşim ve verilerle ilgili dikkat edilecek konular](fundamentals/active-directory-data-storage-eu.md)
## [SSS](fundamentals/active-directory-faq.md)
## [Yenilikler](fundamentals/whats-new.md)


# başlarken
## [Azure AD’yi kullanmaya başlama](fundamentals/get-started-azure-ad.md)
## [Azure AD Premium’a kaydolun](fundamentals/active-directory-get-started-premium.md)
## [Özel bir etki alanı adı ekleme](fundamentals/add-custom-domain.md)
## [Şirket markası yapılandırma](fundamentals/customize-branding.md)
## [Kullanıcıları Azure AD’ye ekleme](fundamentals/add-users-azure-active-directory.md)
## [Kullanıcılara lisans atama](fundamentals/license-users-groups.md)
## [Self servis parola sıfırlamasını yapılandırma](authentication/quickstart-sspr.md)
## [Azure AD’ye kuruluşunuzun gizlilik bilgilerini ekleme](active-directory-properties-area.md)


# Nasıl yapılır
## Planlama ve tasarım
### [Azure AD mimarisini anlama](fundamentals/active-directory-architecture.md)
### [Azure Active Directory’de talep eşlemesi](active-directory-claims-mapping.md)
### [Karma kimlik çözümü dağıtma](active-directory-hybrid-identity-design-considerations-overview.md)
#### Gereksinimlerini belirleme
##### [Kimlik](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Dizin eşitlemesi](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-Factor Authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Kimlik yaşam döngüsü stratejisi](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Veri güvenliği planlama](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Veri koruma](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [İçerik yönetimi](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Erişim denetimi](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Olay yanıtı](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Kimlik yaşam döngünüzü planlama
##### [Görevler](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Benimseme stratejisi](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Sonraki adımlar](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Araç karşılaştırması](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Kullanıcıları yönetme
### [Azure AD’ye yeni kullanıcı ekleme](fundamentals/add-users-azure-active-directory.md)
### [Kullanıcı profillerini yönetme](fundamentals/active-directory-users-profile-azure-portal.md)
### [Kullanıcı parolasını sıfırlama](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Hesapları paylaşma](active-directory-sharing-accounts.md)
### [Kullanıcıları yönetici rollerine atama](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Silinen bir kullanıcıyı geri yükleme](fundamentals/active-directory-users-restore.md)
### [Başka bir dizinden konuk kullanıcılar ekleme (B2B)](b2b/what-is-b2b.md)
#### [B2B kullanıcıları ekleyen yöneticiler](b2b/add-users-administrator.md)
#### [B2B kullanıcıları ekleyen bilgi çalışanları](b2b/add-users-information-worker.md)
#### [API ve özelleştirme](b2b/customize-invitation-api.md)
#### [Kod ve Azure PowerShell örnekleri](b2b/code-samples.md)
#### [Self servis kaydolma portal örneği](b2b/self-service-portal.md)
#### [Davet e-postası](b2b/invitation-email-elements.md)
#### [Davet kullanma](b2b/redemption-experience.md)
#### [Davetiye olmadan B2B kullanıcıları ekleme](b2b/add-user-without-invite.md)
#### [Davetlere izin verme veya davetleri engelleme](b2b/allow-deny-list.md)
#### [B2B için koşullu erişim](b2b/conditional-access.md)
#### [B2B paylaşım ilkeleri](b2b/delegate-invitations.md)
#### [Bir role B2B kullanıcısı ekleme](b2b/add-guest-to-role.md)
#### [Dinamik gruplar ve B2B kullanıcıları](b2b/use-dynamic-groups.md)
#### [Kuruluştan ayrılma](b2b/leave-the-organization.md)
#### [Denetim ve raporlar](b2b/auditing-and-reporting.md)
#### [Hibrit kuruluşlar için B2B](b2b/hybrid-organizations.md)
##### [B2B kullanıcılarına yerel uygulama erişimi verme](b2b/hybrid-cloud-to-on-premises.md)
##### [Yerel kullanıcılara bulut uygulamaları erişimi verme](b2b/hybrid-on-premises-to-cloud.md)
#### [B2B ve Office 365 dış paylaşım](b2b/o365-external-user.md)
#### [B2B lisansı](b2b/licensing-guidance.md)
#### [Geçerli sınırlamalar](b2b/current-limitations.md)
#### [SSS](b2b/faq.md)
#### [B2B sorunlarını giderme](b2b/troubleshoot.md)
#### [B2B kullanıcısını anlama](b2b/user-properties.md)
#### [B2B kullanıcı belirteci](b2b/user-token.md)
#### [Azure AD tümleşik uygulamaları için B2B](b2b/configure-saas-apps.md)
#### [B2B kullanıcı taleplerini eşleme](b2b/claims-mapping.md)
#### [B2B işbirliğini B2C ile karşılaştırma](b2b/compare-with-b2c.md)
#### [B2B desteği alma](b2b/get-support.md)

## [Grupları ve üyeleri yönetme](fundamentals/active-directory-manage-groups.md)
### Grupları yönetme
#### [Azure portalında](fundamentals/active-directory-groups-create-azure-portal.md)
#### [Graph için Azure AD PowerShell (v2)](users-groups-roles/groups-settings-v2-cmdlets.md)
#### [Azure AD PowerShell MSOnline](users-groups-roles/groups-settings-cmdlets.md)
### [Grup üyelerini yönetme](fundamentals/active-directory-groups-members-azure-portal.md)
### [Grup sahiplerini yönetme](fundamentals/active-directory-accessmanagement-managing-group-owners.md)
### [Grup üyeliğini yönetme](fundamentals/active-directory-groups-membership-azure-portal.md)
### [Grupları kullanarak lisansları atama](fundamentals/active-directory-licensing-whatis-azure-portal.md)
#### [Bir gruba lisans atama](users-groups-roles/licensing-groups-assign.md)
#### [Bir gruptaki lisans sorunlarını tanımlama ve çözme](users-groups-roles/licensing-groups-resolve-problems.md)
#### [Tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](users-groups-roles/licensing-groups-migrate-users.md)
#### [Kullanıcıları ürün lisansları arasında geçirme](users-groups-roles/licensing-groups-change-licenses.md)
#### [Grup tabanlı lisanslama için ek senaryolar](users-groups-roles/licensing-group-advanced.md)
#### [Grup tabanlı lisanslama için Azure PowerShell örnekleri](users-groups-roles/licensing-ps-examples.md)
#### [Azure AD’de ürünler ve hizmet planları için başvurular](users-groups-roles/licensing-service-plan-reference.md)
### [Office 365 gruplarının süre sonlarını ayarlama](users-groups-roles/groups-lifecycle.md)
### [Gruplar için bir adlandırma ilkesini zorlama](users-groups-roles/groups-naming-policy.md)
### [Tüm grupları görüntüleme](fundamentals/active-directory-groups-view-azure-portal.md)
### [SaaS uygulamalarına grup erişimi ekleme](users-groups-roles/groups-saasapps.md)
### [Silinen bir Office 365 grubunu geri yükleme](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Grup ayarlarını yönetme](fundamentals/active-directory-groups-settings-azure-portal.md)
### [Self servis gruplarını kurma](users-groups-roles/groups-self-service-management.md)
### Dinamik gruplar
#### [Dinamik grup oluşturma](users-groups-roles/groups-create-rule.md)
#### [Kural söz dizimi ve özellikleri](users-groups-roles/groups-dynamic-membership.md)
#### [Grup üyeliği türünü değiştirme](users-groups-roles/groups-change-type.md)
#### [Sorun giderme](users-groups-roles/groups-troubleshooting.md)

## [Raporları yönetme](active-directory-reporting-azure-portal.md)
### [Oturum açma etkinliği](active-directory-reporting-activity-sign-ins.md)
### [Denetim etkinliği](active-directory-reporting-activity-audit-logs.md)
### [Risk altındaki kullanıcılar](active-directory-reporting-security-user-at-risk.md)
### [Riskli oturum açma işlemleri](active-directory-reporting-security-risky-sign-ins.md)
### [Risk olayları](active-directory-reporting-risk-events.md)
### [Azure İzleyici’yi kullanarak günlükleri izleme](reporting-azure-monitor-diagnostics-overview.md)
### [SSS](active-directory-reporting-faq.md)

### Görevler
#### [Adlandırılmış konumları yapılandırma](active-directory-named-locations.md)
#### [Etkinlik raporlarını bulma](active-directory-reporting-migration.md)
#### [Azure AD Power BI İçerik Paketi’ni kullanma](active-directory-reporting-power-bi-content-pack-how-to.md)
#### [Riskli olduğu belirlenen kullanıcıları düzeltme](active-directory-report-security-user-at-risk-remediation.md)
#### [Etkinlik günlüklerini Azure olay hub’ına yönlendirme](reporting-azure-monitor-diagnostics-azure-event-hub.md)
#### [Etkinlik günlüklerini Azure depolama hesabında arşivleme](reporting-azure-monitor-diagnostics-azure-storage-account.md)
#### [Azure İzleyici kullanarak etkinlik günlüklerini Splunk ile tümleştirme](reporting-azure-monitor-diagnostics-splunk-integration.md)

### Başvuru
#### [Bekletme](active-directory-reporting-retention.md)
#### [Gecikmeler](active-directory-reporting-latencies-azure-portal.md)
#### [Denetim etkinliği başvurusu](active-directory-reporting-activity-audit-reference.md)
#### [Oturum açma etkinliği hata kodları](active-directory-reporting-activity-sign-ins-errors.md)
#### [Azure İzleyici’de denetim günlüğü şemasını yorumlama](reporting-azure-monitor-diagnostics-audit-log-schema.md)
#### [Azure İzleyici’de oturum açma günlüğü şemasını yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)

### Sorun giderme
#### [Eksik denetim verileri](active-directory-reporting-troubleshoot-missing-audit-data.md)
#### [İndirmelerde eksik veriler](active-directory-reporting-troubleshoot-missing-data-download.md)
#### [Azure AD Etkinlik günlükleri içerik paketi hataları](active-directory-reporting-troubleshoot-content-pack.md)
#### [Azure AD Raporlama API’sindeki hatalar](active-directory-reporting-troubleshoot-graph-api.md)

### [Programlı Erişim](active-directory-reporting-api-getting-started-azure-portal.md)
#### [Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md)
#### [Denetim örnekleri](active-directory-reporting-api-audit-samples.md)
#### [Oturum açma örnekleri](active-directory-reporting-api-sign-in-activity-samples.md)
#### [Sertifikaları kullanma](active-directory-reporting-api-with-certificates.md)

## [Parolaları yönetme](authentication/concept-sspr-howitworks.md)
### Kullanıcı belgeleri
#### [Parolanızı sıfırlama veya değiştirme](user-help/active-directory-passwords-update-your-own-password.md)
#### [Self servis parola sıfırlama için kaydolma](user-help/active-directory-passwords-reset-register.md)


## Cihazları yönetme
### [Genel Bakış](devices/overview.md)

### Hızlı Başlangıçlar
#### [Azure AD alanında kayıtlı Windows 10 cihazlarını ayarlama](user-help/device-management-azuread-registered-devices-windows10-setup.md)
#### [Azure AD alanına katılmış cihazları ayarlama](user-help/device-management-azuread-joined-devices-setup.md)

### Öğreticiler
#### [Yönetilen etki alanları için hibrit Azure AD katılımını yapılandırma](devices/hybrid-azuread-join-managed-domains.md)
#### [Federasyon etki alanları için hibrit Azure AD katılımını yapılandırma](devices/hybrid-azuread-join-federated-domains.md)
#### [Hibrit Azure AD katılımını el ile yapılandırma](devices/hybrid-azuread-join-manual-steps.md)
#### [Windows 10’u ilk kez çalıştırma deneyimi sırasında Azure AD katılımını yapılandırma](devices/azuread-joined-devices-frx.md)

### Nasıl yapılır kılavuzları
#### [Azure AD katılımını planlama](devices/azureadjoin-plan.md)
#### [Hibrit Azure AD katılımınızı uygulamayı planlama](devices/hybrid-azuread-join-plan.md)
#### [Cihazlarınızın hibrit Azure AD katılımını denetleme](devices/hybrid-azuread-join-control.md)
#### [Hibrit Azure AD’ye katılmış geçerli Windows cihazlarında sorun giderme](devices/troubleshoot-hybrid-join-windows-current.md)
#### [Hibrit Azure AD’ye katılmış eski Windows cihazlarında sorun giderme](devices/troubleshoot-hybrid-join-windows-legacy.md)

### Kavramlar
### [Azure portalını kullanarak cihazları yönetme](devices/device-management-azure-portal.md)
### [SSS](devices/faq.md)

## Uygulamaları yönetme
### [Genel Bakış](manage-apps/what-is-application-management.md)
### [Başlarken](manage-apps/plan-an-application-integration.md)
### [SaaS uygulama tümleştirmesi öğreticileri](saas-apps/tutorial-list.md)

### [SaaS uygulamalarına kullanıcı hazırlama ve hazırlamayı kaldırma](active-directory-saas-app-provisioning.md) 
#### [Uygulama tümleştirmesi öğreticileri](saas-apps/tutorial-list.md) 
#### [SCIM etkin uygulamalara otomatik sağlama](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md) 
#### [Öznitelik eşlemeleri için ifadeler yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Kapsam belirleme filtrelerini kullanma](active-directory-saas-scoping-filters.md) 
#### [Otomatik kullanıcı hazırlama raporu](active-directory-saas-provisioning-reporting.md) 
#### [Kullanıcı hazırlama sorunlarını giderme](active-directory-application-provisioning-content-map.md) 

### [Uygulama Proxy’si ile uygulamalara uzaktan erişme](manage-apps/application-proxy.md)
#### başlarken
##### [Uygulama Ara Sunucusu](manage-apps/application-proxy-enable.md)
##### [Uygulamaları yayımlama](manage-apps/application-proxy-publish-azure-portal.md)
##### [Özel etki alanları](manage-apps/application-proxy-configure-custom-domain.md)
#### [Çoklu oturum açma](manage-apps/application-proxy-single-sign-on.md)
##### [KCD ile SSO](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [Üst bilgi içeren SSO](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [Parola kasası oluşturma ile SSO](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Kavramlar
##### [Bağlayıcılar](manage-apps/application-proxy-connectors.md)
##### [Güvenlik](manage-apps/application-proxy-security.md)
##### [Ağlar](manage-apps/application-proxy-network-topology.md)


##### [TMG veya UAG’den yükseltme](manage-apps/application-proxy-migration.md)

#### Gelişmiş yapılandırmalar
##### [Ayrı ağlarda yayımlama](manage-apps/application-proxy-connector-groups.md)
##### [Ara sunucular](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Talep kullanan uygulamalar](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Native Client uygulamaları](manage-apps/application-proxy-configure-native-client-application.md)
##### [Sessiz yükleme](manage-apps/application-proxy-register-connector-powershell.md)
##### [Özel giriş sayfası](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Satır içi bağlantıları çevirme](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Joker karakterler](active-directory-application-proxy-wildcard.md)
##### [Kişisel verileri kaldırma](manage-apps/application-proxy-remove-personal-data.md)


#### Adım adım kılavuzlar yayımlama
##### [Uzak Masaüstü](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Sorun giderme](manage-apps/application-proxy-troubleshoot.md)

### Kurumsal uygulamaları yönetme
#### [Uygulama ekleme](manage-apps/add-application-portal.md)
#### [Kiracı uygulamalarını görüntüleme](manage-apps/view-applications-portal.md)
#### [Kullanıcıları atama](manage-apps/assign-user-or-group-access-portal.md)
#### [Markalamayı özelleştirme](manage-apps/change-name-or-logo-portal.md)
#### [Kullanıcılar için oturum açmayı devre dışı bırakma](manage-apps/disable-user-sign-in-portal.md)
#### [Kullanıcıları kaldırma](manage-apps/remove-user-or-group-access-portal.md)

#### [Kullanıcı hesabı hazırlamayı yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)
#### [Kurumsal uygulamalar için çoklu oturum açmayı yönetme](manage-apps/configure-single-sign-on-portal.md)
#### [SAML uygulamaları için gelişmiş sertifika imzalama](manage-apps/certificate-signing-options.md)
#### [Uygulamayı kullanıcı deneyiminden gizleme](manage-apps/hide-application-from-user-portal.md)
### [HRD İlkesi'ni kullanarak Oturum Açma için Otomatik Hızlandırmayı Yapılandırma](manage-apps/configure-authentication-for-federated-users-portal.md)
### [AD FS uygulamalarını Azure AD’ye geçirme](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md)
#### [SSO erişimi](manage-apps/what-is-single-sign-on.md)
#### [SSO için sertifikalar](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Kiracı kısıtlamaları](manage-apps/tenant-restrictions.md)
#### [SCIM kullanıcı sağlama hizmetinden yararlanma](manage-apps/use-scim-to-provision-users-and-groups.md)


### Sorun giderme



#### Erişim Paneli
##### [Uygulama görüntülenmiyor](application-access-panel-unexpected-application-not-appearing.md)
##### [Beklenmeyen uygulama görüntüleniyor](application-access-panel-unexpected-application-appears.md)
##### [Oturum açılamıyor](application-access-panel-web-sign-in-problem.md)
##### [Tarayıcı uzantısı yüklenirken hata oluşuyor](application-access-panel-extension-problem-installing.md)
##### [Self servis uygulama erişiminin kullanma](application-access-panel-self-service-applications-how-to.md)
##### [Self servis uygulama erişiminin kullanırken hata oluşuyor](application-access-panel-self-service-applications-problem.md)

#### Uygulama ekleme
##### [Uygulama türünü seçme](application-config-add-app-problem-how-to-choose-application-type.md)
##### [Yaygın sorunlar - Galeri uygulamaları](application-config-add-app-problem-problem-adding-gallery-app.md)
##### [Yaygın sorunlar - Galeri dışındaki uygulamalar](application-config-add-app-problem-problem-adding-non-gallery-app.md)

#### Uygulama Ara Sunucusu
##### [Uygulama sayfası görüntülenirken sorun oluşuyor](application-proxy-page-appearance-broken-problem.md)
##### [Uygulamanın yüklenmesi çok uzun sürüyor](application-proxy-page-load-speed-problem.md)
##### [Uygulama sayfasındaki bağlantılar çalışmıyor](application-proxy-page-links-broken-problem.md)
##### [Uygulamam için açılacak bağlantı noktaları](application-proxy-connectivity-ports-how-to.md)
##### [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı yok](application-proxy-connectivity-no-working-connector.md)
##### [Azure portalda yapılandırma](application-proxy-config-how-to.md)
##### [Uygulamamda çoklu oturum açmayı yapılandırma](application-proxy-config-sso-how-to.md)
##### [Yönetici portalında uygulama oluştururken sorun oluşuyor](application-proxy-config-problem.md)
##### [Kerberos Kısıtlanmış Temsilini Yapılandırma](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [PingAccess ile yapılandırma](application-proxy-back-end-ping-access-how-to.md)
##### ["Bu Şirket Uygulamasına Erişilemiyor" hatası](application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor](application-proxy-connector-installation-problem.md)


#### Uygulama kaydı
##### [Uygulama nesnesi için alan girme](application-dev-registration-config-specific-application-property-how-to.md)
##### [Belirteç ömrü varsayılanlarını değiştirme](application-dev-registration-config-change-token-lifetime-how-to.md)

#### Kimlik Doğrulaması
##### [Uç noktaları yapılandırma](application-dev-registration-config-how-to.md)

#### Koşullu Erişim
##### [Müşteri Cihaz Kaydı önkoşullarına uymadı](active-directory-conditional-access.md)
##### [Koşullu Erişim ilkeleri yanlış ayarlandığından kiracı engelleniyor](active-directory-conditional-access-device-remediation.md)
##### [Kapalı Corpnet kuralları nasıl ve ne zaman geçerlik kazanır?](https://aka.ms/calocation)
##### [Azure AD’de kullanıcının kaydetmesine izin verilen cihaz sayısını artırma](active-directory-azureadjoin-setup.md)
##### [Exchange Online için Koşullu Erişimi Ayarlama](https://aka.ms/csforexchange)
##### [Windows 7 cihazları için Koşullu Erişimi ayarlama](active-directory-conditional-access.md#device-based-conditional-access)
##### [Koşullu erişim ile desteklenen uygulamalar](active-directory-conditional-access-supported-apps.md)

#### API bulma
##### [API bulma](application-dev-api-find-an-api-how-to.md)

#### Erişimi yönetme
##### [Uygulamaya kullanıcı ve grup atama](application-access-assignment-how-to-add-assignment.md)
##### [Kullanıcıların bir uygulamaya erişimini kaldırma](application-access-assignment-how-to-remove-assignment.md)
##### [Self servis uygulama atamasını yapılandırma](application-access-self-service-how-to.md)
##### [Beklenmeyen kullanıcı ataması](application-access-unexpected-user-assignment.md)
##### [Uygulamalar listesinde beklenmeyen uygulama](application-access-unexpected-application.md)

#### Çok kiracılı uygulamalar
##### [Yeni bir uygulama yapılandırma](application-dev-setup-multi-tenant-app.md)
##### [Uygulama galerisine ekleme](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### İzinler
##### [API için izinleri seçme](application-dev-perms-for-given-api.md)
##### [Uygulamama izin verme](application-dev-registration-config-grant-permissions-how-to.md)
##### [Temsilci izinleri ile uygulama izinlerini karşılaştırması](application-dev-delegated-and-app-perms.md)
##### [Uygulama onayı](application-dev-consent-framework.md)

#### Sağlama
##### [Gereken süre](application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Saatler sürüyor - Galeri uygulaması](application-provisioning-when-will-provisioning-finish.md)
##### [Kullanıcı sağlamayı yapılandırma - galeri uygulaması](application-provisioning-config-how-to.md)
##### [Kullanıcı sağlamayı yapılandırma sorunu - galeri uygulaması](application-provisioning-config-problem.md)
##### [Kullanıcı sağlama galeri uygulamasını yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu](application-provisioning-config-problem-storage-limit.md)
##### [Kullanıcılar sağlanmıyor - galeri uygulaması](application-provisioning-config-problem-no-users-provisioned.md)
##### [Yanlış kullanıcılar sağlanıyor - galeri uygulaması](application-provisioning-config-problem-wrong-users-provisioned.md)

#### Çoklu oturum açma
##### [Yöntem seçme](application-config-sso-how-to-choose-sign-on-method.md)
##### [Yapılandırma](application-dev-registration-config-sso-how-to.md)
##### [Federasyon yapılandırma - galeri uygulamaları](application-config-sso-how-to-configure-federated-sso-gallery.md)
##### [Federasyon yapılandırmayla ilgili yaygın sorunlar - galeri uygulamaları](application-config-sso-problem-configure-federated-sso-gallery.md)
##### [Federasyon yapılandırma - galeri dışındaki uygulamalar](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
##### [Federasyon yapılandırmayla ilgili yaygın sorunlar - galeri dışındaki uygulamalar](application-config-sso-problem-configure-federated-sso-non-gallery.md)
##### [Parola yapılandırma - galeri uygulamaları](application-config-sso-how-to-configure-password-sso-gallery.md)
##### [Parola yapılandırmayla ilgili yaygın sorunlar - galeri uygulamaları](application-config-sso-problem-configure-password-sso-gallery.md)
##### [Parola yapılandırma - galeri dışındaki uygulamalar](application-config-sso-how-to-configure-password-sso-non-gallery.md)
##### [Parola yapılandırmayla ilgili yaygın sorunlar - galeri dışındaki uygulamalar](application-config-sso-problem-configure-password-sso-non-gallery.md)

#### Kullanıcı oturumu açma sorunları
##### [Beklenmeyen izin alma istemi](application-sign-in-unexpected-user-consent-prompt.md)
##### [Kullanıcı onayı hatası](application-sign-in-unexpected-user-consent-error.md)
##### [Özel portaldan oturum açma sorunları](application-sign-in-other-problem-deeplink.md)
##### [Özel bölmeden oturum açma sorunları](application-sign-in-other-problem-access-panel.md)
##### [Uygulama oturum açma sayfasında hata](application-sign-in-problem-application-error.md)
##### [Parolayla çoklu oturum açma sorunu - galeri dışındaki uygulama](application-sign-in-problem-password-sso-non-gallery.md)
##### [Parolayla çoklu oturum açma sorunu - galeri uygulaması](application-sign-in-problem-password-sso-gallery.md)
##### [Microsoft uygulamasında oturum açma sorunu](application-sign-in-problem-first-party-microsoft.md)
##### [Federasyonda çoklu oturum açma sorunu - galeri dışındaki uygulama](application-sign-in-problem-federated-sso-non-gallery.md)
##### [Federasyonda çoklu oturum açma sorunu - galeri uygulaması](application-sign-in-problem-federated-sso-gallery.md)
##### [Özel geliştirilmiş uygulama ile ilgili sorun](application-sign-in-problem-custom-dev.md)
##### [Şirket içi uygulamayla ilgili sorun - Uygulama Ara Sunucusu](application-sign-in-problem-on-premises-application-proxy.md)

### [Uygulama geliştirme](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Belge kitaplığı](active-directory-apps-index.md)

## Dizininizi yönetme
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Özel etki alanı adları
#### [Hızlı Başlangıç](fundamentals/add-custom-domain.md)
#### [Özel etki alanı adı ekleme](users-groups-roles/domains-manage.md)
### [Dizininizi yönetme](fundamentals/active-directory-administer.md)
### [Bir dizini silme](users-groups-roles/directory-delete-howto.md)
### [Birden fazla dizin](users-groups-roles/licensing-directory-independence.md)
### [Self servise kaydolma](users-groups-roles/directory-self-service-signup.md)
### [Yönetilmeyen bir dizini devralma](users-groups-roles/domains-admin-takeover.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Etkinleştirme](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Grup ilkesi ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10 ayarları](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Azure AD Connect kullanarak şirket içi kimlikleri tümleştirme](./connect/active-directory-aadconnect.md)

## Kaynaklara temsilci erişimi
### [Yönetici rolleri](users-groups-roles/directory-assign-admin-roles.md)
#### [Yönetici rolü üyelerini görüntüleme](users-groups-roles//directory-manage-roles-portal.md)
#### [Kullanıcıya yönetici rolü atama](fundamentals/active-directory-users-assign-role-azure-portal.md)
#### [Üye ve konuk kullanıcı izinlerini karşılaştırma](fundamentals/users-default-permissions.md)
### [Yönetici rolü güvenliği](users-groups-roles/directory-admin-roles-secure.md)  
#### [Acil durum erişimi yönetici hesapları oluşturma](users-groups-roles/directory-emergency-access.md)
### [Yönetim birimleri](users-groups-roles/directory-administrative-units.md)
### [Belirteç ömrünü yapılandırma](active-directory-configurable-token-lifetimes.md)

## Erişim gözden geçirmeleri
### [Erişim gözden geçirmelerine genel bakış](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Erişim gözden geçirmesi tamamlama](active-directory-azure-ad-controls-complete-access-review.md)
### [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
### [Erişim değerlendirmesi gerçekleştirme](active-directory-azure-ad-controls-perform-access-review.md)
### [Erişiminizi gözden geçirme](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Erişim gözden geçirmesi ile konuk erişimi](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Gözden geçirmelerle kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Programları ve denetimleri yönetme](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Erişim gözden geçirmesi sonuçlarını alma](active-directory-azure-ad-controls-retrieve-access-review.md)

## Kimliklerinizi güvenli hale getirme
### [Koşullu erişim](active-directory-conditional-access-azure-portal.md)
#### [Kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md)
#### Hızlı Başlangıçlar
##### [Bulut uygulaması MFA başına yapılandırma](active-directory-conditional-access-app-based-mfa.md)
##### [Kullanım koşullarının kabul edilmesini gerektirme](active-directory-conditional-access-tou.md)
##### [Oturum riski algılandığında erişimi engelleme](active-directory-conditional-access-app-sign-in-risk.md)
#### Öğreticiler
##### [Klasik MFA ilkesini geçirme](active-directory-conditional-access-migration-mfa.md)
#### Kavramlar
##### [Anahat Koruma](active-directory-conditional-access-baseline-protection.md)
##### [Koşullar](active-directory-conditional-access-conditions.md)
##### [Konum koşulları](active-directory-conditional-access-locations.md)
##### [Denetimler](active-directory-conditional-access-controls.md)
##### [Durum aracı](active-directory-conditional-access-whatif.md)
##### [Office 365 hizmetleri için cihaz ilkelerini anlama](active-directory-conditional-access-device-policies.md)
#### Nasıl yapılır kılavuzları
##### [En iyi uygulamalar](active-directory-conditional-access-best-practices.md)
##### [Güvenilmeyen ağlardan gelen erişim denemeleri için koşullu erişim ilkelerini yapılandırma](active-directory-conditional-access-untrusted-networks.md)
##### [Cihaz tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-policy-connected-applications.md)
##### [Uygulama tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-mam.md)
##### [Kullanıcılar ve uygulamalar için kullanım koşullarını sağlayın](active-directory-tou.md)
##### [Klasik ilkeleri geçirme](active-directory-conditional-access-migration.md)
##### [VPN bağlantısı ayarlama](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)
##### [SharePoint ve Exchange Online ayarlama](active-directory-conditional-access-no-modern-authentication.md)
##### [Düzeltme](active-directory-conditional-access-device-remediation.md)
#### [Teknik başvuru](active-directory-conditional-access-technical-reference.md)
#### [SSS](active-directory-conditional-faqs.md)

### Sertifika Tabanlı Kimlik Doğrulaması
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md)

### [Azure AD Kimlik Koruması](active-directory-identityprotection.md)
#### [Etkinleştirme](active-directory-identityprotection-enable.md)
#### [Güvenlik açıklarını algılama](active-directory-identityprotection-vulnerabilities.md)
#### [Risk olayları](active-directory-identity-protection-risk-events.md)
#### [Bildirimler](active-directory-identityprotection-notifications.md)
#### [Oturum açma deneyimi](active-directory-identityprotection-flows.md)
#### [Risk olaylarının benzetimini gerçekleştirme](active-directory-identityprotection-playbook.md)
#### [Kullanıcıların engelini kaldırma](active-directory-identityprotection-unblock-howto.md)
#### [SSS](active-directory-identity-protection-faqs.md)
#### [Sözlük](active-directory-identityprotection-glossary.md)
#### [Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

## Diğer hizmetleri Azure AD ile tümleştirme 
### [LinkedIn’i Azure AD ile tümleştirme](users-groups-roles/linkedin-integration.md)

## [Azure’a AD FS dağıtma](active-directory-aadconnect-azure-adfs.md)
### [Yüksek kullanılabilirlik](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Değişiklik imzası karma algoritması](active-directory-federation-sha256-guidance.md)

## [Sorun giderme](fundamentals/active-directory-troubleshooting-support-howto.md)

## Azure AD Kavram Kanıtı (PoC) Dağıtma
### [PoC El Kitabı: Giriş](active-directory-playbook-intro.md)
### [PoC El Kitabı: Malzemeler](active-directory-playbook-ingredients.md)
### [PoC El Kitabı: Uygulama](active-directory-playbook-implementation.md)
### [PoC El Kitabı: Yapı Taşları](active-directory-playbook-building-blocks.md)


# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [Azure PowerShell cmdlet'leri](/powershell/azure/overview)
## [Java API Başvurusu](/java/api)
## [.NET API’si](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)
## [Hizmet sınırlamaları ve kısıtlamalar](users-groups-roles/directory-service-limits-restrictions.md)

# İlgili
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Geliştiriciler için Azure AD](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# Kaynaklar
## [Azure AD dağıtım planları](./fundamentals/active-directory-deployment-plans.md)
## [Azure geri bildirim forumu](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
