---
title: "ModuleNotFoundError: Even when selenium is installed using pip!"
seoDescription: "Installing the selenium package through source instead of pip3 will **not** help!"
datePublished: Sat Dec 31 2022 16:09:36 GMT+0000 (Coordinated Universal Time)
cuid: clcc537pj000008mfdp6xhr4p
slug: modulenotfounderror-even-when-selenium-is-installed-using-pip
tags: python, selenium, scraping, pip

---

Generally, people do lots of timepass explaining how they tried various things and failed but I would like to save time for both parties(reader and writer).

While building [Hunter](https://github.com/amit0617/hunter), I faced this problem using jupyter notebook(first ever time). I tried using the same in a python script and found it was working from that. So you must install selenium inside the jupyter notebook before using that.

```python
%pip3 install selenium
```

```python
from selenium.webdriver import Firefox
...
...
```

Also for some reason, Chrome was crashing for me. So check that for your case yourself. That's it. Thank you.

P.S. some people will explain(actually disguise) that installing through package file works but not through pip. That doesn't make sense and is useless. One important thing is if you are using a virtual environment from python. Then there is a possibility that the kernel for jupyter environment is global and hence would not detect your package installed inside the environment. The above method will install the package in whatever environment your jupyter notebook is running and hence will detect the selenium module.