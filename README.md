# Perfect - mod_perfect - Apache 2.4 Connector

<p align="center">
    <a href="http://perfect.org/get-involved.html" target="_blank">
        <img src="http://perfect.org/assets/github/perfect_github_2_0_0.jpg" alt="Get Involed with Perfect!" width="854" />
    </a>
</p>

<p align="center">
    <a href="https://github.com/PerfectlySoft/Perfect" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_1_Star.jpg" alt="Star Perfect On Github" />
    </a>  
    <a href="http://stackoverflow.com/questions/tagged/perfect" target="_blank">
        <img src="http://www.perfect.org/github/perfect_gh_button_2_SO.jpg" alt="Stack Overflow" />
    </a>  
    <a href="https://twitter.com/perfectlysoft" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_3_twit.jpg" alt="Follow Perfect on Twitter" />
    </a>  
    <a href="http://perfect.ly" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_4_slack.jpg" alt="Join the Perfect Slack" />
    </a>
</p>

<p align="center">
    <a href="https://developer.apple.com/swift/" target="_blank">
        <img src="https://img.shields.io/badge/Swift-3.0-orange.svg?style=flat" alt="Swift 3.0">
    </a>
    <a href="https://developer.apple.com/swift/" target="_blank">
        <img src="https://img.shields.io/badge/Platforms-OS%20X%20%7C%20Linux%20-lightgray.svg?style=flat" alt="Platforms OS X | Linux">
    </a>
    <a href="http://perfect.org/licensing.html" target="_blank">
        <img src="https://img.shields.io/badge/License-Apache-lightgrey.svg?style=flat" alt="License Apache">
    </a>
    <a href="http://twitter.com/PerfectlySoft" target="_blank">
        <img src="https://img.shields.io/badge/Twitter-@PerfectlySoft-blue.svg?style=flat" alt="PerfectlySoft Twitter">
    </a>
    <a href="http://perfect.ly" target="_blank">
        <img src="http://perfect.ly/badge.svg" alt="Slack Status">
    </a>
</p>

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
