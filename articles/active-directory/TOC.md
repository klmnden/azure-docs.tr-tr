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
### [Başka bir dizinden konuk kullanıcılar ekleme (B2B)](b2b/what-is-b2b.md)
#### [B2B kullanıcıları ekleyen yöneticiler](b2b/add-users-administrator.md)
#### [B2B kullanıcıları ekleyen bilgi çalışanları](b2b/add-users-information-worker.md)
#### [API ve özelleştirme](b2b/customize-invitation-api.md)
#### [Google federasyonu](b2b/google-federation.md)
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
### [Grupları yönetme](fundamentals/active-directory-groups-create-azure-portal.md)
### [Grubu ve üyelerini silme](fundamentals/active-directory-groups-delete-group.md)
### [Grup ayarlarını yönetme](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Raporları yönetme](reports-monitoring/overview-reports.md)
### [Oturum açma etkinliği](reports-monitoring/concept-sign-ins.md)
### [Denetim etkinliği](reports-monitoring/concept-audit-logs.md)
### [Risk altındaki kullanıcılar](reports-monitoring/concept-user-at-risk.md)
### [Riskli oturum açma işlemleri](reports-monitoring/concept-risky-sign-ins.md)
### [Risk olayları](reports-monitoring/concept-risk-events.md)
### [Azure İzleyici’yi kullanarak günlükleri izleme](reports-monitoring/overview-activity-logs-in-azure-monitor.md)
### [SSS](reports-monitoring/reports-faq.md)

### Görevler
#### [Adlandırılmış konumları yapılandırma](active-directory-named-locations.md)
#### [Etkinlik raporlarını bulma](reports-monitoring/howto-find-activity-reports.md)
#### [Azure AD Power BI İçerik Paketi’ni kullanma](reports-monitoring/howto-power-bi-content-pack.md)
#### [Riskli olduğu belirlenen kullanıcıları düzeltme](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Etkinlik günlüklerini Azure olay hub’ına yönlendirme](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Etkinlik günlüklerini Azure depolama hesabında arşivleme](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Azure İzleyici kullanarak etkinlik günlüklerini Splunk ile tümleştirme](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [Azure İzleyici kullanarak etkinlik günlüklerini SumoLogic ile tümleştirme](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)

### Başvuru
#### [Bekletme](reports-monitoring/reference-reports-data-retention.md)
#### [Gecikmeler](reports-monitoring/reference-reports-latencies.md)
#### [Denetim etkinliği başvurusu](reports-monitoring/reference-audit-activities.md)
#### [Oturum açma etkinliği hata kodları](reports-monitoring/reference-sign-ins-error-codes.md)
#### [Azure İzleyici’de denetim günlüğü şemasını yorumlama](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [Azure İzleyici’de oturum açma günlüğü şemasını yorumlama](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### Sorun giderme
#### [Azure AD etkinlik günlüklerinde eksik veriler](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [İndirmelerde eksik veriler](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Azure AD etkinlik günlükleri içerik paketi hataları](reports-monitoring/troubleshoot-content-pack.md)
#### [Azure AD Raporlama API’sindeki hatalar](reports-monitoring/troubleshoot-graph-api.md)

### [Programlı Erişim](reports-monitoring/concept-reporting-api.md)
#### [Önkoşullar](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [Sertifikaları kullanma](reports-monitoring/tutorial-access-api-with-certificates.md)

## [Parolaları yönetme](authentication/concept-sspr-howitworks.md)

## Uygulamaları yönetme
### [Genel Bakış](manage-apps/what-is-application-management.md)
### [Başlarken](manage-apps/plan-an-application-integration.md)
### [SaaS uygulama tümleştirmesi öğreticileri](saas-apps/tutorial-list.md)

### [SaaS uygulamalarına kullanıcı hazırlama ve hazırlamayı kaldırma](manage-apps/user-provisioning.md) 
#### [Uygulama tümleştirmesi öğreticileri](saas-apps/tutorial-list.md) 
#### [SCIM etkin uygulamalara otomatik sağlama](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Öznitelik eşlemelerini özelleştirme](manage-apps/customize-application-attributes.md) 
#### [Öznitelik eşlemeleri için ifadeler yazma](manage-apps/functions-for-customizing-application-data.md) 
#### [Kapsam belirleme filtrelerini kullanma](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [Otomatik kullanıcı hazırlama raporu](manage-apps/check-status-user-account-provisioning.md) 
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
##### [Joker karakterler](manage-apps/application-proxy-wildcard.md)
##### [Kişisel verileri kaldırma](manage-apps/application-proxy-remove-personal-data.md)


#### Adım adım kılavuzlar yayımlama
##### [Uzak Masaüstü](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Sorun giderme](manage-apps/application-proxy-troubleshoot.md)

### Kurumsal uygulamaları yönetme
#### [Uygulama ekleme](manage-apps/add-application-portal.md)
#### [Kiracı uygulamalarını görüntüleme](manage-apps/view-applications-portal.md)
#### [Çoklu oturum açmayı yapılandırma](manage-apps/configure-single-sign-on-portal.md)
#### [Kullanıcıları atama](manage-apps/assign-user-or-group-access-portal.md)
#### [Markalamayı özelleştirme](manage-apps/change-name-or-logo-portal.md)
#### [Kullanıcılar için oturum açmayı devre dışı bırakma](manage-apps/disable-user-sign-in-portal.md)
#### [Kullanıcıları kaldırma](manage-apps/remove-user-or-group-access-portal.md)

#### [Kullanıcı hesabı hazırlamayı yönetme](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [SAML uygulamaları için gelişmiş sertifika imzalama](manage-apps/certificate-signing-options.md)
#### [Uygulamayı kullanıcı deneyiminden gizleme](manage-apps/hide-application-from-user-portal.md)
### [HRD İlkesi'ni kullanarak Oturum Açma için Otomatik Hızlandırmayı Yapılandırma](manage-apps/configure-authentication-for-federated-users-portal.md)
### [AD FS uygulamalarını Azure AD’ye geçirme](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md)
#### [SSO erişimi](manage-apps/what-is-single-sign-on.md)
#### [SSO için sertifikalar](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Kiracı kısıtlamaları](manage-apps/tenant-restrictions.md)
#### [SCIM kullanıcı sağlama hizmetinden yararlanma](manage-apps/use-scim-to-provision-users-and-groups.md)

### [Azure AD uygulaması onay deneyimlerini anlama](application-consent-experience.md)

### Sorun giderme



#### Erişim Paneli
##### [Uygulama görüntülenmiyor](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [Beklenmeyen uygulama görüntüleniyor](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [Oturum açılamıyor](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [Tarayıcı uzantısı yüklenirken hata oluşuyor](manage-apps/access-panel-extension-problem-installing.md)
##### [Self servis uygulama erişiminin kullanma](manage-apps/access-panel-manage-self-service-access.md)
##### [Self servis uygulama erişiminin kullanırken hata oluşuyor](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### Uygulama ekleme
##### [Uygulama türünü seçme](manage-apps/choose-application-type.md)
##### [Yaygın sorunlar - Galeri uygulamaları](manage-apps/adding-gallery-app-common-problems.md)
##### [Yaygın sorunlar - Galeri dışındaki uygulamalar](manage-apps/adding-non-gallery-app-common-problems.md)

#### Uygulama Ara Sunucusu
##### [Uygulama sayfası görüntülenirken sorun oluşuyor](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [Uygulamanın yüklenmesi çok uzun sürüyor](manage-apps/application-proxy-page-load-speed-problem.md)
##### [Uygulama sayfasındaki bağlantılar çalışmıyor](manage-apps/application-proxy-page-links-broken-problem.md)
##### [Uygulamam için açılacak bağlantı noktaları](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı yok](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [Azure portalda yapılandırma](manage-apps/application-proxy-config-how-to.md)
##### [Uygulamamda çoklu oturum açmayı yapılandırma](manage-apps/application-proxy-config-sso-how-to.md)
##### [Yönetici portalında uygulama oluştururken sorun oluşuyor](manage-apps/application-proxy-config-problem.md)
##### [Kerberos Kısıtlanmış Temsilini Yapılandırma](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [PingAccess ile yapılandırma](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### ["Bu Şirket Uygulamasına Erişilemiyor" hatası](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor](manage-apps/application-proxy-connector-installation-problem.md)


#### Uygulama kaydı
##### [Uygulama nesnesi için alan girme](develop/registration-config-specific-application-property-how-to.md)
##### [Belirteç ömrü varsayılanlarını değiştirme](develop/registration-config-change-token-lifetime-how-to.md)

#### Kimlik Doğrulaması
##### [Uç noktaları yapılandırma](develop/registration-config-how-to.md)

#### Koşullu Erişim
##### [Müşteri Cihaz Kaydı önkoşullarına uymadı](active-directory-conditional-access.md)
##### [Koşullu Erişim ilkeleri yanlış ayarlandığından kiracı engelleniyor](active-directory-conditional-access-device-remediation.md)
##### [Kapalı Corpnet kuralları nasıl ve ne zaman geçerlik kazanır?](https://aka.ms/calocation)
##### [Azure AD’de kullanıcının kaydetmesine izin verilen cihaz sayısını artırma](active-directory-azureadjoin-setup.md)
##### [Exchange Online için Koşullu Erişimi Ayarlama](https://aka.ms/csforexchange)
##### [Windows 7 cihazları için Koşullu Erişimi ayarlama](active-directory-conditional-access.md#device-based-conditional-access)
##### [Koşullu erişim ile desteklenen uygulamalar](active-directory-conditional-access-supported-apps.md)

#### API bulma
##### [API bulma](develop/api-find-an-api-how-to.md)

#### Erişimi yönetme
##### [Uygulamaya kullanıcı ve grup atama](manage-apps/methods-for-assigning-users-and-groups.md)
##### [Kullanıcıların bir uygulamaya erişimini kaldırma](manage-apps/methods-for-removing-user-access.md)
##### [Self servis uygulama atamasını yapılandırma](manage-apps/manage-self-service-access.md)
##### [Beklenmeyen kullanıcı ataması](manage-apps/ways-users-get-assigned-to-applications.md)
##### [Uygulamalar listesinde beklenmeyen uygulama](manage-apps/application-types.md)

#### Çok kiracılı uygulamalar
##### [Yeni bir uygulama yapılandırma](develop/setup-multi-tenant-app.md)
##### [Uygulama galerisine ekleme](develop/registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### İzinler
##### [API için izinleri seçme](develop/perms-for-given-api.md)
##### [Uygulamama izin verme](develop/registration-config-grant-permissions-how-to.md)
##### [Temsilci izinleri ile uygulama izinlerini karşılaştırması](develop/delegated-and-app-perms.md)
##### [Uygulama onayı](develop/consent-framework.md)

#### Sağlama
##### [Gereken süre](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Saatler sürüyor - Galeri uygulaması](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [Kullanıcı sağlamayı yapılandırma - galeri uygulaması](manage-apps/application-provisioning-config-how-to.md)
##### [Kullanıcı sağlamayı yapılandırma sorunu - galeri uygulaması](manage-apps/application-provisioning-config-problem.md)
##### [Kullanıcı sağlama galeri uygulamasını yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [Kullanıcılar sağlanmıyor - galeri uygulaması](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [Yanlış kullanıcılar sağlanıyor - galeri uygulaması](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)

#### Çoklu oturum açma
##### [Yöntem seçme](manage-apps/single-sign-on-modes.md)
##### [Yapılandırma](develop/registration-config-sso-how-to.md)
##### [Federasyon yapılandırma - galeri uygulamaları](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [Federasyon yapılandırmayla ilgili yaygın sorunlar - galeri uygulamaları](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [Federasyon yapılandırma - galeri dışındaki uygulamalar](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [Federasyon yapılandırmayla ilgili yaygın sorunlar - galeri dışındaki uygulamalar](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [Parola yapılandırma - galeri uygulamaları](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [Parola yapılandırmayla ilgili yaygın sorunlar - galeri uygulamaları](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [Parola yapılandırma - galeri dışındaki uygulamalar](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [Parola yapılandırmayla ilgili yaygın sorunlar - galeri dışındaki uygulamalar](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### Kullanıcı oturumu açma sorunları
##### [Beklenmeyen izin alma istemi](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [Kullanıcı onayı hatası](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [Özel portaldan oturum açma sorunları](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [Özel bölmeden oturum açma sorunları](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [Uygulama oturum açma sayfasında hata](manage-apps/application-sign-in-problem-application-error.md)
##### [Parolayla çoklu oturum açma sorunu - galeri dışındaki uygulama](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [Parolayla çoklu oturum açma sorunu - galeri uygulaması](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [Microsoft uygulamasında oturum açma sorunu](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [Federasyonda çoklu oturum açma sorunu - galeri dışındaki uygulama](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [Federasyonda çoklu oturum açma sorunu - galeri uygulaması](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [Özel geliştirilmiş uygulama ile ilgili sorun](manage-apps/application-sign-in-problem-custom-dev.md)
##### [Şirket içi uygulamayla ilgili sorun - Uygulama Ara Sunucusu](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)

### [Uygulama geliştirme](active-directory-applications-guiding-developers-for-lob-applications.md)


## Dizininizi yönetme
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Özel etki alanı adları
#### [Hızlı Başlangıç](fundamentals/add-custom-domain.md)
### [Dizininizi yönetme](fundamentals/active-directory-administer.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Etkinleştirme](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Grup ilkesi ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10 ayarları](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Azure AD Connect kullanarak şirket içi kimlikleri tümleştirme](./connect/active-directory-aadconnect.md)

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

## [Kullanım koşulları](active-directory-tou.md)

## Kimliklerinizi güvenli hale getirme

### Azure AD Kimlik Koruması
#### [Genel Bakış](identity-protection/overview.md)
#### [Etkinleştirme](identity-protection/enable.md)
#### [Güvenlik açıklarını algılama](identity-protection/vulnerabilities.md)
#### [Risk olayları](active-directory-identity-protection-risk-events.md)
#### [Bildirimler](identity-protection/notifications.md)
#### [Oturum açma deneyimi](identity-protection/flows.md)
#### [Risk olaylarının benzetimini gerçekleştirme](identity-protection/playbook.md)
#### [Kullanıcıların engelini kaldırma](identity-protection/howto-unblock-user.md)
#### [Oturum riski algılandığında erişimi engelleme](identity-protection/quickstart-sign-in-risk-policy.md)
#### [SSS](identity-protection/faqs.md)
#### [Sözlük](identity-protection/glossary.md)
#### [Microsoft Graph](identity-protection/graph-get-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

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
