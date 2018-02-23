# Genel Bakış
## [Azure Active Directory nedir?](active-directory-whatis.md)
## [Azure kimlik yönetimi hakkında](identity-fundamentals.md)
## [Azure kimlik çözümlerini anlama](understand-azure-identity-solutions.md)
## [Karma kimlik çözümü seçin](choose-hybrid-identity-solution.md)
## [Azure aboneliklerini ilişkilendirme](active-directory-how-subscriptions-associated-directory.md)
## [SSS](active-directory-faq.md)
## [Yenilikler](whats-new.md)


# başlarken
## [Azure AD’yi kullanmaya başlama](get-started-azure-ad.md)
## [Azure AD Premium’a kaydolun](active-directory-get-started-premium.md)
## [Özel bir etki alanı adı ekleme](add-custom-domain.md)
## [Şirket markası yapılandırma](customize-branding.md)
## [Kullanıcıları Azure AD’ye ekleme](add-users-azure-active-directory.md)
## [Kullanıcılara lisans atama](license-users-groups.md)
## [Self servis parola sıfırlamasını yapılandırma](active-directory-passwords-getting-started.md)


# Nasıl yapılır
## Planlama ve tasarım
### [Azure AD mimarisini anlama](active-directory-architecture.md)
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
### [Azure AD’ye yeni kullanıcı ekleme](add-users-azure-active-directory.md)
### [Kullanıcı profillerini yönetme](active-directory-users-profile-azure-portal.md)
### [Hesapları paylaşma](active-directory-sharing-accounts.md)
### [Kullanıcıları yönetici rollerine atama](active-directory-users-assign-role-azure-portal.md)
### [Silinen bir kullanıcıyı geri yükleme](active-directory-users-restore.md)
### [Başka bir dizinden konuk kullanıcılar ekleme (B2B)](active-directory-b2b-what-is-azure-ad-b2b.md)
#### [B2B kullanıcıları ekleyen yöneticiler](active-directory-b2b-admin-add-users.md)
#### [B2B kullanıcıları ekleyen bilgi çalışanları](active-directory-b2b-iw-add-users.md)
#### [API ve özelleştirme](active-directory-b2b-api.md)
#### [Kod ve Azure PowerShell örnekleri](active-directory-b2b-code-samples.md)
#### [Self servis kaydolma portal örneği](active-directory-b2b-self-service-portal.md)
#### [Davet e-postası](active-directory-b2b-invitation-email.md)
#### [Davet kullanma](active-directory-b2b-redemption-experience.md)
#### [Davetiye olmadan B2B kullanıcıları ekleme](active-directory-b2b-add-user-without-invite.md)
#### [B2B için koşullu erişim](active-directory-b2b-mfa-instructions.md)
#### [B2B paylaşım ilkeleri](active-directory-b2b-delegate-invitations.md)
#### [Bir role B2B kullanıcısı ekleme](active-directory-b2b-add-guest-to-role.md)
#### [Dinamik gruplar ve B2B kullanıcıları](active-directory-b2b-dynamic-groups.md)
#### [Denetim ve raporlar](active-directory-b2b-auditing-and-reporting.md)
#### [Hibrit kuruluşlar için B2B](active-directory-b2b-hybrid-organizations.md)
#### [B2B ve Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
#### [B2B lisansı](active-directory-b2b-licensing.md)
#### [Geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
#### [SSS](active-directory-b2b-faq.md)
#### [B2B sorunlarını giderme](active-directory-b2b-troubleshooting.md)
#### [B2B kullanıcısını anlama](active-directory-b2b-user-properties.md)
#### [B2B kullanıcı belirteci](active-directory-b2b-user-token.md)
#### [Azure AD tümleşik uygulamaları için B2B](active-directory-b2b-configure-saas-apps.md)
#### [B2B kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
#### [B2B işbirliğini B2C ile karşılaştırma](active-directory-b2b-compare-b2c.md)
#### [B2B desteği alma](active-directory-b2b-support.md)

## [Grupları ve üyeleri yönetme](active-directory-manage-groups.md)
### Grupları yönetme
#### [Azure Portal](active-directory-groups-create-azure-portal.md)
#### [Azure PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
### [Grup üyelerini yönetme](active-directory-groups-members-azure-portal.md)
### [Grup sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md)
### [Grup üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)
### [Grupları kullanarak lisansları atama](active-directory-licensing-whatis-azure-portal.md)
#### [Bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
#### [Bir gruptaki lisans sorunlarını tanımlama ve çözme](active-directory-licensing-group-problem-resolution-azure-portal.md)
#### [Tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](active-directory-licensing-group-migration-azure-portal.md)
#### [Kullanıcıları ürün lisansları arasında geçirme](active-directory-licensing-group-product-migration.md)
#### [Grup tabanlı lisanslama için ek senaryolar](active-directory-licensing-group-advanced.md)
#### [Grup tabanlı lisanslama için Azure PowerShell örnekleri](active-directory-licensing-ps-examples.md)
#### [Azure AD’de ürünler ve hizmet planları için başvurular](active-directory-licensing-product-and-service-plan-reference.md)
### [Office 365 gruplarının süre sonlarını ayarlama](active-directory-groups-lifecycle-azure-portal.md)
### [Gruplar için bir adlandırma ilkesini zorlama](groups-naming-policy.md)
### [Tüm grupları görüntüleme](active-directory-groups-view-azure-portal.md)
### [SaaS uygulamalarına grup erişimi ekleme](active-directory-accessmanagement-group-saasapps.md)
### [Silinen bir Office 365 grubunu geri yükleme](active-directory-groups-restore-azure-portal.md)
### Grup ayarlarını yönetme
#### [Azure portal](active-directory-groups-settings-azure-portal.md)
#### [Cmdlet’ler](active-directory-accessmanagement-groups-settings-cmdlets.md)
### Gelişmiş kurallar oluşturma
#### [Azure Portal](active-directory-groups-dynamic-membership-azure-portal.md)
### [Self servis gruplarını kurma](active-directory-accessmanagement-self-service-group-management.md)
### [Sorun giderme](active-directory-accessmanagement-troubleshooting.md)

## [Raporları yönetme](active-directory-reporting-azure-portal.md)
### [Oturum açma etkinliği](active-directory-reporting-activity-sign-ins.md)
### [Denetim etkinliği](active-directory-reporting-activity-audit-logs.md)
### [Risk altındaki kullanıcılar](active-directory-reporting-security-user-at-risk.md)
### [Riskli oturum açma işlemleri](active-directory-reporting-security-risky-sign-ins.md)
### [Risk olayları](active-directory-reporting-risk-events.md)
### [SSS](active-directory-reporting-faq.md)
### Görevler
#### [Adlandırılmış konumları yapılandırma](active-directory-named-locations.md)
#### [Etkinlik raporlarını bulma](active-directory-reporting-migration.md)
#### [Azure Active Directory Power BI İçerik Paketi’ni kullanma](active-directory-reporting-power-bi-content-pack-how-to.md)
### Başvuru
#### [Bekletme](active-directory-reporting-retention.md)
#### [Gecikmeler](active-directory-reporting-latencies-azure-portal.md)
#### [Bildirimler](active-directory-reporting-notifications.md)
#### [Oturum açma etkinliği hata kodları](active-directory-reporting-activity-sign-ins-errors.md)
#### [Multi-Factor Authentication](active-directory-reporting-activity-sign-ins-mfa.md)
### Sorun giderme
#### [Eksik denetim verileri](active-directory-reporting-troubleshoot-missing-audit-data.md)
#### [İndirmelerde eksik veriler](active-directory-reporting-troubleshoot-missing-data-download.md)
#### [Azure Active Directory Etkinlik günlükleri içerik paketi hataları](active-directory-reporting-troubleshoot-content-pack.md)
### [Programlı Erişim](active-directory-reporting-api-getting-started-azure-portal.md)
#### [Denetim başvurusu](active-directory-reporting-api-audit-reference.md)
#### [Oturum açma başvurusu](active-directory-reporting-api-sign-in-activity-reference.md)
#### [Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md)
#### [Denetim örnekleri](active-directory-reporting-api-audit-samples.md)
#### [Oturum açma örnekleri](active-directory-reporting-api-sign-in-activity-samples.md)
#### [Sertifikaları kullanma](active-directory-reporting-api-with-certificates.md)

## Parolaları yönetme
### [Parolalara genel bakış](active-directory-passwords-overview.md)
### Kullanıcı belgeleri
#### [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
#### [Parolalarla ilgili en iyi yöntemler](active-directory-secure-passwords.md)
#### [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
### [SSPR Nasıl çalışır?](active-directory-passwords-how-it-works.md)
### [SSPR Dağıtım kılavuzu](active-directory-passwords-best-practices.md)
### [SSPR ve Windows 10](active-directory-passwords-login.md)
### [SSPR İlkeleri ](active-directory-passwords-policy.md)
### [SSPR’yi Özelleştirme](active-directory-passwords-customize.md)
### [SSPR Veri gereksinimleri](active-directory-passwords-data.md)
### [SSPR Raporlama](active-directory-passwords-reporting.md)
### BT Yöneticileri: Parolaları sıfırlama
#### [Azure Portal](active-directory-users-reset-password-azure-portal.md)
### [SSPR lisanslama](active-directory-passwords-licensing.md)
### [Parola geri yazma](active-directory-passwords-writeback.md)
### [Sorun giderme](active-directory-passwords-troubleshoot.md)
### [SSS](active-directory-passwords-faq.md)


## Cihazları yönetme
### [Giriş](device-management-introduction.md)
### [Azure portalını kullanma](device-management-azure-portal.md)
### [Azure AD'ye Katılım’ı Planlama](active-directory-azureadjoin-deployment-aadjoindirect.md)
### [SSS](device-management-faq.md)
### Görevler
#### [Azure AD alanında kayıtlı Windows 10 cihazlarını ayarlama](device-management-azuread-registered-devices-windows10-setup.md)
#### [Azure AD alanına katılmış cihazları ayarlama](device-management-azuread-joined-devices-setup.md)
#### [Karma Azure AD alanına katılmış cihazları ayarlama](device-management-hybrid-azuread-joined-devices-setup.md)
#### [Şirket içi dağıtma](active-directory-device-registration-on-premises-setup.md)
#### [Azure AD katılımı sırasında Windows 10’u ilk kez çalıştırma deneyimi](device-management-azuread-joined-devices-frx.md)
### Sorun giderme
#### [Karma Azure AD alanına katılmış Windows 10 ve Windows Server 2016 cihazları](device-management-troubleshoot-hybrid-join-windows-current.md)
#### [Karma Azure AD alanına katılmış eski Windows cihazları](device-management-troubleshoot-hybrid-join-windows-legacy.md)

## Uygulamaları yönetme
### [Genel Bakış](active-directory-enable-sso-scenario.md)
### [Başlarken](active-directory-integrating-applications-getting-started.md)
### [SaaS uygulama tümleştirmesi öğreticileri](active-directory-saas-tutorial-list.md)
### [Cloud App Discovery](cloudappdiscovery-get-started.md)
#### [Anlık görüntü raporları oluşturma](cloudappdiscovery-set-up-snapshots.md)
#### [Sürekli raporlamayı yapılandırma](https://docs.microsoft.com/cloud-app-security/discovery-docker)
#### [Özel günlük ayrıştırıcı kullanma](https://docs.microsoft.com/cloud-app-security/custom-log-parser)
#### Aracı tabanlı bulma
##### [Cloud App Discovery nedir?](active-directory-cloudappdiscovery-whatis.md)
##### [Kayıt defteri ayarlarını güncelleştirme](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
##### [Güvenlik ve gizliliği anlama](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)

### [SaaS uygulamalarına kullanıcı hazırlama ve hazırlamayı kaldırma](active-directory-saas-app-provisioning.md)
#### [Uygulama tümleştirmesi öğreticileri](active-directory-saas-tutorial-list.md)
#### [SCIM etkin uygulamalara otomatik sağlama](active-directory-scim-provisioning.md)
#### [Öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
#### [Öznitelik eşlemeleri için ifadeler yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
#### [Kapsam belirleme filtrelerini kullanma](active-directory-saas-scoping-filters.md)
#### [Otomatik kullanıcı hazırlama raporu](active-directory-saas-provisioning-reporting.md)
#### [Kullanıcı hazırlama sorunlarını giderme](active-directory-application-provisioning-content-map.md)



### [Uygulama Proxy’si ile uygulamalara uzaktan erişme](active-directory-application-proxy-get-started.md)
#### başlarken
##### [Uygulama Ara Sunucusu](active-directory-application-proxy-enable.md)
##### [Uygulamaları yayımlama](application-proxy-publish-azure-portal.md)
##### [Özel etki alanları](active-directory-application-proxy-custom-domains.md)
#### [Çoklu oturum açma](application-proxy-sso-overview.md)
##### [KCD ile SSO](active-directory-application-proxy-sso-using-kcd.md)
##### [Üst bilgi içeren SSO](application-proxy-ping-access.md)
##### [Parola kasası oluşturma ile SSO](application-proxy-sso-azure-portal.md)
#### Kavramlar
##### [Bağlayıcılar](application-proxy-understand-connectors.md)
##### [Güvenlik](application-proxy-security-considerations.md)
##### [Ağlar](application-proxy-network-topology-considerations.md)
##### [TMG veya UAG’den yükseltme](application-proxy-transition-from-uag-tmg.md)

#### Gelişmiş yapılandırmalar
##### [Ayrı ağlarda yayımlama](active-directory-application-proxy-connectors-azure-portal.md)
##### [Ara sunucular](application-proxy-working-with-proxy-servers.md)
##### [Talep kullanan uygulamalar](active-directory-application-proxy-claims-aware-apps.md)
##### [Native Client uygulamaları](active-directory-application-proxy-native-client.md)
##### [Sessiz yükleme](active-directory-application-proxy-silent-installation.md)
##### [Özel giriş sayfası](application-proxy-office365-app-launcher.md)
##### [Satır içi bağlantıları çevirme](application-proxy-link-translation.md)
##### [Joker uygulamalar](active-directory-application-proxy-wildcard.md)

#### Adım adım kılavuzlar yayımlama
##### [Uzak Masaüstü](application-proxy-publish-remote-desktop.md)
##### [SharePoint](application-proxy-enable-remote-access-sharepoint.md)
##### [Microsoft Teams](application-proxy-teams.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread)
#### [Sorun giderme](active-directory-application-proxy-troubleshoot.md)


### Kurumsal uygulamaları yönetme
#### [Kullanıcıları atama](active-directory-coreapps-assign-user-azure-portal.md)
#### [Markalamayı özelleştirme](active-directory-coreapps-change-app-logo-user-azure-portal.md)
#### [Kullanıcılar için oturum açmayı devre dışı bırakma](active-directory-coreapps-disable-app-azure-portal.md)
#### [Kullanıcıları kaldırma](active-directory-coreapps-remove-assignment-azure-portal.md)
#### [Tüm uygulamalarımı görüntüleme](active-directory-coreapps-view-azure-portal.md)
#### [Kullanıcı hesabı hazırlamayı yönetme](active-directory-enterprise-apps-manage-provisioning.md)
#### [Kurumsal uygulamalar için çoklu oturum açmayı yönetme](active-directory-enterprise-apps-manage-sso.md)
#### [SAML uygulamaları için gelişmiş sertifika imzalama](active-directory-enterprise-apps-advance-certificate-options.md)
#### [Uygulamayı kullanıcı deneyiminden gizleme](active-directory-coreapps-hide-third-party-app.md)
### [HRD İlkesi'ni kullanarak Oturum Açma için Otomatik Hızlandırmayı Yapılandırma](active-directory-auto-acceleration-using-hrd.md)
### [AD FS uygulamalarını Azure AD’ye geçirme](migrate-adfs-apps-to-azure.md)
### [Uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md)
#### [SSO erişimi](active-directory-appssoaccess-whatis.md)
#### [SSO için sertifikalar](active-directory-sso-certs.md)
#### [Kiracı kısıtlamaları](active-directory-tenant-restrictions.md)
#### [SCIM kullanıcı sağlama hizmetinden yararlanma](active-directory-scim-provisioning.md)

### [Sorun giderme](active-directory-application-troubleshoot-content-map.md)
#### [Uygulama Geliştirme](active-directory-application-dev-troubleshoot-content-map.md)
##### [Yapılandırma ve Kayıt](active-directory-application-dev-config-content-map.md)
##### [Geliştirme](active-directory-application-dev-development-content-map.md)
#### [Uygulama Yönetimi](active-directory-application-management-troubleshoot-content-map.md)
##### [Yapılandırma](active-directory-application-config-content-map.md)
##### [Oturum açma](active-directory-application-sign-in-content-map.md)
##### [Sağlama](active-directory-application-provisioning-content-map.md)
##### [Erişimi yönetme](active-directory-application-access-content-map.md)
##### [Erişim Paneli](active-directory-application-access-panel-content-map.md)
##### [Uygulama Proxy’si](active-directory-application-proxy-content-map.md)
##### [Koşullu erişim](active-directory-application-conditional-access-content-map.md)
### [Uygulama geliştirme](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Belge kitaplığı](active-directory-apps-index.md)

## Dizininizi yönetme
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Özel etki alanı adları
#### [Hızlı Başlangıç](add-custom-domain.md)
#### [Özel etki alanı adı ekleme](active-directory-domains-manage-azure-portal.md)
### [Dizininizi yönetme](active-directory-administer.md)
### [Birden fazla dizin](active-directory-licensing-directory-independence.md)
### [Self servise kaydolma](active-directory-self-service-signup.md)
### [Yönetilmeyen bir dizini devralma](domains-admin-takeover.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Etkinleştirme](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Grup ilkesi ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Windows 10 ayarları](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Azure AD Connect kullanarak şirket içi kimlikleri tümleştirme](./connect/active-directory-aadconnect.md)

## [Azure’a erişimi yönetme](toc.yml)

## Kaynaklara temsilci erişimi
### [Yönetici rolleri](active-directory-assign-admin-roles-azure-portal.md)
#### [Kullanıcıya yönetici rolü atama](active-directory-users-assign-role-azure-portal.md)
#### [Üye ve konuk kullanıcı izinlerini karşılaştırma](users-default-permissions.md)
### [Yönetim birimleri](active-directory-administrative-units-management.md)
### [Belirteç ömrünü yapılandırma](active-directory-configurable-token-lifetimes.md)
### [Acil durum erişimi yönetici hesaplarını yönetin](active-directory-admin-manage-emergency-access-accounts.md)

## Erişim gözden geçirmeleri
### [Erişim gözden geçirmelerine genel bakış](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Erişim gözden geçirmesi tamamlama](active-directory-azure-ad-controls-complete-access-review.md)
### [Erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
### [Erişim değerlendirmesi gerçekleştirme](active-directory-azure-ad-controls-perform-access-review.md)
### [Erişiminizi gözden geçirme](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Erişim gözden geçirmesi ile konuk erişimi](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Gözden geçirmelerle kullanıcı erişimini yönetme](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Programları ve denetimleri yönetme](active-directory-azure-ad-controls-manage-programs-controls.md)


## Kimliklerinizi güvenli hale getirme
### [Koşullu erişim](active-directory-conditional-access-azure-portal.md)
#### [Koşullar](active-directory-conditional-access-conditions.md)
#### [Konum koşulu](active-directory-conditional-access-locations.md)
#### [Denetimler](active-directory-conditional-access-controls.md)
#### [Kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md)
#### [En iyi uygulamalar](active-directory-conditional-access-best-practices.md)
#### [Office 365 hizmetleri için cihaz ilkelerini anlama](active-directory-conditional-access-device-policies.md)
#### [Klasik ilkeleri geçirme](active-directory-conditional-access-migration.md)
#### [Durum aracı](active-directory-conditional-access-whatif.md)
#### Görevler
##### [Klasik MFA ilkesini geçirme](active-directory-conditional-access-migration-mfa.md)
##### [Cihaz tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-policy-connected-applications.md)
##### [Uygulama tabanlı koşullu erişimi ayarlama](active-directory-conditional-access-mam.md)
##### [Kullanıcılar ve uygulamalar için kullanım koşullarını sağlayın](active-directory-tou.md)
##### [VPN bağlantısı ayarlama](active-directory-conditional-access-vpn-connectivity-windows10.md)
##### [SharePoint ve Exchange Online ayarlama](active-directory-conditional-access-no-modern-authentication.md)
##### [Düzeltme](active-directory-conditional-access-device-remediation.md)
#### [Teknik başvuru](active-directory-conditional-access-technical-reference.md)
#### [SSS](active-directory-conditional-faqs.md)

### Windows Hello
#### [Parolasız kimlik doğrulama](active-directory-azureadjoin-passport.md)
#### [İş İçin Windows Hello’yu etkinleştirme](active-directory-azureadjoin-passport-deployment.md)
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
### [Privileged Identity Management](./privileged-identity-management/active-directory-securing-privileged-access.md)

## Diğer hizmetleri Azure AD ile tümleştirme
### [LinkedIn’i Azure AD ile tümleştirme](linkedin-integration.md)

## [Azure VM’lerinde AD DS dağıtma](virtual-networks-windows-server-active-directory-virtual-machines.md)
### [Azure VM’lerinde Windows Server Active Directory](active-directory-deploying-ws-ad-guidelines.md)
### [Azure Sanal Ağında kopya etki alanı denetleyicisi](active-directory-install-replica-active-directory-domain-controller.md)
### [Azure Sanal Ağında yeni orman](active-directory-new-forest-virtual-machine.md)



## [Azure’a AD FS dağıtma](active-directory-aadconnect-azure-adfs.md)
### [Yüksek kullanılabilirlik](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Değişiklik imzası karma algoritması](active-directory-federation-sha256-guidance.md)

## [Sorun giderme](active-directory-troubleshooting-support-howto.md)
### [Active Directory sorun giderme öğesi eksik ya da mevcut değil](active-directory-troubleshooting.md)

## Azure AD Kavram Kanıtı (PoC) Dağıtma
### [PoC El Kitabı: Giriş](active-directory-playbook-intro.md)
### [PoC El Kitabı: Malzemeler](active-directory-playbook-ingredients.md)
### [PoC El Kitabı: Uygulama](active-directory-playbook-implementation.md)
### [PoC El Kitabı: Yapı Taşları](active-directory-playbook-building-blocks.md)


# Başvuru
## [Kod örnekleri](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory)
## [Azure PowerShell cmdlet'leri](/powershell/azure/overview)
## [Java API Başvurusu](/java/api)
## [.NET API’si](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)
## [Hizmet sınırlamaları ve kısıtlamalar](active-directory-service-limits-restrictions.md)

# İlgili
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Geliştiriciler için Azure AD](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/active-directory-securing-privileged-access.md)

# Kaynaklar
## [Azure geri bildirim forumu](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=security-identity)
## [MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
