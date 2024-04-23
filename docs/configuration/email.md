<style>
table th:first-of-type {
    width: 40%;
    word-break: break-all;
}
table th:nth-of-type(2) {
    width: 60%;
}
</style>

Overleaf supports sending email through two methods: Simple Mail Transfer Protocol (SMTP) and Amazon Simple Email Service (SES). SMTP can be used if you have an email server enabled on your localhost that is listening for local connections.

## Configure ##

Email can be configured via environmental variables passed to the Docker container.

### Sender Configuration ###

| Name | Description |
|------|-------------|
| `OVERLEAF_EMAIL_FROM_ADDRESS` | The from address e.g. `'support@mycompany.com'` <br /><br />- **Required**: yes |
| `OVERLEAF_EMAIL_REPLY_TO` | The reply to address e.g. `'noreply@mycompany.com'` |

### SMTP ###

| Name | Description |
|------|-------------|
| `OVERLEAF_EMAIL_SMTP_HOST` | The hostname or IP address to connect to. Needs to be accessible from the Docker container |
| `OVERLEAF_EMAIL_SMTP_PORT` | The port to connect to |
| `OVERLEAF_EMAIL_SMTP_SECURE` | If `true` the connection will use TLS when connecting to server. If `false` or not set, then TLS is used if server supports the `STARTTLS` extension. In most cases set this value to `true` if you are connecting to port `465`. For port `587` or `25` keep it `false` |
| `OVERLEAF_EMAIL_SMTP_USER` | The username that should be used to authenticate against the SMTP server |
| `OVERLEAF_EMAIL_SMTP_PASS` | The password associated with the SMTP username |
| `OVERLEAF_EMAIL_SMTP_TLS_REJECT_UNAUTH` | If 'false' this would open a connection to TLS server with self-signed or invalid TLS certificate |
| `OVERLEAF_EMAIL_SMTP_IGNORE_TLS` | When `true` and `OVERLEAF_EMAIL_SMTP_SECURE` is `false` then TLS is not used even if the server supports `STARTTLS` extension|
| `OVERLEAF_EMAIL_SMTP_NAME` | Optional hostname for TLS validation if `OVERLEAF_EMAIL_SMTP_HOST` was set to an IP address, defaults to hostname of the machine.|
| `OVERLEAF_EMAIL_SMTP_LOGGER` | When `true` prints logging messages to `web.log`.|

### Amazon SES SMTP interface ###

You can read more about using the Amazon SES SMTP interface to send email <a href="https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html">here</a>.

| Name | Description |
|------|-------------|
| `OVERLEAF_EMAIL_SMTP_HOST` | The hostname or IP address to connect to. Needs to be accessible from the Docker container |
| `OVERLEAF_EMAIL_SMTP_PORT` | The port to connect to |
| `OVERLEAF_EMAIL_SMTP_USER` | The username that should be used to authenticate against the SMTP server |
| `OVERLEAF_EMAIL_SMTP_PASS` | The password associated with the SMTP username |

### Amazon SES API ###

You can read more about using the Amazon SES API to send email <a href="https://docs.aws.amazon.com/ses/latest/dg/send-email-api.html">here</a>.

| Name | Description |
|------|-------------|
| `OVERLEAF_EMAIL_AWS_SES_ACCESS_KEY_ID` | If using AWS SES the access key |
| `OVERLEAF_EMAIL_AWS_SES_SECRET_KEY` | If using AWS SES the secret key |
| `OVERLEAF_EMAIL_AWS_SES_REGION` | If not set, the default region is US-EAST-1 |

### AWS SES with Instance Roles ###

| Name | Description |
|------|-------------|
| `OVERLEAF_EMAIL_DRIVER`: | When this is set to `ses`, the email system will use the SES API method and rely on the configured instance roles to send email. |

### Customisation ###

| Name | Description |
|------|-------------|
| `OVERLEAF_CUSTOM_EMAIL_FOOTER` | Custom HTML which is appended to all emails. e.g.<br /><br /> **Example**: `"<div>This system is run by department x </div> <div> If you have any questions please look at our faq <a href='https://somwhere.com'>here</a></div>"` |