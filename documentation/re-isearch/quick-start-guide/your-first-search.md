# 🎂 Your First Search

<details>

<summary>Installation</summary>

Follow [#installation](your-first-search.md#installation "mention").

</details>

<details>

<summary>Step 2</summary>

In the binary directory (../bin/ from the prespective of the buiild/ directory) you'll find a number of command line tools.

* Iindex
* Isearch
* ...

The Iindex command is used to create indexes.

Example:

```
Iindex -d /tmp/FOO shakespeare.xml
```

will create an index called "FOO" in the /tmp directory containing the contents of the XML file shakespeare.xml

The Iindex command has a lot of options.

</details>

<details>

<summary>Step 3</summary>

To search the FOO index use use the command line tool Isearch

```
Isearch -d /tmp/FOO to be or not to be
```

This will perform a "smart search" in the index defined in /tmp/FOO for the ordered words: to be or not to be

</details>

<details>

<summary>Step 4</summary>

The engine supports not just "smart search" but a number of other query languages as well as a large number of ranking functions. Please read the documentation for additional details.

</details>

For more detailed information, check out the Design Document:

{% content-ref url="../handbook/design-document.md" %}
[design-document.md](../handbook/design-document.md)
{% endcontent-ref %}
