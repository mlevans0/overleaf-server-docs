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

## Configure

Email can be configured via environmental variables passed to the Docker container.

### Sender Configuration

| Name | Description |
|------|-------------|
| `SHARELATEX_EMAIL_FROM_ADDRESS` | The from address e.g. `'support@mycompany.com'` <br /><br />- **Required**: yes |
| `SHARELATEX_EMAIL_REPLY_TO` | The reply to address e.g. `'noreply@mycompany.com'` |

### AWS SES

| Name | Description |
|------|-------------|
| `SHARELATEX_EMAIL_AWS_SES_ACCESS_KEY_ID` | If using AWS SES the access key |
| `SHARELATEX_EMAIL_AWS_SES_SECRET_KEY` | If using AWS SES the secret key |

### AWS SES with Instance Roles

| Name | Description |
|------|-------------|
| `SHARELATEX_EMAIL_DRIVER`: | When this is set to `ses`, the email system will rely on the configured instance roles to send email. |

### SMTP

| Name | Description |
|------|-------------|
| `SHARELATEX_EMAIL_SMTP_HOST` | The hostname or IP address to connect to. Needs to be accessible from the Docker container |
| `SHARELATEX_EMAIL_SMTP_PORT` | The port to connect to |
| `SHARELATEX_EMAIL_SMTP_SECURE` | If `true` the connection will use TLS when connecting to server. If `false` or not set, then TLS is used if server supports the `STARTTLS` extension. In most cases set this value to `true` if you are connecting to port `465`. For port `587` or `25` keep it `false` |
| `SHARELATEX_EMAIL_SMTP_USER` | The username that should be used to authenticate against the SMTP server |
| `SHARELATEX_EMAIL_SMTP_PASS` | The password associated with the SMTP username |
| `SHARELATEX_EMAIL_SMTP_TLS_REJECT_UNAUTH` | If 'false' this would open a connection to TLS server with self-signed or invalid TLS certificate |
| `SHARELATEX_EMAIL_SMTP_IGNORE_TLS` | When `true` and `SHARELATEX_EMAIL_SMTP_SECURE` is `false` then TLS is not used even if the server supports `STARTTLS` extension|
| `SHARELATEX_EMAIL_SMTP_NAME` | Optional hostname for TLS validation if `SHARELATEX_EMAIL_SMTP_HOST` was set to an IP address, defaults to hostname of the machine.|
| `SHARELATEX_EMAIL_SMTP_LOGGER` | When `true` prints logging messages to `web.log`.|

#### Customisation

| Name | Description |
|------|-------------|
| `SHARELATEX_CUSTOM_EMAIL_FOOTER` | Custom HTML which is appended to all emails. e.g.<br /><br /> **Example**: `"<div>This system is run by department x </div> <div> If you have any questions please look at our faq <a href='https://somwhere.com'>here</a></div>"` |