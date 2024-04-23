0.1.3 was mostly bug fixes. The notable thing for users running upgrading from old versions in 0.1.4 is user management, and the addition and removal of some settings parameters:

### User Management ###

Public registration is now removed in the open source version. It is a significant security risk to allow public access to a LaTeX installation on your server, and most users have request some for of private user management. See [[Creating and managing users]] for an overview of how it now works.

### Added settings ###

* `appName` - This should be set to the name of your ShareLaTeX install. E.g. "Acme Inc's ShareLaTeX Server".
* `adminEmail` - The contact email address of whoever is responsible for running the server.

### Deprecated settings ###

* `websocketsUrl` - This was the source of most problems with 0.1.2 and 0.1.3 so has been removed. The web service now proxies to the real-time service so the client does not need to know where to find it.