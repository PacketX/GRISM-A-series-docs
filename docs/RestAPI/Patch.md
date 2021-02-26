Patch
=============

Patch is to revise a [`<filter>`](Element/run/filter.md) tag on existing task. Before doing patching a filter, must have an existing XML task.

Supported operation:

* `add`

<h2 id="patch_filter">Filter</h2>

|          |          Patch Filter          |
|:--------:|:------------------------------:|
|  Method  |              Patch             |
|   Path   |      /submit/patch/filter      |
|   Query  |                                |
|   Form   |   { "Base64Filter": "xxxxx" }  |
| Response | {} or {"ErrorMessage": "xxxx"} |

Let the XML task be like:

```
<run>

<filter id="1">
<or>
    <find name="ip.dst" relation="==" content="8.8.8.8"/>
</or>
</filter>

<regular>

<chain>
    <in>P1</in>
    <fid>F1</fid>
    <out>P2</out>
</chain>

</regular>

</run>
```

As same as the API [`/submit/renew`](RestAPI/Submit.md#renew), encode as `base64`.

```
ENCODED=$(cat <<'EOF' | base64
{
    "Operation": "add",
    "FilterId": 1,
    "Find": [
        {
            "Name": "ip.src",
            "Relation": "==",
            "Content": [
                "2.2.2.2"
            ],
            "IdleTimeout": 30,
            "HardTimeout": 60
        },
        {
            "Name": "ip.dst",
            "Relation": "==",
            "Content": [
                "3.3.3.3"
            ],
            "IdleTimeout": 30,
            "HardTimeout": 60
        },
        {
            "Name": "ip.addr",
            "Relation": "==",
            "Content": [
                "4.4.4.4",
                "5.5.5.5"
            ]
        }
    ]
}
EOF
)
```

PATCH it.

```
$ curl -k \
    -X PATCH \
    -H "Content-Type: application/json" \
    -H "X-PacketX-Username: packetx" \
    -d "{ \"Base64Filter\": \"$ENCODED\" }" \
    https://${IP_ADDR}/submit/patch/filter \
    -b cookie.txt
```

**Don't forget to cleanup**

```
$ rm -f cookie.txt
```

<h2>Timeout</h2>

As you noticed, there is two directives: `IdleTimeout` and `HardTimeout`

* `IdleTimeout`: The flow to expire after the given number of seconds of inactivity.
* `HardTimeout`: The flow to expire after the given number of seconds, regardless of activity.

If don't need that, just give it to `0` or omit it.

<h2>Full Script</h2>

```
#!/bin/sh

IP_ADDR=192.168.1.140

# login
curl -k \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"Username":"packetx", "PasswordHash":"xxxxx"}' \
    -c cookie.txt \
    https://${IP_ADDR}/direct_login

# encode as base64
ENCODED=$(cat <<'EOF' | base64
{
    "Operation": "add",
    "FilterId": 1,
    "Find": [
        {
            "Name": "ip.src",
            "Relation": "==",
            "Content": [
                "2.2.2.2"
            ],
            "IdleTimeout": 30,
            "HardTimeout": 60
        },
        {
            "Name": "ip.dst",
            "Relation": "==",
            "Content": [
                "3.3.3.3"
            ],
            "IdleTimeout": 30,
            "HardTimeout": 60
        },
        {
            "Name": "ip.addr",
            "Relation": "==",
            "Content": [
                "4.4.4.4",
                "5.5.5.5"
            ]
        }
    ]
}
EOF
)

# patch
curl -k \
    -X PATCH \
    -H "Content-Type: application/json" \
    -H "X-PacketX-Username: packetx" \
    -d "{ \"Base64Filter\": \"$ENCODED\" }" \
    https://${IP_ADDR}/submit/patch/filter \
    -b cookie.txt

# cleanup
rm -f cookie.txt
```
