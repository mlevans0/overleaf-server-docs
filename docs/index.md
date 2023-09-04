<h1 align="center">
  <a href="https://www.overleaf.com"><img src="assets/logo.png" alt="Overleaf" width="300"></a>
</h1>

## Welcome

Welcome to the technical documentation for {{ versions['community-edition-full'] }} and [{{ versions['server-pro-full'] }}](https://www.overleaf.com/for/enterprises)! This comprehensive guide is designed to provide you with an in-depth understanding of the features, configuration options, and best practices for managing and utilizing Overleaf's collaborative writing and publishing platform within your own infrastructure.

Overleaf is a powerful tool that facilitates collaborative authoring and publishing of LaTeX documents. The {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} versions allow organizations to host their own instances of Overleaf, providing enhanced control, security, and customization over the platform's capabilities.

Whether you're an administrator setting up Overleaf for your organization or a user looking to leverage its collaborative features for document creation for your lab, this documentation will walk you through the necessary steps to install, configure, and utilize {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} effectively.

## What is the difference between {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }}?

Overleaf has two on-premise solutions - {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }}.

### {{ versions['community-edition-short'] }} 

{{ versions['community-edition-short'] }} is a lite version of our LaTeX editing platform which can be freely [self-installed](https://github.com/overleaf/overleaf) and {{ versions['server-pro-short'] }} is our full, officially supported, premium service. {{ versions['server-pro-short'] }} is an excellent option for enterprise organizations that are looking for the ease and functionality of the Overleaf platform, but require full control of the infrastructure in which the application runs.

### {{ versions['server-pro-short'] }} 

{{ versions['server-pro-short'] }} comes as a Docker container and is a drop in replacement for {{ versions['community-edition-short'] }}. {{ versions['server-pro-short'] }} includes features such as SSO provided via LDAP and SAML2, improved security so users can not access the host machine, tracked changes, comments, our optimized version of TexLive, visual editor mode, templates and admin panel. You can find more information about {{ versions['server-pro-short'] }} for enterprise [over on our website](https://www.overleaf.com/for/enterprises).

!!! info

    We recommend enterprise organizations begin with {{ versions['community-edition-short'] }} to familiarize themselves with the platform's requirements and ensure a smooth experience before considering an upgrade to {{ versions['server-pro-short'] }}.

### Support 

For customers using a self-hosted {{ versions['server-pro-short'] }} instance we will provide reasonable support to assist with the resolution of technical issues relating to the installation, configuration, maintenance and general usage of {{ versions['server-pro-short'] }}, to a single, named staff member.

Support services for {{ versions['server-pro-short'] }} specifically exclude:

- direct support for the LaTeX language including layout, typesetting, and programming errors;
- defects or errors resulting from any modifications to {{ versions['server-pro-short'] }} unless made, instructed, or approved by us in writing;
- support for any version of {{ versions['server-pro-short'] }} other than:
    - the two (2) most current point releases of the current major version (currently {{versions['latest-two-point-releases-of-current-version']}}); and
    - the last released point release of the previous major version (currently {{versions['last-released-point-release-of-previous-major-version']}});
- support for any non-current version of {{ versions['server-pro-short'] }} that is more than twenty-four (24) months old;
- any fault in the environment or in any software or hardware used in conjunction with {{ versions['server-pro-short'] }}; and
- defects or errors caused by the use with any software products other than those specifically certified for use with {{ versions['server-pro-short'] }} in this documentation.

If you have any questions regarding your {{ versions['server-pro-short'] }} instance, please reach out to either your Account Manager directly, or send us a message via our general contact form. 

!!! note

    Please note that we are **not** able to provide support for local installations of the free {{ versions['community-edition-short'] }}.

## Keeping up to date

Stay in the loop with the latest advancements and updates regarding Overleaf Server {{ versions['community-edition-short'] }} and {{ versions['server-pro-short'] }} by signing up to our [mailing list](https://mailchi.mp/overleaf.com/community-edition-and-server-pro). 

By subscribing, you'll receive timely notifications about new features, enhancements, security patches, and valuable insights from the team. Keeping up to date is essential to ensure that you're making the most of your Overleaf server, staying secure, and leveraging the newest functionalities to enhance your collaborative writing and document editing workflows. 