---
title: Enabling GitHub Advanced Security for your enterprise
shortTitle: Enabling GitHub Advanced Security
intro: 'You can configure {% data variables.product.product_name %} to include {% data variables.product.prodname_GH_advanced_security %}. This provides extra features that help users find and fix security problems in their code.'
product: '{% data reusables.gated-features.ghas %}'
versions:
  enterprise-server: '>=2.22'
type: how_to
topics:
  - Advanced Security
  - Code scanning
  - Enterprise
  - Secret scanning
  - Security
---

### About enabling {% data variables.product.prodname_GH_advanced_security %}

{% data reusables.advanced-security.ghas-helps-developers %}

{% if currentVersion ver_gt "enterprise-server@3.0" %}
When you enable {% data variables.product.prodname_GH_advanced_security %} for your enterprise, repository administrators in all organizations can enable the features unless you set up a policy to restrict access. For more information, see "[Enforcing policies for {% data variables.product.prodname_advanced_security %} in your enterprise](/admin/policies/enforcing-policies-for-advanced-security-in-your-enterprise)."
{% else %}
When you enable {% data variables.product.prodname_GH_advanced_security %} for your enterprise, repository administrators in all organizations can enable the features. {% if currentVersion == "enterprise-server@3.0" %}詳しい情報については、「[Organization のセキュリティおよび分析設定を管理する](/organizations/keeping-your-organization-secure/managing-security-and-analysis-settings-for-your-organization)」と「[リポジトリのセキュリティと分析設定を管理する](/github/administering-a-repository/managing-security-and-analysis-settings-for-your-repository)」を参照してください。{% endif %}
{% endif %}

### Prerequisites for enabling {% data variables.product.prodname_GH_advanced_security %}

1. Upgrade your license for {% data variables.product.product_name %} to include {% data variables.product.prodname_GH_advanced_security %}.{% if currentVersion ver_gt "enterprise-server@3.0" %} For information about licensing, see "[About licensing for {% data variables.product.prodname_GH_advanced_security %}](/admin/advanced-security/about-licensing-for-github-advanced-security)."{% endif %}
2. Upload the new license to {% data variables.product.product_location %}. For more information, see "[Managing your GitHub Enterprise license](/admin/overview/managing-your-github-enterprise-license#uploading-a-new-license-to-github-enterprise-server)."{% if currentVersion ver_gt "enterprise-server@2.22" %}
3. Review the prerequisites for the features you plan to enable.

    - {% data variables.product.prodname_code_scanning_capc %}, see "[Configuring {% data variables.product.prodname_code_scanning %} for your appliance](/admin/advanced-security/configuring-code-scanning-for-your-appliance#prerequisites-for-code-scanning)."
    - {% data variables.product.prodname_secret_scanning_caps %}, see "[Configuring {% data variables.product.prodname_secret_scanning %} for your appliance](/admin/advanced-security/configuring-secret-scanning-for-your-appliance#prerequisites-for-secret-scanning)."{% endif %}

### Checking whether your license includes {% data variables.product.prodname_GH_advanced_security %}

{% if currentVersion ver_gt "enterprise-server@3.0" %}
{% data reusables.enterprise-accounts.access-enterprise %}
{% data reusables.enterprise-accounts.settings-tab %}
{% data reusables.enterprise-accounts.license-tab %}
1. If your license includes {% data variables.product.prodname_GH_advanced_security %}, the license page includes a section showing details of current usage. ![{% data variables.product.prodname_GH_advanced_security %} section of Enterprise license](/assets/images/help/billing/ghas-orgs-list-enterprise-ghes.png)
{% endif %}

{% if currentVersion == "enterprise-server@2.22" or currentVersion == "enterprise-server@3.0" %}
{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
1. If your license includes {% data variables.product.prodname_GH_advanced_security %}, there is an **{% data variables.product.prodname_advanced_security %}** entry in the left sidebar. ![[Advanced Security] サイドバー](/assets/images/enterprise/management-console/sidebar-advanced-security.png)

{% data reusables.enterprise_management_console.advanced-security-license %}
{% endif %}

### Enabling and disabling {% data variables.product.prodname_GH_advanced_security %} features

{% data reusables.enterprise_management_console.enable-disable-security-features %}

{% data reusables.enterprise_site_admin_settings.access-settings %}
{% data reusables.enterprise_site_admin_settings.management-console %}
{% data reusables.enterprise_management_console.advanced-security-tab %}{% if currentVersion ver_gt "enterprise-server@2.22" %}
1. Under "{% data variables.product.prodname_advanced_security %}," select the features that you want to enable and deselect any features you want to disable. ![Checkbox to enable or disable {% data variables.product.prodname_advanced_security %} features](/assets/images/enterprise/management-console/enable-advanced-security-checkboxes.png){% else %}
1. [{% data variables.product.prodname_advanced_security %}] で、[**{% data variables.product.prodname_code_scanning_capc %}**] をクリックします。 ![Checkbox to enable or disable {% data variables.product.prodname_code_scanning %}](/assets/images/enterprise/management-console/enable-code-scanning-checkbox.png){% endif %}
{% data reusables.enterprise_management_console.save-settings %}

When {% data variables.product.product_name %} has finished restarting, you're ready to set up any additional resources required for newly enabled features. 詳しい情報については「[アプライアンスのための{% data variables.product.prodname_code_scanning %}の設定](/admin/advanced-security/configuring-code-scanning-for-your-appliance)」を参照してください。

### 管理シェル (SSH) を介した {% data variables.product.prodname_GH_advanced_security %} の有効化または無効化

You can enable or disable features programmatically on {% data variables.product.product_location %}. {% data variables.product.prodname_ghe_server %} の管理シェルおよびコマンドラインユーティリティの詳細については、「[管理シェル (SSH) へのアクセス](/admin/configuration/accessing-the-administrative-shell-ssh)」および「[コマンドラインユーティリティ](/admin/configuration/command-line-utilities#ghe-config)」を参照してください。

たとえば、ステージングまたはシステム災害復旧に対してインスタンスをデプロイするときに、infrastructure-as-code ツールを使用して {% data variables.product.prodname_code_scanning %} を有効化できます。

1. {% data variables.product.product_location %}にSSHでアクセスしてください。
1. {% data variables.product.prodname_code_scanning %} を有効化します。
    ```shell
    ghe-config app.minio.enabled true
    ghe-config app.code-scanning.enabled true
    ```
2. 必要に応じて、{% data variables.product.prodname_code_scanning %} を無効化します。
    ```shell
    ghe-config app.minio.enabled false
    ghe-config app.code-scanning.enabled false
    ```
3. 設定を適用します。
    ```shell
  ghe-config-apply
  ```

{% if currentVersion ver_gt "enterprise-server@2.22" %}To enable and disable {% data variables.product.prodname_secret_scanning %} in the same way, set: `ghe-config app.secret-scanning.enabled` true or false and apply the configuration.{% endif %}
