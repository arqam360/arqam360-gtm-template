# Arqam360 Consent Management Platform: Google Tag Manager template

A Google Tag Manager tag template that loads the [Arqam360](https://arqam360.com)
cookie consent banner and sets Google Consent Mode v2 for you.

- Sets the default consent state with Tag Manager's consent API, for all
  visitors and per region (ISO 3166-2), with `wait_for_update`.
- Applies a returning visitor's stored choice from the Arqam360 consent cookie
  as soon as the tag fires.
- Loads the banner from `https://cdn.arqam360.com/widget-v2.js` and applies
  every later choice through `updateConsentState`.
- Optional `ads_data_redaction` and `url_passthrough`.

## Install

1. In Google Tag Manager, open **Templates → Tag Templates → Search Gallery**,
   search for **Arqam360** and click **Add to workspace**.
   (Or download `template.tpl` from this repository and use
   **Templates → Tag Templates → New → ⋮ → Import**.)
2. Go to **Tags → New → Tag Configuration** and choose
   **Arqam360 Consent Management Platform**.
3. Paste your public site key (`ciq_live_…`, from **Settings → API Keys** in
   your Arqam360 dashboard). Never paste a secret key (`ciq_sk_…`).
4. Review the default consent for all visitors, and add region rows if you
   need different defaults for some regions.
5. Set the trigger to **Consent Initialization - All Pages**.
6. Remove any other Arqam360 script tag from your site, so the banner loads
   once. Then preview and publish.

Full guide: https://arqam360.com/help/installation/google-tag-manager-consent-mode

## How consent updates reach Tag Manager

Before it injects the banner, the template registers
`window.arqam360GtmConsentUpdate` with `setInWindow`. The banner calls it on
every consent change with a Google consent-type object
(`ad_storage`, `ad_user_data`, `ad_personalization`, `analytics_storage`,
`functionality_storage`, `personalization_storage`, `security_storage`), and
the template applies it with `updateConsentState`. Only those seven types with
the values `granted` or `denied` are accepted. When the function is present
the banner does not push its own `gtag('consent', 'default', …)`, so the
template's defaults (including per-region rows) are the only ones.

| Arqam360 category | Google consent types |
| --- | --- |
| Marketing | `ad_storage`, `ad_user_data`, `ad_personalization` |
| Analytics | `analytics_storage` |
| Preferences | `personalization_storage` |
| Necessary (always on) | `functionality_storage`, `security_storage` |

## Permissions

| Permission | Scope |
| --- | --- |
| Injects scripts | `https://cdn.arqam360.com/*` |
| Accesses consent state | write: the seven consent types above |
| Reads cookie values | `consentiq_consent` |
| Accesses global variables | write: `arqam360GtmConsentUpdate` |
| Writes to the data layer | `ads_data_redaction`, `url_passthrough` |
| Logs to console | debug mode only |

## Support

admin@arqam360.com · https://arqam360.com/contact

## License

Apache 2.0. See [LICENSE](LICENSE).
