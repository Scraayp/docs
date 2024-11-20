---
id: building-a-php-extension
title: Building a PHP Extension
sidebar_label: Building a PHP Extension
---

import useBaseUrl from '@docusaurus/useBaseUrl';

On this site, you find a **step-by-step guide** on how to build a **PHP Extension** for a specific **PHP Version**.

## Building a PHP Extension

1. Make a **backup** of your instance.

2. Install the **dev** package for the **PHP Version**.

```
apt update && apt -y install php8.4-dev
```

3. Go to [https://pecl.php.net/](https://pecl.php.net/) and search for the extension like [ssh2](https://pecl.php.net/package/ssh2):

<img class="border" src={useBaseUrl('img/php/guides/building-a-php-extension/download-extension.png?v=1')} />

4. Download the **tgz** via **cURL**:

```
curl -O https://pecl.php.net/get/ssh2-1.4.1.tgz
```

5. Extract the **tgz**:

```
tar xf ssh2-1.4.1.tgz
```

6. **phpize** the extension:

```
cd ssh2-1.4.1
phpize8.4
```

7. **Compile** and **build** the extension:

```
./configure --with-php-config=/usr/bin/php-config8.4
make
make install
```

8. Register the extension for **CLI** and **FPM** in the **php.ini**:

**CLI**

```
nano /etc/php/8.4/cli/php.ini
```

**Add the following line at the end:**

```
extension=ssh2.so
```

**FPM**

```
nano /etc/php/8.4/fpm/php.ini
```

**Add the following line at the end:**

```
extension=ssh2.so
```

9. Restart the **PHP-FPM** service:

```
systemctl restart php8.4-fpm
```

## Testing

After registering the **PHP Extension**, you can check if the extension is loaded for **CLI** and **FPM** correctly.

### CLI

You can use **grep** to check if the extension is loaded:

```
php8.4 -m |grep 'ssh2' 
```

If you don't get an output, then the extension is **NOT** loaded.

### FPM

To check if the extension is loaded for **FPM**, you can check the **phpinfo**.

1. Create the file **t.php** in the **root directory**.

2. Put the following content in the file:

```php
<?php
phpinfo();
```

3. Open **t.php** in your browser and search for the **extension**.

<img class="border" src={useBaseUrl('img/php/guides/building-a-php-extension/php-info.png?v=1')} />

If you don't find information about the extension, then it's **NOT** loaded.


