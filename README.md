# Perfect - mod_perfect - Apache 2.4 Connector

[![Perfect logo](http://www.perfect.org/github/Perfect_GH_header_854.jpg)](http://perfect.org/get-involved.html)

[![Perfect logo](http://www.perfect.org/github/Perfect_GH_button_1_Star.jpg)](https://github.com/PerfectlySoft/Perfect)
[![Perfect logo](http://www.perfect.org/github/Perfect_GH_button_2_Git.jpg)](https://gitter.im/PerfectlySoft/Perfect)
[![Perfect logo](http://www.perfect.org/github/Perfect_GH_button_3_twit.jpg)](https://twitter.com/perfectlysoft)
[![Perfect logo](http://www.perfect.org/github/Perfect_GH_button_4_slack.jpg)](http://perfect.ly)


[![Swift 3.0](https://img.shields.io/badge/Swift-3.0-orange.svg?style=flat)](https://developer.apple.com/swift/)
[![Platforms OS X | Linux](https://img.shields.io/badge/Platforms-OS%20X%20%7C%20Linux%20-lightgray.svg?style=flat)](https://developer.apple.com/swift/)
[![License Apache](https://img.shields.io/badge/License-Apache-lightgrey.svg?style=flat)](http://perfect.org/licensing.html)
[![Twitter](https://img.shields.io/badge/Twitter-@PerfectlySoft-blue.svg?style=flat)](http://twitter.com/PerfectlySoft)
[![Join the chat at https://gitter.im/PerfectlySoft/Perfect](https://img.shields.io/badge/Gitter-Join%20Chat-brightgreen.svg)](https://gitter.im/PerfectlySoft/Perfect?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Slack Status](http://perfect.ly/badge.svg)](http://perfect.ly)

## Issues

We are transitioning to using JIRA for all bugs and support related issues, therefore the GitHub issues has been disabled.

If you find a mistake, bug, or any other helpful suggestion you'd like to make on the docs please head over to [http://jira.perfect.org:8080/servicedesk/customer/portal/1](http://jira.perfect.org:8080/servicedesk/customer/portal/1) and raise it.

A comprehensive list of open issues can be found at [http://jira.perfect.org:8080/projects/ISS/issues](http://jira.perfect.org:8080/projects/ISS/issues)


## Linux Build Notes
* cd Sources
* make

This will automatically install the appropriate Apache development resources if required, potentially prompting you for your sudo password. You must then install the produced mod_perfect.so as you would any other Apache module.

## Configuration

Here is an Apache conf snippet which pipes files/directories which do not exist through to Perfect Server. This is handy if you are using Perfect's URL routing system (or your own system).

```
	RewriteEngine on
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule (.*) - [L,NS,H=perfect-handler]

```

Here is a sample Apache .conf snippet. This uses mod_rewrite in order to provide extension-less URLs for mustache templates.

```
<IfModule !perfect_module>
	LoadModule perfect_module /path/to/mod_perfect.so
</IfModule>

<IfModule !rewrite_module>
	LoadModule rewrite_module libexec/apache2/mod_rewrite.so
</IfModule>

### Sample vhost
<VirtualHost *:80>

	ServerName my-server.local
	DocumentRoot "/path/to/my-server/webroot"
	
	<Directory "/path/to/my-server/webroot">
		Require all granted
		DirectoryIndex index.mustache index.html
	</Directory>
	
	RewriteEngine on
	
	# unless a directory, remove trailing slash
	RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-d
	RewriteRule ^(.*)/$ $1 [R=301,L]
	
	# resolve .mustache file for extensionless mustache urls
	RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-d
	RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-f
	RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME}\.mustache -f
	RewriteRule ^(.*)$ $1.mustache [NC,PT,L]
	
	# redirect external .mustache requests to extensionless url
	RewriteCond %{THE_REQUEST} ^[A-Z]+\ /([^/]+/)*[^.#?\ ]+\.mustache([#?][^\ ]*)?\ HTTP/
	RewriteRule ^(([^/]+/)*[^.]+)\.mustache $1 [R=301,L]
	
	<Location ~ "^.*\.[Mm][Uu][Ss][Tt][Aa][Cc][Hh][Ee]$">
		SetHandler perfect-handler
	</Location>

</VirtualHost>
### Sample vhost

```



## Further Information
For more information on the Perfect project, please visit [perfect.org](http://perfect.org).
