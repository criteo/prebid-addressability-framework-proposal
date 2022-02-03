# Identifiers and optional preferences

A list of identifiers and optionally, some preferences

<table>

<tr>
    <th> Property </th>
    <th> Description </th>
</tr>

<tr>
<td>
<pre><b>preferences</b></pre>
</td>
<td>
<b>object</b>

The current preferences of the user

<table>

<tr>
<td>
<pre><b>version</b></pre>
</td>
<td>
<b>enum (of integer)</b>

A version number. To be detailed.

Can only take **one of these values**:
* `0`
</td>
</tr>

<tr>
<td>
<pre><b>data</b></pre>
</td>
<td>
<b>object</b>

<table>

<tr>
<td>
<pre><b>use_browsing_for_personalization</b></pre>
</td>
<td>
<b>boolean</b>

`true` if the user accepted the usage of browsing history for ad personalization, `false` otherwise

</td>
</tr>

</table>

</td>
</tr>

<tr>
<td>
<pre><b>source</b></pre>
</td>
<td>
<b>object</b>

Source of data representing what contracting party created and signed the data

<table>

<tr>
<td>
<pre><b>timestamp</b></pre>
</td>
<td>
<b>integer</b>

Time when data was signed

**Example:** 

```json
1643297316
```

</td>
</tr>

<tr>
<td>
<pre><b>domain</b></pre>
</td>
<td>
<b>string</b>

The domain name of the entity that signed this data

**Examples:** 

```json
"a-domain-name.com"
```

```json
"another.domain.co.uk"
```

</td>
</tr>

<tr>
<td>
<pre><b>signature</b></pre>
</td>
<td>
<b>string</b>

The base64 representation of a data signature

**Example:** 

```json
"RYGHYsBUEwMgFgOJ9aUQl7ywl4xnqdmwWIgPbaIowbXbmZAFKLa7mcBJQuWh1wEskpu57SHn2mmCF6V5+cESgw=="
```

</td>
</tr>

</table>

</td>
</tr>

</table>

</td>
</tr>

<tr>
<td>
<pre><b>identifiers</b></pre>
</td>
<td>
<b>array</b>

Type of array items:

A pseudonymous identifier generated for a web user

<table>

<tr>
<td>
<pre><b>version</b></pre>
</td>
<td>
<b>enum (of integer)</b>

A version number. To be detailed.

Can only take **one of these values**:
* `0`
</td>
</tr>

<tr>
<td>
<pre><b>type</b></pre>
</td>
<td>
<b>enum (of string)</b>

The identifier type, identifier of type `paf_browser_id` is mandatory and is "pivot"

Can only take **one of these values**:
* `"paf_browser_id"`
</td>
</tr>

<tr>
<td>
<pre><b>persisted</b></pre>
</td>
<td>
<b>boolean</b>

If set to `false`, means the identifier has not yet been persisted as a cookie.<br>Otherwise, means this identifier is persisted as a PAF cookie<br>(default value = `true` meaning if the property is omitted the identifier *is* persisted)

</td>
</tr>

<tr>
<td>
<pre><b>value</b></pre>
</td>
<td>
<b>string</b>

The identifier value

**Example:** 

```json
"7435313e-caee-4889-8ad7-0acd0114ae3c"
```

</td>
</tr>

<tr>
<td>
<pre><b>source</b></pre>
</td>
<td>
<b>object</b>

Source of data representing what contracting party created and signed the data

<table>

<tr>
<td>
<pre><b>timestamp</b></pre>
</td>
<td>
<b>integer</b>

Time when data was signed

**Example:** 

```json
1643297316
```

</td>
</tr>

<tr>
<td>
<pre><b>domain</b></pre>
</td>
<td>
<b>string</b>

The domain name of the entity that signed this data

**Examples:** 

```json
"a-domain-name.com"
```

```json
"another.domain.co.uk"
```

</td>
</tr>

<tr>
<td>
<pre><b>signature</b></pre>
</td>
<td>
<b>string</b>

The base64 representation of a data signature

**Example:** 

```json
"RYGHYsBUEwMgFgOJ9aUQl7ywl4xnqdmwWIgPbaIowbXbmZAFKLa7mcBJQuWh1wEskpu57SHn2mmCF6V5+cESgw=="
```

</td>
</tr>

</table>

</td>
</tr>

</table>

</td>
</tr>

</table>
