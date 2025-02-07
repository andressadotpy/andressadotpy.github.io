---
layout: post
title: How to run Playwright in Fedora with headed mode
tags: [Python, Fedora, Playwright, Pytest]
---

If you arrived here you are probably searching about how to run this amazing (highly popular) new framework - *Playwright* - and the fact that it's not currently supported by Fedora. Or you ended up installing it and getting this stack error message:

```bash
>           raise rewrite_error(error, f"{parsed_st['apiName']}: {error}") from None
E           playwright._impl._errors.Error: BrowserType.launch: 
E           ╔══════════════════════════════════════════════════════╗
E           ║ Host system is missing dependencies to run browsers. ║
E           ║ Missing libraries:                                   ║
E           ║     libicudata.so.66                                 ║
E           ║     libicui18n.so.66                                 ║
E           ║     libicuuc.so.66                                   ║
E           ║     libjpeg.so.8                                     ║
E           ║     libwebp.so.6                                     ║
E           ║     libffi.so.7                                      ║
E           ║     libx264.so                                       ║
E           ╚══════════════════════════════════════════════════════
```

## So, what's the matter?

Right now Fedora is not officially supported by Playwright. Running it inside Docker is what developers are usually doing in such a scenario (you can read more about it in this [issue](https://github.com/microsoft/playwright/issues/29559)).

When using *pytest-playwright* the issue only starts happening when the tests run in headed mode. We can see some libraries missing from that stack error.

## Install dependencies for Fedora 41

```bash
sudo dnf install -y libicu \
    libjpeg-turbo \
    libwebp \
    flite \
    pcre \
    libffi \
    mesa-libgbm \
    libdrm \
    xorg-x11-server-Xvfb \
    atk \
    at-spi2-atk \
    libXcomposite \
    libXdamage \
    libXrandr \
    libXtst \
    cups-libs \
    libxcb \
    libXScrnSaver \
    alsa-lib \
    pango \
    cairo \
    nss

sudo dnf install -y gtk3 \
    libnotify \
    liberation-fonts \
    wqy-zenhei-fonts \
    liberation-sans-fonts \
    liberation-serif-fonts
```

## Running pytest-playwright in headed mode

With the dependencies installed, now it's possible to run **pytest-playwright** in **headed** mode.

First make sure to `pip install pytest-playwright`.

Add the simple following test in the tests folder with a nomenclature that pytest will be able to find it, e.g `tests/test_playwright_page.py`.

```python
import re
from playwright.sync_api import Page, expect

def test_has_title(page: Page):
    page.goto("https://playwright.dev/")

    expect(page).to_have_title(re.compile("Playwright"))
```

The command to run this small test in headed mode is `pytest --headed`. You should be able to see a small window open and shutting real fast and a similar output in your terminal:

```bash
$ pytest --headed
======================= test session starts =======================
platform linux -- Python 3.12.8, pytest-8.3.4, pluggy-1.5.0
rootdir: /home/acabista/Documents/automations/web-automation-framework
configfile: pyproject.toml
plugins: anyio-4.8.0, base-url-2.1.0, playwright-0.7.0
collected 1 item                                                  

tests/test_playwright_page.py .                             [100%]

======================== 1 passed in 0.95s ========================
```

## Conclusion

I hope this helped you with your Playwright setup in Fedora. If it didn't, there are many discussions happening in the [GitHub issues page for Playwright](https://github.com/microsoft/playwright/issues) about the missing support for Linux OS and the community is posting a lot of different solutions in the comments. I recommend you to take a look at the issues to check for different solutions.
