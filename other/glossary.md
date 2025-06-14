---
icon: spell-check
---

# Glossary

## A.

<details>

<summary><mark style="color:blue;"><strong>App</strong></mark></summary>

_A ByteNite App is a custom software program consisting of application scripts and configuration files, designed by the user to operate on the ByteNite platform. An app can integrate with a partitioner and an assembler to enhance its distributed processing capabilities._&#x20;

</details>

<details>

<summary><mark style="color:blue;"><strong>Assembler</strong></mark></summary>

_An Assembler is a custom software program designed by the user to handle post-processing and fan-in operations on the ByteNite platform._&#x20;



**Additional Info:**

* AKA: <mark style="color:blue;">**Assembling Engine**</mark>

**Related Guides:**

* [assembling-engines.md](../create-with-bytenite/building-blocks/assembling-engines.md "mention")
* [building-blocks](../create-with-bytenite/building-blocks/ "mention")

</details>



## B.

<details>

<summary><mark style="color:blue;"><strong>ByteChip</strong></mark></summary>

_ByteChip is the internal currency used within the ByteNite platform for measuring and trading computing services. It is the standard unit to evaluate computing throughput provided by task runners. One ByteChip is equivalent to the price of an hour of computation on a machine with 1 CPU core and 1 GiB of RAM._



**Additional Info:**

* Abbreviation/ticker: <mark style="color:blue;">**BYC**</mark>
* Symbol: <img src="../.gitbook/assets/ByteChips-symbol-iconsize.png" alt="" data-size="line">
* ByteChips are **not** cryptocurrency

</details>



## P.

<details>

<summary><mark style="color:blue;"><strong>Partitioner</strong></mark></summary>

_A Partitioner is a custom software program designed by the user to handle pre-processing and fan-out operations on the ByteNite platform._&#x20;



**Additional Info:**

* AKA: <mark style="color:blue;">**Partitioning Engine**</mark>

**Related Guides:**

* [partitioning-engines.md](../create-with-bytenite/building-blocks/partitioning-engines.md "mention")
* [building-blocks](../create-with-bytenite/building-blocks/ "mention")

</details>



## T.

<details>

<summary><mark style="color:blue;"><strong>Tag</strong></mark></summary>

_In ByteNite, a tag is a string used to identify a record by version. The tag format includes a required record name and an optional version in the format: `[name]@[major].[minor]` . The minor version is optional. Tag behavior follows these principles:_

*   _`[name]@[major].[minor]`_

    _Retrieves the record matching the exact name, major version, and minor version._
*   _`[name]@[major]`_

    _Retrieves the record matching the exact name and major version, with the highest minor version._
*   _`[name]`_

    _Retrieves the record by name with the highest major and minor versions._

</details>
