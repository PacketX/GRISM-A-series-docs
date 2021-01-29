Filter
============

`<filter>` tag described [`<find>`](filter/find.md) tags set.

<h2>Attribute</h2>

| Attribute |       Description       |   Type  | Must |
|:---------:|:-----------------------:|:-------:|:----:|
|     id    | Unique ID in whole task | Integer |  Yes |

<h2>Condition</h2>

`<or>` tag is the only child element of `<filter>` tag.

<h2>Example</h2>

```
<run>

<filter id="1">
<or>

...

</or>
</filter>

</run>
```

<h2>Usage</h2>

`<filter id="1">` will be compiled as `F1`. Using by `<fid>F1</fid>` form.
