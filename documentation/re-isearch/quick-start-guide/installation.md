# ⬇ Installation

{% hint style="danger" %}
In this reduced guide we assume you have g++/gcc-10 installed and are running Ubuntu x86 64-bit. If you run into any problems when building the source or want to customize your installation, please refer to the [handbook](../handbook/ "mention") for further information.
{% endhint %}

### Prerequisites

**Required:**

* libdl - This is used for dynamic loading. It should be part of your core OS.
* libz In package zlib. It should be part of your core OS.

**You may want and/or need:**

* libmagic (in Package: libmagic-dev )
* Berkeley DB (in Package: libdb-dev)

<details>

<summary>Clone the Repository</summary>

Pull source from `master` using

```bash
git clone https://github.com/re-Isearch/re-Isearch.git
```

</details>

<details>

<summary>Build</summary>

Go into the `build/` directory

```bash
cd re-Isearch/build/
```

Run the Makefile

```bash
Make
```

</details>
